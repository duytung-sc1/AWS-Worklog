---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Code Protector Platform
## Nền tảng SaaS Bảo mật Mã nguồn Python trên AWS Serverless

### 1. Tóm tắt điều hành
Nền tảng Code Protector là một giải pháp SaaS được thiết kế nhằm bảo vệ tài sản trí tuệ phần mềm bằng cách mã hóa và làm rối mã nguồn. Tập trung chủ yếu vào các ứng dụng Python, nền tảng này ngăn chặn việc dịch ngược trái phép, can thiệp logic và đánh cắp mã nguồn. Bằng cách tận dụng kiến trúc AWS Serverless, hệ thống cung cấp một giao diện web an toàn, có khả năng mở rộng tự động và tiết kiệm chi phí. Dự án được phát triển bởi đội ngũ 6 thành viên trong lộ trình thực tập 3 tháng, với mục tiêu xây dựng một quy trình xử lý mã nguồn khép kín từ khâu tải lên, bảo mật đến lưu trữ và thông báo cho người dùng.

### 2. Tuyên bố vấn đề
*Vấn đề hiện tại*
Các ngôn ngữ thông dịch như Python vốn dĩ rất dễ bị dịch ngược (reverse engineering). Do không được biên dịch thành mã máy, mã nguồn cơ bản chỉ là văn bản thuần túy. Điều này khiến các đối thủ hoặc kẻ xấu dễ dàng đọc, đánh cắp hoặc chỉnh sửa các thuật toán độc quyền, dẫn đến thất thoát trực tiếp tài sản trí tuệ và lợi thế cạnh tranh.

*Giải pháp*
Code Protector cung cấp một nền tảng web tập trung nơi người dùng tải mã nguồn lên một cách an toàn. Hệ thống sử dụng cơ chế bảo vệ hai lớp: làm rối mã (obfuscation) và mã hóa (encryption) kèm khóa (key). Hệ thống sử dụng **Amazon S3** kết hợp **CloudFront** để host website, giao tiếp với backend qua **API Gateway** và **Lambda**. Dữ liệu người dùng và siêu dữ liệu (metadata/keys) được lưu trong **DynamoDB**. **CloudWatch** giám sát quá trình xử lý, và **SES** tự động gửi email thông báo khi hoàn tất.

*Lợi ích và hoàn vốn đầu tư (ROI)*
Giải pháp giúp các lập trình viên bảo vệ bản quyền mà không cần tự xây dựng công cụ bảo mật phức tạp. Kiến trúc Serverless 100% giúp chi phí vận hành hàng tháng cực thấp (gần như bằng 0 trong giai đoạn đầu nhờ AWS Free Tier), loại bỏ hoàn toàn chi phí duy trì máy chủ truyền thống. Nền tảng này giúp tiết kiệm đáng kể nguồn lực và thời gian R&D của các công ty phần mềm.

### 3. Kiến trúc giải pháp
Nền tảng sử dụng hệ sinh thái AWS Serverless để tự động hóa hoàn toàn quy trình nhận, xử lý và trả kết quả file an toàn:

![Code Protector Architecture](/images/2-Proposal/kientruc_giaiphap.png)

*Dịch vụ AWS sử dụng*
- *Amazon CloudFront*: Mạng phân phối nội dung (CDN) giúp tải giao diện website tốc độ cao và bảo mật HTTPS.
- *Amazon S3*: 2 bucket lưu trữ tĩnh: một cho Frontend web tĩnh và một để lưu trữ an toàn các file Python nguyên bản cũng như file đầu ra.
- *AWS Lambda*: Engine xử lý cốt lõi, chạy các đoạn script bằng Python để thực thi thuật toán làm rối và mã hóa.
- *Amazon API Gateway*: Quản lý và định tuyến các lệnh gọi API RESTful từ ứng dụng web đến Lambda.
- *Amazon DynamoDB*: Cơ sở dữ liệu NoSQL lưu trữ thông tin người dùng, lịch sử giao dịch và quản lý khóa giải mã.
- *Amazon CloudWatch*: Giám sát sức khỏe hệ thống, ghi log (nhật ký) các tiến trình xử lý và cảnh báo lỗi.
- *Amazon SES (Simple Email Service)*: Dịch vụ gửi email tự động thông báo cho khách hàng khi tiến trình mã hóa hoàn tất.

*Thiết kế thành phần*
- *Frontend Web*: Được host trên S3 & CloudFront, cung cấp Dashboard cho người dùng đăng nhập, upload file và lấy khóa bảo mật.
- *Xử lý API*: API Gateway tiếp nhận file từ người dùng, kích hoạt Lambda.
- *Bảo mật & Mã hóa*: Lambda nhận file từ S3, thực thi các thuật toán biến đổi mã nguồn (AST manipulation), lưu khóa giải mã vào DynamoDB.
- *Trả kết quả*: Sau khi file mã hóa được lưu lại vào S3, Lambda kích hoạt SES gửi email đính kèm link tải cho người dùng.

