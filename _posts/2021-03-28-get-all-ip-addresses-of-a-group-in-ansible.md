---
layout: post
title:  "How to get all the IP addresses of a group in Ansible"
categories: [ ansible ]
image: assets/images/ansible-ip.jpeg
tags: [ ansible, jinja2, elasticsearch ]
---
Usually when developing an Ansible role, most of the times we need to write a jinja2 template for some configuration file. Speaking from personal experience, I was writing an elasticsearch role and I needed to add an ```elasticsearch.yml``` jinja2 template, and I was struggling a bit, because I had to add all of the elasticsearch group nodes IP addresses into the following line:
```bash
{% raw  %}
discovery.zen.ping.unicast.hosts: ["192.168.10.1", "192.168.10.2", "192.168.10.3"]
{% endraw  %}
```

After a little bit of googling, I found a temporary solution on [stack overflow](https://stackoverflow.com/questions/36328907/ansible-get-all-the-ip-addresses-of-a-group):   
```bash
{% raw  %}
discovery.zen.ping.unicast.hosts: [{{ elasticsearch_hosts }}]
elasticsearch_hosts: "{{ groups['elasticnodes'] | map('extract', hostvars, ['ansible_default_ipv4', 'address']) | join(', ') }}"
{% endraw  %}
```

which lead me to the following result:
```bash
{% raw  %}
discovery.zen.ping.unicast.hosts: [192.168.10.1, 192.168.10.2, 192.168.10.3]
{% endraw  %}
```

We are still missing the double quotes, so I had to make some small adjustments. But first, let's talk a bit about ```map``` and ```join``` functions. 

## Prerequisites
* Ansible

## ```map``` function
The ```map``` function is allowing us to change every item in a list. So, we can try to extract all of the IP addresses of an Ansible group with:
```bash
{% raw  %}
{{ groups['elasticnodes'] | map('extract', hostvars, ['ansible_default_ipv4', 'address'])
{% endraw  %}
```

Note that you can specify an interface rather then the ansible default ipv4 address, but keep in mind that ```ansible_default_ipv4``` is more reliable since interface names can become unpredictable.

## ```join``` function
The ```join``` function is a filter that's allowing us to join a list of items into a string. Now, that we have IP addresses with the map function, we can concatenate them with double quotes:
```bash
{% raw  %}
{{ groups['elastic'] | map('extract', hostvars, ['ansible_default_ipv4', 'address']) | join('", "') }}
{% endraw  %}
```

The final result:
```bash
{% raw  %}
discovery.zen.ping.unicast.hosts: ["192.168.10.1", "192.168.10.2", "192.168.10.3"]
{% endraw  %}
```

## Conclusion
In this post, we covered all the needed steps to concatenate a list of IP addresses of an Ansible group with double quotes and space between them.  
More about the [map](https://jinja.palletsprojects.com/en/2.11.x/templates/#map) function.  
More about the [join](https://jinja.palletsprojects.com/en/2.11.x/templates/#join) function.  
Feel free to leave a comment below if you find this tutorial useful and follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.
