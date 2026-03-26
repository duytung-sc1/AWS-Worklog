---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# IrisAuth — Code Protector Platform

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

**AWS Lambda** · **S3** · **DynamoDB** · **CloudFront** · **CloudWatch** · **SES**

---

# I. GIỚI THIỆU DỰ ÁN

## 1.1. Bối cảnh và vấn đề

Trong môi trường phát triển phần mềm hiện đại, việc bảo vệ mã nguồn là một thách thức cấp thiết. Khi các developer phân phối script Python hoặc JavaScript đến người dùng cuối, họ đối mặt với rủi ro source code bị sao chép, chỉnh sửa, hoặc tái phân phối trái phép mà không có cơ chế kiểm soát hiệu quả.

Các giải pháp hiện có như obfuscation đơn thuần hay đóng gói file `.exe` còn nhiều hạn chế: dễ bị reverse engineering, không kiểm soát được ai đang chạy code, không thu hồi được quyền truy cập khi cần thiết. Thị trường thiếu một giải pháp tích hợp vừa bảo vệ mã nguồn, vừa quản lý phân phối và kiểm soát truy cập một cách toàn diện.

## 1.2. Giải pháp đề xuất

**Code Protector** (tên hệ thống: **IrisAuth**) là nền tảng SaaS cho phép developer upload mã nguồn lên server và phân phối đến người dùng qua cơ chế loader được mã hóa. Thay vì chia sẻ source code trực tiếp, người dùng cuối chỉ nhận một đoạn loader nhỏ 1–2 dòng. Server trả về code đã mã hóa chỉ khi người dùng vượt qua toàn bộ các kiểm tra bảo mật, và code chỉ tồn tại trong RAM khi thực thi — không bao giờ được lưu ra file.

Hệ thống được triển khai trên nền tảng **AWS Cloud**, tận dụng kiến trúc **serverless** và các dịch vụ **managed** để đảm bảo khả năng mở rộng cao, độ trễ thấp toàn cầu, và vận hành không cần quản lý hạ tầng thủ công.

---

# II. MỤC TIÊU DỰ ÁN

## 2.1. Mục tiêu tổng quát

Xây dựng một nền tảng web hoàn chỉnh cung cấp dịch vụ bảo vệ và phân phối mã nguồn an toàn cho Python và JavaScript, với hệ thống xác thực đa lớp, giao diện quản trị trực quan, và hạ tầng cloud AWS có khả năng mở rộng toàn cầu.

## 2.2. Mục tiêu cụ thể

- Thiết kế và triển khai hệ thống phân phối code qua loader mã hóa, đảm bảo source code không tiếp xúc người dùng cuối ở dạng plaintext.
- Xây dựng cơ chế trao đổi khóa **ECDH X25519** kết hợp mã hóa **AES-256-GCM** để bảo vệ kênh truyền dữ liệu.
- Phát triển hệ thống quản lý license với tính năng **HWID Lock** (khóa theo phần cứng), ngày hết hạn và giới hạn số lần thực thi.
- Xây dựng hệ thống workspace và team collaboration với nhiều vai trò người dùng.
- Phát triển giao diện web quản trị responsive, phân phối qua **Amazon CloudFront** với độ trễ thấp toàn cầu.
- Tích hợp monitoring và alerting qua **Amazon CloudWatch**, ghi log tập trung và thông báo qua Discord Webhook.
- Triển khai kiến trúc serverless trên **AWS Lambda**, lưu trữ đối tượng trên **Amazon S3**, dữ liệu cấu trúc trên **Amazon DynamoDB**.
- Tích hợp **Amazon SES** để gửi email mời thành viên workspace và thông báo hệ thống tự động.

---

# III. PHẠM VI DỰ ÁN

## 3.1. Trong phạm vi

### Backend & API

- RESTful API và WebSocket với **Node.js / Express.js** chạy trên **AWS Lambda** (qua Lambda Function URL hoặc API Gateway)
- Xử lý **ECDH Handshake**, **AES-256-GCM encryption**, **XOR protocol** cho loader distribution
- Rate limiting, IP access control, request signature validation

