---
layout:     post
title:      "JavaWeb网络图片的上传与下载"
date:       2024-2-27
author:     "Mtz"
catalog: true
tags:
    - Java
    - Java项目
---

环境：HTML+SpringBoot

流程：前端选择文件，上传到服务器本地，回显url。

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

<h2>文件查看</h2>
<!-- 显示上传后的图片 -->
<img id="uploadedImage" style="max-width: 400px; max-height: 400px;" />

<script src="js/jquery-3.3.1.min.js"></script>
<script src="js/bootstrap.min.js"></script>
<script>
    // 文件上传函数
    function uploadFile() {
// 获取文件输入框的 DOM 元素
        var fileInput = $('#fileInput')[0];

// 获取用户选择的文件对象
        var file = fileInput.files[0];

// 创建 FormData 对象，用于将文件数据发送到服务器
        var formData = new FormData();

// 将用户选择的文件添加到 FormData 对象中，字段名为 'file'
        formData.append('file', file);
        $.ajax({
            url: '/api/upload',
            type: 'POST',
            data: formData,
            processData: false,
            contentType: false,
            success: function(data) {
                console.log('文件上传成功:', data);
                // 显示上传后的图片
                $('#uploadedImage').attr('src', data.url);
            },
            error: function(error) {
                console.error('文件上传失败:', error);
            }
        });
    }
</script>
</body>
</html>

```

# 后端

```java
package com.ma.book.common;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class MyWebConfig implements WebMvcConfigurer {
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/images/**").addResourceLocations("file:D:/newImg/");
    }
}
```

分析：

1. @Configuration 注解： 表明这是一个配置类，Spring IoC容器会处理它，使得配置生效。

2. MyWebConfig 类实现 WebMvcConfigurer 接口： 通过实现这个接口，你可以自定义Spring MVC的配置。

3. addResourceHandlers 方法： 覆盖了WebMvcConfigurer接口中的方法，用于添加静态资源的处理。这里配置了一个资源处理器，将访问 /images/** 路径的请求映射到本地文件系统路径 "file:D:/newImg/"。

4. addResourceHandler("/images/**")：指定了匹配的URL模式，即以 /images/ 开头的URL将被处理。

5. addResourceLocations("file:D:/newImg/")：指定了本地文件系统中的路径，即 /images/ 路径下的请求将映射到本地文件系统的 D:/newImg/ 目录。

这段配置的目的是使得应用程序能够通过 /images/ 路径访问 D:/newImg/ 目录下的静态资源，例如图片文件。这对于提供静态资源服务，如用户上传的图片，是非常有用的。

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

    private static final String UPLOAD_DIR = "D:\\newImg\\";
    private static final String FILE_NAME = "test.jpg";

    @PostMapping("/upload")
    public ResponseEntity<UploadResponse> handleFileUpload(@RequestParam("file") MultipartFile file) {
        try {
            byte[] bytes = file.getBytes();
            Path path = Paths.get(UPLOAD_DIR + "/" + FILE_NAME);
            Files.write(path, bytes);

            // 构建文件URL路径
            String fileUrl = "/images/" + FILE_NAME;

            // 返回上传成功的响应，包含文件的URL路径
            return ResponseEntity.ok().body(new UploadResponse("文件上传成功", fileUrl));
        } catch (IOException e) {
            return ResponseEntity.status(500).body(new UploadResponse("上传文件时发生错误", null));
        }
    }
    // 用于封装上传成功后的响应，包含文件的URL路径
    public static class UploadResponse {
        private String message;
        private String url;

        public UploadResponse(String message, String url) {
            this.message = message;
            this.url = url;
        }

        public String getMessage() {
            return message;
        }

        public String getUrl() {
            return url;
        }
    }
}

```



