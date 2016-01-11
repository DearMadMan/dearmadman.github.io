title: use es6 with gulp
date: 2016-01-04 14:55:33
categories: other
tags:
---
![](/images/s37.jpg)

# useing es6 with gulp

## check your gulp version

Firstly make sure you have at least version `3.9` of both the CLI and local version of gulp.
To check which version you have,open up terminal and type:

```
gulp -v

```

if you get any versions lower than `3.9`,update gulp in your `package.json` file, and run the follwing to update both versions:

```
npm install gulp && npm install gulp -g
```

## create an es6 gulpfile

To leverage es6 you will need to install `babel`(make sure you have Babel 6) as a dependancy to your project,
along with the es2015 plugin preset:

```
npm install babel-core babel-preset-es2015 --save-dev
```
once this has finished, we need to create a `.babelrc` config file to enable the es2015 preset:

```
touch .babelrc
```

and add the following to the file:

```
{
	"presets": ["es2015"]
}
```

we then need to instruct gulp to use babel. to do this,we need to rename the `gulpfile.js` to `gulpfile.babel.js`:

```
mv gulpfile.js gulpfile.babel.js
```

we can now use es6 via babel!

[https://markgoodyear.com/2015/06/using-es6-with-gulp/](https://markgoodyear.com/2015/06/using-es6-with-gulp/)
