Dưới đây là bài làm **gộp chung** gồm **Prompt** và **bảng NFR** theo yêu cầu của đề. Chỉ cần copy sang Word là có thể nộp.

# BÀI 4: XÂY DỰNG YÊU CẦU PHI CHỨC NĂNG (NFR)

## Prompt thiết kế

### Vai trò (Role)

Bạn là Senior Solution Architect kiêm Senior System Analyst với hơn 10 năm kinh nghiệm thiết kế kiến trúc hệ thống Web API quy mô lớn, chuyên xây dựng tài liệu Software Requirement Specification (SRS) theo chuẩn IEEE 830 và tối ưu hiệu năng cho các hệ thống thương mại điện tử sử dụng Spring Boot, MySQL, JWT và Role-Based Access Control (RBAC).

### Mục tiêu (Objective)

Hãy xây dựng phần **Non-Functional Requirements (NFR)** cho hệ thống **Guai-api (Shop AI)** nhằm đảm bảo hệ thống đáp ứng yêu cầu về hiệu năng, khả năng mở rộng và bảo mật khi phục vụ lượng truy cập lớn.

### Ngữ cảnh (Context)

Hệ thống Guai-api là nền tảng thương mại điện tử cung cấp REST API cho ứng dụng Web và Mobile.

Công nghệ sử dụng:

* Spring Boot
* MySQL
* JWT Authentication
* RBAC
* RESTful API

Hệ thống cần chuẩn bị hạ tầng cho các sự kiện có lưu lượng truy cập lớn (Flash Sale, khuyến mãi, lễ hội mua sắm...).

### Ràng buộc (Constraints)

AI bắt buộc phải phân tích và đưa ra chỉ tiêu kỹ thuật rõ ràng cho các nội dung sau:

1. **Response Time**

   * Quy định thời gian phản hồi tối đa cho các API tra cứu sản phẩm.
   * Đề xuất mục tiêu Response Time trong điều kiện bình thường và khi tải cao.

2. **Database Performance**

   * Đề xuất chiến lược tối ưu MySQL.
   * Xác định các cột cần tạo Index.
   * Đưa ra khuyến nghị về Composite Index và tối ưu truy vấn.

3. **JWT Security**

   * Quy định thời gian xử lý cấp phát và xác thực JWT.
   * Yêu cầu bảo mật đối với Access Token và Refresh Token.
   * Đề xuất cơ chế chống giả mạo Token và ghi Security Log.

Ngoài ra, bổ sung các yêu cầu về:

* Scalability
* Availability
* Reliability
* Logging & Monitoring
* Error Handling
* Backup & Recovery

### Định dạng (Output Format)

Xuất kết quả dưới dạng **Markdown Table** gồm các cột:

* NFR ID
* Category
* Requirement
* Technical Target
* Acceptance Criteria

---

# Tài liệu Non-Functional Requirements (NFR)

| NFR ID | Category             | Requirement                                                                     | Technical Target                                                            | Acceptance Criteria                                          |
| ------ | -------------------- | ------------------------------------------------------------------------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------ |
| NFR-01 | Response Time        | API tra cứu sản phẩm phải phản hồi nhanh trong điều kiện hoạt động bình thường. | Thời gian phản hồi ≤ 500 ms với 95% request.                                | 95% request hoàn thành trong 500 ms.                         |
| NFR-02 | Response Time        | Hệ thống vẫn đảm bảo hiệu năng khi có lưu lượng truy cập cao.                   | Thời gian phản hồi ≤ 1 giây với 95% request khi tải cao.                    | Không vượt quá 1 giây trong các đợt Flash Sale.              |
| NFR-03 | Database Performance | Tối ưu MySQL bằng Indexing trên các trường thường xuyên truy vấn.               | Tạo Index cho: product_id, category_id, user_id, email, status, created_at. | Thời gian truy vấn giảm đáng kể, hạn chế Full Table Scan.    |
| NFR-04 | Database Performance | Sử dụng Composite Index cho các truy vấn nhiều điều kiện.                       | Ví dụ: (category_id, status), (user_id, created_at).                        | Các truy vấn lọc và sắp xếp sử dụng Index hiệu quả.          |
| NFR-05 | JWT Security         | Thời gian cấp phát Access Token và Refresh Token phải nhanh.                    | Thời gian sinh JWT ≤ 100 ms.                                                | Người dùng nhận Token gần như tức thời sau khi đăng nhập.    |
| NFR-06 | JWT Security         | Thời gian xác thực JWT phải tối ưu.                                             | Thời gian xác thực Token ≤ 50 ms cho mỗi request.                           | API không bị ảnh hưởng đáng kể bởi bước xác thực.            |
| NFR-07 | Security             | Access Token có thời gian sống ngắn, Refresh Token có thời gian sống dài hơn.   | Access Token: 15 phút; Refresh Token: 7 ngày.                               | Giảm nguy cơ lộ Token và vẫn đảm bảo trải nghiệm người dùng. |
| NFR-08 | Security             | JWT phải được ký bằng thuật toán mạnh và lưu trữ an toàn.                       | Sử dụng HS256 hoặc RS256, Secret Key tối thiểu 256 bit.                     | Không chấp nhận Token bị chỉnh sửa hoặc giả mạo.             |
| NFR-09 | Scalability          | Hệ thống phải có khả năng mở rộng khi số lượng người dùng tăng.                 | Hỗ trợ mở rộng theo chiều ngang (Horizontal Scaling) và Load Balancer.      | Có thể tăng số lượng server mà không thay đổi mã nguồn.      |
| NFR-10 | Availability         | Hệ thống phải hoạt động liên tục với độ sẵn sàng cao.                           | Uptime ≥ 99.9%.                                                             | Dịch vụ luôn sẵn sàng phục vụ người dùng.                    |
| NFR-11 | Logging & Monitoring | Ghi nhận đầy đủ các sự kiện hệ thống và bảo mật.                                | Ghi Audit Log, Security Log và Error Log; tích hợp công cụ giám sát.        | Có thể truy vết và phân tích sự cố nhanh chóng.              |
| NFR-12 | Backup & Recovery    | Dữ liệu phải được sao lưu và có khả năng khôi phục.                             | Sao lưu tự động hằng ngày; thời gian khôi phục (RTO) ≤ 30 phút.             | Hệ thống khôi phục dữ liệu thành công khi xảy ra sự cố.      |
| NFR-13 | Reliability          | Hệ thống phải xử lý lỗi ổn định và không làm mất dữ liệu.                       | Các giao dịch quan trọng sử dụng Transaction và cơ chế Rollback.            | Đảm bảo tính toàn vẹn dữ liệu khi phát sinh lỗi.             |
| NFR-14 | Error Handling       | API phải trả về mã lỗi thống nhất và dễ xử lý.                                  | Sử dụng chuẩn HTTP Status Code cùng định dạng JSON Error Response.          | Client nhận được thông tin lỗi rõ ràng và nhất quán.         |

## Kết luận

Các yêu cầu phi chức năng trên giúp hệ thống Guai-api đáp ứng tốt các đợt truy cập lớn, đảm bảo thời gian phản hồi nhanh, tối ưu hiệu suất cơ sở dữ liệu MySQL thông qua chiến lược Indexing, đồng thời tăng cường độ an toàn và hiệu quả của cơ chế xác thực JWT. Bộ NFR này cũng cung cấp các tiêu chí kỹ thuật và tiêu chuẩn nghiệm thu rõ ràng để nhóm phát triển và kiểm thử sử dụng trong quá trình triển khai.
