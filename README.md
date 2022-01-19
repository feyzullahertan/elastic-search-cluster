# Elastic Search Cluster + Security Configuration
[![](https://img.shields.io/badge/Documantation-1.0-brightgreen)]()

![App Screenshot](https://www.ozgurozkok.com/wp-content/uploads/2019/12/elasticsearch.png)


I will show how to install Elastic Search Cluster structure. This repo is first aid kit. :ambulance:

Don't you know Elastic Search? Lets jump to [Elastic Search 101](https://towardsdatascience.com/an-overview-on-elasticsearch-and-its-usage-e26df1d1d24a)


## Installation

### Create VM Instances

**CentOS** image is used as Linux.

The example was made on **Google Cloud.**

You can do it if you allocate **2 GB** to each node on your own computer.

We are click Compute Engine -> VM Instance side.

Requirements before creating VM Instance

**>> The instances we will install must be in the same region and zone.**

**>> The instance will be E2 Series and machine type is e2 medium(2 vCPU, 4GB Memory).**

**>> Change boot disk with CentOS. Version is CentOS 7. Size is 20 GB.**

**>> Check is Allow HTTP Trafic and Allow HTTPS Trafic in Firewall settings.**

Our first instance names "elastic-master" which is my master node, then I created 2 data node instances. We created 3 VM Instances in total.

**>> Our data nodes named "elastic-data-1" and"elastic-data-2".**

![elastic](https://user-images.githubusercontent.com/37136204/148647015-9e1f08f5-fb6b-415d-9be9-cb9fc6df0ccd.PNG)

VM Instances are completed! 

### Installation Elasticsearch

[Download Elasticsearch](https://towardsdatascience.com/an-overview-on-elasticsearch-and-its-usage-e26df1d1d24a)

**Choose Linuxx86_64**

**>> Get link at 'Download Linux86_64'.**

And lets go to VM Intances with SSH. You can switch to root user. That's how I preferred it.

```bash
  yum install wget
  yum install nano
```
```bash
  wget <elastic_search_download_link> -y
```
```bash
  rpm -ivh <elasticsearch_file.rpm>
```
### Configuration

We must do same action other instances (which is elastic-data-1 and elastic-data-2)

```bash
  cd /etc/elasticsearch
  ls (You can see elasticsearch.yml)
  nano elasticsearch.yml
```
First I at Master VM Instance
You can see everthing is comment mode. We need to delete "#" and configuration some lines:

```bash
  cluster-name
  node.name : master
  node.data : false
  node.ingest : false
  
   ---- Network ----
  network.host :  0.0.0.0
  http.port : 9200
  
  ---- Discovery ----
  Make sure that VM Instances are same regions.
  discovery.seed_hosts : ["elastic-master's internal IP","elastic-data-1's internal IP", "elastic-data-2's internal IP"]
  cluster.initial_master_nodes: ["elastic-master's internal IP"]
  
```

### Security (xpack) Configuration with SSL

We must do same action other instances (which is elastic-data-1 and elastic-data-2)

```bash
  cd /usr/share/elasticsearch/
  ./bin/elasticsearch-certutil ca
  Enter + define some password what you want. (but dont forget,it will work for us)
  You can see "elastic-stack-ca.p12"
```
Lets go elasticsearch.yml file.
```bash
  cd /etc/elasticsearch/
  nano elasticsearch.yml
```
At the bottom of the document we write:
```bash
  xpack.security.transport.ssl.enabled: true
  xpack.security.transport.ssl.verification_node: certificate
  xpack.security.transport.ssl.client_authentication: required
  xpack.security.transport.ssl.keystore.path: /usr/share/elastic-stack-ca.p12
  xpack.security.transport.ssl.truststore.path: /usr/share/elastic-stack-ca.p12
```

We created certification (elastic-stack-ca.p12), this certification must be have other VM Instances (elastic-data-1 & elastic-data-2).
If they do not have this certification, **your configuration will not run.**

How to move elastic-stack-ca.p12 file to other instances?

1) Lets move Cloud Storage and create bucket.
2) Open elastic-master's terminal 
3)
  ```bash
  gcloud auth login  // autorize your GCP account
  gsutil cp elastic-stack-ca.p12 gs://your_bucket_name
  ```
  
4) Open elastic-data-1 & elastic-data-2's terminal
5) 
  ```bash
  gsutil cp gs://your_bucket_name/elastic-stack-ca.p12 /usr/share/elasticsearch/
  ```
  
6)This 2 command must do all of VM Instances.
  ```bash
  chmod 660 elastic-stack-ca.p12
  chown root:elasticsearch elastic-stack-ca.p12
  ```
 
 ### Configuration "HTTPS"
 ```bash
  cd /usr/share/elasticsearch/
  ./bin/elasticsearch-cerutil http
  ```
 Do not create CSR file, because we have CA file.
 ```bash
 CA Path:/usr/share/elasticsearch/elastic-stack-ca.p12
 Generate cerfitication per node? : No
  ```
  Now, It will want hostnames. 
  If you do not know hostname and internal hostname, you can write like this at VM Instances's terminals:
 ```bash
 hostname
 hostname -f
  ```
  You can write hostname names below.
 ```bash
 localhost
 elastic-master
 elastic-master_bla_bla_bla
 .
 ..
 ...
  ```
  Then, you must write IP addresses (which is localhost IP, internal IP and External IP)
   ```bash
 127.0.0.1
 .
 ..
 ...
 ....
  ```
 Now, you have "elasticsearch-ssl-http.zip", location is:
 ```bash
 usr/share/elasticsearch/elasticsearch-ssl-http.zip
 unzip elasticsearch-ssl-http.zip
 ```
You can see elasticsearch and kibana files. Elasticsearch's file have http.p12 files. We'll send other VM Instances like moving "elastic-stack-ca.p12".
We need to add a few security parameters to the **elasticsearch.yml** file.
 ```bash
cd /etc/elasticsearch/
nano elasticsearch.yml
 ```

Add these parameters:
 ```bash
xpack.security.http.ssl.enabled: true
xpack.security.http.ssl.keystore.path:  /usr/share/elasticsearch/elasticsearch/http.p12 //Location of http.p12
xpack.security.http.ssl.truststore.path:  /usr/share/elasticsearch/elasticsearch/http.p12 //Location of http.p12
 ```


 ### Running
```bash
service elasticsearch start
```
When we start to run the service, we can monitor it in the log file.
```bash
cd /var/log/elasticsearch/
tail -100f my-applcation.log
```



**Elasticsearch is live !**

<img src="https://c.tenor.com/qdg13PqYbxMAAAAM/yes-baby.gif"  width="300" height="250">


## TODO
   - [ ] Youtube Video
   - [ ] Fleet manager
   - [ ] Kibana



## Authors
Thank you very much to my beloved wife for her support. [@busetorunlarertan](https://github.com/busetorunlarertan)

