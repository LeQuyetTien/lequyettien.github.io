---
layout: post
title: Tạo hybrid app với Ionic
author: Le Quyet Tien
date: '2020-03-01 17:00:00 +0700'
category: ionic
summary: Hướng dẫn tạo ứng dụng đầu tiên với Ionic
thumbnail: ionic-framework-og.png
---

# Create hybrid app with Ionic

## Yêu cầu

Trước khi bắt đầu với Ionic chúng ta cần cài đặt [Node.js](https://nodejs.org/en/). Trong trang chủ Node.js chúng ta sẽ có 2 phiên bản là LTS (Bản ổn định) và Current (Bản mới nhất). Chúng ta nên chọn cài đặt bản LTS vì nó ổn định hơn.

## Cài đặt Ionic

```bash
npm install -g @ionic/cli
```

## Tạo ứng dụng đầu tiên

```bash
ionic start myApp tabs
```

> - ionic start: là cú pháp tạo mới ứng dụng của Ionic
>
> - myApp: là tên ứng dụng
>
> - tabs: là loại ứng dụng. Có 3 loại chính sau: *blank*, *tabs*, *sidemenu*

## Chạy ứng dụng

```bash
cd myApp
ionic serve
```
