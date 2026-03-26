---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# GuardScript — Code Protector Platform

## Đề xuất dự án

### Thông tin sinh viên / Nhóm thực hiện

- **Võ Tấn Phát (Nhóm Trưởng)** – MSSV: **SE194484**
- **Bùi Minh Hiển** – MSSV: **SE190829**
- **Dương Nguyên Bình** – MSSV: **SE194067**
- **Vinh Trần** – MSSV: **SE193927**
- **Nguyễn Duy Tùng** – MSSV: **SE196572**
- **Nguyễn Trí** – MSSV: **[Mã số]**

---

## Nền tảng bảo vệ mã nguồn và phân phối script an toàn đa ngôn ngữ

### Triển khai trên hạ tầng AWS Cloud

**AWS Lambda** · **Amazon S3** · **Amazon DynamoDB** · **Amazon CloudFront** · **Amazon CloudWatch** · **Amazon SES**

---

# I. GIỚI THIỆU DỰ ÁN

## 1.1. Bối cảnh và vấn đề

Trong môi trường phát triển phần mềm hiện đại, việc **bảo vệ mã nguồn** là một thách thức cấp thiết. Khi các developer phân phối script **Python** hoặc **JavaScript** đến người dùng cuối, họ đối mặt với rủi ro source code bị sao chép, chỉnh sửa, hoặc tái phân phối trái phép mà không có cơ chế kiểm soát hiệu quả.

Các giải pháp hiện có như **obfuscation** đơn thuần hay đóng gói file `.exe` còn nhiều hạn chế: dễ bị **reverse engineering**, không kiểm soát được ai đang chạy code, không thu hồi được quyền truy cập khi cần thiết. Thị trường thiếu một giải pháp tích hợp vừa bảo vệ mã nguồn, vừa quản lý phân phối và kiểm soát truy cập một cách toàn diện.

## 1.2. Giải pháp đề xuất

**Code Protector** (tên hệ thống: **IrisAuth**) là nền tảng **SaaS** cho phép developer upload mã nguồn lên server và phân phối đến người dùng qua cơ chế **loader được mã hóa**. Thay vì chia sẻ source code trực tiếp, người dùng cuối chỉ nhận một đoạn loader nhỏ 1–2 dòng. Server trả về code đã mã hóa chỉ khi người dùng vượt qua toàn bộ các kiểm tra bảo mật, và code chỉ tồn tại trong **RAM** khi thực thi — không bao giờ được lưu ra file.

Hệ thống được triển khai trên nền tảng **AWS Cloud**, tận dụng kiến trúc **serverless** và các dịch vụ **managed** để đảm bảo khả năng mở rộng cao, độ trễ thấp toàn cầu, và vận hành không cần quản lý hạ tầng thủ công.

---

# II. MỤC TIÊU DỰ ÁN

## 2.1. Mục tiêu tổng quát

Xây dựng một nền tảng web hoàn chỉnh cung cấp dịch vụ **bảo vệ và phân phối mã nguồn an toàn** cho **Python** và **JavaScript**, với hệ thống xác thực đa lớp, giao diện quản trị trực quan, và hạ tầng **AWS Cloud** có khả năng mở rộng toàn cầu.

## 2.2. Mục tiêu cụ thể

- Thiết kế và triển khai hệ thống phân phối code qua **loader mã hóa**, đảm bảo source code không tiếp xúc người dùng cuối ở dạng plaintext.
- Xây dựng cơ chế trao đổi khóa **ECDH X25519** kết hợp mã hóa **AES-256-GCM** để bảo vệ kênh truyền dữ liệu.
- Phát triển hệ thống quản lý **license** với tính năng **HWID Lock**, ngày hết hạn và giới hạn số lần thực thi.
- Xây dựng hệ thống **workspace** và **team collaboration** với nhiều vai trò người dùng.
- Phát triển giao diện web quản trị responsive, phân phối qua **Amazon CloudFront** với độ trễ thấp toàn cầu.
- Tích hợp monitoring và alerting qua **Amazon CloudWatch**, ghi log tập trung và thông báo qua **Discord Webhook**.
- Triển khai kiến trúc **serverless** trên **AWS Lambda**, lưu trữ đối tượng trên **Amazon S3**, dữ liệu cấu trúc trên **Amazon DynamoDB**.
- Tích hợp **Amazon SES** để gửi email mời thành viên workspace và thông báo hệ thống tự động.

