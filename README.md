# Elastic Search Cluster + Security Comfiguration
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
  node.name= master
  
```





















**Elasticsearch is live !**

<img src="https://c.tenor.com/qdg13PqYbxMAAAAM/yes-baby.gif"  width="300" height="250">


## TODO
   - [ ] Youtube Video
   - [ ] Add Kibana


## Authors
Thank you very much to my beloved wife for her support. [@busetorunlarertan](https://github.com/busetorunlarertan)

