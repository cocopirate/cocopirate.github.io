---
title: "Hugo中使用Google Analytics 4"
keywords: ["Hugo","Google Analytics"]
description: "解决Hugo使用Google Analytics 4中G-开头的跟踪标识"
tags: ["建站","网站统计","Google Analytics"]
categories: ["网站"]
series: ["教程"]
draft: false
date: 2023-04-23
author: "壹點漫談"
---

# 关于Google Analytics

Google Analytics是一个由Google所提供的网站流量统计服务，通过在网站中埋入Google Analytics追踪码，可以方便查看网站流量信息，包括来源、用户、设备、访问路径等，对于网站入门来说是非常推荐使用的工具。

在实际使用中，会发现Google Analytics 4的Measurement ID是以G-开头的，而Hugo中的Google Analytics ID是以UA-开头的，这就导致了无法使用Google Analytics 4的问题。

# Hugo使用Google Analytics 4使用方法

将Google Analyics ID（Measurement ID）放置在`config.toml`中的`[params]`中。

{{< highlight shell >}}
# Google Analytics 4
googleAnalyticsID = "G-00000XXXXX"
{{< /highlight >}}

创建一个名为`analytics-gtag.html`的文件，放置在`/layouts/partials`中，代码如下：

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

在`/layouts/_default/baseof.html`中的`<head>`标签后调用这个partial，代码如下：

``` html
<head>
  {{ if .Site.Params.GoogleAnalyticsID }}
  {{ partial "analytics-gtag.html" . }}
  {{ end }}
  ...
 </head>
```

以上就是在Hugo中使用Google Analytics 4的方法。

