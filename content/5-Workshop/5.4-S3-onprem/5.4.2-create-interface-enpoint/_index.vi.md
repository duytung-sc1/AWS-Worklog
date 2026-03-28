---
title : "Cách ly các tệp độc hại"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 4.4.2. </b> "
---

## Điều kiện tiên quyết

### 1. Tạo các bucket Amazon S3
* Tạo một ‘Promote bucket’ để nhận các tệp sạch. Ví dụ: `fss-promote`.
* Tạo một ‘Quarantine bucket’ để nhận các tệp bị cách ly. Ví dụ: `fss-quarantine`.

> Nếu bạn cần trợ giúp về cách tạo bucket Amazon S3, đây là các bước hướng dẫn: [Link](https://s3-protection.awsworkshop.io/20_deploy.html#s3-bucket-creation)

### 2. Tìm ARN của SNS topic ‘ScanResultTopic’
* Trong bảng điều khiển AWS (AWS Console), đi tới **Services > CloudFormation > stack all-in-one của bạn > Resources > storage stack của bạn > Resources**.
* Cuộn xuống để tìm Logical ID có tên `ScanResultTopic`.
* Sao chép mã ARN của `ScanResultTopic` vào một nơi tạm thời. Nó sẽ có dạng như sau: 
  `arn:aws:sns:us-east-1:123445678901:FileStorageSecurity-All-In-One-Stack-StorageStack-1IDPU1PZ2W5RN-ScanResultTopic-N8DD2JH1GRKF`

<img width="" alt="image" src="https://github.com/user-attachments/assets/64dce4d6-9258-40bf-b0a1-fa670f474898" />

---

## Triển khai Hành động sau khi quét (Functions) - Chấp nhận và Cách ly

Trong trường hợp này, chúng ta sẽ sử dụng Serverless Application Repository.

1. Truy cập [trang của ứng dụng trên AWS Lambda Console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/create/app?applicationId=arn:aws:serverlessrepo:us-east-1:415485722356:applications/cloudone-filestorage-plugin-action-promote-or-quarantine).
2. Điền các tham số (parameters):
   * `ScanResultTopic`
   * `ScanningBucketName`
   * `PromoteBucketName`
   * `QuarantineBucketName`
   * `Tùy chọn: bạn có thể tùy chỉnh tên của CloudFormation stack sẽ được tạo`
3. Tích chọn vào ô **I acknowledge that this app creates custom IAM roles** (Tôi xác nhận rằng ứng dụng này tạo các vai trò IAM tùy chỉnh).
4. Nhấn **Deploy**.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/8dec44e3-dfb5-4d1f-b6e5-295ce6367b05" />
<img width="800" alt="image" src="https://github.com/user-attachments/assets/e98b71a4-1753-44e5-a0ce-8e1348752f9e" />

5. Sau vài phút, bạn có thể nhấn vào tab **Deployments** và mở rộng phần triển khai để xem trạng thái đã hiển thị là hoàn tất (complete) hay chưa. Sau đó, bạn có thể chuyển sang bước tiếp theo để kiểm tra.

<img width="800" alt="image" src="https://github.com/user-attachments/assets/26bb5e19-0174-47b0-993d-b1d844c1cf41" />

---

## Kiểm tra ứng dụng

Để kiểm tra xem ứng dụng đã được triển khai đúng cách hay chưa, bạn cần tạo một cảnh báo phát hiện mã độc bằng tệp thử nghiệm `eicar`, sau đó kiểm tra bucket Cách ly để đảm bảo tệp `eicar` đã được gửi đến đó thành công.

### 1. Tải tệp thử nghiệm Eicar
> **LƯU Ý:** Chúng tôi khuyên bạn nên sử dụng quy trình AWS CloudShell trong phần “Kiểm tra triển khai” trước đó vì hầu hết người dùng không thể tắt trình quét virus trên máy cá nhân.

* Tạm thời tắt trình quét virus của bạn hoặc tạo một ngoại lệ (exception), nếu không nó sẽ phát hiện và xóa tệp `eicar`.
* **Trình duyệt:** Truy cập [trang tệp eicar](https://secure.eicar.org/eicar_com.zip) và tải xuống `eicar_com.zip` hoặc bất kỳ phiên bản nào khác của tệp này.
* **CLI:** Chạy lệnh sau:
    ```bash
    curl -O [https://secure.eicar.org/eicar_com.zip](https://secure.eicar.org/eicar_com.zip)
    ```

### 2. Tải tệp eicar lên ScanningBucket

**Sử dụng bảng điều khiển AWS (Console):**
1. Đi tới **CloudFormation > Stacks > [stack all-in-one của bạn] > [storage stack phụ trợ]**.
2. Ở khung chính, nhấn vào tab **Outputs** và sao chép chuỗi ký tự **ScanningBucket**. Tìm kiếm chuỗi này trong bảng điều khiển Amazon S3 để tìm ScanningBucket của bạn.
3. Nhấn **Upload** và tải tệp `eicar_com.zip` lên. File Storage Security sẽ quét tệp và phát hiện mã độc.
4. Vẫn trong Amazon S3, đi tới **Quarantine bucket** (Bucket cách ly) của bạn và đảm bảo rằng tệp `eicar.zip` đã xuất hiện tại đó.
5. Quay lại **ScanningBucket** và đảm bảo rằng tệp `eicar.zip` không còn ở đó nữa.

> *Có thể mất từ 15-30 giây hoặc lâu hơn để thao tác 'di chuyển' hoàn tất, trong thời gian đó, bạn có thể thấy tệp xuất hiện ở cả hai bucket.*

**Sử dụng AWS CLI:**
Nhập lệnh AWS CLI sau để tải tệp thử nghiệm Eicar lên bucket quét:
`aws s3 cp eicar_com.zip s3://<TEN_SCANNING_BUCKET_CUA_BAN>`
Trong đó: `<TEN_SCANNING_BUCKET_CUA_BAN>` được thay thế bằng tên ScanningBucket thực tế của bạn.

> *LƯU Ý: Có thể mất khoảng 15-30 giây hoặc lâu hơn để tệp được di chuyển.*

<img width="800" alt="image" src="https://github.com/user-attachments/assets/92eee487-6ac2-4615-bccf-d437f806b973" />

**Sử dụng AWS CLI hoặc AWS Console, bạn sẽ có thể thấy tệp eicar trong `QuarantineBucketName` với các thẻ tag chính xác.**
