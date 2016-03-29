title: setup eslint with es6 in sublime text
date: 2016-02-15 09:15:26
categories: javascript
tags: javascript
---

![](/images/s42.jpg)

# setup eslint with es6 in sublime text

## get started with eslint

```
  npm install eslint -g 
  # or 
  npm install eslint 

  npm install babel-eslint -g
```

you can install it either globally or locally.


## configure

create a `.eslintrc` in the root of your project.Then you can add `globals`,
set up your environment with `env`, and add rules as well.

```

{
  "globals": {
    // Put things like jQuery, etc
    "jQuery": true,
    "$": true
  },
  "env": {
    // I write for browser
    "browser": true,
    // in CommonJS
    "node": true
  },
  // To give you an idea how to override rule options:
  "rules": {
    // Tons of rules you can use, for example...
    "quotes": [1, "double"]
  }
}

```

### rules 
to use a rule, add it's name, then pass an array. the array is an array of options.
the first is either...

* 0 - Disable the rule 
* 1 - Warn about the rule
* 2 - Throw error about the rule

see the full list of rules [here](http://eslint.org/docs/rules/)

## es2015

### parsers 
there are different types of parsers avaliable. One of them is the `babel-eslint`...

```
  "parsers": "babel-eslint"
```

this is will allow you to utilize es6 syntax.

## sublime-linter 
there's a great plugin for sublime text called `sublime-linter` . once it is installed, there are a bunch of other plugins for it as well, including one for eslint. install it whit package control.


### sublimelinter-contrilb-eslint

configure sublime-linter and install `sublimelinter-contrilb-eslint` ,
now when you have invalid javascript including es6, you'll see warning in the code 


reference:
[http://jonathancreamer.com/setup-eslint-with-es6-in-sublime-text/](http://jonathancreamer.com/setup-eslint-with-es6-in-sublime-text/)





