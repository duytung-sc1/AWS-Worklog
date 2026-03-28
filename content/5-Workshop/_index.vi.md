---
title : "WORKSHOP"
date : 2026-03-27
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

# AWS S3 BUCKET MALWARE SCANNING WITH TREND MICRO

---
![trendmicro](/images/5-Workshop/TM_logo.png)
## Welcome

Chào mừng bạn đến với workshop thực hành **AWS S3 Bucket Malware Scanning with Trend Micro**.

Trong workshop này, bạn sẽ học cách quét các object được tải lên Amazon S3 buckets để phát hiện mã độc và tích hợp việc quét vào các workflow tùy chỉnh của bạn thông qua tự động hóa với các tài nguyên sẵn có trong môi trường AWS.

Sử dụng Trend Micro Cloud One cùng với AWS S3, AWS Lambda, AWS SQS, AWS SNS và AWS CloudFormation, bạn sẽ có trải nghiệm thực hành trong việc triển khai một hệ thống bảo mật tự động nhằm cung cấp khả năng bảo vệ dữ liệu cho các bucket Amazon S3.

---

## Trend Micro là ai?

Trend Micro là một trong những công ty hàng đầu thế giới về an ninh mạng, giúp đảm bảo an toàn cho việc trao đổi thông tin số.

Với hơn 30 năm kinh nghiệm trong lĩnh vực bảo mật, nghiên cứu mối đe dọa toàn cầu và đổi mới liên tục, Trend Micro giúp nâng cao khả năng chống chịu cho doanh nghiệp, chính phủ và người dùng cá nhân. Bảo mật là trọng tâm cốt lõi của họ, được thể hiện qua các giải pháp luôn theo kịp những mối đe dọa ngày càng phức tạp, môi trường CNTT thay đổi liên tục và hành vi người dùng ngày càng rủi ro.

Các chuyên gia nghiên cứu mối đe dọa và nhà khoa học dữ liệu của Trend Micro sử dụng các kỹ thuật tiên tiến để phân tích dữ liệu và phát hiện mối đe dọa theo thời gian thực.

Điều này đạt được thông qua **trí tuệ an ninh mạng nâng cao (augmented cyber intelligence)**, kết hợp giữa trí tuệ nhân tạo (AI), machine learning và kiến thức chuyên sâu từ các chuyên gia bảo mật – những người liên tục nghiên cứu các chiến thuật, kỹ thuật và quy trình (TTPs) của tội phạm mạng.

Trend Micro sử dụng các hệ thống phân tích bảo mật tự động để nhanh chóng xử lý dữ liệu mối đe dọa toàn cầu và cung cấp khả năng bảo vệ phù hợp với các rủi ro có khả năng ảnh hưởng đến bạn.

Để hỗ trợ quy mô này, họ đã xây dựng một trong những hạ tầng cloud lớn nhất thế giới, cho phép tương quan hóa các mối đe dọa trên nhiều lớp bảo mật, đồng thời cung cấp khả năng quan sát toàn diện trên nhiều nền tảng, hệ thống và người dùng trên toàn cầu.

[Connected Security for a Connected World](https://youtu.be/vh-hA2IwDhI?si=kSbfG3VVRuogjDjN)

---

## Mục tiêu học tập

- Học cách xây dựng kiến trúc cloud-native để bảo vệ dữ liệu lưu trữ trong Amazon S3  
- Hiểu cách thiết kế workflow bảo mật liền mạch để bảo vệ file được upload từ người dùng trên toàn cầu  
- Có trải nghiệm thực hành triển khai và cấu hình Cloud One – File Storage Security trên AWS  
- Xác định các luồng dữ liệu cần tích hợp quét mã độc tự động trong pipeline xử lý dữ liệu  

---

## Đối tượng tham gia

- Site Reliability Engineers (SREs)  
- Developers  
- DevOps Engineers  
- Cloud Architects  
- Solution Architects  
- Nhóm bảo mật thông tin  
- GRC teams  
- Tech Leads và Program Managers (PM)  