### Frontend

- 5 trang giao diện: **Landing**, **Login**, **Register**, **Dashboard**, **Workspace**
- Static assets phân phối toàn cầu qua **Amazon CloudFront CDN**
- **Vanilla HTML5/CSS3/JS**, không phụ thuộc framework nặng
- WebSocket real-time sync với **AWS API Gateway WebSocket API**

### Hạ tầng AWS (Mới — thay thế self-hosted)

- **Amazon S3**: lưu trữ mã nguồn được mã hóa (thay thế local filesystem storage)
- **Amazon DynamoDB**: cơ sở dữ liệu NoSQL thay thế SQLite — lưu users, workspaces, projects, licenses, logs, rate_limits, config
- **AWS Lambda**: runtime cho Express.js API (serverless, auto-scaling)
- **Amazon CloudFront**: CDN phân phối static files, cache S3 assets, terminate SSL, custom domain
- **Amazon CloudWatch**: monitoring, log aggregation, metric alarms, dashboard hệ thống
- **Amazon SES**: gửi email mời thành viên workspace qua token

### Loader & Client

- Python loader (`urllib`, Python 3.7+)
- Node.js loader
- Browser Userscript (Tampermonkey)
- **ECDH handshake v3** và **XOR execute v2** — hoạt động qua CloudFront endpoint

### Tính năng nghiệp vụ

- Hệ thống license: tạo đơn lẻ / hàng loạt, HWID binding, export CSV
- IP Blacklist/Whitelist, Rate Limiting (sliding window trên DynamoDB), Workspace PIN
- Team management: mời thành viên qua email token (gửi qua SES), 3 vai trò (**owner / editor / viewer**)
- Quản lý dự án đa file với bundle generation (**Python & Node.js**)
- Python AST Obfuscator (module độc lập)
- Đồng bộ thời gian thực qua WebSocket, thông báo Discord Webhook

## 3.2. Ngoài phạm vi

- Ứng dụng mobile native
- Hỗ trợ ngôn ngữ lập trình khác ngoài Python và JavaScript
- Tích hợp cổng thanh toán
- Tính năng CI/CD tự động hóa deployment pipeline ngoài **AWS SAM / CDK** cơ bản

---

# IV. KIẾN TRÚC HỆ THỐNG & CÔNG NGHỆ

## 4.1. Stack công nghệ (AWS-native)

| Tầng | Dịch vụ / Công nghệ | Chi tiết |
|------|----------------------|----------|
| Runtime API | Node.js v18+ (AWS Lambda) | ES Module, Express.js wrapped bằng aws-serverless-express |
| Database | Amazon DynamoDB | NoSQL key-value, on-demand capacity, TTL tự động cho `rate_limits` & `sessions` |
| Object Storage | Amazon S3 | Lưu script đã mã hóa + gzip (`key = UUID`), versioning bật, server-side encryption AES-256 |
| CDN & Edge | Amazon CloudFront | Phân phối static frontend, terminate SSL, cache S3 assets, custom domain, WAF tích hợp |
| Compute | AWS Lambda | Auto-scale 0→∞, cold start <1s (Node.js 18), function URL hoặc API Gateway |
| Email | Amazon SES | Gửi workspace invitation email, cấu hình DKIM + SPF, sandbox → production |
| Monitoring | Amazon CloudWatch | Log Groups cho từng Lambda function, metric alarms (error rate, latency), dashboard |
| Real-time | API Gateway WebSocket API | Broadcast per-workspace updates, thay thế `ws` standalone server |
| Frontend | HTML5, CSS3, Vanilla JS | 5 trang static, deploy lên S3 bucket, phân phối qua CloudFront |
| Nén dữ liệu | Pako (Gzip) | Nén code trước khi lưu S3 |

## 4.2. Kiến trúc tổng quan AWS

Hệ thống **IrisAuth** được triển khai theo mô hình **serverless** thuần túy trên AWS, loại bỏ hoàn toàn việc quản lý server vật lý. Luồng request tiêu biểu:

- Client (browser / Python loader) → **CloudFront Distribution** (SSL termination, cache layer)
- Static assets (HTML/CSS/JS) được phục vụ trực tiếp từ **S3 bucket** qua CloudFront
- API request `/api/*` được forward tới **API Gateway** → **AWS Lambda function**
- Lambda function xử lý business logic, đọc/ghi **DynamoDB** (metadata) và **S3** (script content)
- Email invitation gửi qua **Amazon SES** với template được lưu trên SES
- Mọi log của Lambda được tự động stream vào **CloudWatch Logs**; **CloudWatch Alarms** cảnh báo khi error rate tăng bất thường
- WebSocket connection (execute endpoint) đi qua **API Gateway WebSocket API**, route tới Lambda handler

## 4.3. Hệ thống xác thực (tự xây dựng, không dùng thư viện JWT)

Hệ thống không dùng thư viện JWT bên ngoài.

- Ký token bằng **HMAC-SHA256**, secret sinh ngẫu nhiên 48 bytes lưu trong **DynamoDB** (bảng `app_config`)
- Token TTL: **7 ngày**
- Khi người dùng đổi mật khẩu, toàn bộ token cũ bị vô hiệu hóa tức thì (so sánh `iat` với `password_changed_at`)
- Gửi qua **HttpOnly Cookie** (tên `token`) kết hợp `Authorization` header
- Mật khẩu băm bằng **PBKDF2-SHA256**, **210.000 iterations**, salt ngẫu nhiên 16 bytes — đạt chuẩn **OWASP**
- Tự động nâng cấp tài khoản cũ dùng SHA-256 thuần khi họ đăng nhập lại
- Workspace PIN cũng dùng **PBKDF2**, sinh pin session token riêng lưu bảng `pin_verifications` trong **DynamoDB** với TTL tự động

## 4.4. Hai protocol phân phối code

### Protocol v2 — `GET /api/v5/execute` (XOR)

- Client gửi: `id`, `license key`, `HWID`, `timestamp`, `nonce`, `HMAC-SHA256 signature`
- Server xác thực signature, kiểm tra timestamp ±5 phút (chống replay attack)
- Mã hóa response bằng XOR với key = `SHA256(derivedSecret:hwid:nonce:id)`
- Response gồm header 16 bytes (magic bytes + content length + timestamp + random) ghép với script bytes, sau đó XOR toàn bộ
- Script content được đọc từ **Amazon S3**

### Protocol v3 — `POST /api/v5/handshake` (ECDH + AES-GCM)

- Client tạo cặp khóa **X25519**, gửi public key lên server cùng signature
- Lambda function tạo cặp khóa X25519 riêng, tính shared secret bằng `diffieHellman()`
- Dẫn xuất AES key bằng **HKDF-SHA256** (RFC 5869), `salt = "IrisAuth-Salt"`, `info = "IrisAuth-{nonce}"`
- Mã hóa script bằng **AES-256-GCM**, trả về server public key + ciphertext + signature xác nhận
- Mỗi phiên có session key hoàn toàn khác nhau — **perfect forward secrecy**

## 4.5. Thuật toán và chuẩn bảo mật

| Thuật toán | Ứng dụng cụ thể trong hệ thống |
|-----------|--------------------------------|
| PBKDF2-SHA256 (210.000 iter) | Băm mật khẩu người dùng, băm Workspace PIN — lưu DynamoDB |
| HMAC-SHA256 | Ký auth token, ký request loader, ký response server |
| ECDH X25519 | Trao đổi khóa Protocol v3 (handshake) |
| HKDF-SHA256 | Dẫn xuất AES key từ ECDH shared secret (RFC 5869) |
| AES-256-GCM | Mã hóa script lưu trữ trên S3 + mã hóa truyền tải Protocol v3 |
| XOR + SHA-256 | Mã hóa truyền tải Protocol v2 |
| Gzip (Pako) | Nén content trước khi lưu S3 |
| `crypto.timingSafeEqual` | So sánh token/signature — chống timing attack |
| Nonce + Timestamp ±5 phút | Chống replay attack trên mọi request loader |
| S3 Server-Side Encryption | Mã hóa at-rest mọi object trên S3 bằng AWS KMS (SSE-KMS) |
| CloudFront + ACM | TLS 1.2+ enforce trên toàn bộ kết nối client-server |

