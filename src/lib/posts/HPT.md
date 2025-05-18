---
title: "các kiến trúc trong HPT"
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

# Tìm hiểu các giao thức mạng và công nghệ phân tán

#### 1. TCP/IP
- **Mục đích sử dụng:** Là bộ giao thức nền tảng cho toàn bộ mạng Internet, dùng để truyền dữ liệu giữa các thiết bị một cách đáng tin cậy.
- **Đặc điểm:**
  - TCP (Transmission Control Protocol) đảm bảo dữ liệu được truyền chính xác, đúng thứ tự và không bị mất mát.
  - IP (Internet Protocol) chịu trách nhiệm định tuyến các gói dữ liệu tới đúng địa chỉ.
- **Ứng dụng:** Hầu hết các giao thức mạng cao hơn như HTTP, FTP, SMTP đều chạy trên TCP/IP.
- **Liên quan:** HTTP, RPC, gRPC đều chạy trên TCP.

#### 2. UDP (User Datagram Protocol)
- **Mục đích sử dụng:** Giao thức truyền dữ liệu không kết nối, không đảm bảo độ tin cậy, dùng khi tốc độ quan trọng hơn tính toàn vẹn dữ liệu.
- **Đặc điểm:**
  - Không thiết lập kết nối trước khi truyền dữ liệu.
  - Không kiểm tra lỗi hay sắp xếp thứ tự gói tin.
  - Phù hợp cho các ứng dụng như streaming video, game online, VoIP.
- **Khác biệt với TCP:** Nhanh hơn nhưng không đảm bảo dữ liệu đến.

#### 3. HTTP (Hypertext Transfer Protocol)
- **Mục đích sử dụng:** Giao thức ứng dụng dùng để truyền tải các tài nguyên web (HTML, hình ảnh, dữ liệu API).
- **Đặc điểm:**
  - Chạy trên TCP.
  - Là giao thức request-response: client gửi yêu cầu, server trả về phản hồi.
  - Phiên bản phổ biến nhất là HTTP/1.1, mới hơn là HTTP/2 hỗ trợ multiplexing.
- **Liên quan:** Nền tảng cho các API kiểu REST, SOAP, GraphQL và các kỹ thuật AJAX.

#### 4. REST (Representational State Transfer)
- **Mục đích sử dụng:** Kiến trúc thiết kế API trên HTTP để truy cập và thao tác tài nguyên.
- **Đặc điểm:**
  - Dựa trên HTTP verbs: GET (lấy dữ liệu), POST (tạo mới), PUT (cập nhật), DELETE (xóa).
  - Dữ liệu truyền thường ở dạng JSON hoặc XML.
  - Đơn giản, dễ phát triển và mở rộng.
- **Ưu điểm:** Dễ hiểu, nhẹ, phù hợp ứng dụng web hiện đại.
- **Liên quan:** Giao tiếp qua HTTP.

#### 5. GraphQL
- **Mục đích sử dụng:** Ngôn ngữ truy vấn API do Facebook phát triển, cho phép client chỉ yêu cầu chính xác dữ liệu cần thiết.
- **Đặc điểm:**
  - Chạy trên HTTP.
  - Client có thể định nghĩa cấu trúc dữ liệu trả về, giảm dư thừa dữ liệu.
  - Hỗ trợ truy vấn phức tạp, nhiều nguồn dữ liệu.
- **Ưu điểm:** Linh hoạt hơn REST, tối ưu băng thông và hiệu suất.
- **Liên quan:** Một lựa chọn thay thế REST.

#### 6. SOAP (Simple Object Access Protocol)
- **Mục đích sử dụng:** Giao thức cho Web Services dựa trên XML để trao đổi dữ liệu giữa các hệ thống.
- **Đặc điểm:**
  - Dữ liệu truyền dưới dạng XML.
  - Hỗ trợ các tính năng bảo mật, giao dịch phức tạp, xác thực, mở rộng.
  - Thường dùng HTTP hoặc SMTP làm kênh truyền.
- **Ưu điểm:** Phù hợp môi trường doanh nghiệp cần tính ổn định và bảo mật cao.
- **Nhược điểm:** Nặng nề hơn REST, phức tạp hơn.
- **Liên quan:** Chạy trên HTTP.

