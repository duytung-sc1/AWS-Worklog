---
title: "Cloud One - File Storage Security"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 4.1.2. </b> "
---

## File Storage Security (Bảo mật Lưu trữ Tệp)

File Storage Security là một phần của nền tảng dịch vụ bảo mật Trend Micro Cloud One™, giúp tổ chức của bạn xây dựng và vận hành các ứng dụng một cách an toàn bằng cách cung cấp các biện pháp kiểm soát hoạt động trơn tru trên cả cơ sở hạ tầng hiện tại của bạn lẫn các luồng mã (code streams) hiện đại, chuỗi công cụ phát triển và các yêu cầu đa nền tảng. File Storage Security giúp đảm bảo các Amazon S3 bucket của bạn không chứa phần mềm độc hại bằng cách triển khai bảo mật cloud-native (nguyên bản đám mây) có khả năng tích hợp linh hoạt vào các quy trình làm việc (workflows) Amazon S3 tùy chỉnh của bạn.

File Storage Security được hỗ trợ bởi Trend Micro Research, bộ phận liên tục theo dõi và thu thập dữ liệu về các mối đe dọa từ khắp nơi trên toàn cầu bằng cách sử dụng các phân tích phát hiện nâng cao nhằm ngay lập tức chặn đứng các cuộc tấn công trước khi chúng có thể gây hại cho tổ chức của bạn.

## Hiểu cách Trend Micro có thể giúp Bảo mật Object Storage của bạn

Có hai cách chính để sử dụng File Storage Security trong Cơ sở hạ tầng AWS của bạn:

* **Quét tệp tải lên (File Upload Scan)**: Bất kỳ ứng dụng cloud-native nào sử dụng sức mạnh của đám mây để cung cấp các tính năng tích hợp với Amazon S3 bucket đều có nguy cơ tiếp xúc với phần mềm độc hại. File Storage Security cho phép khách hàng tích hợp khả năng quét trực tiếp vào tài khoản AWS của họ, để tất cả các tệp bạn nhận được trong Amazon S3 bucket từ các nguồn bên ngoài đều có thể được quét phần mềm độc hại trước khi ứng dụng của bạn kịp sử dụng nguồn dữ liệu đó.
* **Quy trình làm việc tự động (Automated Workflow)**: Các nhóm phát triển tận dụng các thiết kế theo hướng sự kiện (event-driven) với Amazon S3 để tự động hóa việc xử lý dữ liệu tải lên bucket. File Storage Security được thiết kế dựa trên kiến trúc này, cho phép các nhóm phát triển tích hợp liền mạch việc quét tệp vào quy trình làm việc tự động của họ.
* **Quét toàn bộ hoặc Quét theo lịch trình S3 bucket (S3 bucket Full Scan or Scheduled Scan)**: Giúp các nhóm bảo mật quét toàn bộ đối tượng bên trong AWS S3 bucket để tìm các nội dung độc hại. [Liên kết GitHub cho Plugin](#) 

## Kiến trúc

Các kiến trúc ứng dụng cloud-native tích hợp các dịch vụ lưu trữ tệp/đối tượng trên đám mây vào quy trình làm việc của chúng, tạo ra một vectơ tấn công mới khiến chúng dễ bị tổn thương trước các tệp độc hại. File Storage Security bảo vệ quy trình làm việc thông qua các kỹ thuật đổi mới, chẳng hạn như quét phần mềm độc hại, tích hợp vào các quy trình tùy chỉnh của bạn và hỗ trợ nền tảng lưu trữ đám mây diện rộng – giải phóng bạn để tiến xa hơn và làm được nhiều việc hơn. File Storage Security cung cấp các tùy chọn triển khai linh hoạt, bao gồm mô hình quảng bá đa bucket (multi-bucket promotional model - quét từ bucket này sang bucket làm sạch/cách ly khác) hoặc kiến trúc một bucket hiệu quả.
![fss](/images/5-Workshop/5.1-Workshop-overview/fss.png)

Kiến trúc của File Storage Security được xây dựng để dễ hiểu và dễ giám sát tất cả các bucket. Ngay khi một tệp mới được tải lên bucket, hệ thống sẽ tạo ra một tin nhắn SQS bên trong tài khoản AWS của bạn nhằm kích hoạt một hàm AWS Lambda. Hàm này sẽ thực thi quá trình quét và gắn thẻ (tag) tệp là độc hại (malicious) hoặc sạch (clean), tùy thuộc vào kết quả quét. Bạn cũng có thể kết nối các plugin để thực hiện các hành động bổ sung, ví dụ: ngay khi tệp được gắn thẻ là độc hại, plugin sẽ tự động di chuyển tệp đó đến một bucket cách ly (quarantine bucket).

Đi sâu hơn một chút vào các chi tiết kiến trúc, toàn bộ cấu trúc triển khai được tạo thành từ hai ngăn xếp (stack) khác nhau:

* **Storage Stack (Ngăn xếp Lưu trữ)**: Ngăn xếp này chịu trách nhiệm tiếp nhận thông báo cho Amazon S3 bucket, cũng như gửi các tệp mới tải lên cho Scanner Stack để quét bảo mật. Sau khi quá trình quét hoàn tất, một chủ đề Amazon SNS sẽ được xuất bản và tệp được gắn thẻ là "độc hại" hoặc "sạch" — có sẵn các plugin bổ sung để thêm nhiều chức năng hơn cho ngăn xếp này.
* **Scanner Stack (Ngăn xếp Quét)**: Ngăn xếp này chịu trách nhiệm thực thi quá trình quét và xuất bản kết quả lên Amazon SNS ScanResultTopic. Khi Scanner Stack nhận được yêu cầu từ Storage Stack, nó sẽ xử lý và sử dụng một hàm AWS Lambda để thực thi việc quét. Giống như nhiều công nghệ của Trend Micro, File Storage Security có thể tận dụng Trend Micro™ Smart Protection Network™ để cập nhật thông tin về các mối đe dọa mới nhất.
![fss_archi](/images/5-Workshop/5.1-Workshop-overview/fss_architecture.png)

## Thông tin Quét Tệp

Mã băm (hash) của tệp sẽ được gửi đến Trend Micro Global Smart Scan Server khi quá trình quét tệp diễn ra, cho phép File Storage Security xác định các mã băm tệp độc hại.

Trong giải pháp smart scan (quét thông minh), client sẽ gửi các mã băm của tệp được xác định bởi công nghệ Trend Micro đến các Smart Scan Server. **Cloud One - File Storage Security không bao giờ gửi toàn bộ tệp đi** và mức độ rủi ro của tệp hoàn toàn được xác định bằng cách sử dụng các mã băm này.
