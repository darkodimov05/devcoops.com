---
layout: post
title:  "How to install and setup ELK Stack on Ubuntu"
categories: [ elasticsearch ]
image: assets/images/elkend.jpg
tags: [elk, cloud, elasticsearch, Ubuntu]
---
ELK Stack is a combination of three open source tools which forms a log management tool. ELK Stack is a platform that helps in deep searching, analyzing, and visualizing the log generated from different machines.

The ELK Stack consists of four main parts:

`Elasticsearch`: is a tool which plays a major role in storing the logs in the JSON format, indexing it and allowing the searching of the logs. One of the important features of the Elasticsearch are:
* It is a search engine/ search server.
* It is a NoSQL based database. It means that Elasticsearch doesn’t use SQL for queries.
* It is based on Apache Lucene and provides RESTful API.
* It Uses indexes to search, which makes it faster.

`Logstash`: is an open-source tool which used to collect, parse and filter system logs as the input. One of the important features of the Logstash are:
* It is a data pipeline tool
* Centralizes the data processing
* Collects, parses and analyzes a large variety of structured/unstructured data and events.

`Kibana`: is a web interface which is allowing us to search, display and compile the data. One of the important features of the Kibana are:
* Visualization tool.
* Provides real-time analysis, summarization, charting and debugging capabilities.
* Provides instinctive and user-friendly interface.
* Allows sharing of snapshots of the logs searched through.
* Permits saving the dashboard and managing multiple dashboards.
* Beats: are used to collect data from various sources and transport them to Logstash or Elasticsearch.

## Prerequisites
* You will have to make sure that you are logged into your Ubuntu machine as a user with `sudo` privileges

{% include in-article-ad.html %}

## Installing and Configuring Elasticsearch

Before we started with the installation, let's update the system:
```bash
sudo apt-get update
```
As an addition requirement before we can install Elasticsearch, we will have to install Java 8. Elasticsearch requires Java 8 and it is recommended to use the Oracle JDK 1.8. 
```bash
sudo apt-get install python-software-properties software-properties-common apt-transport-https
sudo add-apt-repository ppa:webupd8team/java
```
To install the java8-installer, execute the following command:
```bash
sudo apt-get install oracle-java8-installer
```
Once the installation is done, to make sure that the java is installed successfully run:
```bash
java -version
```
`Output`:
```ruby
java version "1.8.0_201"
Java(TM) SE Runtime Environment (build 1.8.0_201-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.201-b09, mixed mode)
```
After installing Java, we will import the Elasticsearch public GPG key into APT and add the elastic repository to the system:
```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
```
Next we will update the repository and install elasticsearch package:
```bash
apt-get update
apt-get install elasticsearch
```
Once the elasticsearch installation is done, go to `/etc/elasticsearch` directory and edit the configuration file `elasticsearch.yml`.
```bash
cd /etc/elasticsearch/
nano elasticsearch.yml
```
To get your elasticsearch working uncomment the `network.host` line and change the value to `localhost`, and uncomment the default port for elasticsearch `http.port`:
```bash
network.host: localhost
http.port: 9200
```
After the changes save the file and close it.
Next, we will start the elasticsearch service and enable it by running te following commands:
```bash
systemctl start elasticsearch
systemctl enable elasticsearch
```
After a successful installation it is always a good practice to test the installed service. We will test our Elasticsearch service by sending an HTTP request:
```bash
curl -X GET "localhost:9200"
```
If you followed the above steps and if your Elasticsearch service is up and running you will get some basic information about your local node, similar to this:
```json
{
 "name" : "yeIWhmj",
 "cluster_name" : "elasticsearch",
 "cluster_uuid" : "2e6vC_sITGeT-PLy2r1UZA",
 "version" : {
 "number" : "6.6.0",
 "build_flavor" : "default",
 "build_type" : "deb",
 "build_hash" : "a9861f4",
 "build_date" : "2019-01-24T11:27:09.439740Z",
 "build_snapshot" : false,
 "lucene_version" : "7.6.0",
 "minimum_wire_compatibility_version" : "5.6.0",
 "minimum_index_compatibility_version" : "5.0.0"
 },
 "tagline" : "You Know, for Search"
}
```