---

# III. PHẠM VI DỰ ÁN

## 3.1. Trong phạm vi

### Backend & API

- **RESTful API** và **WebSocket** với **Node.js / Express.js** chạy trên **AWS Lambda** thông qua **API Gateway** hoặc **Lambda Function URL**
- Xử lý **ECDH Handshake**, **AES-256-GCM encryption**, **XOR protocol** cho loader distribution
- **Rate limiting**, **IP access control**, **request signature validation**

### Frontend

- 5 trang giao diện: **Landing**, **Login**, **Register**, **Dashboard**, **Workspace**
- Static assets phân phối toàn cầu qua **Amazon CloudFront CDN**
- **Vanilla HTML5/CSS3/JavaScript**, không phụ thuộc framework nặng
- Đồng bộ thời gian thực với **AWS API Gateway WebSocket API**

### Hạ tầng AWS

- **Amazon S3**: lưu trữ mã nguồn đã mã hóa, thay thế local filesystem storage
- **Amazon DynamoDB**: cơ sở dữ liệu **NoSQL** thay thế **SQLite**, lưu users, workspaces, projects, licenses, logs, rate limits và cấu hình
- **AWS Lambda**: runtime cho **Express.js API**, hỗ trợ **serverless auto-scaling**
- **Amazon CloudFront**: phân phối static files, cache S3 assets, SSL termination, custom domain
- **Amazon CloudWatch**: monitoring, log aggregation, metric alarms, dashboard hệ thống
- **Amazon SES**: gửi email mời thành viên workspace qua token

### Loader & Client

- **Python loader** dùng `urllib` cho **Python 3.7+**
- **Node.js loader**
- **Browser Userscript** với **Tampermonkey**
- Hai cơ chế phân phối: **ECDH handshake v3** và **XOR execute v2**

### Tính năng nghiệp vụ

- Hệ thống **license**: tạo đơn lẻ, tạo hàng loạt, **HWID binding**, export **CSV**
- **IP Blacklist**, **IP Whitelist**, **Rate Limiting**, **Workspace PIN**
- Team management: mời thành viên qua email token với 3 vai trò `owner`, `editor`, `viewer`
- Quản lý dự án đa file với **bundle generation** cho **Python** và **Node.js**
- **Python AST Obfuscator** như một module độc lập
- Đồng bộ thời gian thực qua **WebSocket**, thông báo qua **Discord Webhook**

## 3.2. Ngoài phạm vi

- Ứng dụng **mobile native**
- Hỗ trợ ngôn ngữ lập trình khác ngoài **Python** và **JavaScript**
- Tích hợp **cổng thanh toán**
- Tính năng **CI/CD** tự động hóa deployment pipeline ngoài mức cơ bản với **AWS SAM** hoặc **AWS CDK**

---

# IV. KIẾN TRÚC HỆ THỐNG & CÔNG NGHỆ

## 4.1. Stack công nghệ

| Tầng | Dịch vụ / Công nghệ | Chi tiết |
|------|----------------------|----------|
| Runtime API | **Node.js v18+** trên **AWS Lambda** | ES Module, Express.js |
| Database | **Amazon DynamoDB** | NoSQL key-value, on-demand capacity, TTL tự động cho `rate_limits` và `sessions` |
| Object Storage | **Amazon S3** | Lưu script đã mã hóa và nén gzip, versioning bật, server-side encryption |
| CDN & Edge | **Amazon CloudFront** | Phân phối static frontend, cache S3 assets, SSL termination, custom domain |
| Compute | **AWS Lambda** | Auto-scale, serverless, giảm chi phí vận hành |
| Email | **Amazon SES** | Gửi workspace invitation email, cấu hình DKIM và SPF |
| Monitoring | **Amazon CloudWatch** | Log Groups, metric alarms, dashboard |
| Real-time | **API Gateway WebSocket API** | Broadcast update theo từng workspace |
| Frontend | **HTML5, CSS3, Vanilla JS** | 5 trang static, deploy lên S3 bucket |
| Compression | **Pako (Gzip)** | Nén code trước khi lưu S3 |

## 4.2. Kiến trúc tổng quan AWS

Hệ thống **IrisAuth** được triển khai theo mô hình **serverless** trên **AWS**, loại bỏ hoàn toàn việc quản lý server vật lý. Luồng request tiêu biểu như sau:

