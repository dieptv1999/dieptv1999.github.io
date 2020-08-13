---

layout: post
title: Công nghệ truyền phát video/âm thanh trực tuyến
date: 2020-06-25 00:30:20 +0700
description: # Add post description (optional)
img:  /video-streaming-technology/caption.png # Add image post (optional)
fig-caption: /video-streaming-technology/caption.png # Add figcaption (optional)
tags: [video streaming]
published: true

---

<h1> Công nghệ truyền video client-server</h1>
Công nghệ truyền phát video là một cách để cung cấp video qua internet. Sử dụng các công nghệ phát trực tuyến, việc phân phối các video và âm thanh qua internet có thể tiếp cận được số lượng lớn khách hàng (lên đến hàng triệu) sử dụng máy tính cá nhân, máy tính xách tay, điện thoại thông minh và một số thiết bị chuyên dụng khác. Gần đây các công nghệ này ngày càng trở lên phổ biến, một số lí do cho sự phát triển của công nghệ truyền phát video là :

- Mạng băng thông rộng đã và đang được triển khai
- Kĩ thuật nén âm thành và video ngày càng hiệu quả
- Chất lượng và sự đa dạng của các dịch vụ âm thanh và video trên internet ngày càng tăng lên.

Có hai cách chính để truyền video/âm thanh qua internet đó là:

- *Chế độ download*: Dữ liệu được tải toàn bộ trước khi được phát. Chế độ này yêu cầu thời gian tải toàn bộ dữ liệu (trễ phụ thuộc vào kích thước dữ liệu) và tốn không gian nhớ.
- *Chế độ Streaming*: Chế độ này không cần bạn phải tải toàn bộ dữ liệu mà có thể tải từng phần của dữ liệu và phát ngay khi nhận được và giải mã dữ liệu.

Trình phát đa phương tiện hiện này là một phần không thể thiếu trong các trình duyệt, plug-in, các phần mềm và thiết bị chuyên dụng như AppleTV, iPod,... Các tập tin video cũng có thể bao gồm các trình phát được nhúng.

<figure style="text-align: center;">
  <img src="/assets/img/video-streaming-technology/streaming-video-transmitter.png" alt="transmitter data">
  <figcaption style="text-align: center;">Kiến trúc truyền phát video ở phía máy chủ</figcaption>
</figure>

Có hai kiểu phân phối nội dung video/âm thanh trực tuyến: truyền dữ liệu theo yêu cầu và theo lịch trình của chương trình (programmed-time).<br>
Trong phân phối theo yêu cầu, người xem có thể chọn nội dung để phát bất cứ lúc nào. Kiểu phân phối này làm tăng chi phí băng thông vì phải thiết lập một luồng mạng riêng cho mỗi người xem.<br>
Trong phân phối theo lịch trình, máy chủ thiết lập một kênh cho người xem theo lịch trình của chương trình đã được định sẵn.

<figure style="text-align: center;">
  <img src="/assets/img/video-streaming-technology/streaming-video-receiver.png" alt="receiver data">
  <figcaption style="text-align: center;">Kiến trúc truyền phát video ở phía bên nhận</figcaption>
</figure>

Đối với những ứng dụng truyền phát video thời gian thực, các gói tin được gửi đi phải đến phía bên nhận kịp thời. Các gói tin trễ quá lâu được thì không được sử dụng và được coi là bị mất ( treated as lost). Công nghệ truyền phát trực tuyến cũng giả định rằng một số gói tin có thể bị loại bỏ, để đáp ứng về mặt thời gian và (hoặc) bằng thông, kết nối người dùng có thể được điều chỉnh theo băng thông người dùng có sẵn.<br>
Trong công nghệ phát trực tuyến, giao thức UDP/IP (User Datagram Protocol) được sử dụng để cung cấp các luồng đa phương tiện dưới dạng một danh sách các gói tin nhỏ. Các giao thức lớp ứng dụng và RTP/RTSP (Real-time Transport Protocol /Real Time Streaming Protocol) được triển khai trên UDP/IP để cung cấp truyền tải mạng end-to-end (đầu cuối) để truyền phát video. <br>
Luồng video được nén bằng bộ mã hóa và giải mã video như *H.264* hoặc *VP8*. Luồng âm thanh được nén bằng bộ mã hóa và giải mã như MP3, Vorbis, AAC. Các luồng âm thanh và video đã được mã hóa được đặt trong một container bitstream (như là một khung/luồng dữ liệu nhị phân) như MP4, FLV, WebM, ASF, ISMA.<br>
Một số bài viết có liên quan:

- <a href="https://www.vocal.com/video/video-streaming-protocols-rtp-rtcp-and-rtsp/">Video Streaming Protocols
- <a href="https://www.vocal.com/video/user-adaptive-video-streaming/">User Adaptive Video Streaming
- <a href="https://www.vocal.com/video/p2p-video-streaming/">P2P Video Streaming
- <a href="https://www.vocal.com/video/video-streaming-to-mobile-device/">Video Streaming to Mobile
- <a href="https://www.vocal.com/video/quality-of-experience-qoe-for-streaming-video/">QoE for Streaming Video
- <a href="https://www.vocal.com/video-codecs/">Video Codecs
- <a href="https://www.vocal.com/resources/research/video/">Video Design
- <a href="https://www.vocal.com/resources/reference-designs/video/">Video Reference Designs