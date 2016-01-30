title: php exec debug
date: 2016-01-30 11:38:21
categories: other
tags: other
---
![](/images/s40.jpg)

# php exec debug way


```
exec('sh /home/www/shell/update.sh 2>&1', $output);

print_r($output);
```