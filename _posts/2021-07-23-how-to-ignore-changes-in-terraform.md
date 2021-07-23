---
layout: post
title:  "How to ignore changes in Terraform"
categories: [ terraform ]
image: assets/images/tf-ignore-changes.jpg
tags: [ terraform ]
---
Terraform is the most popular Infrastructure as Code tool mostly used for spinning up and managing infrastructure on the public cloud giants AWS, Azure, GCP and DigitalOcean. But, sometimes it doesn't work the way we plan, and resources could be lost if not being careful. And, if you have worked before with Terraform, you've most likely dealt with that one annoying resource attribute that doesn't change but it props at every `terraform apply`. The reasons could be intended or unintender (bugs). Let's see an example of how can we get rid of that annoying resource attribute.

## Prerequisites
* Terraform

## Example
Let's say we have an AWS elasticsearch module. If we don't want to add some logs initially to the cluster, we need to update the `log_publishing_options` attribute to `disabled`. 
```bash
resource "aws_elasticsearch_domain" "this" {
. . . 
    log_publishing_options {
        enabled                  = false
        log_type                 = var.cloudwatch_log_type
        cloudwatch_log_group_arn = var.cloudwatch_log_group_arn
    }
}
```
Now, if we run `terraform plan` and/or `terraform apply`, we'll get the following output every time:  
```bash
- log_publishing_options {
          - enabled  = false -> null
          - log_type = "INDEX_SLOW_LOGS" -> null
        }
```

## The solution
The `lifecycle` block comes to the rescue:
```bash
resource "aws_elasticsearch_domain" "this" {
. . . 
    lifecycle {
        ignore_changes = [log_publishing_options]
    }
}
```
Now, thanks to the `ignore_changes` feature, we can basically ignore any resource attribute that cause us trouble.

## Conclusion
There is an open issue regarding the `log_publishing_option` which can be found on [Github](https://github.com/hashicorp/terraform-provider-aws/issues/5752){:target="_blank"}.  
Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.