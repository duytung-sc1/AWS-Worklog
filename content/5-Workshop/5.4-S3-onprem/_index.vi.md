---
title : "Tự động hóa và Thông báo"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 4.4. </b> "
---


##Tự động hóa việc triển khai và thông báo quét.

Sau khi Cloud One - File Storage Security hoàn tất quá trình quét, kết quả quét sẽ được gắn thẻ (tag) vào tệp và xuất bản lên chủ đề Amazon SNS `ScanResultTopic`.

Nếu bạn muốn thực hiện thêm các thao tác với kết quả này, bạn sẽ cần tạo hoặc thêm một hành động hậu kỳ (post-action) diễn ra sau khi quét. Chúng tôi cung cấp nhiều mẫu tích hợp có thể thực hiện với Cloud One File Storage Security trên trang GitHub của mình. Một trong những Post-Actions được sử dụng nhiều nhất là khả năng gửi các tệp sạch đến một Amazon S3 bucket (chuyển tiếp - promote) và gửi các tệp độc hại đến một Amazon S3 bucket khác (cách ly - quarantine). Chúng tôi cũng cung cấp một API để giúp bạn tạo các hành động sau khi quét của riêng mình.


Hãy bắt đầu tạo các hành động hậu kỳ (post-actions) để tự động hóa và giám sát.
