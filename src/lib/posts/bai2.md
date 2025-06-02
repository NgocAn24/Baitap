---
title: "Deliverable 2"
date: "2025-12-5"
updated: "2025-12-5"
categories:
  - "sveltekit"
  - "markdown"
coverWidth: 16
coverHeight: 9
excerpt: Check out how heading links work with this starter in this post.
---

# Thiết kế hệ thống: Ứng dụng phân tán xử lý đơn hàng với Apache Airflow

## 1. Kiến trúc hệ thống

Hệ thống được thiết kế theo mô hình phân tán, nơi **Apache Airflow** đóng vai trò là trung tâm điều phối các tác vụ xử lý đơn hàng, giao tiếp với nhiều **node kho hàng độc lập** thông qua **RESTful API**.



## 2. Mô tả chi tiết các thành phần trong hệ thống

### 2.1 Apache Airflow

* **Vai trò:** Là công cụ điều phối quy trình xử lý đơn hàng.
* **Chức năng:**

  * Kiểm tra tồn kho trên các node kho.
  * Chọn node phù hợp (node còn hàng).
  * Gửi đơn hàng đến node đã chọn.
  * Cập nhật trạng thái đơn hàng vào PostgreSQL.
* **Cách thức hoạt động:** Thông qua DAG (`order_dag.py`), từng task được thực hiện tuần tự trong luồng định nghĩa sẵn.

### 2.2 Các node kho (Warehouse Nodes)

* **Công nghệ:** Flask REST API, mỗi node chạy ở một cổng khác nhau.
* **Vai trò:**

  * Phản hồi lượng tồn kho khi được yêu cầu.
  * Nhận đơn hàng và xử lý nếu còn hàng.
* **Độc lập và phi tập trung:** Mỗi node là một container riêng biệt, có thể mở rộng hoặc thay thế dễ dàng.

### 2.3 Cơ sở dữ liệu PostgreSQL

* **Vai trò:** Lưu thông tin trạng thái đơn hàng.
* **Giao tiếp:** Được truy cập bởi Airflow để ghi log trạng thái đơn hàng.
* **Cấu hình:** Chạy bằng Docker, kết nối nội bộ với container Airflow.

### 2.4 Giao tiếp giữa các thành phần

* **Giao thức:** HTTP/HTTPS với REST API.
* **Kiến trúc:** Client → Airflow → Nodes → PostgreSQL.
* **Không sử dụng sharding/replication:** Tuy nhiên, hệ thống có thể dễ dàng mở rộng để sử dụng sharding (chia node theo khu vực địa lý) hoặc replication nếu có yêu cầu mở rộng tính sẵn sàng.

---

## 3. Công nghệ và thư viện sử dụng

| Thành phần    | Công nghệ              | Lý do lựa chọn                                          |
| ------------- | ---------------------- | ------------------------------------------------------- |
| Điều phối     | Apache Airflow         | Mạnh trong định nghĩa DAG và xử lý công việc theo luồng |
| API node      | Python Flask           | Nhẹ, đơn giản để triển khai REST API độc lập            |
| Database      | PostgreSQL             | Quan hệ, phù hợp lưu log trạng thái đơn hàng            |
| Container hóa | Docker, Docker Compose | Dễ triển khai, tái tạo môi trường chính xác             |
| Giao tiếp     | HTTP RESTful           | Chuẩn phổ biến, dễ mở rộng và gỡ lỗi                    |
| Visualization | Airflow Web UI         | Giám sát DAG trực quan, log chi tiết từng bước xử lý    |

---

## 4. Mô hình dữ liệu

```sql
-- order_status.sql
CREATE TABLE IF NOT EXISTS order_status (
    id SERIAL PRIMARY KEY,
    order_id VARCHAR(100),
    warehouse_node VARCHAR(50),
    status VARCHAR(50),
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

* `order_id`: Mã định danh đơn hàng (có thể do người dùng hoặc DAG sinh ra).
* `warehouse_node`: Tên node xử lý đơn hàng.
* `status`: Trạng thái đơn hàng: `pending`, `processed`, `failed`.
* `timestamp`: Thời gian ghi nhận trạng thái.

---

## 5. Chiến lược triển khai và cấu hình hệ thống

### 5.1 Docker Compose

* Hệ thống gồm 2 phần chính: `airflow/` và `node/`.
* Mỗi phần có file `docker-compose.yml` riêng biệt để:

  * Dễ quản lý và triển khai riêng biệt theo mục đích.
  * Linh hoạt khi mở rộng số node kho hàng.

### 5.2 Các bước triển khai

1. **Khởi động Airflow:**

   ```bash
   cd airflow
   docker-compose up -d
   ```

2. **Khởi động các node kho:**

   ```bash
   cd ../node
   docker-compose up -d
   ```

3. **Truy cập Airflow Web UI:**

   * [http://localhost:8080](http://localhost:8080)
   * Kích hoạt và Trigger DAG: `order_dag`

### 5.3 Mở rộng hệ thống

* Thêm node mới: tạo thư mục node mới, sửa file docker-compose thêm dịch vụ.
* Dễ tích hợp hệ thống khác như RabbitMQ, Kafka, Prometheus...

---

## Kết luận

Hệ thống sử dụng kiến trúc phân tán đơn giản nhưng rõ ràng, tận dụng khả năng điều phối linh hoạt của Apache Airflow. Bằng cách container hóa các thành phần, việc triển khai và mở rộng trở nên nhanh chóng và hiệu quả.


