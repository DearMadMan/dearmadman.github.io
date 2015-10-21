title: "laravel model's fillable and guarded"
date: 2015-08-14 09:13:25
categories: laravel
tags: laravel
---
![guarded](/images/c1.jpg)
# laravel的模型中fillable与guarded解析

## 简介

laravel的模型中有两个`protected`字段`fillable`与`guarded`，注意：必须是`protected`以上开放程度。
我们经常通过提交表单进行数据的增删改，为了方便的进行数据批量修改操作laravel提供了批量赋值机制：

假如我们想要在数据库表中添加一行，我们可以使用模型这么操作：
```
	$model=Model::creat($request->all());
```
ok，这样我们就直接将表单中提交过来的所有信息直接录入进了数据库,是不是非常非常的方便~,`但是`这样是非常不安全的，对于用户输入的数据，我们应该永远的谨慎对待。
我们不能将用户输入的恶意代码一起提交到数据库中吧~~~。
于是laravel中的模型就提供了`fillable`和`guarded`,使用也是非常简单，两者是互斥关系，存在一个就好,`如果同时存在,fillable优先级较高哦`、

`fillable`变量存储一个`允许`自动填充的数组，这里填写`允许`自动填充的模型字段，比如：
```
    protected  $fillable=['username','password','phone','email'];
```

而`guarded`变量存储一个`不允许`自动填充的数组，这里填写`不允许`自动填充的模型字段，比如：
```
    protected  $guarded=['id','manager_id'];
```

有时候我们希望通过`Model::create($data)`的方式存储表单数据，我们会在$data中存放一些敏感信息，但是一些敏感信息，create方法会直接过滤掉怎么办?
难道要存入数据库之后再....
```
$model->manager_id=1;
$model->save();
```
No!!!,这不是要写入两次数据库嘛！！！
嗯，我们可以这么来
```
$model=new Model($data);
$model->manager_id=1
$model->save();
```
对的，我们先使用laravel自带的批量赋值机制过滤一遍敏感信息，然后我们自己来过滤敏感信息的输入~~

`注意:` 当模型中未指定`fillable`与`guarded`时，即`$fillable=[]` and `$guarded==[*]` 时，传入模型实例化参数时会报`MassAssignmentException`错误；