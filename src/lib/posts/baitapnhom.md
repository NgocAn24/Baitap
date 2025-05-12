---
title: "Deliverable 1 - Website"
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

# Tên đề tài: Ứng dụng Apache Airflow để quản lý và tự động hóa quy trình xử lý dữ liệu cho hệ thống khóa học trực tuyến
________________________________________
## 1. Mục đích của thư viện được giao – Apache Airflow
**Mục đích:**
Apache Airflow là một công cụ mã nguồn mở để lập lịch, theo dõi và quản lý các luồng công việc dữ liệu (workflow) theo dạng DAG (Directed Acyclic Graph). Nó cho phép viết, thực thi và giám sát các pipeline dữ liệu bằng Python.
Vấn đề có thể giải quyết:
Trong hệ thống khóa học trực tuyến (online learning platform), có nhiều tác vụ xử lý dữ liệu phức tạp và định kỳ như:
-	Đồng bộ dữ liệu học viên, khóa học, điểm số từ nhiều nguồn (CSV, cơ sở dữ liệu, APIs,...)
-	Phân tích tương tác học tập (thời gian học, số lượt xem video, hoàn thành bài tập, v.v.)
-	Tự động gửi email nhắc nhở, thông báo kết thúc khóa
-	Tạo báo cáo theo lịch hàng ngày/tuần
-	Huấn luyện mô hình học máy để dự đoán hành vi người học
Tất cả những công việc trên đều có thể được lập lịch và quản lý bởi Apache Airflow.
________________________________________
**Điểm mạnh:**
-	Quản lý các tác vụ phức tạp với điều kiện rẽ nhánh, retry, và logging rõ ràng.
-	Giao diện web UI giúp dễ dàng theo dõi và điều phối công việc.
-	Hỗ trợ nhiều hệ sinh thái (MySQL, Postgres, GCP, AWS, Docker, Spark,...).
-	Tự động hóa mọi pipeline xử lý dữ liệu bằng code Python có thể kiểm soát.

**Điểm yếu:**
-	Cài đặt ban đầu hơi phức tạp.
-	Không phù hợp cho tác vụ thời gian thực (real-time).
-	Cần hiểu rõ kiến trúc DAG để thiết kế hiệu quả.
________________________________________
- So sánh với các thư viện/framework khác:
- 
Tiêu chí	Airflow	Luigi	Prefect	Cron + Script

Lập lịch

Theo dõi trực quan	Web UI đẹp	Web UI cơ bản	Web UI hiện đại	Không

Viết workflow bằng code	Python	Python	Python	Bash/Python

Tính mở rộng	Cao (Kubernetes, Celery)	Vừa	Cao	Thấp

Hỗ trợ phân nhánh logic	Có	Có	Có	Khó khăn
________________________________________
**Ứng dụng:** 
Apache Airflow sẽ được ứng dụng vào hệ thống học trực tuyến để:
-	Tự động xử lý dữ liệu tương tác người học.
-	Tạo báo cáo hằng ngày/tuần/tháng.
-	Tự động gửi email nhắc học viên chưa hoàn thành bài.
-	Kết nối API để lấy dữ liệu từ hệ LMS (Learning Management System).
________________________________________
## 2. Kế hoạch dự kiến về bài giữa kỳ
**Bài toán áp dụng:**
Tự động hóa phân tích dữ liệu học viên và gửi email nhắc nhở trong hệ thống học trực tuyến bằng Apache Airflow.
________________________________________
**🛠 Chi tiết bài toán:**
Trong hệ thống khóa học trực tuyến, có hàng trăm hoặc hàng ngàn học viên tham gia các khóa học khác nhau. Người quản trị muốn:
-	Theo dõi tiến độ học tập của học viên mỗi ngày.
-	Tự động tạo báo cáo tổng hợp các học viên chưa hoàn thành bài.
-	Gửi email nhắc nhở học viên tiếp tục học.
Việc làm thủ công tốn thời gian và dễ sai sót. Do đó, cần một giải pháp tự động và có thể lập lịch định kỳ.
________________________________________
**✅ Giải pháp đề xuất với Apache Airflow:**
Sử dụng Apache Airflow để tạo một pipeline dữ liệu gồm các bước:
1.	Task 1 – Lấy dữ liệu học viên:
-	Kết nối đến cơ sở dữ liệu hoặc API của hệ thống LMS.
-	Tải dữ liệu học viên và tiến độ học tập.
2.	Task 2 – Phân tích dữ liệu:
-	Xác định học viên chưa hoàn thành các bài học yêu cầu.
-	Tạo bảng thống kê theo từng khóa học.
3.	Task 3 – Tạo báo cáo:
-	Xuất kết quả thành file Excel hoặc PDF.
4.	Task 4 – Gửi email:
-	Gửi email nhắc học viên chưa hoàn thành bài.
-	Gửi bản báo cáo cho quản trị viên hệ thống.
________________________________________
**🗓 Lộ trình thực hiện dự kiến:**
Tuần	Công việc chính
Tuần 1	Tìm hiểu cơ bản về Apache Airflow, DAG, Task

Tuần 2	Thiết kế DAG pipeline đơn giản (fetch + process dữ liệu)

Tuần 3	Tích hợp task gửi email và tạo báo cáo Excel

Tuần 4	Thử nghiệm pipeline, xử lý lỗi, tinh chỉnh DAG

Tuần 5	Tích hợp pipeline với web (nếu cần) hoặc mock dashboard

Tuần 6	Viết báo cáo, chuẩn bị file nộp và thuyết trình
________________________________________
**📦 Kết quả kỳ vọng khi nộp bài giữa kỳ:**
-	Tệp DAG chạy được trong Apache Airflow.
-	Hình ảnh Web UI minh họa DAG và trạng thái các task.
-	Mẫu báo cáo (PDF hoặc Excel) được tạo tự động.
-	Email gửi tự động minh họa cho 1-2 học viên mẫu.
-	Báo cáo PDF trình bày pipeline, ý tưởng, kết quả, và hướng mở rộng.