- Client (**browser** hoặc **Python loader**) gửi request tới **CloudFront Distribution**
- Static assets như HTML, CSS, JavaScript được phục vụ trực tiếp từ **Amazon S3** thông qua **CloudFront**
- API request tại đường dẫn ` /api/* ` được chuyển tiếp tới **API Gateway** và sau đó tới **AWS Lambda**
- **Lambda function** xử lý business logic, đọc/ghi **Amazon DynamoDB** đối với metadata và **Amazon S3** đối với script content
- Email invitation được gửi qua **Amazon SES**
- Toàn bộ log của Lambda được stream vào **Amazon CloudWatch Logs**
- Kết nối thời gian thực đi qua **API Gateway WebSocket API**

## 4.3. Hệ thống xác thực

Hệ thống không sử dụng thư viện **JWT** bên ngoài mà tự xây dựng cơ chế xác thực riêng.

- Ký token bằng **HMAC-SHA256**, secret sinh ngẫu nhiên 48 bytes lưu trong **DynamoDB** ở bảng `app_config`
- Token có **TTL 7 ngày**
- Khi người dùng đổi mật khẩu, toàn bộ token cũ bị vô hiệu hóa tức thì bằng cách so sánh với `password_changed_at`
- Token được gửi qua **HttpOnly Cookie** với tên `token`, đồng thời hỗ trợ `Authorization` header
- Mật khẩu được băm bằng **PBKDF2-SHA256** với **210.000 iterations** và salt ngẫu nhiên 16 bytes
- Các tài khoản cũ dùng **SHA-256** thuần sẽ được nâng cấp khi đăng nhập lại
- **Workspace PIN** cũng sử dụng **PBKDF2** và tạo pin session token riêng lưu ở bảng `pin_verifications`

## 4.4. Hai protocol phân phối code

### Protocol v2 — `GET /api/v5/execute`

- Client gửi các tham số gồm `id`, license key, HWID, timestamp, nonce và chữ ký **HMAC-SHA256**
- Server xác thực chữ ký, kiểm tra timestamp trong ngưỡng ±5 phút để chống **replay attack**
- Response được mã hóa bằng **XOR** với key dẫn xuất từ **SHA-256**
- Script content được đọc từ **Amazon S3**

### Protocol v3 — `POST /api/v5/handshake`

- Client tạo cặp khóa **X25519**, gửi public key lên server cùng chữ ký xác thực
- **AWS Lambda** tạo cặp khóa riêng, tính shared secret bằng `diffieHellman()`
- Khóa **AES** được dẫn xuất bằng **HKDF-SHA256**
- Script được mã hóa bằng **AES-256-GCM**, trả về ciphertext cùng server public key
- Mỗi phiên đều có session key riêng, hỗ trợ **perfect forward secrecy**

## 4.5. Thuật toán và chuẩn bảo mật

| Thuật toán | Ứng dụng trong hệ thống |
|-----------|--------------------------|
| **PBKDF2-SHA256** | Băm mật khẩu người dùng và Workspace PIN |
| **HMAC-SHA256** | Ký auth token, ký request loader, ký response |
| **ECDH X25519** | Trao đổi khóa cho protocol v3 |
| **HKDF-SHA256** | Dẫn xuất khóa AES từ shared secret |
| **AES-256-GCM** | Mã hóa script lưu trữ và truyền tải |
| **XOR + SHA-256** | Mã hóa truyền tải trong protocol v2 |
| **Gzip** | Nén content trước khi lưu **Amazon S3** |
| `crypto.timingSafeEqual` | So sánh token và signature an toàn |
| Nonce + Timestamp | Chống replay attack |
| **S3 Server-Side Encryption** | Mã hóa dữ liệu lưu trữ trên S3 |
| **TLS 1.2+** | Bảo vệ toàn bộ kết nối client-server |

## 4.6. Cơ sở dữ liệu — Amazon DynamoDB

Thay thế **SQLite** bằng **Amazon DynamoDB**, một cơ sở dữ liệu **NoSQL fully managed**. Thiết kế dữ liệu có thể áp dụng theo mô hình **single-table** hoặc **multi-table** tùy access pattern.

