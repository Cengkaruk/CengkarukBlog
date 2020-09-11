---
title: "Create an Alpine package for jpegoptim"
layout: post
date: 2016-08-14 18:36
image: '/assets/images/'
headerImage: false
description:
tag:
- linux
- alpine
- docker
- devops
category: blog
author: aji
---

Beberapa waktu yang lalu saya meracik **workspace** image untuk docker. Kebutuhannya untuk PHP7, Composer, NodeJS dan beberapa tool lain seperti image compressor (*pngquant* dan *jpegoptim*). Demi menghemat ukuran image docker, seperti biasa saya menggunakan Alpine.

Khusus untuk *pngquant* dan *jpegoptim* saya ambil dari *branch edge repository testing* dengan cara mengecek terlebih dahulu melalui [situs pkgs](http://pkgs.alpinelinux.org/packages) kedua tool tersebut. Ternyata *jpegoptim* belum ada dalam paket dan saya memutuskan untuk melakukan kompilasi dalam *Dockerfile*

{% highlight sh %}
...
RUN apk add --no-cache freetype \
      libpng \
      libjpeg-turbo \
      libpq \
      freetype-dev \
      libpng-dev \
      libjpeg-turbo-dev \
      libmcrypt-dev \
      postgresql-dev

...
RUN echo '@testing http://dl-cdn.alpinelinux.org/alpine/edge/testing' >> /etc/apk/repositories && \
  apk update && apk add --no-cache wget build-base optipng@testing && \
  wget https://github.com/tjko/jpegoptim/archive/RELEASE.1.4.3.tar.gz -O /tmp/jpegoptim-1.4.3.tar.gz && \
  cd /tmp && tar zxvf jpegoptim-1.4.3.tar.gz && cd /tmp/jpegoptim-RELEASE.1.4.3 && \
  ./configure && make && make strip && make install && \
  apk del --no-cache wget build-base
...
{% endhighlight %}

Singkat cerita container saya punya *jpegoptim*. Tapi karena hari ini minggu, saya memutuskan untuk melakukan hal lebih. Sepertinya asik kalau mencoba membuat paket untuk *Alpine*. Bermodal link berikut, saya pun mulai menyalakan mesin.

* [Creating an Alpine Package](https://wiki.alpinelinux.org/wiki/Creating_an_Alpine_package)
* [jpegoptim](https://github.com/tjko/jpegoptim)

Langsung saja, silahkan ditonton dan semoga bermanfaat.

<center>
<script type="text/javascript" src="https://asciinema.org/a/4x82ohc6rblasjx6oa8igox3c.js" id="asciicast-4x82ohc6rblasjx6oa8igox3c" async></script>
</center>

<center>
<script type="text/javascript" src="https://asciinema.org/a/68oqu6seu4jd6yr5g8xkaf474.js" id="asciicast-68oqu6seu4jd6yr5g8xkaf474" async></script>
</center>
