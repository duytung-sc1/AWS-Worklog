---
title : "Kiểm tra Triển khai"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 4.3.1 </b> "
---

# Kiểm tra Triển khai 

## Tạo Phát hiện Đầu tiên

Để kiểm tra quá trình triển khai của bạn, bạn sẽ cần tạo ra một phát hiện phần mềm độc hại bằng cách sử dụng tệp eicar. Hãy làm theo các hướng dẫn bên dưới.
Video tại đây cũng minh họa từng bước nếu bạn muốn xem thêm chi tiết trước khi thực hiện.
[Trend Micro Cloud One](https://youtu.be/2WDpQ7KgjRo?si=4a3GP_3EHVrXXYdG)

1. Trong bảng điều khiển AWS, hãy mở AWS CloudShell ở một tab mới.
Để tải xuống tệp kiểm tra eicar, hãy chạy lệnh sau trong CloudShell: `wget https://secure.eicar.org/eicar.com.txt`
<img width="800" alt="image" src="https://github.com/user-attachments/assets/9e880df0-6297-4d6f-bbc3-085e41e6cb29" />

Tải tệp eicar lên S3 bucket mà bạn đã tạo trước đó: `aws s3 cp eicar.com.txt s3://name-of-bucket-goes-here/eicar.txt`
<img width="800" alt="image" src="https://github.com/user-attachments/assets/3df5c887-9838-42d5-9b6e-561518f0aa22" />

2. Sau khi tải lên thành công, hãy điều hướng đến phần S3 và tìm S3 bucket mà bạn đã thiết lập để quét bằng File Storage Security.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/b87250f2-b8bc-45f3-99ac-b4b6f461ba76" />

3. Chọn tệp eicar vừa được tải lên.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/c41502b3-bfec-423d-914c-b4eb45bb422f" />

4. Kiểm tra các thẻ (tags) được áp dụng từ kết quả quét.
Cuộn xuống phần Thẻ (Tags), bạn sẽ thấy các chi tiết giống như ví dụ bên dưới:
<img width="800" alt="image" src="https://github.com/user-attachments/assets/50575dee-1c10-446d-8430-bc14e71f21aa" />

Hãy tìm các thẻ sau:
* fss-scan-date "date_and_time"
* fss-scan-result "malicious"
* fss-scanned "true"

Các thẻ này chỉ ra rằng File Storage Security đã quét tệp và gắn thẻ nó là phần mềm độc hại một cách chính xác. Bạn hiện đã kiểm tra thành công quá trình triển khai File Storage Security của mình.

Bạn cũng có thể kiểm tra trong bảng điều khiển Cloud One - File Storage Security để xem có bao nhiêu tệp đã được quét và được nhận dạng là độc hại.
<img width="800" alt="image" src="https://github.com/user-attachments/assets/f0006b00-469c-4dc5-ba9a-0ba75ec69f99" />

Bạn có thể thực hiện quy trình tương tự với một tệp an toàn và xem cách Cloud One - File Storage Security sẽ gắn thẻ cho đối tượng đó như thế nào.