### 4. Triển khai kỹ thuật
*Các giai đoạn triển khai*
Dự án bao gồm 2 phần chính — nghiên cứu thuật toán bảo mật và xây dựng nền tảng SaaS trên AWS — trải qua 4 giai đoạn:
1. *Nghiên cứu thuật toán & Kiến trúc*: Tìm hiểu các phương pháp Obfuscation/Encryption cho Python và phác thảo kiến trúc Serverless (1 tháng trước kỳ thực tập).
2. *Tính toán chi phí và đánh giá*: Sử dụng AWS Pricing Calculator để ước tính ngân sách cho 7 dịch vụ AWS cốt lõi (Tháng 1).
3. *Thiết kế Database & Tối ưu luồng*: Thiết kế schema trên DynamoDB, luồng API Gateway, tối ưu timeout của Lambda để xử lý file lớn (Tháng 2).
4. *Phát triển, kiểm thử, triển khai*: Code logic Python trên Lambda, xây dựng giao diện web, thiết lập CloudWatch/SES, kiểm thử diện rộng và đưa lên môi trường Production (Tháng 2–3).

*Yêu cầu kỹ thuật*
- *Core Security Engine*: Am hiểu thuật toán xử lý AST (Abstract Syntax Tree) của Python để làm rối, sử dụng các chuẩn mã hóa an toàn (AES).
- *Hạ tầng Cloud*: Kỹ năng cấu hình IAM Roles nghiêm ngặt để Lambda có thể đọc/ghi S3, truy vấn DynamoDB và gọi SES một cách bảo mật. 

### 5. Lộ trình & Mốc triển khai
- *Trước thực tập (Tháng 0)*: 1 tháng để lên kế hoạch, phân chia công việc cho 6 thành viên và nghiên cứu các công cụ mã hóa hiện có.
- *Thực tập (Tháng 1–3)*: 3 tháng triển khai thực tế.
    - Tháng 1: Học và làm quen với hệ sinh thái AWS (Lambda, DynamoDB, S3, CloudFront). Bắt đầu thiết kế giao diện UI/UX.
    - Tháng 2: Thiết kế kiến trúc database (DynamoDB), phát triển logic làm rối/mã hóa (Python Lambda) và điều chỉnh kiến trúc.
    - Tháng 3: Lập trình ghép nối Frontend với Backend (API), kiểm thử (test với các file Python phức tạp), thiết lập CloudWatch, SES và ra mắt (Launch).
- *Sau triển khai*: Lên đến 1 năm để bảo trì và nghiên cứu mở rộng (ví dụ: hỗ trợ ngôn ngữ JavaScript).

### 6. Ước tính ngân sách
Có thể xem chi phí trên [AWS Pricing Calculator](https://calculator.aws/#/estimate).


Tận dụng AWS Free Tier (bậc miễn phí 12 tháng), chi phí phát triển cực kỳ tối ưu.
*Chi phí hạ tầng (Ước tính hàng tháng)*
- AWS Lambda & API Gateway: 0,00 USD (Miễn phí 1 triệu request/tháng).
- Amazon S3 & CloudFront: ~0,50 USD (Lưu trữ và phân phối web/file).
- Amazon DynamoDB: 0,00 USD (Miễn phí 25GB dung lượng).
- Amazon CloudWatch: 0,00 USD (Miễn phí log cơ bản).
- Amazon SES: 0,00 USD (Miễn phí 3.000 email/tháng từ AWS).

*Tổng*: < 1,00 USD/tháng cho môi trường phát triển ban đầu.
- *Phần cứng*: 0 USD (Dự án thuần phần mềm, không yêu cầu phần cứng IoT).

### 7. Đánh giá rủi ro
*Ma trận rủi ro*
- Lỗi logic mã nguồn sau mã hóa: Ảnh hưởng cao, xác suất trung bình. (Mã bị lỗi biến/hàm khiến phần mềm của khách không chạy được).
- Lambda Timeout / Quá tải bộ nhớ RAM: Ảnh hưởng trung bình, xác suất trung bình. (File tải lên quá nặng).
- Rò rỉ khóa (Key Leakage): Ảnh hưởng cao, xác suất thấp.

*Chiến lược giảm thiểu*
- Lỗi logic: Xây dựng bộ Unit Test tự động, test file đầu ra trước khi trả về cho khách.
- Timeout: Giới hạn dung lượng file upload tối đa và chia nhỏ luồng xử lý.
- Bảo mật khóa: Mã hóa dữ liệu lưu trong DynamoDB, phân quyền IAM chặt chẽ, không in dữ liệu nhạy cảm ra CloudWatch logs.

*Kế hoạch dự phòng*
- Bổ sung tính năng "Safe Mode" để khách hàng test thử một đoạn code nhỏ trước khi mã hóa toàn bộ dự án.

### 8. Kết quả kỳ vọng
*Cải tiến kỹ thuật*: Số hóa quy trình bảo vệ bản quyền bằng nền tảng Cloud trực quan thay vì lệnh CLI thủ công. Kiến trúc xử lý được hàng ngàn request đồng thời nhờ CloudFront và Lambda.
*Giá trị dài hạn*: Tạo ra bộ khung (framework) bảo mật mã nguồn độc lập, có thể đóng gói thương mại hóa (SaaS) hoặc tái sử dụng làm internal tool cho các dự án của doanh nghiệp trong tương lai.
