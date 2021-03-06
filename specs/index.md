---
layout: page
status: publish
published: true
title: 技术参数
author:
  display_name: ZE3kr
  login: ZE3kr
  email: ze3kr@icloud.com
  url: https://ze3kr.com
author_login: ZE3kr
author_email: ze3kr@icloud.com
author_url: https://ze3kr.com
wordpress_id: 578
wordpress_url: https://ze3kr.com/?page_id=578
date: '2016-01-03 13:33:56 +0000'
date_gmt: '2016-01-03 05:33:56 +0000'
categories: []
tags: []
---
<p>使用 Google Cloud Platform（简称 GCP，下同）搭建，在云端，几乎完全静态化，主机位于台湾。主机使用 LNMP 配置，详情：<a href="https://ze3kr.com/2016/12/go-on-google-cloud/">全面迁移到 Google Cloud Platform</a>。</p>
<h2>兼容性与先进技术</h2>
<ul>
<li>网站本身的 HTTPS 几乎支持所有的浏览器，CDN 资源需要 SNI 支持</li>
<li>网站开启了 HSTS，并已进入各大浏览器的 Preload List，包括子域名</li>
<li>网站域名可以在纯 IPv6 的递归解析服务器上解析到 IP 地址</li>
<li><del>网站能够在纯 IPv6 情况下正常加载所有基本内容</del>（等待 Google Cloud 支持 IPv6 中）（Gravatar 除外）</li>
<li>前端使用 WordPress 自带主题，最低能兼容到 Windows XP 的 IE6。</li>
</ul>
<h3></h3>
<h2>视频参数</h2>
<h3>视频格式</h3>
<p>此网站使用自己的 CDN 加速视频，使用 MP4 格式，从 2016 年开始，所有的视频还将有 WEBM 格式，并且我还会逐渐的让以前的视频也支持 WEBM。</p>
<h3>视频质量</h3>
<p>使用 qHD（540p）准高清（腾讯视频将其称作为超清也是醉了）。同时使用 MP4 封装的 H.264 + AAC 和 WEBM 封装的 VP9 + Vorbis，每个视频的每种格式使用近似相等的码率，通常后者画质更高一些。本站对于 30 帧的视频使用 1000kbps 的平均码率，60 帧的视频使用 1500kbps 的平均码率；音频均为单声道 64kbps 码率。通常本站的视频（无论是什么格式）在 720p 或更低分辨率下的屏幕上都显得十分清晰（例如 iPhone 6，这只是主观测试）。更高分辨率下的屏幕下若使用 MP4 格式，能看到明显的画质下降的痕迹，WEBM 有明显优势。</p>
<h4>MP4 压缩方式</h4>
<ul>
<li>视频：H.264 多通路，平均码率 1000k（对于 60 帧的，使用 1500k），540p 画质，降分辨率使用线性滤镜，多次通过</li>
<li>音频：AAC 单声道，平均码率 64k</li>
</ul>
<h4>WEBM 压缩方式</h4>
<ul>
<li>视频：libvpx-VP9，平均码率 1000k（对于 60 帧的，使用 1500k），540p 画质，两次通过</li>
<li>音频：Vorbis 单声道，平均码率 64k</li>
</ul>
<h3>压制参数</h3>
<h4>30帧 MP4</h4>
<pre class="lang:sh decode:true ">ffmpeg -y -i input -c:v libx264 -r 30000/1001 -c:a aac -preset veryslow -s 960x540 -b:v 1000k -pass 1 -b:a 64k -ac 1 -f mp4 /dev/null

ffmpeg -y -i input -c:v libx264 -r 30000/1001 -c:a aac -preset veryslow -s 960x540 -b:v 1000k -pass 2 -b:a 64k -ac 1 output.mp4</pre>
<h4>60帧 MP4</h4>
<pre class="lang:sh decode:true ">ffmpeg -y -i input -c:v libx264 -r 60000/1001 -c:a aac -preset veryslow -s 960x540 -b:v 1500k -pass 1 -b:a 64k -ac 1 -f mp4 /dev/null

ffmpeg -y -i input -c:v libx264 -r 60000/1001 -c:a aac -preset veryslow -s 960x540 -b:v 1500k -pass 2 -b:a 64k -ac 1 output.mp4</pre>
<h4>30帧 WEBM</h4>
<pre class="lang:sh decode:true ">ffmpeg -y -i &lt;source&gt; -c:v libvpx-vp9 -pass 1 -b:v 1000K -threads 8 -speed 4 -tile-columns 6 -frame-parallel 1 -b:a 64k -ac 1 -s 960x540 -g 150 -r 30000/1001 -an -f webm /dev/null

ffmpeg -y -i &lt;source&gt; -c:v libvpx-vp9 -pass 2 -b:v 1000K -threads 8 -speed 1 -tile-columns 6 -frame-parallel 1 -auto-alt-ref 1 -lag-in-frames 25 -ac 1 -s 960x540 -r 30000/1001 -c:a libopus -b:a 64k -f webm out.webm</pre>
<h4>60帧 WEBM</h4>
<pre class="lang:sh decode:true">ffmpeg -y -i &lt;source&gt; -c:v libvpx-vp9 -pass 1 -b:v 1500K -threads 8 -speed 4 -tile-columns 6 -frame-parallel 1 -b:a 64k -ac 1 -s 960x540 -g 250 -r 60000/1001 -an -f webm /dev/null

ffmpeg -y -i &lt;source&gt; -c:v libvpx-vp9 -pass 2 -b:v 1500K -threads 8 -speed 1 -tile-columns 6 -frame-parallel 1 -auto-alt-ref 1 -lag-in-frames 25 -ac 1 -s 960x540 -r 60000/1001 -c:a libopus -b:a 64k -f webm out.webm
</pre>
