---
layout: default
title: BroadcastReceiver for BOOT_COMPLETED is too slow
nav_order: 11
---
BOOT_COMPLETED thường sẽ được system ném ra sau khi hoàn thành các tác vụ đảm bảo hoạt động, sau thời điểm này system ở trạng thái stable đủ để làm việc
Vì lý do đó, thông thường nếu developer đăng ký nhận BOOT_COMPLETED trong AndroidMainifest - với mong muốn lắng nghe thời điểm device khởi động lại để xử lý một vấn đề gì đó - sẽ luôn bị delay ko như expect, cụ thể: *sau khi device khởi động, một khoảng thời gian nhất định sau đó chúng ta mới nhận được BOOT_COMPLETED*

Có thể thử thêm các cách sau:
# LOCKED_BOOT_COMPLETED
Official Document - tuy nhiên system ở thời điểm này có nhiều limitation, ví dụ hạn chế storage, internet..etc - nên đọc lại kỹ doc để biết bài toán mình có sử dụng được ko? 
# BOOT_COMPLETED with category tag 
*Cách làm này chưa có tài liệu official để kiểm chứng*
![alt text](https://i.ibb.co/QPV7FfX/Screenshot-2023-03-06-at-09-06-20.png)
https://stackoverflow.com/questions/43988566/broadcastreceiver-for-boot-completed-is-too-slow