## 4.6. Cơ sở dữ liệu — Amazon DynamoDB

Thay thế SQLite bằng **Amazon DynamoDB** (fully managed NoSQL). Thiết kế bảng theo mô hình single-table design hoặc multi-table tùy access pattern:

| Bảng DynamoDB | Partition Key / Sort Key | Chức năng |
|--------------|---------------------------|-----------|
| users | PK: `userId` | Tài khoản người dùng, role, trạng thái, `password_changed_at` |
| workspaces | PK: `workspaceId` | Workspace, `loader_key`, ngôn ngữ, `encryption_key`, PIN hash |
| projects | PK: `workspaceId`, SK: `projectId` | Project (script), cài đặt bảo mật, đếm execution |
| project_files | PK: `projectId`, SK: `fileId` | File/folder trong project, entry point, ngôn ngữ, `sort_order` |
| licenses | PK: `workspaceId`, SK: `licenseKey` | License key, HWID, ngày hết hạn, usage count, activated OS |
| access_lists | PK: `workspaceId`, SK: `ip#type` | IP blacklist / whitelist theo workspace |
| workspace_members | PK: `workspaceId`, SK: `userId` | Thành viên được mời, vai trò |
| workspace_invitations | PK: `token` | Token mời qua email (SES), TTL tự động |
| pin_verifications | PK: `sessionToken` | Session token sau xác thực PIN, TTL tự động |
| logs | PK: `workspaceId`, SK: `timestamp#uuid` | Nhật ký sự kiện: IP, quốc gia, action — GSI trên country, timestamp |
| app_config | PK: `configKey` | Cấu hình hệ thống (HMAC secret, loader secret...) |
| rate_limits | PK: `rateLimitKey` | Rate limit sliding window với TTL tự động dọn dẹp |

**Lưu ý thiết kế:** DynamoDB TTL được bật trên các bảng `workspace_invitations`, `pin_verifications`, và `rate_limits` để tự động xóa records hết hạn mà không cần cronjob. GSI (Global Secondary Index) được tạo trên trường `country` và `timestamp` của bảng `logs` để hỗ trợ truy vấn phân tích.

## 4.7. Amazon S3 — Object Storage cho Script Content

- Thay thế hoàn toàn local filesystem storage (thư mục `/storage`)
- Mỗi script file được lưu với key dạng: `{workspaceId}/{uuid}_{timestamp}_{hash}.gz`
- Content được **Gzip** nén (Pako) rồi **AES-256-GCM** mã hóa trước khi PUT lên **S3**
- S3 bucket bật **versioning**, **server-side encryption SSE-KMS**, **Block Public Access** hoàn toàn
- Lambda function truy cập S3 qua **IAM Role** với policy **least-privilege**
- Static frontend assets (HTML/CSS/JS/images) lưu trên S3 bucket riêng, phân phối qua **CloudFront**

## 4.8. Amazon CloudFront — CDN & Edge

- Distribution duy nhất phục vụ cả static frontend và API proxy
- Behavior rules: `/api/*` → API Gateway origin; `/*` → S3 static bucket origin
- Enforce HTTPS, TLS 1.2 minimum, HTTP/2 bật mặc định
- Cache TTL tối ưu cho static assets (`86400s`); `no-cache` cho API responses
- Custom domain với certificate từ **AWS Certificate Manager (ACM)**
- Tích hợp **AWS WAF** để chặn bad bots, SQL injection, XSS tại edge layer
- CloudFront header `cf-ipcountry` được forward để hệ thống ghi country trong logs

## 4.9. Amazon CloudWatch — Monitoring & Observability

