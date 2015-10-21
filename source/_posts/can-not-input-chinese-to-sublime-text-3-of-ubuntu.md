title: 解决ubuntu下sublime text 3无法输入中文
date: 2015-08-29 23:00:40
categories: linux
tags: [linux, ubuntu, sublime]
---
![](/images/s01.jpg)
# can't input chinese to sublime of ubuntu

## 测试环境
* ubuntu 14.04
* 搜狗输入法 for linux
* Sublime Text 3

## 依赖
* libgtk2.0-dev

```
sudo apt-get install libgtk2.0-dev
```


## 编译共享库

```
vi sublime_imfix.c
```
讲下面代码写入 sublime_imfix.c

```
#include <gtk/gtkimcontext.h>
void gtk_im_context_set_client_window (GtkIMContext *context,
         GdkWindow    *window)
{
 GtkIMContextClass *klass;
 g_return_if_fail (GTK_IS_IM_CONTEXT (context));
 klass = GTK_IM_CONTEXT_GET_CLASS (context);
 if (klass->set_client_window)
   klass->set_client_window (context, window);
 g_object_set_data(G_OBJECT(context),"window",window);
 if(!GDK_IS_WINDOW (window))
   return;
 int width = gdk_window_get_width(window);
 int height = gdk_window_get_height(window);
 if(width != 0 && height !=0)
   gtk_im_context_focus_in(context);
}
```

进行便以操作
```
gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
```

将编译完成的共享库移动到sublime目录下:
```
sudo mv libsublime-imfix.so /opt/sublime_text/
```

## 代码嵌入

先修改subl命令 

```
sudo gedit /usr/bin/subl
```

优先加载共享库，使用gtk处理输入，替换：
```
exec /opt/sublime_text/sublime_text "$@"
```
为:
```
LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text "$@"
```
此时使用terminal使用subl命令打开sublime编辑器可以输入中文。

想要在launcher或者Open with 打开的sublime可以输入中文，需要修改desktop文件

```
sudo gedit /usr/share/applications/sublime_text.desktop
```
进行处理：

```
#Exec=/opt/sublime_text/sublime_text %F
Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text %F"

#Exec=/opt/sublime_text/sublime_text -n
Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text -n"

#Exec=/opt/sublime_text/sublime_text --command new_file
Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text --command new_file"

```
保存之后就可以在sublime中输入中文了
