---
layout: post
title: Kết nối Firebase và Arduino
date: 2020-05-30 18:50:20 +0700
description: # Add post description (optional)
img:  /firebase-arduino/maxresdefault.jpg # Add image post (optional)
fig-caption: /firebase-arduino/maxresdefault.jpg # Add figcaption (optional)
tags: [Firebase, Arduino]
published: true
---
<h1>Kết nối firebase và Arduino</h1>
<h2>Mô tả dự án</h2>
Firebase là một dịch vụ database thời gian thực của google. Firebase có miễn phí đối với Realtime Database Spark (100MB) rất phù hợp với nhu cầu của sinh viên (dùng để nghiên cứu). Với cách dùng cloud này, bạn có thể lưu dữ liệu cảm biến của mình trên cloud và có thể truy cập bất kì đâu có internet (app, web, ...). Và đặc biệt, độ trễ upload dữ liệu đều phụ thuộc và đường truyền tốc độ internet của ban.
Trong dự án này, tôi sẽ hướng dẫn bạn tạo tài khoản Firebase, kết nối Arduino Wemos D1 với Firebase.
<h2>1.Firebase là gì?</h2>
![firebase](https://i.ytimg.com/vi/fgT6r4f9Apc/maxresdefault.jpg) 

FireBase có thể rất mạnh mẽ đối với ứng dụng backend, nó bao gồm: lưu trữ dữ liệu, xác thực người dùng, static hosting (tức là chỉ có trang html và javascript). Nên lập trình viên chỉ cần chú tâm đến việc nâng cao trải nghiệm người dùng.

> Vì firebase không hề miễn phí khi scaleout ra nên mình khuyên các bạn chỉ dùng firebase cho các ứng dụng học tập hoặc các ứng dụng bán ở thị trường nước ngoài. Giá firebase rất đắt khi nhu cầu dùng tăng cao nhất là khi bị đối thủ ddos giả lập lại lưu lượng truy cập.
> 
> Luôn ghi nhớ một câu: "Không có buổi trưa nào miễn phí cả!".

Và cái quan trọng trong bài viết ngày hôm nay mà mình cùng bạn sẽ khám phá, đó là Firebase Realtime Database. Một loại cơ sở dữ liệu dạng JSON rất dễ để tiếp cận đối với dân kỹ thuật không chuyên về database! Bạn cứ tưởng tượng nó là một cái mảng chứa dữ liệu mà khi có dữ liệu mới thì nó sẽ cập nhập ngay lập tức lên web mà không tốn bất kỳ một dòng code nào. Rất phù hợp cho các bạn làm prototype mà không hiểu sâu về database.
<h2>2.Tạo dự án trên Firebase.</h2>
Bạn truy cập vào [firebase](https://firebase.google.com/).
Đăng kí tài khoản firebase (thường bạn dùng tài khoản google là được).
Chọn Go to console. Tại đây bạn chọn Add project:
![add project](/assets/img/firebase-arduino/add-project-firebase.png)
> Các bạn điền đầy đủ thông tin và làm theo hướng dẫn để tạo project.
> đến đây mọi rất easy phải không nào 😀.

Kích hoạt Realtime Database: chọn Database > Realtime Database:
![create realtime db](/assets/img/firebase-arduino/create-realtimedb.png)
Tiếp theo chúng ta sẽ thay đổi quyền để tất cả mọi người cùng có quyền truy cập vào project (vấn đề an ninh chưa thực sự cần thiết). Vẫn chọn Realtime Database như trên, vào mục Rules, và thay đổi như hình.<br>
![change rules](/assets/img/firebase-arduino/change-rules.png)

Chúng ta sẽ lấy ID project và key secrets để dùng trong code Arduino.
<h3>Lấy key secrets</h3>
*Vào Project Overview > Users and permissions > Service accounts.*<br>

![key secrets](/assets/img/firebase-arduino/get-key-secrets.png)
<h2>ID</h2>

![firebase id](/assets/img/firebase-arduino/firebase-id.png)
<h2>3. Lập trình Arduino</h2>
> Chuẩn bị phần cứng:<br>
	- Kit Wemos D1 (uno wifi).<br>
	- DHT11<br>

**Sơ đồ nối chân**<br>
	
<style>
.tablelines table, .tablelines td, .tablelines th {
        border: 1px solid black;
        }
</style>
|DHT11|Wemos D1  |
|--|--|
| GND |GND  |
|VCC|VCC|
|OUT|D9|
{: .tablelines}

> Các bạn phải setup cho Arduino IDE để phù hợp với Wemos D1 trước khi nạp code. (trong document của nhà sản xuất Wemos D1, tôi không nhắc lại).

Các bạn nạp đoạn code sau cho Arduino.

    #include <Adafruit_Sensor.h>
    #include <DHT.h>
    #include <ESP8266WiFi.h>
    
    #include <Firebase.h>
    #include <FirebaseArduino.h>
    #include <FirebaseCloudMessaging.h>
    #include <string>
    
    #define WIFI_SSID "xxxx"
    #define WIFI_PASSWORD "yyyy"
    #define FIREBASE_HOST "uuuu"
    #define FIREBASE_AUTH "vvvv"
    
    const int DHTTYPE = DHT11;
    const int DHTPIN = 2; //chân D9
    
    DHT dht(DHTPIN, DHTTYPE);
    
    void setup() {
      Serial.begin(9600);
      Firebase.begin(FIREBASE_HOST, FIREBASE_AUTH);
      dht.begin();
      delay(2000);
    }
    
    void loop() {
      float t = dht.readTemperature();
      float h = dht.readHumidity();
      if (isnan(h) || isnan(t)) {
        Serial.println(F("Failed to read from DHT sensor!"));
        return;
      }
      Firebase.setFloat("Temp",t);
      Firebase.setFloat("Humidity",h);
      delay(5000);
    }

> Thay xxxx bằng trên wifi bạn kết nối vào<br>
> yyyy là mật khẩu wifi<br>
> uuuu là ID của project Firebase các bạn đã lấy ở phần trên<br>
> vvvv là key secrets.<br>

Các bạn nhớ cài đặt đầy đủ thư viện nhé! Đều có hết trên libraries của Arduino IDE.<br>
Cuối cùng chúng ta biên dịch và chạy thôi. Rất là dễ đúng không nào!<br>
Tôi kết thúc bài viết tại đây, series tiếp theo tôi sẽ hướng dẫn lập trình firebase với android để điều khiển bật tắt các thiết bị (arduino). Chúc các bạn lập trình vui vẻ. 