- Log Groups tự động cho mỗi Lambda function với retention 30 ngày
- Metric Filters trích xuất custom metrics: `execution_count`, `license_error_rate`, `ecdh_handshake_rate`
- CloudWatch Alarms: cảnh báo khi Lambda error rate > 1%, p99 latency > 2s, DynamoDB throttle events
- CloudWatch Dashboard tổng hợp: request volume, latency distribution, error breakdown, S3 storage usage
- Structured JSON logging từ Lambda để CloudWatch Insights query được (filter theo `workspaceId`, `action`, `IP`)

## 4.10. Amazon SES — Email Service

- Gửi workspace invitation email có chứa token link
- Template email được lưu trên SES Template hoặc inline HTML
- Cấu hình **DKIM**, **SPF**, **DMARC** để đảm bảo deliverability
- SES Event destination kết nối với CloudWatch để theo dõi bounce, complaint rate
- Sandbox mode → production sau khi verify sending domain

## 4.11. Role-Based Access Control (RBAC)

| Vai trò | Quản lý Project | Quản lý License | Xem Log | Cài đặt Workspace | Mời thành viên |
|--------|------------------|------------------|---------|-------------------|----------------|
| Owner | ✓ Toàn quyền | ✓ Toàn quyền | ✓ Toàn quyền | ✓ Toàn quyền | ✓ Toàn quyền |
| Admin | ✓ Toàn quyền | ✓ Toàn quyền | ✓ Toàn quyền | ✓ Giới hạn | ✓ Có thể |
| Editor | ✓ CRUD | ✓ Tạo/Xem | ✓ Xem | ✗ | ✗ |
| Viewer | ✗ Chỉ đọc | ✗ Chỉ xem | ✓ Xem | ✗ | ✗ |

---

# V. TÍNH NĂNG CHI TIẾT

## 5.1. Luồng hoạt động cốt lõi (với AWS)

- Developer đăng nhập, tạo workspace → metadata lưu **DynamoDB**
- Developer upload hoặc nhập mã nguồn → code được **Gzip** nén (Pako), mã hóa **AES-256-GCM** với `encryption_key` của workspace, PUT lên **Amazon S3** với key UUID
- Developer lấy đoạn loader 1–2 dòng (Python / JS), CloudFront endpoint được nhúng trong loader
- End-user chạy loader → loader sinh HWID từ MAC address, tên máy, CPU, OS, username → hash **SHA-256**
- Loader thực hiện ECDH Handshake hoặc gọi `/api/v5/execute` kèm **HMAC signature + nonce + timestamp** qua CloudFront → API Gateway → Lambda
- Lambda xác thực signature, kiểm tra toàn bộ điều kiện truy cập (**DynamoDB**: license, HWID, rate_limit, access_list)
- Nếu hợp lệ: Lambda GET object từ **S3**, giải mã, trả về code mã hóa theo protocol v2/v3. Loader giải mã và `exec()` trong RAM — không lưu file
- Lambda ghi log vào **DynamoDB** (bảng `logs`), cập nhật `execution_count`, broadcast WebSocket event qua **API Gateway WebSocket API**
- **CloudWatch** tự động nhận log stream, metrics được cập nhật real-time

## 5.2. Hệ thống License

- Tạo license key đơn lẻ với định dạng `[prefix]LIC-XXXXXXXX-XXXXXXXX[suffix]`, hỗ trợ custom key
- Tạo hàng loạt tối đa 100 license/lần (**DynamoDB batch write**)
- **HWID Lock**: khi end-user chạy lần đầu, HWID ghi vào DynamoDB. Lần sau nếu HWID khác bị từ chối
- Bật/tắt `hwid_lock` riêng cho từng license
- Đặt ngày hết hạn (`expiration_date`), kiểm tra realtime mỗi lần execute
- Gắn license với project cụ thể hoặc toàn workspace
- Reset HWID khi người dùng đổi máy
- Export toàn bộ danh sách ra CSV (`key`, `note`, `trạng thái`, `HWID`, `OS`, `lần dùng`, `lần cuối dùng`)

