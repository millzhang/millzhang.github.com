---
layout:     post
title:      "Node读取本地图片上传至S3"
subtitle:   "图片批量上传服务"
date:       2018-6-5
author:     "millzhang"
tags:
    - node
---

> 浏览器批量上传图片，如果图片数量过多的话，会导致input读取图片过程过长，程序卡死，故而采用node直接读取本地图片的方式，进行批量上传。

### 配置文件（`config.js`）


### 初始化S3上传服务

```js
let s3 = (function() {
  AWS.config.update({
    accessKeyId: config.accessKeyId,
    secretAccessKey: config.secretAccessKey,
    region: config.region
  })
  let s3 = new AWS.S3({
    //apiVersion: '2006-03-01',
    params: { Bucket: config.bucketName }
  })
  return s3
})()

```

### 上传方法

```js
function upload(filePath, fileType) {
  // 异步读取文件
  fs.readFile(filePath, (err, data) => {
    let params = {
      Key: UUID.v1() + `.${fileType}`,//使用node-uuid保证图片key值唯一
      Body: data,
      ACL: 'public-read',
      ContentType: 'image/jpg'
    }

    logger.debug(`图片读取完成，图片名称${filePath}`)
    s3.upload(params, (err, data) => {
      if (err) {
        logger.error('<=文件上传出错=>')
      } else {
        console.log('Successfully uploaded!')
        logger.debug(`第${counter + 1}张图片上传成功!!!`)
        logger.info(data.Location)
        counter += 1
        if (counter == fileLength) {
          logger.debug(`上传任务完成!!!`)
        }
      }
    })
  })
}
``` 

### 读取图片

```js
  //异步读取图片
  fs.readdir(path, function(err, files) {
    if (err) {
      console.log(`<=读取文件出错=>`)
      return
    }
    counter = 0 //重置计数器
    fileLength = files.length
    for (let i = 0; i < files.length; i++) {
      let fileName = files[i],
        fileType = fileName.split('.')[1]
      if (config.extention.includes(fileType)) {
        upload(`${path}/${fileName}`, fileType)
      }
    }
  })
```

### 日志模块

这里使用了`log4js`来进行日志的输出管理，主要目的是记录上传后的文件路径。