| Bảng DynamoDB | Partition Key / Sort Key | Chức năng |
|--------------|---------------------------|-----------|
| `users` | PK: `userId` | Quản lý tài khoản người dùng |
| `workspaces` | PK: `workspaceId` | Quản lý workspace, loader key, ngôn ngữ, khóa mã hóa |
| `projects` | PK: `workspaceId`, SK: `projectId` | Quản lý project và cài đặt bảo mật |
| `project_files` | PK: `projectId`, SK: `fileId` | Quản lý file và folder trong project |
| `licenses` | PK: `workspaceId`, SK: `licenseKey` | Quản lý license key, HWID, ngày hết hạn |
| `access_lists` | PK: `workspaceId`, SK: `ip#type` | Quản lý blacklist và whitelist |
| `workspace_members` | PK: `workspaceId`, SK: `userId` | Quản lý thành viên và vai trò |
| `workspace_invitations` | PK: `token` | Lưu token mời qua email |
| `pin_verifications` | PK: `sessionToken` | Quản lý session sau khi xác thực PIN |
| `logs` | PK: `workspaceId`, SK: `timestamp#uuid` | Nhật ký hoạt động hệ thống |
| `app_config` | PK: `configKey` | Cấu hình hệ thống |
| `rate_limits` | PK: `rateLimitKey` | Dữ liệu giới hạn tần suất request |

**TTL** được bật cho các bảng `workspace_invitations`, `pin_verifications` và `rate_limits` để tự động xóa dữ liệu hết hạn.

## 4.7. Amazon S3 — Object Storage cho Script Content

- Thay thế hoàn toàn local filesystem storage
- Mỗi script file được lưu với key theo dạng `{workspaceId}/{uuid}_{timestamp}_{hash}.gz`
- Content được nén bằng **Gzip** và mã hóa bằng **AES-256-GCM** trước khi lưu
- S3 bucket bật **versioning**, **server-side encryption**, và **Block Public Access**
- **AWS Lambda** truy cập S3 thông qua **IAM Role** theo nguyên tắc **least privilege**
- Static frontend assets cũng được lưu trên một **S3 bucket** riêng và phân phối qua **CloudFront**

## 4.8. Amazon CloudFront — CDN & Edge

- Sử dụng một **CloudFront Distribution** để phục vụ cả static frontend và API proxy
- Cấu hình behavior:
  - ` /api/* ` trỏ tới **API Gateway**
  - ` /* ` trỏ tới **S3 static bucket**
- Bắt buộc **HTTPS**, sử dụng **TLS 1.2** trở lên
- Tối ưu cache cho static assets, đồng thời hạn chế cache đối với API response
- Hỗ trợ custom domain với chứng chỉ từ **AWS Certificate Manager**
- Có thể tích hợp **AWS WAF** để chặn bot, SQL injection và XSS từ edge layer

## 4.9. Amazon CloudWatch — Monitoring & Observability

- Tạo **Log Groups** cho từng **Lambda function**
- Tạo **Metric Filters** để trích xuất các chỉ số như execution count, handshake rate, error rate
- Thiết lập **CloudWatch Alarms** khi:
  - Lambda error rate vượt ngưỡng
  - p99 latency vượt mức cho phép
  - **DynamoDB** xuất hiện throttle events
- Xây dựng **CloudWatch Dashboard** để theo dõi request volume, latency, error breakdown và storage usage
- Dùng **structured JSON logging** để dễ truy vấn qua **CloudWatch Logs Insights**

## 4.10. Amazon SES — Email Service

- Gửi email mời thành viên workspace có chứa token
- Có thể dùng **SES Template** hoặc HTML inline
- Cấu hình **DKIM**, **SPF**, **DMARC** để tăng khả năng deliverability
- Theo dõi **bounce rate** và **complaint rate** thông qua tích hợp với **CloudWatch**
- Chuyển từ **sandbox mode** sang **production** sau khi verify sending domain

## 4.11. Role-Based Access Control

| Vai trò | Quản lý Project | Quản lý License | Xem Log | Cài đặt Workspace | Mời thành viên |
|--------|------------------|------------------|---------|-------------------|----------------|
| `owner` | Toàn quyền | Toàn quyền | Toàn quyền | Toàn quyền | Toàn quyền |
| `admin` | Toàn quyền | Toàn quyền | Toàn quyền | Giới hạn | Có thể |
| `editor` | CRUD | Tạo/Xem | Xem | Không | Không |
| `viewer` | Chỉ đọc | Chỉ xem | Xem | Không | Không |

