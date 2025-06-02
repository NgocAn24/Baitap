---
title: "Ứng dụng Phân tán - Giữa kỳ"
date: "2025-10-26"
updated: "2025-10-26"
categories:
  - "ungdungphantan"
  - "baitaonhom"
coverImage: "https://raw.githubusercontent.com/apache/airflow/19ebcac2395ef9a6b6ded3a2faa29dc960c1e635/docs/apache-airflow/img/logos/wordmark_1.png"
coverWidth: 16
coverHeight: 9
excerpt: cre Nhom 20.
---

## Tổng Quan
Báo cáo này mô tả quá trình triển khai một hệ thống quản lý đơn hàng phân tán, tích hợp các khái niệm chính trong hệ thống phân tán như chịu lỗi, giao tiếp phân tán, sharding, giám sát và kiểm thử hiệu năng. Hệ thống bao gồm nhiều node kho hàng (HCM, HN, Đà Nẵng), một lớp điều phối sử dụng Airflow và công cụ kiểm thử hiệu năng.

---

## Tiêu Chí Bắt Buộc

### 1. Chịu Lỗi (Fault Tolerance)
![Screenshot (84)](https://github.com/user-attachments/assets/fb66e040-e5b0-4e72-b86c-72401dfe9779)

- Hệ thống sử dụng các kiểm tra sức khỏe (health check) trên các node kho hàng trước khi gán đơn hàng.
- Nếu một node không truy cập được hoặc không khỏe mạnh, DAG của Airflow sẽ gán đơn hàng cho một node thay thế.
- Các tác vụ xử lý đơn hàng thất bại sẽ được thử lại tự động tối đa 3 lần với độ trễ 1 phút giữa các lần thử.
- Cơ chế chịu lỗi nhiều lớp (kiểm tra sức khỏe, chuyển hướng node thay thế, và thử lại) đảm bảo hệ thống vẫn hoạt động khi xảy ra lỗi.
- Việc kiểm thử đã xác nhận cơ chế thử lại hoạt động đúng: khi node HCM trả về lỗi 500, tác vụ được đánh dấu để thử lại và ghi log đầy đủ.

### 2. Giao Tiếp Phân Tán
- Giao tiếp giữa lớp điều phối Airflow và các node kho hàng được thực hiện qua các API HTTP REST.
- Mỗi node kho hàng cung cấp các endpoint cho kiểm tra sức khỏe và xử lý đơn hàng.
- Hệ thống được thiết kế để chạy trên nhiều máy, các node truy cập được qua các địa chỉ mạng.

### 3. Sharding hoặc Replication
![Screenshot (93)](https://github.com/user-attachments/assets/89bbe493-7ce9-44ed-9761-fadcf7cf3ab9)

- Hệ thống triển khai sharding theo vùng miền.
- Đơn hàng được phân phối đến các node kho hàng dựa trên trường `region` trong đơn.
- Mỗi node duy trì cơ sở dữ liệu SQLite riêng để lưu trữ đơn hàng cho shard của mình.


### 4. Giám Sát / Ghi Log Cơ Bản

![Screenshot (92)](https://github.com/user-attachments/assets/ab35d6f4-f299-4d0f-8fd1-589f894d70d9)

- Mỗi node kho ghi log các sự kiện chính như tạo đơn hàng, lỗi.
- Endpoint kiểm tra sức khỏe cung cấp dữ liệu giám sát cơ bản như trạng thái node và tải hiện tại (số đơn đang xử lý).
- DAG của Airflow ghi log việc nhận đơn, gán node, và kết quả xử lý đơn hàng.
- Script kiểm thử ghi log chi tiết về tỷ lệ thành công và hiệu năng.

### 5. Kiểm Thử Hiệu Năng Cơ Bản
- Script `simulate_orders.py` tạo đơn hàng đồng thời gửi đến API của Airflow.
- Hỗ trợ cấu hình số lượng đơn và mức độ đồng thời.
- Ghi log chi tiết kết quả bao gồm tỷ lệ thành công và thông lượng.

---

## Tiêu Chí Tùy Chọn (Đã Xem Xét)

Hệ thống hiện tại chưa triển khai hầu hết các tiêu chí tùy chọn. Đánh giá chi tiết bên dưới:

- **Phục hồi hệ thống sau khi lỗi:** Chưa triển khai. Các node không có cơ chế tự tham gia lại hoặc đồng bộ sau khi gặp lỗi.
- **Cân bằng tải:** Cân bằng tải đơn giản được thực hiện gián tiếp bằng cách chuyển hướng đến node khỏe mạnh, chưa có thuật toán nâng cao.
- **Bảo đảm tính nhất quán:** Không có giao thức đảm bảo nhất quán; mỗi node quản lý dữ liệu riêng lẻ.
- **Bầu chọn leader:** Chưa triển khai.
- **Bảo mật:** Giao tiếp HTTP cơ bản, không có xác thực hoặc mã hóa.
- **Tự động triển khai:** Có file Docker Compose cho một số thành phần, nhưng chưa có quy trình triển khai tự động hoàn chỉnh.


---

## Tóm Tắt

| Tiêu chí                    | Đã triển khai | Ghi chú                                          |
|-----------------------------|----------------|--------------------------------------------------|
| Chịu lỗi                    | Có             | Kiểm tra sức khỏe và chuyển hướng node thay thế |
| Giao tiếp phân tán         | Có             | API REST giữa Airflow và các node               |
| Sharding                   | Có             | Phân chia đơn hàng theo vùng                    |
| Replication                | Không          | Không sao chép dữ liệu giữa các node            |
| Giám sát / Ghi log         | Có             | Log và endpoint kiểm tra sức khỏe               |
| Kiểm thử hiệu năng cơ bản  | Có             | Script simulate_orders.py                       |
| Cân bằng tải                | Một phần       | Chỉ có cơ chế fallback cơ bản                   |


---

## Đề Xuất Cải Tiến

- Triển khai replication hoặc giao thức đồng thuận phân tán để đảm bảo dữ liệu và chịu lỗi.
- Thêm cơ chế bầu chọn leader để điều phối và chuyển đổi khi node lỗi.
- Tăng cường bảo mật với xác thực và mã hóa giao tiếp.
- Tự động hóa triển khai toàn bộ bằng Docker Compose hoặc Kubernetes.
- Triển khai cơ chế phục hồi để node có thể tham gia lại hệ thống sau khi lỗi.
- Thêm thuật toán cân bằng tải nâng cao dựa trên chỉ số tải của node.

---

## Kế Hoạch Kiểm Thử

### Kiểm Thử Thành Phần
1. Node Kho Hàng
   - Kiểm tra endpoint `/health`
   - Tạo đơn hàng (`POST /order`)
   - Truy xuất đơn (`GET /order/<id>`)
   - Xử lý lỗi và xác thực dữ liệu
   - Kiểm tra hoạt động cơ sở dữ liệu

2. Quy trình Airflow
   - Nhận đơn và xác thực dữ liệu
   - Gán node dựa trên vùng và trạng thái node
   - Xử lý đơn với cơ chế thử lại
   - Kiểm tra ghi log và xử lý lỗi

3. Kiểm Thử Tải
   - Gửi đơn hàng đồng thời
   - Mô phỏng tình huống lỗi node
   - Kiểm tra hành vi phục hồi
   - Ghi nhận thông lượng và hiệu năng

### Kiểm Thử Tích Hợp
1. Quy trình từ đầu đến cuối
   - Gửi đơn qua script stress test
   - DAG của Airflow xử lý đơn
   - Node kho nhận và xử lý
   - Đơn được lưu trong CSDL

2. Chịu lỗi
   - Xử lý khi node bị lỗi
   - Cơ chế thử lại và fallback

3. Giao tiếp phân tán
   - Giao tiếp HTTP giữa các thành phần
   - Kiểm tra dữ liệu phản hồi và lỗi

### Triển Khai
1. Các dịch vụ Docker Compose
2. 
3. ![Screenshot (89)](https://github.com/user-attachments/assets/c9e74365-5699-4ee2-829c-f09cb731eedb)

   - Các node kho (HCM, HN, Đà Nẵng)
   - Thành phần Airflow (webserver, scheduler, PostgreSQL)
   - Cấu hình mạng và volume

4. Giám sát
   - Thu thập log
   - Trạng thái `/health` của node
   - Chỉ số hiệu năng hệ thống

## Kết Luận

Hệ thống quản lý đơn hàng phân tán thể hiện rõ các khái niệm cơ bản trong hệ thống phân tán với khả năng chịu lỗi, chia vùng dữ liệu, giao tiếp phân tán, giám sát, và kiểm thử hiệu năng. Kiến trúc và triển khai hiện tại tạo nền tảng vững chắc để mở rộng và nâng cấp nhằm đáp ứng các yêu cầu nâng cao hơn trong hệ thống phân tán hiện đại.

Kế hoạch kiểm thử đề xuất giúp đảm bảo tính ổn định, độ tin cậy và khả năng mở rộng của hệ thống trong nhiều tình huống thực tế.
""")
