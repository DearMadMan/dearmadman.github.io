title: git Continuous integration practice
date: 2015-10-13 11:22:03
categories: [linux]
tags: git 
---

![](/images/s26.jpg)

# git Continuous integration practice

## step 1: Clone your project on your server

```bash
git clone git@somegitserver:yourusername/yourprojectname.git

```

## step 2: Add WebHook on you gitServer

```bash
 #webhoos url : http://test.domian.com/sync.php

```


## step 3: Config your server auth authorization
```bash
useradd sync_user

```

## step 4: Config your php-fpm run level

```bash
vi /usr/local/web/php/etc/php-fpm.conf

```
`change`:

```bash
user = sync_user
group = sync_user

```

## step 5: set you web dir's owner
```bash
chown sync_user:sync_user -R  myweb

```

## step 6: add sync.php into your web root dir
```bash
	vi myweb/sync.php
```
`change`

```bash

	<?php

	date_default_timezone_set('PRC');
	$valid_ip = array('127.0.0.1');
	$client_ip = $_SERVER['REMOTE_ADDR'];
	$fs = fopen('/home/www/logs/example_hook.log', 'a');
	fwrite($fs, 'Request on ['.date("Y-m-d H:i:s").'] from ['.$client_ip.']'.PHP_EOL);

	if ( ! in_array($client_ip, $valid_ip))
	{
	    echo "error 10002";
	    fwrite($fs, "Invalid ip [{$client_ip}]".PHP_EOL);
	    exit(0);
	}
	$json = file_get_contents('php://input');
	$data = json_decode($json, true);
	fwrite($fs, 'Data: '.print_r($data, true).PHP_EOL);
	fwrite($fs, '======================================================================='.PHP_EOL);
	fclose($fs);
	exec("sh /home/www/shell/update.sh",$tt);
	print_r($tt);

```

## step 7: code the shell:

```bash
vi /home/www/shell/update.sh

```

`change:`


```bash
#!/etc/bash
cd /home/www/myweb
git remote update -p
git checkout -f origin/master
git submodule update --init

```

## step 8: login your server with user sync_user 

## step 9: generate your public key and add to your gitserver

```bash
ssh-keygen -t rsa -C "YOUR_EMAIL@YOUREMAIL.COM"

#when done

cat ~/.ssh/id_rsa.pub

#copy to your gitserver

```

## step 10: test the shell (must run once)
```bash
sh update.sh

```


