--- 
title: "Hashicorp at Home"
date: 2019-01-02T06:33:12+01:00 
description: 'What happens when you take Hashicorp out of the cloud and on to a home network?'
summary: 'What happens when you take Hashicorp out of the cloud and on to a home network?'
image: /blog/2019-01-05-hashicorp-at-home/homeautomationTheoldway.jpg
tags:
  - Automation 
  - Hashicorp
  - IoT
resources:
  - name: header
    src: homeautomationTheoldway.jpg
  - name: vaultlogo
    src: vault.png
  - name: consullogo
    src: consul.png
  - name: nomadlogo
    src: nomad.png
  - name: traefiklogo
    src: traefik.png
  - name: datadoglogo
    src: datadog.png
  - name: minicloudnetwork
    src: MiniCloud.png

---
We're [Hashicorp partners](https://www.hashicorp.com/partners#mockingbird-consulting), however up until now we've kept our experience and deployments
firmly in the cloud. We wondered if the Hashicorp tooling would help us in
other situations, and found that it performed even better than we had hoped.

Matt Wallace (one of our Directors) decided to take the plunge over the
Christmas and New Year break and apply the Hashicorp stack (plus a few other
goodies) to his home management setup...

## The challenge

Up until very recently, all of the home management devices that I used were
running on a multitude of Raspberry Pi's or similar devices. I had one pi for
[Home Assistant](https://www.home-assistant.io/) to run my home automation,
another for [NXFilter](https://www.nxfilter.org) to keep my kids safe online,
more still to manage my 3D printer and CNC Machine, and a couple connected to
the network "just in case" for experimentation purposes.

I'd mounted them on a bit of 9mm Plywood, along with power supplies and a
network switch, and it had been working fine, until I upgraded the networking
kit to Ubiquiti Unifi.

All of a sudden, I had managed switches that would deal with 1Gbps traffic,
and the number of devices connecting via WiFi was increasing too. The home
management side of things needed an upgrade to match the new challenges.

* * *

## The idea

I've been working in Systems Administration and automation for nearly 20 years
so I knew that whatever I replaced the Raspberry Pi's with, it would need to
be easy to maintain and even easier to add to.

### The criteria

* It must be easy to maintain
* Everything should resolve via DNS (No more having to remember which IP address the 3d Printer was on!)
* Everything should be available via either HTTP or HTTPS (No more having to remember which port a service resides on either!)
* Whilst the data should remain "behind the firewall", deploying new services should be as easy as using AWS, Azure, Google Cloud or some other cloud provider
* Some of the data used in configuration files is sensitive, and therefore needs to be kept encrypted

* * *

## The Solution

As much as I enjoy configuring software and infrastructure during the working
day, when I'm at home I just want things to work without intervention.

The idea that I could run a "mini cloud" and launch the various applications
into it gathered pace and I found myself evaluating several options including
Openstack (far too powerful for this particular use-case!), Kubernetes
(probably complete overkill because I don't need the distributed clustering
etc), Docker Swarm (I've been bitten hard by this in a production environment
in the past, so didn't really want to experience those pains again!), before
finally turning to the Hashicorp stack.

Hashicorp's stack comprises of [Vault](https://vaultproject.io/) for managing
secrets, [Consul](https://consul.io) for managing DNS and service discovery,
and [Nomad](https://nomadproject.io/) for managing containers and other jobs.
That solved the "private cloud" aspect so all I needed now was a way to map
the various ports to HTTP(S) without thinking about it, and that's where
[Traefik](https://traefik.io/) came in to the equation.

{{< bundle-image  alt="Hashicorp Vault Logo" name="vaultlogo" >}}

Vault tightly controls access to secrets and encryption keys by authenticating
against trusted sources of identity such as Active Directory, LDAP,
Kubernetes, CloudFoundry, and cloud platforms. Vault enables fine grained
authorization of which users and applications are permitted access to secrets
and keys.

In the past, we've used it to secure all kinds of information from
Cryptocurrency-based Escrow systems to one-time-passwords for major financial
institutions. My plan was to replace any credentials that might need to be
stored in a container or in Ansible with a call to the Vault API.

{{< bundle-image  alt="Hashicorp Consul Logo" name="consullogo" >}}

Consul is a service mesh solution providing a full featured control plane with
service discovery, configuration, and segmentation functionality. Each of
these features can be used individually as needed, or they can be used
together to build a full service mesh.

We've used Consul extensively in the past for everything from scaling out
logging clusters through to service discovery for clustered IoT solutions, so
I knew it would easily cope with anything my home network required.


{{< bundle-image  alt="Hashicorp Nomad Logo" name="nomadlogo" >}}

HashiCorp Nomad is a single binary that schedules applications and services on
Linux, Windows, and Mac. It is an open source scheduler that uses a
declarative job file for scheduling virtualized, containerized, and standalone
applications.

I'd used Nomad previously when advising a FinTech organisation on their
infrastructure and CI/CD platform, and I fell in love with the powerful
configuration language almost immediately! (more on that later...)

{{< bundle-image  alt="The Traefik Logo" name="traefiklogo" >}}

A reverse proxy / load balancer that's easy, dynamic, automatic, fast, full-
featured, open source, production proven, provides metrics, and integrates
with every major cluster technology, Traefik is a relative newcomer to the
world of reverse proxies.

{{< bundle-image  alt="The Datadog Logo" name="datadoglogo" >}}

We're a busy household, so I also wanted to know when there was an issue with
the system. Naturally, as we're also DataDog partners, I installed the DataDog
and configured the integrations for Consul, Vault, Docker, and Traefik so I
could monitor and get alerts if something failed.

* * *

## The Architecture

The architecture of the system based on the criteria was relatively simple.
The "core" services would run on the physical host, with everything else
running in containers.

This meant installing Consul, Docker, Nomad, Vault, and Traefik onto the
server, and then writing Nomad jobs for HomeAssistant, OctoPi, and the other
services to take advantage of the underlying "infrastructure". It also allows
me to "extend" the platform in future by adding more physical servers and
installing Docker/Nomad on them to join them into a cluster of container
nodes.

All DNS Queries for the local network would be handled by Consul, with any
records for which it was not authoritative being handed off to the global DNS
network. Thankfully, with DNSMasq this was going to be easy to do!

{{< bundle-image  alt="The network diagram for our 'Mini Cloud'" name="minicloudnetwork" >}}

[Ansible](https://www.ansible.com/) has been my configuration management tool
of choice for many years now, and as I had a clear idea of what I needed to
install, I headed over to [Ansible Galaxy](https://galaxy.ansible.com/) to
find roles that I could use to install the base components before starting to
launch the containers...

In [part two of this series](/blog/2019-01-08-hashicorp-at-home-part-2/), I'll
talk more about how the various components are configured to interact with
each other, and show just how easy it is to launch multiple containers with
automatic DNS resolution using Nomad, Docker, Consul, and Traefik.

