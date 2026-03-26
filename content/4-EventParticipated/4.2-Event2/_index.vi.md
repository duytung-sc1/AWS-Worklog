---
title: "Sự kiện 2"
date: 2026-01-01
weight: 2
chapter: false
pre: "<b>3.2.</b> "
---

# AI Agents, Prompt Engineering, and AIoT Projects on AWS

## Thông tin sự kiện

| Mục | Chi tiết |
| --- | --- |
| **Tên sự kiện** | AI Agents, Prompt Engineering, and AIoT Projects on AWS |
| **Thời gian** | 14/03/2026 |
| **Địa điểm** | tầng 26 tòa nhà Bitexco |
| **Vai trò** | Người tham dự (FCJ Cloud Intern) |

## Mục tiêu sự kiện
- Hiểu các khái niệm cốt lõi về AI Agents và cách chúng mở rộng khả năng của mô hình ngôn ngữ lớn.
- Tìm hiểu cách prompt engineering giúp cải thiện chất lượng đầu ra, tính ổn định và chi phí khi làm việc với LLM.
- Khám phá các dự án AIoT thực tế kết hợp thiết bị phần cứng, dịch vụ AWS trên cloud và AI.
- Tìm ra những ý tưởng có thể áp dụng vào các dự án cloud và AI trong thực tế.

## Diễn giả
- **Banh Cam Vinh** – Chủ đề: Xây dựng AI Agents với Strands
- **Nguyen Tuan Thinh** – DevOps Engineer, First Cloud AI Journey  
  Chủ đề: Automated Prompt Engineering: Enhancing LLM Output Quality
- **Aiden Dinh** – Operation Engineer, Katalon  
  Chủ đề: AIoT Projects

## Nội dung nổi bật

### Phiên 1: Xây dựng AI Agents với Strands
Phiên này tập trung vào việc giải thích giới hạn của các LLM hoạt động đơn lẻ và lý do vì sao AI agent đang trở thành hướng phát triển quan trọng. Thay vì chỉ sinh ra văn bản, AI agent có thể suy luận theo nhiều bước, gọi công cụ, truy cập dịch vụ bên ngoài và đưa ra quyết định dựa trên ngữ cảnh. Nội dung cũng trình bày workflow của **Strands Agents**, bao gồm built-in tools, tool calling, system prompt và khả năng kết nối knowledge base. Phần demo trực tiếp giúp minh họa rõ cách xây dựng agent bằng Strands một cách nhanh chóng và có cấu trúc hơn.

### Phiên 2: Prompt Engineering và tối ưu prompt
Phiên này nhấn mạnh rằng prompt tốt sẽ tạo ra kết quả tốt hơn. Các điểm chính bao gồm sự rõ ràng trong yêu cầu, xác định vai trò, cách viết instruction, định dạng output, ràng buộc và ví dụ minh họa. Diễn giả cũng chia sẻ các nguyên tắc quan trọng như viết prompt cụ thể, tách các phần rõ ràng, tránh mơ hồ và cho phép mô hình thừa nhận khi không chắc chắn. Ngoài ra, phiên này còn đề cập đến token economics và ảnh hưởng của chất lượng prompt đến chi phí sử dụng mô hình. Những kỹ thuật nâng cao như Chain-of-Thought, Tree-of-Thoughts, RAG và role prompting cũng được giới thiệu, cùng với công cụ **Proptimizer** để hỗ trợ tối ưu prompt tự động.

### Phiên 3: AIoT Projects và kiến trúc thực tế
Phiên AIoT trình bày một dự án **Locker Management** được xây dựng từ thiết bị phần cứng kết hợp với dịch vụ AWS. Mục tiêu của hệ thống là tự động hóa việc quản lý locker, giảm thao tác thủ công và cải thiện việc theo dõi tài sản. Hệ thống sử dụng các thiết bị như Raspberry Pi, Arduino, cảm biến reed switch, đầu đọc thẻ RFID, màn hình LCD và camera. Ở phía AWS, các dịch vụ như **AWS IoT Core**, **Lambda**, **DynamoDB**, **S3**, **Amplify** và **Rekognition** được dùng để kết nối thiết bị, lưu trữ dữ liệu, xử lý sự kiện và nhận diện khuôn mặt. Ngoài ra, phiên trình bày còn giới thiệu thêm dự án **Plutus: Financial Budget App** như một ví dụ khác về thiết kế hệ thống thực tế.

## Những điểm rút ra chính