---

# V. TÍNH NĂNG CHI TIẾT

## 5.1. Luồng hoạt động cốt lõi

- Developer đăng nhập và tạo workspace, metadata được lưu vào **Amazon DynamoDB**
- Developer upload hoặc nhập mã nguồn, code được nén bằng **Gzip**, mã hóa bằng **AES-256-GCM**, sau đó lưu vào **Amazon S3**
- Developer nhận một đoạn loader ngắn cho **Python** hoặc **JavaScript**
- End-user chạy loader, loader sinh **HWID** từ thông tin thiết bị và hash bằng **SHA-256**
- Loader gọi `GET /api/v5/execute` hoặc `POST /api/v5/handshake` qua **CloudFront**
- **AWS Lambda** xác thực chữ ký, kiểm tra license, HWID, rate limit và access list trong **DynamoDB**
- Nếu hợp lệ, Lambda đọc script từ **S3**, giải mã và trả về nội dung theo protocol tương ứng
- Loader giải mã và thực thi trực tiếp trong **RAM**, không lưu thành file
- Hệ thống ghi log vào bảng `logs`, cập nhật execution count, và broadcast sự kiện qua **WebSocket**
- **CloudWatch** thu thập log và metrics theo thời gian thực

## 5.2. Hệ thống License

- Tạo **license key** đơn lẻ với định dạng **[prefix]LIC-XXXXXXXX-XXXXXXXX[suffix]**, hỗ trợ custom key
- Tạo hàng loạt tối đa **100 license/lần** bằng **DynamoDB batch write**
- **HWID Lock**: khi end-user chạy lần đầu, HWID được ghi vào **DynamoDB**; các lần sau nếu HWID thay đổi thì bị từ chối
- Bật hoặc tắt `hwid_lock` riêng cho từng license
- Thiết lập ngày hết hạn qua trường `expiration_date`, kiểm tra realtime mỗi lần execute
- Gắn license với project cụ thể hoặc toàn workspace
- Hỗ trợ reset HWID khi người dùng đổi máy
- Export danh sách ra **CSV** gồm `key`, `note`, trạng thái, `HWID`, `OS`, số lần dùng và lần cuối dùng

## 5.3. Kiểm soát truy cập

- **IP Blacklist**: chặn IP cụ thể khỏi workspace
- **IP Whitelist**: chỉ cho phép truy cập từ các IP được chỉ định
- **CloudFront WAF**: bổ sung lớp bảo vệ edge
- **Max executions**: giới hạn tổng số lần project được chạy
- **Rate limiting** theo mô hình **sliding window** trên **DynamoDB**
  - Login: tối đa 10 request/phút/IP
  - Register: tối đa 5 request/phút/IP
  - Loader execute: tối đa 120 request/phút/IP
  - ECDH handshake: tối đa 60 request/phút/IP
  - Tạo license: tối đa 30 request/phút
- **TTL** trên `rate_limits` tự động dọn dữ liệu hết hạn
- **Workspace PIN** được băm bằng **PBKDF2**
- Mọi request tới loader phải có **HMAC-SHA256 signature** hợp lệ

## 5.4. Quản lý dự án đa file

- Tổ chức theo cấu trúc thư mục với `file` và `folder`
- Chỉ định **entry point** cho project nhiều file
- **Bundle generation**:
  - **Python**: ghép file, xử lý import nội bộ
  - **Node.js**: ghép module theo cấu trúc project
- Hỗ trợ nhận diện hơn 30 loại extension
- Các thao tác: tạo, đổi tên, di chuyển, sao chép, xóa, upload, tìm kiếm
- Nội dung lớn được lưu trên **S3** và đọc lại khi cần

## 5.5. Workspace & Team Collaboration

- Mỗi user có thể sở hữu nhiều workspace độc lập
- Mỗi workspace có `loader_key` riêng làm định danh công khai
- Mời thành viên qua email token bằng **Amazon SES**
- Token mời được lưu trong `workspace_invitations`
- Hỗ trợ 4 vai trò: `owner`, `admin`, `editor`, `viewer`
- Thay đổi trong workspace được broadcast realtime qua **API Gateway WebSocket API**
- Có thể cấu hình **Discord Webhook** để gửi thông báo sự kiện quan trọng
- Dashboard hiển thị thống kê tổng số execution, license và thành viên

