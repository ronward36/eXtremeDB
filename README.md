<!--
Copyright (c) 2012 - 2020 YCSB contributors. All rights reserved.
# eXtremeDB
eXtremeDB YCSB Benchmark

Licensed under the Apache License, Version 2.0 (the "License"); you
may not use this file except in compliance with the License. You
may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
implied. See the License for the specific language governing
permissions and limitations under the License. See accompanying
LICENSE file.
-->

## Quick Start

This section describes how to run YCSB on eXtremeDB. 

### 1. Start eXtremeDB

First, download eXtremeDB and start `eXtremeDB`. For example, to start eXtremeDB
on x86-64 Linux box:

    wget http://mcobject.com/linux/eXtremeDB-linux-x86_64.tgz
    tar xfvz eXtremeDB-linux-x86_64-*.tgz
    mkdir /tmp/eXtremeDB
    cd eXtremeDB-linux-x86_64-*
    ./bin/eXtremeDB --dbpath /tmp/eXtremeDB

### 2. Install Java and Maven

Go to http://www.oracle.com/technetwork/java/javase/downloads/index.html

and get the url to download the rpm into your server. For example:

    wget http://download.oracle.com/otn-pub/java/jdk/7u40-b43/jdk-7u40-linux-x64.rpm?AuthParam=11232426132 -o jdk-7u40-linux-x64.rpm
    rpm -Uvh jdk-7u40-linux-x64.rpm
    
Or install via yum/apt-get

    sudo yum install java-devel

Download MVN from http://maven.apache.org/download.cgi

    wget http://ftp.heanet.ie/mirrors/www.apache.org/dist/maven/maven-3/3.1.1/binaries/apache-maven-3.1.1-bin.tar.gz
    sudo tar xzf apache-maven-*-bin.tar.gz -C /usr/local
    cd /usr/local
    sudo ln -s apache-maven-* maven
    sudo vi /etc/profile.d/maven.sh

Add the following to `maven.sh`

    export M2_HOME=/usr/local/maven
    export PATH=${M2_HOME}/bin:${PATH}

Reload bash and test mvn

    bash
    mvn -version

### 3. Set Up YCSB

Download the YCSB zip file and compile:

    curl -O --location https://github.com/brianfrankcooper/YCSB/releases/download/0.5.0/ycsb-0.5.0.tar.gz
    tar xfvz ycsb-0.5.0.tar.gz
    cd ycsb-0.5.0

### 4. Run YCSB

Now you are ready to run! First, use the eXtremeDB driver to load the data:

    ./bin/ycsb load eXtremeDB -s -P workloads/workloada > outputLoad.txt

Then, run the workload:

    ./bin/ycsb run eXtremeDB -s -P workloads/workloada > outputRun.txt
        
See the next section for the list of configuration parameters for eXtremeDB.

## Log Level Control
Due to the eXtremeDB driver defaulting to a log level of DEBUG, a logback.xml file is included with this module that restricts the org.eXtremeDB logging to WARN. You can control this by overriding the logback.xml and defining it in your ycsb command by adding this flag:

```
bin/ycsb run eXtremeDB -jvm-args="-Dlogback.configurationFile=/path/to/logback.xml"
```

## eXtremeDB Configuration Parameters

- `eXtremeDB.url`
  - This should be an eXtremeDB URI or connection string. 
    - See http://docs.eXtremeDB.org/manual/reference/connection-string/ for the standard options.
      - http://www.allanbank.com/eXtremeDB-driver/apidocs/index.html?com/allanbank/mongodb/MongoDbUri.html
    - For the complete set of options for the synchronous driver see:
      - http://api.eXtremeDB.org/java/current/index.html?com/eXtremeDB/eXtremeDBClientURI.html
  - Default value is `eXtremeDB://localhost:8083/ycsb?w=1`
  - Default value of database is `ycsb`

- `eXtremeDB.batchsize`
  - Useful for the insert workload as it will submit the inserts in batches inproving throughput.
  - Default value is `1`.

For example:

    ./bin/ycsb load eXtremeDB -s -P workloads/workloada -p eXtremeDB.url=eXtremeDB://localhost:8083/ycsb?w=0
