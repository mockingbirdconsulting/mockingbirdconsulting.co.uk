---
title: "Making better wine with the Internet of Things"
date: 2018-11-13T06:00:12+01:00
description: "How do you ensure the best possible crop when you're producing bottles of wine that sell for over £1,000?"
resources: 
  - name: header
    src: lushvines.png
  - name: oldsensors
    src: oldsensors.jpg
  - name: newdashboard
    src: newdashboard.png
tags:
  - Smart Vineyards
  - Vineyard Management
  - Vineyard IoT
  - Rural IoT
  - IoT Managed Infrastructure
  - Managed IoT
  - Smart Wine
---
<div class="row">
  <div class="col-md-12">
    <p>When JoTo Systems were asked by a collective of local vineyards in <a target="_blank" href="https://coonawarra.org/" title="The Coonwarra Wine Region">Coonwarra</a>, Australia to maintain the vineyard monitoring solution that was in place, their founder Tony Smith found a solution that was in serious need of an update</p>
  </div>
</div>
<div class="row">
  <div class="col-md-6">
    <p>Tony explained "We were given a number of arduinos and SD-cards, some of which were seriously water damaged.  We had to drive 4 hours each way once a month to collect the data from the SD cards and then provide the analysis back to the vineyards in a format which was easy for them to understand."</p>
    <p>"The devices were running on batteries charged by solar panels but, even in the South Australian sun, we were lucky if they stayed charged for more than 24 hours at a time.  It was clear a long-term, power efficient, real-time solution was needed, so we started to turn to the Internet of Things."</p>
    <p class="quotation">Mockingbird Consulting were able to provide us with a <a href="/managed_iot/managed_infrastructure/" alt="Our Private LoRaWAN Infrastructure Solution">private LoRaWAN solution</a> including data storage and visualisation, allowing us to focus on developing and deploying the sensors</p>    
    <p class="quotation_footer">Tony Smith, Founder, JoTo Systems</p>
  </div>
  <div class="col-md-6">
      {{< bundle-image name="oldsensors" caption="The previous sensors">}}
  </div>
</div>
<div class="row">
  <div class="col-md-6">
      {{< bundle-image name="newdashboard" caption="The new dashboard created by JoTo Systems to monitor the vines in realtime.">}}
    </figure>
  </div>
</div>
<div class="row">
  <div class="col-md-12">
    <p>JoTo Systems had already installed and configured a LoRaWAN Gateway to send data to The Things Network, however they required a managed platform that included not just the LoRaWAN Infrastructure, but metrics storage and analysis software, and that's when they turned to Mockingbird Consulting for help.<p>
    <p>Leveraging our relationships with <a target="_blank" href="https://www.hashicorp.com/">Hashicorp</a>, <a target="_blank" href="https://www.influxdata.com/">InfluxData</a>, and the wider open-source community, we were able to provide a LoRaWAN stack based on <a target="_blank" href="https://www.loraserver.io">loraserver.io</a> and a metrics platform running InfluxDB and the <a target="_blank" href="https://www.grafana.com">Grafana</a> open-source dashboarding platform.
    </p>
  </div>
</div>
<div class="row">
  <div class="col-md-12">
    <p>Once the platform was in place, we talked JoTo Systems through reconfiguring their gateway to point to the new infrastructure, and watched as the data started to roll in.</p>
  </div>
</div>
<div class="row">
  <div class="col-md-12">
    <p class="h4">What's next for JoTo Systems?</p>
    <p>Tony Smith explains "We've already solved the power issues, and have designed custom cases for the sensors that are watertight to ensure we don't see the same damage that affected the previous sensors.  We're now going to look at installing GPS sensors into the devices so that we can map the vineyards and track the changes in temperature across the vines, and even start to look at how we might monitor what's going on inside the vine itself."
    <p>At Mockingbird Consulting, we're looking forward to working alongside the project as it moves to the next phase, helping Tony and his team design dashboards and visualisations for their customers, and enabling the wine-makers of the Coonwarra region to create even more impressive wines.</p>
<p>If you'd like to learn more about how we can help you adopt the Internet of Things, then take a look at our <a href="/managed_iot/launch_kits" alt="Link to the launch kits">launch kits</a>, our <a href="/managed_iot/managed_infrastructure/" alt="Link to the managed infrastructure details">managed infrastructure solutions</a>, or fill in the form below and we'll get in touch within 24 hours.</p>
<script type="text/javascript" src="//crm.mockingbirdconsulting.co.uk/form/generate.js?id=6"></script>
  </div>
</div>