#### 7. AJAX (Asynchronous JavaScript and XML)
- **Mục đích sử dụng:** Kỹ thuật gọi API bất đồng bộ từ trình duyệt, giúp web tương tác động mà không cần tải lại trang.
- **Đặc điểm:**
  - Dùng XMLHttpRequest hoặc Fetch API để gửi và nhận dữ liệu HTTP.
  - Hỗ trợ JSON, XML hoặc dữ liệu văn bản.
- **Ứng dụng:** Tạo giao diện người dùng mượt mà, cải thiện trải nghiệm web.
- **Liên quan:** Hoạt động trên giao thức HTTP.

#### 8. RPC (Remote Procedure Call)
- **Mục đích sử dụng:** Cung cấp phương thức gọi hàm từ xa như gọi hàm cục bộ, giúp ẩn chi tiết truyền thông mạng.
- **Đặc điểm:**
  - Có thể chạy trên TCP hoặc HTTP.
  - Truyền tham số và nhận kết quả qua mạng.
  - Giúp đơn giản hóa phát triển hệ thống phân tán.
- **Ví dụ:** DCE RPC, Java RMI.
- **Liên quan:** Cơ sở cho RMI, gRPC.

#### 9. gRPC
- **Mục đích sử dụng:** Một framework RPC hiệu suất cao, do Google phát triển, dùng giao thức HTTP/2 và Protocol Buffers.
- **Đặc điểm:**
  - Truyền dữ liệu nhị phân giúp giảm kích thước và tăng tốc độ.
  - Hỗ trợ đa ngôn ngữ (C++, Java, Python, Go, v.v).
  - Hỗ trợ streaming và đa luồng qua HTTP/2.
- **Ưu điểm:** Hiệu suất cao, mở rộng tốt cho microservices.
- **Liên quan:** Cải tiến của RPC, dựa trên HTTP/2 thay vì HTTP/1.x.

---

#### Tóm tắt mối quan hệ giữa các giao thức và công nghệ
- **TCP/IP** là tầng nền tảng cho tất cả giao thức mạng.
- **UDP** cung cấp truyền tải không kết nối, nhanh nhưng không tin cậy, khác với TCP.
- **HTTP** là giao thức ứng dụng phổ biến nhất, nằm trên TCP.
- Các giao thức API như **REST, SOAP, GraphQL, AJAX** đều sử dụng HTTP.
- **RPC** và **gRPC** là các giao thức gọi hàm từ xa, khác với API HTTP ở cách tiếp cận; gRPC tối ưu hơn với HTTP/2 và nhị phân.
- **AJAX** là kỹ thuật gọi HTTP bất đồng bộ trong trình duyệt, cải thiện trải nghiệm web.
- **GraphQL** và **REST** đều dùng HTTP nhưng GraphQL linh hoạt hơn trong truy vấn dữ liệu.

---

#### Kết luận
Mỗi giao thức và công nghệ có ưu nhược điểm và ứng dụng riêng, phù hợp với từng bài toán trong phát triển ứng dụng phân tán. Hiểu rõ sự tương quan giúp lựa chọn giải pháp thích hợp để tối ưu hiệu quả hệ thống.

# Nghiên cứu về thư viện OpenMPI và ứng dụng phân tán tính số nguyên tố

### 1. Thư viện OpenMPI là gì?

OpenMPI (Open Message Passing Interface) là một thư viện mã nguồn mở triển khai chuẩn giao tiếp MPI - giao diện truyền thông tin điệp giữa các tiến trình trong hệ thống phân tán hoặc đa xử lý song song.

- **Tính năng chính:**
  - Hỗ trợ giao tiếp giữa các tiến trình chạy trên nhiều máy, nhiều node với nhau.
  - Cho phép chia nhỏ công việc thành các tiến trình (process) chạy song song.
  - Quản lý việc gửi và nhận dữ liệu giữa các tiến trình một cách hiệu quả.
  - Hỗ trợ đa nền tảng: Linux, Windows, macOS.
  - Dễ dàng tích hợp với nhiều ngôn ngữ như C, C++, Fortran, Python (thông qua mpi4py).

### 2. Các hàm phổ biến trong OpenMPI