## 5.3. Kiểm soát truy cập

- **IP Blacklist**: chặn IP cụ thể vĩnh viễn khỏi workspace — lưu DynamoDB bảng `access_lists`
- **IP Whitelist**: chỉ cho phép IP trong danh sách, bật/tắt theo từng project
- **CloudFront WAF**: lớp bảo vệ edge bổ sung (rate-based rules, geo-blocking nếu cần)
- **Max executions**: giới hạn tổng số lần project được chạy, sau đó tự khóa
- **Rate limiting** (sliding window, lưu DynamoDB với TTL):
  - Login: tối đa 10 request/phút/IP, 5 request/phút/email
  - Register: tối đa 5 request/phút/IP
  - Loader execute: tối đa 120 request/phút/IP (global), giới hạn riêng theo script (1–600/phút)
  - ECDH handshake: tối đa 60 request/phút/IP
  - Tạo license: 30/phút; batch create: 5/phút
- DynamoDB TTL tự động xóa `rate_limit entries` hết hạn — không cần cronjob
- Workspace PIN: băm **PBKDF2**, cấp pin session token riêng lưu DynamoDB với TTL tự động
- Request signature: mọi request tới loader phải kèm **HMAC-SHA256 signature** hợp lệ

## 5.4. Quản lý dự án đa file

- Tổ chức theo cấu trúc thư mục (`type: file | folder`), lưu DynamoDB bảng `project_files` với `parent_id`, `sort_order`
- Chỉ định entry point: đánh dấu file chính (`is_entry_point`) cho project nhiều file
- Bundle generation: Lambda tự động tạo bundle lúc phân phối:
  - **Python**: ghép tất cả file thành một script, resolve đường dẫn import theo cấu trúc thư mục
  - **Node.js**: tương tự, wrap theo cấu trúc module
- Hỗ trợ 30+ ngôn ngữ (detect từ extension: `.py`, `.js`, `.ts`, `.lua`, `.go`, `.rs`, `.java`, `.c`, `.cpp`, `.sql`...)
- Thao tác: tạo, đổi tên, di chuyển, sao chép, xóa, upload, tìm kiếm toàn văn
- Batch operations, tải xuống file đơn lẻ
- Content lớn lưu S3 (`key dạng s3:<bucket>/<uuid>`), giải mã + giải nén khi đọc

## 5.5. Workspace & Team Collaboration

- Mỗi user sở hữu nhiều workspace độc lập, mỗi workspace có `loader_key` riêng (ID công khai)
- Mời thành viên qua email token — Lambda gọi **Amazon SES** để gửi email tự động
- Token lưu DynamoDB bảng `workspace_invitations` với TTL tự động. Người được mời nhận link chấp nhận qua email
- 4 vai trò phân quyền: `owner / admin / editor / viewer`
- Cập nhật thời gian thực qua **API Gateway WebSocket API**: khi có thay đổi license, project, team → broadcast tới toàn bộ client trong workspace
- **Discord Webhook**: tự động gửi notification khi có sự kiện quan trọng (cấu hình per workspace)
- Thống kê tổng hợp trên dashboard: tổng `execution count`, số license, số thành viên

## 5.6. Logging & Monitoring

- Ghi log mọi sự kiện: `LOAD_SCRIPT`, `ECDH_HANDSHAKE`, `BLOCK_ACCESS`, `INVALID_LICENSE`, `INVALID_HWID`, `INVALID_SIGNATURE`, `CREATE_LICENSE`, `BATCH_CREATE_LICENSE`, `TOGGLE_LICENSE`...
- Mỗi log entry: `workspace_id`, `action`, `details`, `IP`, `quốc gia`, `timestamp` — lưu **DynamoDB**
- Lambda log stream tự động vào **CloudWatch Logs** (structured JSON) — query bằng **CloudWatch Logs Insights**
- CloudWatch Alarms cảnh báo proactive: Lambda error spike, DynamoDB throttle, SES bounce rate cao
- CloudWatch Dashboard tổng hợp KPI: request rate, latency percentiles, error rate, execution count theo workspace
- Xem log theo workspace, xóa log theo workspace từ giao diện quản trị
- Tất cả thay đổi broadcast realtime qua **API Gateway WebSocket API**

