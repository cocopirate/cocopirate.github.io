+++
author = "AntStu"
title = "Hugo中使用Google Analytics 4"
date = "2021-03-26"
description = "解决Hugo使用Google Analytics 4中G-开头的跟踪标识"
tags = [
    "建站",
    "网站统计",
]
categories = [
    "网站",
]
series = ["教程"]
aliases = ["migrate-from-jekyl"]
+++

Google Analytics是一个由Google所提供的网站流量统计服务，通过在网站中埋入Google Analytics追踪码，可以方便查看网站流量信息，包括来源、用户、设备、访问路径等，对于网站入门来说是非常推荐使用的工具。

## Hugo使用Google Analytics 4 失效

Hugo原有的

## 使用方法

Place the GoogleAnalyicsID (Measurement ID) in config.toml within [params].

``` shell
# Google Analytics 4
googleAnalyticsID = "G-00000XXXXX"
```

Create the partial, analytics-gtag.html, and place it in /layouts/partials. Code below.

Call the partial just after the <head> tag in /layouts/_default/baseof.html.

``` html
<head>
  {{ if .Site.Params.GoogleAnalyticsID }}
  {{ partial "analytics-gtag.html" . }}
  {{ end }}
  ...
 </head>
```

``` html
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id={{ .Site.Params.GoogleAnalyticsID }}"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', '{{ .Site.Params.GoogleAnalyticsID }}');
</script>
```