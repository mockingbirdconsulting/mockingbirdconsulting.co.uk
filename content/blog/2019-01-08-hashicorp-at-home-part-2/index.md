---
title: "Hashicorp at Home - Part 2"
date: 2019-01-08T06:33:12+01:00
description: 'Part two in our series on building a home lab'
image: /blog/2019-01-08-hashicorp-at-home-part-2/NomadConsole.png
tags: 
  - Automation 
  - Hashicorp 
  - IoT 
resources:
  - name: header
    src: NomadConsole.png
  - name: theorderofthings
    src: TheOrderOfThings.png
  - name: datadogdocker
    src: dockermonitoring.png
  - name: consulsvc
    src: consularchref.png
  - name: vaultarch
    src: vaultarch.png
---

We're [Hashicorp partners](https://www.hashicorp.com/partners#mockingbird-consulting), however up until now we've kept our experience and deployments
firmly in the cloud. We wondered if the Hashicorp tooling would help us in
other situations, and found that it performed even better than we had hoped.

Matt Wallace (one of our Directors) decided to take the plunge over the
Christmas and New Year break and apply the Hashicorp stack (plus a few other
goodies) to his home management setup...

In [part one](/blog/2019-01-05-hashicorp-at-home/) I covered the technology
I'd be using and the architecture. In this post I'll go into more detail about
how each component is configured, including configuration files and why I've
taken those particular decisions.

* * *

#### The naming of parts