## 5.6. Logging & Monitoring

- Ghi log cho các sự kiện như:
  - `LOAD_SCRIPT`
  - `ECDH_HANDSHAKE`
  - `BLOCK_ACCESS`
  - `INVALID_LICENSE`
  - `INVALID_HWID`
  - `INVALID_SIGNATURE`
  - `CREATE_LICENSE`
  - `BATCH_CREATE_LICENSE`
- Mỗi log gồm `workspace_id`, `action`, chi tiết, IP, quốc gia và timestamp
- Log nghiệp vụ lưu trong **DynamoDB**
- Log hệ thống của Lambda lưu trong **CloudWatch Logs**
- Thiết lập **CloudWatch Alarms** để cảnh báo sớm
- Xây dựng **CloudWatch Dashboard** theo dõi KPI
- Hỗ trợ xem log theo workspace từ giao diện quản trị

## 5.7. Python AST Obfuscator

File `obfuscator.py` là công cụ obfuscate code **Python** hoạt động độc lập ở tầng **AST**.

- Phân tích cú pháp và kiểm tra compatibility issues
- Đổi tên biến, hàm và tham số thành chuỗi ngẫu nhiên
- Thêm **dead code** để gây nhiễu
- Xuất báo cáo các vấn đề tương thích theo mức độ `error` hoặc `warning`

---

# VI. KẾ HOẠCH THỰC HIỆN

| Giai đoạn | Nội dung | Thời gian dự kiến |
|----------|----------|-------------------|
| 1. Phân tích & Thiết kế | Xác định yêu cầu, thiết kế schema **DynamoDB**, API và **CloudFront behavior** | Tuần 1–2 |
| 2. Hạ tầng AWS | Provision **Lambda**, **DynamoDB**, **S3**, **CloudFront**, **SES**, **CloudWatch** bằng **AWS SAM** hoặc **AWS CDK** | Tuần 3 |
| 3. Backend Core | Xây dựng auth, workspace, project, file management API | Tuần 4–6 |
| 4. Bảo mật & Loader | Triển khai **ECDH handshake**, **AES encryption**, loader **Python** và **JavaScript** | Tuần 7–8 |
| 5. License & Access | Hoàn thiện license system, HWID, rate limit, blacklist, whitelist | Tuần 9 |
| 6. Frontend & Email | Hoàn thiện giao diện, tích hợp WebSocket và **Amazon SES** | Tuần 10–11 |
| 7. Obfuscator | Hoàn thiện **Python AST Obfuscator** và bundle generation | Tuần 12 |
| 8. Testing & Deploy | Unit test, integration test, load test, deploy production | Tuần 13–14 |

---

# VII. KẾT QUẢ DỰ KIẾN

Sau khi hoàn thành, dự án sẽ cung cấp:

- Một nền tảng web hoàn chỉnh chạy trên **AWS Cloud** với kiến trúc **serverless**
- Hệ thống bảo vệ mã nguồn với hai protocol: **XOR execute v2** và **ECDH/AES-GCM v3**
- Giao diện quản trị trực quan, phân phối qua **Amazon CloudFront**
- Loader client tương thích với **Python 3.7+**, **Node.js v14+** và **Tampermonkey**
- Hệ thống email invitation tự động thông qua **Amazon SES**
- Nền tảng monitoring và alerting đầy đủ với **Amazon CloudWatch**
- Bộ tài liệu kỹ thuật gồm kiến trúc, API spec và hướng dẫn deploy bằng **AWS SAM** hoặc **AWS CDK**

---

# VIII. CÔNG NGHỆ SỬ DỤNG

**Node.js**, **Express.js**, **AWS Lambda**, **Amazon DynamoDB**, **Amazon S3**, **Amazon CloudFront**, **Amazon CloudWatch**, **Amazon SES**, **AWS API Gateway**, **AWS WAF**, **AWS KMS**, **AWS IAM**, **ECDH X25519**, **AES-256-GCM**, **HMAC-SHA256**, **HKDF-SHA256**, **PBKDF2-SHA256**, **Python urllib**, **Python AST Manipulation**, **Gzip Compression**, **CORS**, **Rate Limiting**, **Role-Based Access Control**, **Discord Webhook**.
