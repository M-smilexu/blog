---
title: "mongodb 教程"
date: 2015-05-20 16:22:15
photos: /images/mongodb.png
categories: [coding, mongodb]
layout: post
description: "MongoDB 是一个基于分布式文件存储的数据库。由C++语言编写。旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。"
---

### mac 上安装mongodb

使用brew安装，更多安装方式请参考[mongodb官网](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-os-x/)

#### 1、安装mongodb

    brew update

    brew install mongodb

#### 2、建立存放数据文件夹

    sudo mkdir -p /data/db

#### 3、修改文件夹权限

    sudo chmod a+wr /data/db

#### 4、运行

    mongod


### 查找，更新，删除操作

删除collection

    db.users.remove({});

    db.users.drop();

更新collection

    var user = db.users.findOne({'name': 'smilexu'});
    user.friends = 32;
    db.users.update({'name': 'smilexu'}, user);

更新collection, 加1

    db.users.update({'name': 'smilexu'}, {$inc: {'age': 1} });

更新collection, 设置新字段

    db.users.update({'name': 'smilexu'}, {$set: {'enemies': 2} });

更新collection, 删除字段

    db.users.update({'name': 'smilexu'}, {$unset: {'enemies': 2} });

更新collection, 更新字段类型

    db.users.update({'name': 'smilexu'}, {$set: {'friends': []} });

更新collection, 增加数组数据

    db.users.update({'name': 'smilexu'}, {$push: {'friends': {'name': 'liuyi', 'age': 20} }});

更新collection, 增加数组数据（避免重复数据）

    db.users.update({'name': 'smilexu'}, {$addToSet: {'friends': {'name': 'adu', 'age': 20}}});

更新collection, 循环增加数组数据（避免重复数据）

    db.users.update({'name': 'smilexu'}, {$addToSet:{'friends': {$each: [{'name': 'adu', 'age': 20}, {'name': 'xiaoa', 'age': 20}, {'name': 'oushuai', 'age': 20}]}}});

更新collection, 删除数组最后一个数据

    db.users.update({'name': 'smilexu'},{$pop: {'friends': 1}});

更新collection, 删除数组中指定字段

    db.users.update({'name': 'smilexu'},{$set:{'drinks': ['tea', 'coffee','juice']}});

    db.users.update({'name':'smilexu'},{$pull: {'drinks': 'tea'}});

更新collection, 更新指定字段+1

    db.users.update({'name': 'smilexu'},{$inc: {"friends.0.age": 1}});

更新collection, 更新匹配字段数据（在不知道索引的情况下）

    db.users.update({"friends.name": "liuyi"}, {$set: {"friends.$.age": 10}});

更新collection, 更新所有匹配的数据

    db.users.update({},{$set: {'age': 20}}, false, true);

查看更新信息

    db.runCommand({getLastError: 1})

### 数据库备份，还原

备份

    mongodump -h host -d dbname -o outputdirectory

    mongodump -h 127.0.0.1 -d test -o /data/dump

还原

    mongorestore -h host -d dbname --directoryperdb sourcedirectory

    mongorestore -h 127.0.0.1 -d test --directoryperdb /data/dump/test
