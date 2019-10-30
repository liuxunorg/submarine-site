---
layout: page
title: "Apache Submarine Configuration"
description: "This page will guide you to configure Apache Submarine using either environment variables or Java properties. Also, you can configure SSL for Zeppelin."
group: setup/operation 
---
<!--
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->
{% include JB/setup %}

# Apache Submarine Configuration

<div id="toc"></div>

## Zeppelin Properties
There are two locations you can configure Apache Submarine.

* **Environment variables** can be defined `conf/zeppelin-env.sh`(`conf\zeppelin-env.cmd` for Windows).
* **Java properties** can be defined in `conf/zeppelin-site.xml`.

If both are defined, then the **environment variables** will take priority.
> Mouse hover on each property and click <i class="fa fa-link fa-flip-horizontal"></i> then you can get a link for that.

<table class="table-configuration">
  <tr>
    <th>zeppelin-env.sh</th>
    <th>zeppelin-site.xml</th>
    <th>Default value</th>
    <th class="col-md-4">Description</th>
  </tr>
  <tr>
    <td><h6 class="properties">ZEPPELIN_PORT</h6></td>
    <td><h6 class="properties">zeppelin.server.port</h6></td>
    <td>8080</td>
    <td>Zeppelin server port </br>
      <span style="font-style:italic; color: gray"> Note: Please make sure you're not using the same port with
      <a href="https://submarine.apache.org/contribution/webapplication.html#dev-mode" target="_blank">Zeppelin web application development port</a> (default: 9000).</span></td>
  </tr>
  <tr>
    <td><h6 class="properties">ZEPPELIN_SSL_PORT</h6></td>
    <td><h6 class="properties">zeppelin.server.ssl.port</h6></td>
    <td>8443</td>
    <td>Zeppelin Server ssl port (used when ssl environment/property is set to true)</td>
  </tr>
  <tr>
    <td><h6 class="properties">ZEPPELIN_JMX_ENABLE</h6></td>
    <td><h6 class="properties">N/A</h6></td>
    <td></td>
    <td>Enable JMX by defining "true"</td>
  </tr>
  <tr>
    <td><h6 class="properties">ZEPPELIN_JMX_PORT</h6></td>
    <td><h6 class="properties">N/A</h6></td>
    <td>9996</td>
    <td>Port number which JMX uses</td>
  </tr>
  <tr>
    <td><h6 class="properties">ZEPPELIN_MEM</h6></td>
    <td>N/A</td>
    <td>-Xmx1024m -XX:MaxPermSize=512m</td>
    <td>JVM mem options</td>
  </tr>
  <tr>
    <td><h6 class="properties">ZEPPELIN_INTP_MEM</h6></td>
    <td>N/A</td>
    <td>ZEPPELIN_MEM</td>
    <td>JVM mem options for interpreter process</td>
  </tr>
  <tr>
    <td><h6 class="properties">ZEPPELIN_JAVA_OPTS</h6></td>
    <td>N/A</td>
    <td></td>
    <td>JVM options</td>
  </tr>
</table>

