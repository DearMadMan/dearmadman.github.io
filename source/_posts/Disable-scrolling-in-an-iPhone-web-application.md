title: Disable scrolling in an iPhone web application
date: 2015-10-17 09:34:43
categories: [other]
tags: other
---
![](/images/s30.jpg)
# Disable scrolling in an iPhone web application

## add meta
```
<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes"/>
```


## Disble DOM default event

```
<script>
document.body.addEventListener('touchmove', function(e){ e.preventDefault(); }); 
document.body.addEventListener('touchstart', function(e){ e.preventDefault(); }); 
</script>
```