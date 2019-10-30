---
layout: page
title: "How to Build Zeppelin from source"
description: "How to build Zeppelin from source"
group: setup/basics
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

## How to Build Zeppelin from Source

<div id="toc"></div>

#### 0. Requirements

If you want to build from source, you must first install the following dependencies:

<table class="table-configuration">
  <tr>
    <th>Name</th>
    <th>Value</th>
  </tr>
  <tr>
    <td>Git</td>
    <td>(Any Version)</td>
  </tr>
  <tr>
    <td>Maven</td>
    <td>3.1.x or higher</td>
  </tr>
  <tr>
    <td>JDK</td>
    <td>1.8</td>
  </tr>
</table>


If you haven't installed Git and Maven yet, check the [Build requirements](#build-requirements) section and follow the step by step instructions from there.

#### 1. Clone the Apache Submarine repository

```bash
git clone https://github.com/apache/submarine.git
```

#### 2. Build source


You can build Zeppelin with following maven command:

```bash
mvn clean package -DskipTests [Options]
```

If you're unsure about the options, use the same commands that creates official binary package.

```bash

# build submarine with version of Apache hadoop
mvn clean package -DskipTests -Phadoop-2.9
```

#### 3. Done
You can directly start Zeppelin by running after successful build:

```bash
./bin/zeppelin-daemon.sh start
```

Check [build-profiles](#build-profiles) section for further build options.
If you are behind proxy, follow instructions in [Proxy setting](#proxy-setting-optional) section.

If you're interested in contribution, please check [Contributing to Apache Submarine (Code)](../../development/contribution/how_to_contribute_code.html) and [Contributing to Apache Submarine (Website)](../../development/contribution/how_to_contribute_website.html).

### Build profiles

#### Hadoop

To build with a specific Hadoop version, Hadoop version or specific features, define one or more of the following profiles and options:


##### `-Phadoop-[version]`

set hadoop major version

Available profiles are

```
-Phadoop-2.7
-Phadoop-2.9 (recommend)
-Phadoop-3.1
-Phadoop-3.2
```


### Build command examples
Here are some examples with several options:

```bash
# build with hadoop-2.7
mvn clean package -Phadoop-2.7 -DskipTests

# build with hadoop-2.9
mvn clean package -Phadoop-2.9 -DskipTests

# build with hadoop-3.1
mvn clean package -Phadoop-3.1 -DskipTests

# build with hadoop-3.2
mvn clean package -Phadoop-3.2 -DskipTests

```


## Build requirements

### Install requirements

If you don't have requirements prepared, install it.
(The installation method may vary according to your environment, example is for Ubuntu.)

```bash
sudo apt-get update
sudo apt-get install git
sudo apt-get install openjdk-8-jdk
```



### Install maven

```bash
wget http://www.eu.apache.org/dist/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz
sudo tar -zxf apache-maven-3.3.9-bin.tar.gz -C /usr/local/
sudo ln -s /usr/local/apache-maven-3.3.9/bin/mvn /usr/local/bin/mvn
```

_Notes:_
 - Ensure node is installed by running `node --version`  
 - Ensure maven is running version 3.1.x or higher with `mvn -version`
 - Configure maven to use more memory than usual by `export MAVEN_OPTS="-Xmx2g -XX:MaxPermSize=1024m"`



## Proxy setting (optional)

If you're behind the proxy, you'll need to configure maven and npm to pass through it.

First of all, configure maven in your `~/.m2/settings.xml`.

```xml
<settings>
  <proxies>
    <proxy>
      <id>proxy-http</id>
      <active>true</active>
      <protocol>http</protocol>
      <host>localhost</host>
      <port>3128</port>
      <!-- <username>usr</username>
      <password>pwd</password> -->
      <nonProxyHosts>localhost|127.0.0.1</nonProxyHosts>
    </proxy>
    <proxy>
      <id>proxy-https</id>
      <active>true</active>
      <protocol>https</protocol>
      <host>localhost</host>
      <port>3128</port>
      <!-- <username>usr</username>
      <password>pwd</password> -->
      <nonProxyHosts>localhost|127.0.0.1</nonProxyHosts>
    </proxy>
  </proxies>
</settings>
```

Then, next commands will configure npm.

```bash
npm config set proxy http://localhost:3128
npm config set https-proxy http://localhost:3128
npm config set registry "http://registry.npmjs.org/"
npm config set strict-ssl false
```

Configure git as well

```bash
git config --global http.proxy http://localhost:3128
git config --global https.proxy http://localhost:3128
git config --global url."http://".insteadOf git://
```

To clean up, set `active false` in Maven `settings.xml` and run these commands.

```bash
npm config rm proxy
npm config rm https-proxy
git config --global --unset http.proxy
git config --global --unset https.proxy
git config --global --unset url."http://".insteadOf
```

_Notes:_
 - If you are behind NTLM proxy you can use [Cntlm Authentication Proxy](http://cntlm.sourceforge.net/).
 - Replace `localhost:3128` with the standard pattern `http://user:pwd@host:port`.


## Package
To package the final distribution including the compressed archive, run:

```sh
mvn clean package -Pbuild-distr
```

To build a distribution with specific profiles, run:

```sh
mvn clean package -Pbuild-distr -Phadoop-2.9
```

The profiles `-Phadoop-2.9` can be adjusted if you wish to build to a specific spark versions.

The archive is generated under _`zeppelin-dist/target`_ directory

## Run end-to-end tests
Zeppelin comes with a set of end-to-end acceptance tests driving headless selenium browser

```sh
# assumes zeppelin-server running on localhost:8080 (use -Durl=.. to override)
mvn verify

# or take care of starting/stoping zeppelin-server from packaged zeppelin-distribuion/target
mvn verify -P using-packaged-distr
```

