---
layout: post
title: Two-Factor Authentication (2FA) with Google Authenticator & PHP
author: Le Quyet Tien
date: '2020-03-25 15:34:00 +0700'
category: php
summary: Xác thực hai yếu tố đơn giản với Google Authenticator & PHP
thumbnail: google-authentication-php.jpg
---

# Two-Factor Authentication (2FA) with Google Authenticator & PHP

Gần đây mình có nghiên cứu và triển khai chức năng xác thực hai yếu tố trong một project PHP bằng cách sử dụng Google Authenticator. Vì vậy hôm nay mình muốn chia sẻ nó cho những ai chưa làm được.

## 1. Xác thực 2 yếu tố là gì?

Xác thực hai yếu tố, hay còn được viết tắt là 2FA (Two-Factor Authentication), là một cách rất an toàn để bảo vệ tài khoản trực tuyến của bạn. Nó hoạt động bằng cách yêu cầu bạn thêm một bước nữa vào quá trình đăng nhập thông thường của bạn để định danh người đăng nhập có phải là bạn hay không, hay là một ai khác. Qúa trình 2FA dựa trên 2 yếu tố cơ bản, yếu tố thứ nhất được hiểu đơn giản là những gì bạn biết (username và password của bạn) và yếu tố thứ hai gắn liền với những gì bạn có (điện thoại di động của bạn).

## 2. Google Authenticator hoạt động như thế nào?

Google Authenticator là một ứng dụng miễn phí cho smartphone. Nó hoạt động như sau:

1. Khi bật 2FA, ứng dụng mà bạn đang bảo mật sẽ tạo ra một mã QR, người dùng scan nó bằng Google Authenticator để thêm profile vào ứng dụng.

2. Sau khi đăng nhập bằng username và password, người dùng sẽ sử dụng mã xác thực từ Google Authenticator để điền vào bước xác thực thứ 2.

Mã xác thực sẽ được tạo mới sau mỗi 30 giây.

## 3. Cài đặt Google Authenticator

Cài đặt ứng dụng Google Authenticator trên điện thoại:

- [Android](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2)

- [iOS](https://apps.apple.com/us/app/google-authenticator/id388497605)

## 4. Triển khai Google Authenticator trên website sử dụng PHP

### 4.1. Cài đặt Google Authenticator PHP class

Chúng ta sẽ sử dụng thư viện nguồn mở [PHPGangsta/GoogleAuthenticator](https://github.com/PHPGangsta/GoogleAuthenticator) để:

- Tạo mã QR cho người dùng scan khi họ bật 2FA.

- Xác minh mã người dùng nhập khi đăng nhập.

```bash
composer require phpgangsta/googleauthenticator:dev-master
```

### 4.2. Tạo mã QR

```php
<?php
require_once 'PHPGangsta/GoogleAuthenticator.php';

$ga = new PHPGangsta_GoogleAuthenticator();

$secret = $ga->createSecret();
// Lưu biến này vào database để verify sau này

$qrCodeUrl = $ga->getQRCodeGoogleUrl('Blog', $secret);  
// Trả về mã QR này để người dùng scan bằng ứng dụng Google Authenticator trên điện thoại
echo '<img src="'.$qrCodeUrl.'"/>';
```

### 4.3. Xác minh mã

```php
require_once 'PHPGangsta/GoogleAuthenticator.php';

$ga = new PHPGangsta_GoogleAuthenticator();
// TODO: lấy giá trị biến $secret của người dùng đã đăng nhập từ database
$check_this_code = $_POST['code'];

$checkResult = $ga->verifyCode($secret, $check_this_code, 2);
if ($checkResult) {
  echo 'Success!';
} else {
  echo 'Invalid login';
}
```