## Install and Configure Kibana
We already added the Elastic package source in the previous step, so now we will install the Kibana Dashboard and configure the kibana service to run on the localhost address.
```bash
sudo apt install kibana
```
To edit the configuration file `kibana.yml` run the following commands:
```bash
cd /etc/kibana/
nano kibana.yml
```
To get things working uncomment the  `server.port`, `server.host` and `elasticsearch.url` :
```yaml
server.port: 5601
server.host: "localhost"
elasticsearch.hosts: ["http://localhost:9200"]
```
Save the file and close it.
Now, we will start the kibana service and enable it to start on boot:
```bash
systemctl enable kibana
systemctl start kibana
```
The kibana dashboard is now up and running on the ‘localhost’ address and the default port `5601`.

To access the Kibana dashboard we will be using the Nginx web server as a reverse proxy. First we have to install the Nginx web server:
```bash
sudo apt install nginx apache2-utils
```
It is recommended to disable the default Nginx configuration file and create a new virtual host file for the Kibanba dashboard.
```bash
cd /etc/nginx/sites-available
rm /etc/nginx/sites-enabled/default
```
Create a new file `kibana` and paste the following information:
```nginx
server {
 listen 80;
 server_name ip_address;

 auth_basic "Restricted Access";
 auth_basic_user_file /etc/nginx/.htpasswd;

 location / {
 proxy_pass http://localhost:5601;
 proxy_http_version 1.1;
 proxy_set_header Upgrade $http_upgrade;
 proxy_set_header Connection 'upgrade';
 proxy_set_header Host $host;
 proxy_cache_bypass $http_upgrade;
 }
}
```
For the security reason, we will create a new basic authentication web server for accessing the Kibana dashboard.
```bash
htpasswd -c /etc/nginx/.htpasswd elastic
```
Now, it should ask you for the elastic user password.
Next, we will activate the kibana virtual host and test all nginx configuration:
```bash
ln -s /etc/nginx/sites-available/kibana /etc/nginx/sites-enabled/
nginx -t
```
If you passed the nginx configuration, activate and enable it with the following commands:
```bash
systemctl enable nginx
systemctl restart nginx
```
Now Kibana is accessible via your server public IP address. Open your web browser and type:
```bash
http://your_server_ip/status
```
You will be prompted to enter the username and password which we created previously. After successful login, you can access the Kibana dashboard.

![Kibana dashboard](/assets/images/screenshots/kibana.png)

## Install and Configure Logstash
We will use Logstash to centralize server logs from client sources. It is possible to use Beats to send data directly to the Elasticsearch database, but it is recommended using Logstash to process the data.

Install  Logstash with the following command:
```bash
sudo apt-get install logstash
```
Logstash has three main components:
* Input
* Filter
* Output

