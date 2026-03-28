---
title : "Khả năng Hiển thị với CloudWatch"
date : 2024-01-01
weight : 
chapter : false
pre : " <b> 4.4.1 </b> "
---

## Giám sát các Bản quét của bạn với CloudWatch

Nhật ký (logs) của File Storage Security được lưu trong AWS CloudWatch Logs. Những nhật ký này chứa nhiều thông tin hơn một chút so với những gì hiển thị trong các thẻ `fss-*`.

1. Để xem nhật ký kết quả quét trong AWS CloudWatch Logs, hãy đi tới tài khoản AWS của bạn, vào **CloudFormation > chọn scanner stack của bạn > Resources > nhấp vào liên kết ScannerLogGroup**.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/788ccafc-edec-4d7f-9925-a75263fa025d" />

2. Dịch vụ AWS CloudWatch xuất hiện với mục Log groups (Nhóm nhật ký) được chọn ở bên trái. Trong phần Log streams (Luồng nhật ký), hãy nhấp vào một luồng nhật ký có thời gian sự kiện mới nhất (muộn hơn hoặc bằng thời điểm bạn thêm tệp vào S3 bucket để quét) và mở rộng thông báo sự kiện bắt đầu bằng chữ `scanner result`.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/83aa5aff-7a72-4641-94c3-da8b32fcdd04" />

3. Sự kiện nhật ký sẽ hiển thị và bạn sẽ thấy một khối mã JSON xuất hiện chứa thông tin về bản quét cùng một số chi tiết bổ sung về Lambda Scanner. Hãy thêm vào ô **Filter Events** (Lọc Sự kiện): `scanner result`

Bây giờ bạn sẽ thấy các sự kiện bảo mật dựa trên File Storage Security Scanner, chẳng hạn như:
* **timestamp**: Một số duy nhất tương ứng với thời điểm diễn ra quá trình quét.
* **sqs-message-id**: ID duy nhất của sự kiện này.
* **file_url**: Đường dẫn (URL) đến tệp được quét trong Amazon S3.
* **scanner_status** và **scanner_status_message**
<img width="800" alt="image" src="https://github.com/user-attachments/assets/f7d6d54f-86bb-4242-936f-1d9aff249540" />

Trường `scanner_status` (trạng thái quét) có các giá trị sau:
* **0: “successful scan” (quét thành công)**: Cho biết quá trình quét đã hoàn tất thành công.
* **-1: “invalid license status” (trạng thái giấy phép không hợp lệ)**: Thường cho biết File Storage Security chưa được cấu hình đầy đủ. Lý do phổ biến nhất là các ARN chưa được gửi qua bảng điều khiển hoặc API của File Storage Security. Để biết hướng dẫn về cách gửi ARN, hãy xem phần Thêm Ngăn xếp (Add Stacks) hoặc Triển khai ngăn xếp bằng API. Thông báo này cũng có thể cho biết giấy phép của bạn không hợp lệ hoặc File Storage Security không thể đẩy giấy phép mới vào ngăn xếp của bạn.
* **-2: “unsuccessful scan” (quét không thành công)**: Cho biết hàm ScannerLambda không thể quét tệp.
* **-3: “scanner error” (lỗi máy quét)**: Cho biết đã xảy ra lỗi nội bộ trong hàm ScannerLambda.
* **-4: “unsuccessful scanner invocation” (gọi máy quét không thành công)**: Cho biết hàm ScannerLambda không thể hoàn thành quá trình quét. Có thể đã hết thời gian chờ (timeout) của bản quét, hoặc có quá nhiều tệp cần quét cùng lúc gây ra lỗi điều tiết Lambda (Lambda throttling error). 
* **scanning_result**: Cho biết các chi tiết quét như kích thước của tệp được quét cũng như bất kỳ phần mềm độc hại hoặc lỗi nào được tìm thấy.

---

## Truy vấn bổ sung sử dụng CloudWatch

1. Bạn có thể tìm kiếm kết quả quét bằng AWS CloudWatch Logs Insights. Dưới đây là ví dụ về cách thiết lập một truy vấn:
   * Trong AWS, đi tới dịch vụ **CloudWatch**.
   * Ở menu bên trái, bên dưới mục Logs, hãy nhấp vào **Logs Insights**.
   * Trong khung chính, nhấp vào bên trong trường **Select log group(s)** và nhập `ScannerLambda`.
   * Chọn Nhóm nhật ký (Log Group) chỉ chứa chữ “ScannerLambda” giống như ví dụ bên dưới.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/f5f4bf5d-e9aa-4b51-babe-e14004690c52" />

2. Thay thế nội dung của hộp văn bản bằng các dòng sau và nhấp vào **Run Query** (Chạy Truy vấn):
   ```text
   fields @timestamp, @message
   | filter @message like "scanner result"
   | sort @timestamp desc
   | limit 20
3. Bạn sẽ có thể nhìn thấy các sự kiện mà mình đã tạo:
<img width="800" alt="image" src="https://github.com/user-attachments/assets/aba5c72a-3c5a-4890-8811-fb586c38e9ee" />

4. Bằng cách mở rộng chi tiết của sự kiện, bạn sẽ có thể thấy chi tiết sự kiện do File Storage Security tạo ra ở định dạng JSON:

   ```json
   {
      "scanner result":{
         "timestamp":1630102559.6158442,
         "sqs_message_id":"abcf8e38-7a53-4880-9547-6418a4e1d018",
         "xamz_request_id":"",
         "file_url":"[https://modernization-workshop-devdays-fernando.s3.amazonaws.com/eicarcom2.zip](https://modernization-workshop-devdays-fernando.s3.amazonaws.com/eicarcom2.zip)",
         "scanner_status":0,
         "scanner_status_message":"successful scan",
         "scanning_result":{
            "TotalBytesOfFile":308,
            "Findings":[
               {
                  "malware":"Eicar_test_file",
                  "type":"Virus"
               }
            ],
            "Error":"",
            "Codes":[
               
            ]
         }
      }
   }
