---
title: "Workshop"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

# Nền tảng Đánh giá Bảo mật Website Serverless

#### Tổng quan

**Kiến trúc Serverless trên AWS** cung cấp giải pháp để xây dựng và vận hành các ứng dụng mà không cần quản lý hạ tầng máy chủ. Mô hình này cho phép bạn tập trung hoàn toàn vào mã nguồn và logic nghiệp vụ, đồng thời tự động tối ưu hóa tính sẵn sàng cao và chi phí.

Trong bài lab này, bạn sẽ học cách xây dựng, triển khai và kiểm thử một ứng dụng web serverless hoàn chỉnh, có chức năng thực hiện các đánh giá an toàn thông tin cơ bản đối với các URL mục tiêu.

Bạn sẽ sử dụng ba dịch vụ lõi của AWS để tạo ra nền tảng bảo mật và có khả năng mở rộng này: Amazon S3, Amazon CloudFront và AWS Lambda. Ba dịch vụ này kết hợp chặt chẽ với nhau để cung cấp một giao diện frontend an toàn và một bộ máy xử lý backend linh hoạt:

+ **Amazon S3** - Đóng vai trò là nơi lưu trữ an toàn cho các tệp giao diện web tĩnh (HTML, CSS, JS). Quyền truy cập vào bucket này được thiết lập hạn chế tuyệt đối, chỉ cho phép phân phối nội dung thông qua hệ thống CDN.
+ **Amazon CloudFront** - Đóng vai trò là Mạng phân phối nội dung (CDN). Dịch vụ này cung cấp mã hóa đường truyền HTTPS, bộ nhớ đệm toàn cầu và phân phối an toàn frontend từ S3 đến người dùng cuối thông qua tính năng Origin Access Control (OAC), giúp ngăn chặn mọi truy cập trực tiếp từ Internet.
+ **AWS Lambda** - Hoạt động như bộ máy xử lý tính toán backend. Dịch vụ này tiếp nhận các yêu cầu đánh giá từ frontend, thực thi đoạn mã logic quét bảo mật và trả về kết quả quét.
#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Truy cập đến S3 từ VPC](5.3-S3-vpc/)
4. [Truy cập đến S3 từ TTDL On-premises](5.4-S3-onprem/)
5. [VPC Endpoint Policies (làm thêm)](5.5-Policy/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)
