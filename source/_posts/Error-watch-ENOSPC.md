title: 'Error: watch ENOSPC'
date: 2015-10-11 20:48:38
categories: other
tags: other
---

![](/images/s25.jpg)

#  Error: watch ENOSPC


`solution`
```
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```