### AI đang chuyển từ mô hình prompt sang agent
Một trong những thông điệp quan trọng nhất của sự kiện là hệ thống AI hiện đại đang dần vượt ra ngoài việc chỉ nhập prompt và nhận câu trả lời. AI agent là bước tiến cao hơn, nơi mô hình có thể suy luận theo từng bước, sử dụng công cụ, truy xuất thông tin và hành động với mức độ tự động cao hơn. Điều này khiến agent trở nên phù hợp hơn cho các workflow thực tế cần tích hợp, ra quyết định và thực thi tác vụ.

### Chất lượng prompt ảnh hưởng trực tiếp đến kết quả
Phiên prompt engineering cho thấy việc thiết kế prompt không chỉ là kỹ năng viết mà còn là một kỹ năng kỹ thuật. Một prompt tốt cần xác định rõ vai trò, nhiệm vụ, ngữ cảnh, input, định dạng đầu ra và các ràng buộc. Khi prompt được xây dựng tốt, kết quả sẽ ổn định hơn, ít mơ hồ hơn và tiết kiệm token hơn.

### AWS hỗ trợ mạnh cho các hệ thống AI và IoT thực tế
Dự án AIoT cho thấy các dịch vụ AWS có thể được kết hợp với thiết bị phần cứng để tạo ra hệ thống thông minh và tự động hóa. AWS IoT Core đảm nhiệm giao tiếp an toàn với thiết bị, trong khi Lambda, DynamoDB, S3 và Rekognition xử lý logic, lưu trữ và nhận diện. Điều này minh họa rõ cách cloud services có thể hỗ trợ những giải pháp AIoT thực tế và có khả năng mở rộng.

## Áp dụng vào công việc

### Ứng dụng AI Agents
Phiên Strands Agent mang lại nhiều ý tưởng hữu ích cho việc xây dựng các tính năng AI định hướng workflow. Trong các dự án tương lai, agent có thể được dùng cho các tác vụ như phân tích log, tạo báo cáo nhiều bước hoặc hỗ trợ tự động trong quy trình nội bộ khi hệ thống cần kết hợp giữa suy luận và sử dụng công cụ.

### Cải thiện đầu ra AI bằng prompt tốt hơn
Kiến thức từ phiên prompt engineering có thể áp dụng ngay cho mọi dự án có sử dụng LLM. Khi prompt được thiết kế rõ ràng và có cấu trúc hơn, chất lượng phản hồi sẽ được cải thiện, độ mơ hồ giảm đi và chi phí token cũng được tối ưu hơn. Điều này đặc biệt hữu ích cho các tính năng như báo cáo tự động, tóm tắt nội dung và hỗ trợ AI trong quy trình làm việc.

### Học từ kiến trúc cloud thực tế
Dự án AIoT là một ví dụ thực tế rất rõ về cách kết hợp phần cứng, backend, lưu trữ và dịch vụ AI trong cùng một hệ thống. Điều này giúp củng cố hiểu biết về cách kết nối thiết bị, xử lý logic sự kiện, lưu dữ liệu và xây dựng một kiến trúc hoàn chỉnh trên AWS.

## Trải nghiệm tại sự kiện
Sự kiện này có giá trị vì nó kết nối được giữa lý thuyết nền tảng và triển khai thực tế. Chuỗi nội dung từ AI agents, đến prompt engineering, rồi đến AIoT architecture tạo ra một lộ trình học rất liền mạch và dễ hình dung. Thay vì chỉ nói về khái niệm, các phiên đều cho thấy cách AWS services và kỹ thuật AI có thể phối hợp với nhau trong các hệ thống thực tế. Sự kiện cũng giúp mình nhận ra rằng các dự án cloud hiện đại ngày càng đòi hỏi sự kết hợp giữa năng lực AI, tư duy thiết kế hệ thống và góc nhìn vận hành.

## Tổng kết
Nhìn chung, sự kiện này giúp mình mở rộng hiểu biết ở ba hướng quan trọng: AI agents, prompt engineering và thiết kế hệ thống AIoT thực tế. Nó cho thấy rằng để xây dựng một hệ thống AI hiệu quả, không chỉ cần mô hình tốt mà còn cần workflow hợp lý, prompt được thiết kế cẩn thận và sự kết hợp đúng giữa các dịch vụ AWS. Những nội dung trong sự kiện mang lại cả sự rõ ràng về mặt khái niệm lẫn cảm hứng thực tế để áp dụng vào các dự án cloud và AI sau này.