We will create a file called `02-beats-input.conf` and  set up Filebeat input:
```bash
nano /etc/logstash/conf.d/02-beats-input.conf
```
Insert the following input configuration:
```yaml
input {
 beats {
 port => 5044
 }
}
```
Next, we will create a file for filtering system logs, also known as syslogs:
```bash
nano /etc/logstash/conf.d/10-syslog-filter.conf
```
Insert the following system log filter configuration:
```yaml
filter {
if [fileset][module] == "system" {
if [fileset][name] == "auth" {
grok {
match => { "message" => ["%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: %{DATA:[system][auth][ssh][event]} %{DATA:[system][auth][ssh][method]} for (invalid user )?%{DATA:[system][auth][user]} from %{IPORHOST:[system][auth][ssh][ip]} port %{NUMBER:[system][auth][ssh][port]} ssh2(: %{GREEDYDATA:[system][auth][ssh][signature]})?",
"%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: %{DATA:[system][auth][ssh][event]} user %{DATA:[system][auth][user]} from %{IPORHOST:[system][auth][ssh][ip]}",
"%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sshd(?:\[%{POSINT:[system][auth][pid]}\])?: Did not receive identification string from %{IPORHOST:[system][auth][ssh][dropped_ip]}",
"%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} sudo(?:\[%{POSINT:[system][auth][pid]}\])?: \s*%{DATA:[system][auth][user]} :( %{DATA:[system][auth][sudo][error]} ;)? TTY=%{DATA:[system][auth][sudo][tty]} ; PWD=%{DATA:[system][auth][sudo][pwd]} ; USER=%{DATA:[system][auth][sudo][user]} ; COMMAND=%{GREEDYDATA:[system][auth][sudo][command]}",
"%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} groupadd(?:\[%{POSINT:[system][auth][pid]}\])?: new group: name=%{DATA:system.auth.groupadd.name}, GID=%{NUMBER:system.auth.groupadd.gid}",
"%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} useradd(?:\[%{POSINT:[system][auth][pid]}\])?: new user: name=%{DATA:[system][auth][user][add][name]}, UID=%{NUMBER:[system][auth][user][add][uid]}, GID=%{NUMBER:[system][auth][user][add][gid]}, home=%{DATA:[system][auth][user][add][home]}, shell=%{DATA:[system][auth][user][add][shell]}$",
"%{SYSLOGTIMESTAMP:[system][auth][timestamp]} %{SYSLOGHOST:[system][auth][hostname]} %{DATA:[system][auth][program]}(?:\[%{POSINT:[system][auth][pid]}\])?: %{GREEDYMULTILINE:[system][auth][message]}"] }
pattern_definitions => {
"GREEDYMULTILINE"=> "(.|\n)*"
    }
remove_field => "message"
    }
date {
match => [ "[system][auth][timestamp]", "MMM d HH:mm:ss", "MMM dd HH:mm:ss" ]
    }
geoip {
source => "[system][auth][ssh][ip]"
target => "[system][auth][ssh][geoip]"
    }
}
else if [fileset][name] == "syslog" {
grok {
match => { "message" => ["%{SYSLOGTIMESTAMP:[system][syslog][timestamp]} %{SYSLOGHOST:[system][syslog][hostname]} %{DATA:[system][syslog][program]}(?:\[%{POSINT:[system][syslog][pid]}\])?: %{GREEDYMULTILINE:[system][syslog][message]}"] }
pattern_definitions => { "GREEDYMULTILINE" => "(.|\n)*" }
remove_field => "message"
}
date {
match => [ "[system][syslog][timestamp]", "MMM d HH:mm:ss", "MMM dd HH:mm:ss" ]
    }
  }
 }
}
```
Finally we will create a file for output configuration:
```bash
nano /etc/logstash/conf.d/30-elasticsearch-output.conf
```
Insert the following output configuration:
```yaml
output {
 elasticsearch {
 hosts => ["localhost:9200"]
 manage_template => false
 index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
 }
}
```
Now we should test the Logstash configuration with the following command:
```bash
sudo -u logstash /usr/share/logstash/bin/logstash --path.settings /etc/logstash -t
```
If you get a succesfull configuration output, you can start and enable Logstash:
```bash
systemctl start logstash
systemctl enable logstash
```

## Conclusion
Finally, you learned how to install and set up an ELK stack on your Ubuntu machine. To be honest, it's not a quick and easy process but if you get your hands dirty and follow all the steps above, you will have your ELK stack installed. For any further questions and discussions, you can leave a comment below and we will reply as soon as possible. Also you can take a look at the Elastic documentation by visiting the [Elasticsearch documentation](https://www.elastic.co/guide/index.html).