| Hàm               | Mục đích                                               |
|-------------------|-------------------------------------------------------|
| `MPI_Init`        | Khởi tạo môi trường MPI, phải gọi đầu tiên            |
| `MPI_Comm_size`   | Lấy số lượng tiến trình trong communicator            |
| `MPI_Comm_rank`   | Lấy chỉ số (rank) của tiến trình hiện tại              |
| `MPI_Send`        | Gửi dữ liệu tới tiến trình khác                        |
| `MPI_Recv`        | Nhận dữ liệu từ tiến trình khác                        |
| `MPI_Bcast`       | Phát dữ liệu từ tiến trình gốc tới tất cả tiến trình   |
| `MPI_Reduce`      | Thu thập dữ liệu từ các tiến trình và thực hiện phép toán (sum, max...) |
| `MPI_Barrier`     | Đồng bộ tất cả các tiến trình, chờ mọi tiến trình đến điểm này |
| `MPI_Finalize`    | Kết thúc môi trường MPI, phải gọi cuối cùng            |

### 3. Giải pháp sử dụng OpenMPI cho bài toán tính 10,000,000 số nguyên tố đầu tiên

#### Bài toán:
Tính các số nguyên tố đầu tiên, ví dụ dùng thuật toán Sàng Eratosthenes, phân tán trên 16 máy, mỗi máy 2 core (tổng 32 core).

#### Mục tiêu:
- Tận dụng được tối đa số core hiện có.
- Đảm bảo kết quả chính xác, không trùng lặp.
- Linh hoạt thay đổi số core (8, 10, 12, 16, ...), vẫn cho kết quả đúng.

#### Ý tưởng phân tán:

1. **Phân chia khoảng tìm kiếm:**
   - Tổng số cần tìm: 10,000,000 số nguyên tố đầu tiên.
   - Tính toán giới hạn trên (n_max) của sàng Eratosthenes sao cho đủ để lấy 10 triệu số nguyên tố (có thể tham khảo bảng hoặc ước lượng sơ bộ).
   - Chia vùng từ 2 đến n_max thành 32 đoạn con, mỗi core xử lý một đoạn.

2. **Chạy song song:**
   - Mỗi tiến trình (process) chạy trên một core.
   - Tiến trình 0 có thể thực hiện sàng các số nhỏ đầu tiên (các số nguyên tố cơ bản) để dùng cho sàng tiếp theo.
   - Các tiến trình khác nhận các đoạn khác nhau và loại bỏ các bội số dựa trên số nguyên tố nhỏ đã có.

3. **Gộp kết quả:**
   - Sau khi sàng xong đoạn của mình, mỗi tiến trình trả về các số nguyên tố tìm được.
   - Tiến trình chủ (rank 0) thu thập kết quả từ các tiến trình khác (MPI_Gather hoặc MPI_Reduce tùy mục đích).
   - Kết hợp lại thành danh sách cuối cùng.
   - Cắt lấy đúng 10 triệu số nguyên tố đầu tiên.

4. **Linh hoạt số core:**
   - Số tiến trình khởi tạo có thể thay đổi (MPI_Comm_size lấy số core).
   - Việc chia đoạn tìm kiếm dựa vào số tiến trình này để đảm bảo không thừa hoặc thiếu vùng xử lý.
   - Thuật toán không phụ thuộc cứng vào số core, dễ mở rộng hoặc thu nhỏ.

#### Ưu điểm:
- Giảm thời gian chạy nhờ chia công việc đồng đều.
- Kết quả chính xác do quy trình tổng hợp cuối cùng.
- Linh hoạt cấu hình tài nguyên.

---

### Tham khảo thuật toán Sàng Eratosthenes

- Thuật toán loại bỏ các bội số của số nguyên tố bắt đầu từ 2.
- Để tìm `n` số nguyên tố đầu tiên, cần ước lượng phạm vi giới hạn để sàng (ví dụ, khoảng 179,424,673 để có 10 triệu số nguyên tố).
- Sàng song song giúp chia vùng số cần kiểm tra, giảm tải cho mỗi tiến trình.

---

### Tài liệu tham khảo

- [OpenMPI Official Site](https://www.open-mpi.org/)
- [MPI Tutorial](https://mpitutorial.com/)
- [Wikipedia Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)
