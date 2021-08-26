---
layout: post
title:  "Filter the output of docker image ls"
categories: [ docker ]
image: assets/images/docker-images-ls.jpg
tags: [ docker, images, docker image, cli ]
---
Sometimes using the `filter` flag as a part of the `docker image ls` command can be useful to figure out which images are taking space, which are dangling images or unused. In this tutorial, I'm going to show you some useful scenarios on how can you use the `--filter` flag to filter the list of images returned by `docker image ls`.

## Prerequisites
* Docker installed

## Filter only dangling images
The following example will only return dangling images:
```bash
sudo docker image ls --filter dangling=true
```
`Output:`
```bash
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
<none>        <none>    4fd34165afe0   2 days ago     14.5MB
```
A dangling image is an image that is no longer tagged, and appears in listings as `<none>:<none>` . A common way they occur is when building a new image giving it a tag that already exists. When this happens, Docker will build the new image, notice that an existing image already has the same tag, remove the tag from the existing image and give it to the new image.
Docker currently supports the following filters:
* `dangling:` Accepts true or false , and returns only dangling images (true), or non-dangling images (false).
* `before:`   Requires an image name or ID as argument, and returns all images created before it.
* `since:`    Same as above, but returns images created after the specified image.
* `label:`    Filters images based on the presence of a label or label and value. The `docker image ls` command does not display labels in its output.

## Display only images tagged as latest
Here’s an example using `reference` to display only images tagged as `latest`:
```bash
sudo docker image ls --filter=reference="*:latest"
```
`Output:`
```bash
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
debian        latest    fe3c5de03486   9 days ago     124MB
amazonlinux   latest    d85ab0980c91   3 weeks ago    163MB
ubuntu        latest    9873176a8ff5   2 months ago   72.7MB
wordpress     latest    c01290f258b3   4 months ago   550MB
nginx         latest    62d49f9bab67   4 months ago   133MB
```

## Return the size property of images on a Docker host
You can use the `--format` flag to format output using Go templates:
{% raw %}
```bash
sudo docker image ls --format "{{.Size}}"
```
{% endraw %}
`Output:`
```bash
124MB
163MB
227MB
72.7MB
```

## Return all images, but only display repo, tag and size
Use the following command to return all images with repo, tag and size only:
{% raw %}
```bash
sudo docker image ls --format "{{.Repository}}: {{.Tag}}: {{.Size}}"
```
{% endraw %}
`Output:`
```bash
debian: latest: 124MB
amazonlinux: latest: 163MB
rockylinux/rockylinux: latest: 227MB
ubuntu: latest: 72.7MB
mysql: 5.7: 447MB
wordpress: latest: 550MB
nginx: latest: 133MB
```

## Conclusion
These are some basic examples that show us how to filter the output of the `docker image ls` command and if you need more powerful filtering, you can always use the tools provided by your OS and shell such as `grep` and `awk`. Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.