## 5.7. Python AST Obfuscator (module độc lập)

File `obfuscator.py` là công cụ obfuscate code Python hoàn toàn độc lập, hoạt động ở tầng AST:

- Phân tích cú pháp và kiểm tra compatibility issues trước khi obfuscate
- Đổi tên biến, hàm, tham số thành chuỗi ngẫu nhiên không có nghĩa
- Thêm dead code để gây nhiễu khi đọc
- Báo cáo chi tiết các vấn đề tương thích (`severity: error / warning`) kèm số dòng, cột

---

# VI. KẾ HOẠCH THỰC HIỆN

| Giai đoạn | Nội dung | Thời gian dự kiến |
|----------|----------|-------------------|
| 1. Phân tích & Thiết kế | Xác định yêu cầu chi tiết, thiết kế DynamoDB schema, thiết kế API, thiết kế CloudFront behavior rules | Tuần 1–2 |
| 2. Hạ tầng AWS | Provisioning Lambda, DynamoDB, S3, CloudFront, SES, CloudWatch (IaC bằng AWS SAM hoặc CDK) | Tuần 3 |
| 3. Backend Core | Auth, Workspace, Project, File management API — migrate từ SQLite sang DynamoDB, filesystem sang S3 | Tuần 4–6 |
| 4. Bảo mật & Loader | ECDH handshake, AES encryption, Python/JS loader hoạt động qua CloudFront endpoint | Tuần 7–8 |
| 5. License & Access | License system, HWID, Rate limit (DynamoDB TTL), Blacklist, IP Whitelist | Tuần 9 |
| 6. Frontend & Email | 5 trang giao diện, WebSocket integration (API GW), SES email invitation | Tuần 10–11 |
| 7. Obfuscator | Python AST obfuscator, bundle generation | Tuần 12 |
| 8. Testing & Deploy | Unit test, integration test, load test, CloudWatch alarm tuning, deploy production | Tuần 13–14 |

---

# VII. KẾT QUẢ DỰ KIẾN

Sau khi hoàn thành, dự án sẽ cung cấp:

- Nền tảng web hoạt động đầy đủ, deploy trên **AWS** với kiến trúc **serverless** (`Lambda + DynamoDB + S3 + CloudFront`)
- Hệ thống bảo vệ mã nguồn với 2 protocol mã hóa (**v2 XOR** và **v3 ECDH/AES-GCM**), script content lưu an toàn trên **S3**
- Giao diện quản trị trực quan phân phối qua **CloudFront** với độ trễ thấp toàn cầu
- Loader client tương thích **Python 3.7+**, **Node.js v14+**, và **Tampermonkey** — hoạt động qua CloudFront endpoint
- Email invitation tự động qua **Amazon SES** — thành viên nhận link trực tiếp vào hộp thư
- Monitoring & alerting đầy đủ qua **CloudWatch**: log tập trung, alarm tự động, dashboard KPI
- Tài liệu kỹ thuật đầy đủ: kiến trúc AWS, API spec, hướng dẫn deploy bằng **AWS SAM/CDK**

---

# VIII. CÔNG NGHỆ SỬ DỤNG

**Node.js**, **Express.js**, **AWS Lambda**, **Amazon DynamoDB**, **Amazon S3**, **Amazon CloudFront**, **Amazon CloudWatch**, **Amazon SES**, **AWS API Gateway (REST + WebSocket)**, **AWS WAF**, **AWS KMS**, **AWS IAM**, **ECDH X25519**, **AES-256-GCM**, **HMAC-SHA256**, **HKDF-SHA256**, **PBKDF2-SHA256**, **Python urllib**, **Python AST Manipulation**, **Gzip Compression**, **CORS**, **Rate Limiting**, **Role-Based Access Control (RBAC)**, **Discord Webhook**.
