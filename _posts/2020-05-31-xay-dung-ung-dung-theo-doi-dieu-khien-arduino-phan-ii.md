---
layout: post
title: Xây dựng ứng dụng realtime theo dõi nhiệt độ, độ ẩm, ánh sáng sử dụng Firebase Phần II
date: 2020-06-1 0:50:20 +0700
description: # Add post description (optional)
img:  /app-project-ii/fig-caption-firebase-android-arduino.png # Add image post (optional)
fig-caption: /app-project-ii/fig-caption-firebase-android-arduino.png # Add figcaption (optional)
tags: [Firebase, Arduino, Android, Mermaid]
published: true

---

Tiếp nối dự án về ứng dụng theo dõi nhiệt độ, độ ẩm, ánh sáng. Trong bài viết này t sẽ bắt tay và phân tích và thiết kế hệ thống đối với ứng dụng này. Phần trước về Arduino các bạn có thể theo dõi tại [Link]({% post_url  2020-05-31-xay-dung-ung-dung-theo-doi-dieu-khien-arduino %}) <br>

<h2> Sơ đồ luồng dữ liệu của hệ thống</h2>

<div class="mermaid">
graph LR
    A[Android App] --> B{Realtime Database}
    B --> A
    B --> D{Uno wifi}
    D --> B
    D --> C(Adapter)
    E(Sensor) --> D
</div>

<h2> 1. Tính năng của ứng dụng</h2>

- Hiển thị nhiệt độ, độ ẩm, ánh sáng trên màn hình theo thời gian thực tại từng vị trí có thiết bị đo, giúp người dùng theo dõi và giám sát môi trường tại từng vị trí.

- Theo dõi nhiều vị trí cùng một thời điểm.

- Hiển thị màn hình điều khiển và điều khiển từ xa các thiết bị tai từng vị trí có thiết bị.

- Điều khiển thiết bị từ xa.
<h2>2. Tạo ứng dụng và cấu hình thư viện</h2>
Vì là ứng dụng Android nên chúng ta sẽ sử dụng Android Studio để lập trình. Các bạn cài đặt trước Android Studio.

Sau khi khởi động Android Studio, thực hiện theo các bước để tạo dự án:<br>

> File => New => New Project => Empty Activity => đặt tên dự án và Finish

> Yêu cầu hệ thống đối với ứng dụng:

- API level 21 hoặc hơn

- Gradle 4.1 hoặc hơn

- com.android.tools.build:gradle v3.2.1 hoặc lớn hơn

- compileSdkVersion 29

> Thêm google.services.plugin: tại app/build.gradle thêm vào

    apply plugin: 'com.android.application'
    apply plugin: 'com.google.gms.google-services'// Google Services plugin**

> Thêm Firebase SDKs vào ứng dụng: tại app/build.gradle thêm vào

    dependencies {  
	    implementation 'com.google.firebase:firebase-auth:19.2.0'  	
	    implementation 'com.google.firebase:firebase-firestore:21.4.1' 
    }

Đồng bộ ứng dụng và quá trình tạo project hoàn thành.

<h2>3. Phân tích thiết kế ứng dụng</h2>
<h3>3.1. Biểu đồ ca sử dụng tổng quan</h3>

![Biểu đồ ca sử dụng tổng quan](/assets/img/app-project-ii/bieu-do-ca-su-dung-tong-quan.png)

<h3>3.2 Biểu đồ trình tự</h3>
<h4>3.2.1 Biểu đồ trình tự xem thông tin của từng module.</h4>

![Xem thông tin từng module](/assets/img/app-project-ii/bieu-do-trinh-tu-xem-thong-tin-tung-module.png)

<h4>3.2.2 Biểu đồ trình tự chọn module theo vị trí.</h4>

![Chọn module theo vị trí](/assets/img/app-project-ii/bieu-do-trinh-tu-chon-module-theo-vi-tri.png)

<h4>3.2.3 Biểu đồ trình tự điều khiển thiết bị từ xa</h4>

![Điều khiển thiết bị từ xa](/assets/img/app-project-ii/bieu-do-trinh-tu-dieu-khien-thiet-bi-tu-xa.png)
