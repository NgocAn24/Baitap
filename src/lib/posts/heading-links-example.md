---
title: "Hệ thống phân tán là gì?"
date: "2025-10-26"
updated: "2025-10-26"
categories:
  - "sveltekit"
  - "markdown"
coverImage: "/images/jefferson-santos-fCEJGBzAkrU-unsplash.jpg"
coverWidth: 16
coverHeight: 9
excerpt: cre Nguyen Ngoc An.
---
# Hệ thống phân tán là gì?

Hệ thống phân tán là một tập hợp các máy tính độc lập, phối hợp với nhau để thực hiện một nhiệm vụ chung. Chúng chia sẻ tài nguyên và giao tiếp qua mạng, nhằm tăng cường hiệu suất, độ tin cậy và khả năng mở rộng.

# Các ứng dụng của hệ thống phân tán

- **Web và ứng dụng di động**: Các dịch vụ như Google, Facebook, và Netflix sử dụng hệ thống phân tán để phục vụ hàng triệu người dùng đồng thời.
- **Điện toán đám mây**: AWS, Google Cloud, và Azure cung cấp hạ tầng phân tán cho các ứng dụng doanh nghiệp.
- **Blockchain**: Các mạng như Bitcoin và Ethereum là ví dụ điển hình của hệ thống phân tán phi tập trung.

# Các khái niệm chính của hệ thống phân tán

## Scalability (Khả năng mở rộng)

Scalability là khả năng của hệ thống để mở rộng và xử lý tăng trưởng về người dùng hoặc dữ liệu bằng cách thêm tài nguyên.

- **Vertical Scaling**: Tăng cường phần cứng của một máy chủ.
- **Horizontal Scaling**: Thêm nhiều máy chủ vào hệ thống.

## Fault Tolerance (Khả năng chịu lỗi)

Fault tolerance cho phép hệ thống tiếp tục hoạt động ngay cả khi một phần của nó gặp sự cố. Khi một máy chủ gặp lỗi, các máy chủ khác vẫn tiếp tục hoạt động bình thường, giúp giảm thiểu downtime.

## Availability (Khả năng sẵn sàng)

Availability đảm bảo rằng hệ thống luôn sẵn sàng và có thể truy cập được. Đây là một trong những yếu tố quan trọng trong các dịch vụ web và ứng dụng di động.

## Transparency (Tính minh bạch)

Transparency là khả năng của hệ thống khiến cho người dùng và ứng dụng không cần biết về các chi tiết nội bộ của hệ thống. Điều này giúp giảm sự phức tạp trong việc sử dụng và bảo trì hệ thống.

## Concurrency (Đồng thời)

Concurrency đề cập đến khả năng của hệ thống thực hiện nhiều tác vụ cùng lúc mà không gây xung đột. Điều này rất quan trọng trong các hệ thống có nhiều người dùng đồng thời.

## Parallelism (Tính song song)

Parallelism là khả năng của hệ thống thực hiện nhiều tác vụ đồng thời để tăng tốc độ xử lý. Hệ thống có thể chia nhỏ tác vụ và chạy chúng trên nhiều máy chủ hoặc nhiều lõi CPU.

## Openness (Tính mở)

Openness đề cập đến khả năng của hệ thống trong việc tích hợp và tương tác với các hệ thống khác. Điều này giúp tăng cường tính linh hoạt và khả năng mở rộng của hệ thống.

## Load Balancer (Cân bằng tải)

Load balancer là một phần của hệ thống phân tán, có nhiệm vụ phân phối đều lưu lượng truy cập đến các máy chủ để tối ưu hóa hiệu suất và tránh tình trạng quá tải một máy chủ duy nhất.

## Replication (Nhân bản)

Replication là việc sao chép dữ liệu từ một máy chủ sang các máy chủ khác. Điều này giúp tăng cường độ tin cậy và khả năng phục hồi của hệ thống. Nếu một máy chủ gặp sự cố, dữ liệu vẫn có thể được phục hồi từ các máy chủ khác.

# Ví dụ và giải thích các thuật ngữ

Giả sử bạn xây dựng một ứng dụng web cho phép người dùng đăng nhập và chia sẻ bài viết.

- **Scalability**: Khi số lượng người dùng tăng, bạn có thể thêm máy chủ để xử lý lưu lượng tăng lên.
- **Fault Tolerance**: Nếu một máy chủ gặp sự cố, các máy chủ khác vẫn tiếp tục phục vụ người dùng.
- **Availability**: Dịch vụ luôn sẵn sàng, người dùng có thể truy cập bất cứ lúc nào.
- **Transparency**: Người dùng không cần biết hệ thống backend hoạt động như thế nào.
- **Concurrency**: Nhiều người dùng có thể đăng nhập và chia sẻ bài viết cùng lúc mà không gây xung đột.
- **Parallelism**: Hệ thống xử lý nhiều yêu cầu đồng thời để tăng tốc độ.
- **Openness**: Hệ thống có thể tích hợp với các dịch vụ bên ngoài như Google Auth hoặc Facebook Login.
- **Load Balancer**: Phân phối yêu cầu người dùng đến các máy chủ khác nhau để tối ưu hóa hiệu suất.
- **Replication**: Dữ liệu người dùng được sao chép trên nhiều máy chủ để đảm bảo an toàn và khả năng phục hồi.

# Kiến trúc của hệ thống phân tán

Các mô hình kiến trúc phổ biến bao gồm:

- **Client-Server**: Máy khách gửi yêu cầu đến máy chủ, máy chủ xử lý và trả về kết quả.
- **Peer-to-Peer (P2P)**: Mỗi nút trong mạng có thể vừa là máy khách vừa là máy chủ, chia sẻ tài nguyên với nhau.
- **Microservices**: Ứng dụng được chia thành các dịch vụ nhỏ, độc lập, mỗi dịch vụ thực hiện một chức năng cụ thể.

# Tham khảo

- [GeeksforGeeks](https://www.geeksforgeeks.org/what-is-a-distributed-system/)
- [Investopedia](https://www.investopedia.com/terms/d/distributed-applications-apps.asp)