There's an [old
joke](https://twitter.com/codinghorror/status/506010907021828096?lang=en) in
the IT industry which goes "There are two hard things in computer science:
cache invalidation, naming things, and off-by-one errors."

The need for the rest of the family to be able to access the homeassistant.io
control panel, 3D Printer Server, or other things on the network means that I
needed to resolve services by hostname rather than by IP address, which meant
I had to tackle the picking of a domain.

Think very carefully about what you're going to use as a domain. Once it's
been set, it's very difficult to change it without a rebuild of Consul

To keep things simple, I decided on a one-word domain which would be easy to
remember, however it's just as easy to stick with the default ".consul" domain
and we'll use that in our examples here.

* * *

#### The order of things...

{{< bundle-image  alt="The order in which things need to be installed" name="theorderofthings" >}}

Although this is a home network/lab setup, we still need to ensure that we
install the components in the correct order. To do this, I'm using
[Ansible](https://ansible.com) and the following roles:

  * [Consul](https://galaxy.ansible.com/brianshumate/consul)
  * [Nomad](https://galaxy.ansible.com/brianshumate/nomad)
  * [Vault](https://galaxy.ansible.com/brianshumate/vault)
  * [Docker](https://galaxy.ansible.com/geerlingguy/docker)
  * [Traefik](https://galaxy.ansible.com/kibatic/traefik)
  * [DataDog](https://galaxy.ansible.com/DataDog/datadog)

(The installation and configuration of DNSMasq is carried out by the Consul
role.)

The process of installation follows what has become my standard method of
deployment over the years - install the base operating system (in this case
[CentOS Linux](https://www.centos.org)), then install monitoring so we know
when something goes wrong, and then install everything else.

* * *

#### The code

The code for the project is [available on
Github.com](https://github.com/mockingbirdconsulting/HashicorpAtHome) so we
won't replicate it here, however we will explore each of the components in
turn, discussing the configuration changes and smoe of the reasoning behind
things on the way.

##### _The Operating System_

I've been using Linux as my primary operating system both at home and at work
for nearly 20 years, so this server was always going to run Linux. The
decision was then which flavour to use.

Many "home linux server" tutorials will suggest that you use Ubuntu, and
Canonical's ease of use and setup has certainly made it the desktop OS of
choice for thousands of users around the world, however in my professional
life I encounter far more companies using either RedHat Enterprise Linux
(RHEL), or the Open Source community distribution CentOS which is based on
RHEL.

I wanted this server to be as close to the production environments that I
manage as possible so I chose CentOS. I also chose to set
[SELinux](https://access.redhat.com/documentation/en-
us/red_hat_enterprise_linux/7/html/selinux_users_and_administrators_guide/index)
to "Enforcing" mode, because this is our default setup for client solutions.

##### _The Monitoring_

As mentioned in part 1 of this series, we're
[Datadog](https://www.datadoghq.com/) partners, so it made sense to use
Datadog to monitor the server.

The wide range of plugins available includes all of the components of this
particular platform, and as you get five hosts with ten containers per host as
part of the "free" tier, it wasn't going to cost us any extra either.

{{< bundle-image  alt="Part of the docker dashboard in DataDog" name="datadogdocker" >}}

The DataDog documentation on enabling monitoring for
[Docker](https://docs.datadoghq.com/integrations/docker_daemon/),
[Consul](https://docs.datadoghq.com/integrations/consul/),
[Nomad](https://docs.datadoghq.com/integrations/nomad/),
[Nomad](https://docs.datadoghq.com/integrations/docker_daemon/),
[Vault](https://docs.datadoghq.com/integrations/vault/), and
[Traefik](https://docs.datadoghq.com/integrations/traefik/) is excellent, so
we won't go into the detail of configuring monitoring for the services apart
from to remind you to set your `datadog_api_key` variable in your Ansible
vault secrets file as stated in [README
file](https://github.com/mockingbirdconsulting/HashicorpAtHome/blob/master/README.md).

##### _Consul and Service Discovery_

Consul is the magic that brings everything together. Instead of trying to
remember whether the 3d printer is at 192.168.1.2 or 192.168.1.90, Consul's
built-in service discovery via DNS means that anything registered with Consul
appears at `<service_name>.service.consul`, so as long as you know what the
service is called, you can easily find it in a browser.

There are a number of ways you can register a service with Consul, including
via a Consul Agent which runs on each host, or via calls to the Consul HTTP
API, however in our case because we're managing our containers using Nomad we
get service registration for free!

{{< bundle-image  alt="How Consul discovers services" name="consulsvc" >}}

The key to the automatic service discovery is [integrating Nomad and
Consul](https://www.nomadproject.io/docs/configuration/consul.html), and then
adding a `service` stanza to your Nomad job description like the one I've
created for Octoprint:

    
    
            
          service {
            name = "octoprint"
            tags = ["octoprint"]
            port = "octoprint"
            check {
              name     = "alive"
              type     = "tcp"
              interval = "10s"
              timeout  = "2s"
            }
          }
            
          

There will be more about creating a Nomad job later in the post, however this
snippet takes a predefined network port and registers it as a named service
within Consul along with a basic TCP connection check to ensure that the end-
point is still alive, allowing us to see the Octoprint UI at
`http://octoprint.service.consul`.

Finally, the Ansible play configures DNSMasq (a lightweight DNS server) to
listen on port 53 and forward requests to Consul, allowing us to reference the
host machine as a primary DNS server on our network and "automatically" lookup
the services we register.

##### _Vault and Secrets Management_

Vault isn't strictly necessary for my home setup right now however as this
will also be a lab for playing with new technology, it made sense to add it
from the beginning rather than retro-fit it.

As with the other tools we install Vault using Ansible, however when it gets
to the point that `Vault API reachable?` is being repeated on the screen, you
will need to SSH in to the server and run the [`operator
init`](https://www.vaultproject.io/docs/commands/operator/init.html) and
[`operator
unseal`](https://www.vaultproject.io/docs/commands/operator/unseal.html)
commands.

{{< bundle-image  alt="How Vault Works" name="vaultarch" >}}

You will need to run the `operator unseal` process each and every time you
reboot the server or restart the `vault` service, so make sure you store the
keys generated by `operator init` somewhere safe!

Once the vault is unsealed, we need to generate a token for Nomad to use. This
allows Nomad and the containers that it orchestrates to create and access
secrets in Vault including passwords, certificates, API Keys, and pretty much
anything else you want to be able to store.

In the first instance, we want to make sure that Vault and Nomad are talking
to each other without having to troubleshoot Vault policies, so we'll use the
root token that was generated for us as part of the `init` process.

Switching to Vault Policies is something that will be addressed in a future
blog post as it is too involved to include here, you should **never** grant
Nomad root access to Vault in a production environment.

Add the vault root token to the Ansible configuration by creating an [Ansible
Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html) file at
`inventory/group_vars/docker_instances` with the following content:

    
    
                
    ---
    nomad_vault_token: <The Vault Root Token>
                
             

Now re-run Ansible against the server, Nomad will be configured with the
appropriate token, and you will be able to authenticate against Vault and
retrieve the secrets.

##### _Containers and Orchestration_

Although Docker and Nomad aren't at the same level on our order of precedence
diagram, they are so tightly connected that we'll discuss them both here.

Many people are familiar with the Docker/Moby project and related frameworks
even if it's only through packaging and running a "Hello World" container to
test things out, however when you start to run hundreds of containers across
multiple hosts you start to run into issues with how to maintain and
orchestrate those workloads.

Docker is probably the most popular container format out there, so it made
sense for me to use it as well. The `Dockerfile` format is reasonably well
documented, and the [Docker Hub](https://hub.docker.com/) contains pre-built
containers for most of the software you'd ever need to run, however when we've
run the Docker Swarm orchestration layer in production environments in the
past, we've had issues with changes not being backwards compatible and a
release cycle that's so fast it's difficult to get support.

As I wasn't going to use Docker Swarm, and I'd ruled out Kubernetes as well, I
settled on Nomad as my orchestration tool.

Both Docker and Nomad were installed using Ansible, and as Nomad provides
[native support for
Docker](https://www.nomadproject.io/docs/drivers/docker.html) no configuration
was required.

Why not Kubernetes? I considered using Kubernetes because it seemed to be the
most popular choice, however it's designed for a large cluster of hosts and
whilst there are scripts available to get it all running on one host it felt
like to many moving parts for this particular use case.

I've put some example Nomad configuration files into the git repository,
however as an example of running the Redis database server on Docker, you just
define a task as follows:

    
    
                
    task "webservice" {
      driver = "docker"
    
      config {
        image = "redis:3.2"
        labels {
          group = "webservice-cache"
        }
      }
    }
                
            

Now when you run `nomad job run` against the file, it will schedule an
instance of the Redis container onto the host.

##### _Traefik_

Traefik is billed as a "dynamic, automated, cloud native edge router". That's
a lot of buzzwords, but essentially it will sit facing the public access
points to your network and provide reverse proxying and load balancing from
DNS entries to the provided hosts and ports on the network behind it.

One of the main reasons for chosing Traefik over Nginx or other options is
that it has native support for Consul as a service catalog, and will add and
remove new services without needing a restart. I've found in the past that
even when using `consul-template` and Nginx there can be brief outages when
adding and removing new services.

Configuring Traefik is pretty easy, you just need to set the Consul Catalog
and point it to the Consul cluster. Traefik takes over from there and any
service that registers in Consul will automatically be served over http (or
another protocol if configured that way).

It is possible to configure Traefik to use LetsEncrypt to serve TLS traffic
(HTTPS etc), however in order to do that you need to either expose Traefik to
the internet or have a domain that will resolve externally from the house.

As we have neither of these in our example (and I don't intend to allow
traffic from the internet into the house other than via a VPN connection, we
will skip this configuration for now, however the [documentation on the
Traefik website](https://docs.traefik.io/user-guide/docker-and-lets-encrypt/)
is excellent and includes how to [link into most DNS providers
automatically](https://docs.traefik.io/configuration/acme/#dnschallenge) if
you choose the DNS Challenge route.

The only thing left to do is configure your router to set `.consul` as your
local domain, and ensure that the DHCP settings set your new host as the
primary DNS server for all devices on the network.

###### A sample Traefik configuration file

    
    
                
    {#
        Check traefik sample for full config options
        https://raw.githubusercontent.com/containous/traefik/master/traefik.sample.toml
    #}
    
    # Entrypoints to be used by frontends that do not specify any entrypoint.
    # Each frontend can specify its own entrypoints.
    #
    # Optional
    # Default: ["http"]
    #
    defaultEntryPoints = ["http"]
    
    # Entrypoints definition
    #
    # Optional
    # Default:
    [entryPoints]
        [entryPoints.http]
        address = ":80"
    
    logLevel = "INFO"
    
    [docker]
    endpoint = "unix:///var/run/docker.sock"
    domain = "consul"
    watch = true
    
    [consul]
    endpoint = "127.0.0.1:8500"
    watch = true
    prefix = "traefik"
    
    [consulCatalog]
    endpoint = "127.0.0.1:8500"
    domain = "service.consul"
    exposedByDefault = true
    prefix = "traefik"
    
    [api]
      entryPoint = "traefik"
      dashboard = true
      debug = true
                
            

* * *

#### The summary

Over the past two blog posts, we've looked at setting up a home network/lab
server using the Hashicorp stack and Traefik. If you've followed along and
used the Ansible playbooks as well as the example Traefik configuration, you
should now have Vault, Consul, Nomad, Docker, and Traefik all running on a
single host and automatically publishing services that are registered in
Nomad.

We've added a sample Nomad job configs to the [git
repository](https://github.com/mockingbirdconsulting/HashicorpAtHome) to get
you up and running with [Home Assistant](https://homeassistant.io), and we'd
welcome any improvements you might have to the repository or these articles so
please do log [Github
issues](https://github.com/mockingbirdconsulting/HashicorpAtHome/issues) and
we'll get to fixing/adding things as soon as we can.

