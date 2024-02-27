---
layout:     post
title:      "JavaWeb文件的上传与下载"
date:       2024-2-27
author:     "Mtz"
catalog: true
tags:
    - Java
    - Java项目
---

环境：HTML+SpringBoot

# 前端

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>我的</title>
    <link rel="stylesheet" href="css/bootstrap.min.css">
</head>
<body>
<a href="/book/Navigation.html" class="btn btn-primary">导航条</a>
<a href="/book/index.html" class="btn btn-primary">登录页面</a>

<h2>文件上传</h2>
<form id="uploadForm" enctype="multipart/form-data">
    <input type="file" name="file" id="fileInput" />
    <button type="button" onclick="uploadFile()">上传文件</button>
</form>

<h2>文件下载</h2>
<button type="button" onclick="downloadFile()">下载文件</button>

<script src="js/jquery-3.3.1.min.js"></script>
<script src="js/bootstrap.min.js"></script>
<script>
    // 文件上传函数
    function uploadFile() {
        // 获取文件输入框的 DOM 元素
        var fileInput = $('#fileInput')[0];
        console.log(fileInput)

        // 获取选择的文件
        var file = fileInput.files[0];
        console.log(file)

        // 创建 FormData 对象，用于将文件数据发送到服务器
        var formData = new FormData();
        formData.append('file', file);

        // 使用 jQuery 的 AJAX 方法发送文件到后端
        $.ajax({
            // 后端接收文件的URL
            url: '/api/upload',

            // 请求类型为 POST
            type: 'POST',

            // 发送的数据为 FormData 对象
            data: formData,

            // 不对数据进行处理，由 FormData 处理
            processData: false,

            // 不设置内容类型，让浏览器自动识别
            contentType: false,

            // 成功时的回调函数
            success: function(data) {
                console.log('文件上传成功:', data);
            },

            // 失败时的回调函数
            error: function(error) {
                console.error('文件上传失败:', error);
            }
        });
    }

    // 文件下载
    function downloadFile() {
        window.location.href = '/api/download';
    }
</script>
</body>
</html>

```

# 后端

```java
package com.ma.book.controller;

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

@RestController
@RequestMapping("/api")
public class FileController {
    // 文件上传目录
    private static final String UPLOAD_DIR = "D:\\";

    // 上传的文件名
    private static final String FILE_NAME = "test.txt";

    // 处理文件上传请求
    @PostMapping("/upload")
    public ResponseEntity<String> handleFileUpload(@RequestParam("file") MultipartFile file) {

        System.out.println(file);

        try {
            // 获取上传文件的字节数组
            byte[] bytes = file.getBytes();

            // 构建文件路径
            Path path = Paths.get(UPLOAD_DIR + "/" + FILE_NAME);

            // 将字节数组写入文件
            Files.write(path, bytes);

            // 返回上传成功的响应
            return ResponseEntity.ok().body("文件上传成功");
        } catch (IOException e) {
            // 处理上传文件异常
            return ResponseEntity.status(500).body("上传文件时发生错误");
        }
    }

    // 处理文件下载请求
    @GetMapping("/download")
    public ResponseEntity<byte[]> downloadFile() throws IOException {
        // 构建文件路径
        Path path = Paths.get(UPLOAD_DIR + "/" + FILE_NAME);

        // 读取文件的字节数组
        byte[] fileContent = Files.readAllBytes(path);

        // 设置响应头，告诉浏览器文件是一个附件，并设置下载文件名
        return ResponseEntity.ok()
                .header("Content-Disposition", "attachment; filename=" + FILE_NAME)
                .body(fileContent);
    }
}

```

