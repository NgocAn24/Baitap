---
title: "Syntax highlighting with mdsvex"
date: "2023-01-05"
updated: "2023-01-05"
categories:
  - "sveltekit"
  - "web"
  - "css"
  - "markdown"
coverImage: "/images/linus-nylund-Q5QspluNZmM-unsplash.jpg"
coverWidth: 16
coverHeight: 9
excerpt: This post shows you how syntax highlighting works here.
---


# BÀI TẬP 1  
**Chủ đề:** Tìm hiểu cơ chế, chức năng và cài đặt RabbitMQ – Một hệ thống truyền thông điệp phổ biến.

---

## 1. Giới thiệu tổng quan về RabbitMQ

![Điện toán đám mây](https://lacoski.github.io/media/rabbitmq.png)

RabbitMQ là một phần mềm **message broker** mã nguồn mở sử dụng giao thức **AMQP (Advanced Message Queuing Protocol)**. Nó cho phép các ứng dụng giao tiếp với nhau thông qua việc gửi và nhận thông điệp, giúp tách rời các thành phần của hệ thống và tăng tính mở rộng, chịu lỗi.

---

## 2. Cơ chế hoạt động


### 2.1 Kiến trúc tổng thể
![Điện toán đám mây](https://images.viblo.asia/a1571d98-cb4e-4f3a-9757-117a492be32c.png)
RabbitMQ sử dụng mô hình **Producer – Broker – Consumer**:

- **Producer**: Gửi thông điệp đến RabbitMQ.
- **Broker**: RabbitMQ server tiếp nhận và phân phối thông điệp.
- **Consumer**: Nhận và xử lý thông điệp.

Thông điệp sẽ đi qua **Exchange**, sau đó được định tuyến đến một hoặc nhiều **Queue**.

### 2.2 Các thành phần chính

- **Exchange**: Định tuyến thông điệp đến các queue dựa trên type (direct, topic, fanout, headers).
- **Queue**: Hàng đợi chứa thông điệp, nơi consumer có thể lấy ra để xử lý.
- **Binding**: Quy tắc liên kết giữa Exchange và Queue.

### 2.3 Các loại Exchange

- **Direct Exchange**: Gửi thông điệp đến queue có binding key khớp với routing key.
- **Fanout Exchange**: Gửi thông điệp đến tất cả các queue đã bind.
- **Topic Exchange**: Dựa theo pattern để định tuyến thông điệp.
- **Headers Exchange**: Dựa trên headers thay vì routing key.

---

## 3. Chức năng chính

- Truyền thông điệp phi đồng bộ giữa các ứng dụng.
- Đảm bảo **độ tin cậy** thông qua cơ chế **acknowledgement** và **persistence**.
- **Load balancing**: nhiều consumer có thể chia sẻ xử lý thông điệp trong một queue.
- **Routing nâng cao** với Exchange và Binding.
- **Plugins** hỗ trợ các tính năng như quản lý qua giao diện web, thống kê, xác thực.

---

## 4. Cài đặt RabbitMQ

### 4.1 Yêu cầu hệ thống

- Hệ điều hành: Windows, macOS, Linux
- Yêu cầu: Erlang (RabbitMQ phụ thuộc vào Erlang)

### 4.2 Hướng dẫn cài đặt trên Ubuntu

```bash
# Cài đặt Erlang
sudo apt-get update
sudo apt-get install erlang -y

# Cài đặt RabbitMQ
sudo apt-get install rabbitmq-server -y

# Bật dịch vụ
sudo systemctl enable rabbitmq-server
sudo systemctl start rabbitmq-server

# Bật giao diện quản lý Web
sudo rabbitmq-plugins enable rabbitmq_management


```
Truy cập giao diện web: http://localhost:15672
Tài khoản mặc định:

Username: guest

Password: guest

## 5. Ví dụ sử dụng RabbitMQ bằng Python
### 5.1 Gửi thông điệp (Producer)

```
import pika

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()
channel.queue_declare(queue='hello')

channel.basic_publish(exchange='', routing_key='hello', body='Hello RabbitMQ!')
print(" [x] Sent 'Hello RabbitMQ!'")

connection.close()
```
### 5.2 Nhận thông điệp (Consumer)

```
import pika

# Kết nối tới RabbitMQ server
connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# Đảm bảo queue 'hello' tồn tại
channel.queue_declare(queue='hello')

# Hàm callback để xử lý thông điệp nhận được
def callback(ch, method, properties, body):
    print(" [x] Received %r" % body)

# Đăng ký consumer lắng nghe queue 'hello'
channel.basic_consume(queue='hello', on_message_callback=callback, auto_ack=True)

print(' [*] Waiting for messages. To exit press CTRL+C')
channel.start_consuming()
```

📌 Hai đoạn mã này minh họa cho cơ chế gửi và nhận thông điệp cơ bản với RabbitMQ. Chạy file producer trước để gửi thông điệp, sau đó chạy file consumer để nhận và in ra thông điệp đó.

## 6. Ưu điểm và Nhược điểm của RabbitMQ
** ✅ Ưu điểm **

Dễ cài đặt và cấu hình
RabbitMQ có thể được thiết lập nhanh chóng trên nhiều hệ điều hành, đặc biệt là Linux.

Hỗ trợ giao diện quản lý Web
RabbitMQ cung cấp plugin rabbitmq_management cho phép giám sát và quản trị thông điệp, queues, exchanges trực quan.

Hỗ trợ đa ngôn ngữ
RabbitMQ có client libraries cho nhiều ngôn ngữ như Python, Java, .NET, Go, Ruby,...

Routing linh hoạt
Với các loại exchange như direct, topic, fanout, headers, RabbitMQ cho phép định tuyến thông điệp theo nhu cầu rất cụ thể.

Đảm bảo độ tin cậy cao
Thông qua cơ chế xác nhận (ack), lưu trữ bền vững (message persistence) và retry khi thất bại.

Hệ sinh thái plugin phong phú
Bao gồm xác thực nâng cao, thống kê, giám sát và hỗ trợ giao thức khác như MQTT, STOMP.

** ❌ Nhược điểm  **

Tốc độ không cao bằng Kafka
RabbitMQ được tối ưu cho độ tin cậy hơn là thông lượng cực lớn. Trong các hệ thống Big Data, Kafka thường là lựa chọn ưu tiên.

Phụ thuộc vào Erlang
Việc RabbitMQ chạy trên nền Erlang có thể gây khó khăn trong việc cấu hình sâu hoặc debug lỗi liên quan đến runtime.

Cấu hình nâng cao phức tạp
Dù đơn giản với cấu hình mặc định, khi dùng các tính năng như cluster, federation, hoặc phân mảnh thì khá phức tạp.

Yêu cầu xử lý message theo thứ tự cao hơn
Không giống như Kafka, RabbitMQ không đảm bảo thứ tự tuyệt đối trong tất cả các tình huống, đặc biệt khi có nhiều consumer song song.

## 7. Kết luận
RabbitMQ là một hệ thống hàng đợi thông điệp (message broker) phổ biến, đóng vai trò quan trọng trong kiến trúc hệ thống phân tán hiện đại. Với khả năng định tuyến linh hoạt, hỗ trợ nhiều giao thức và ngôn ngữ lập trình, cùng giao diện quản trị trực quan, RabbitMQ giúp các dịch vụ giao tiếp hiệu quả mà không cần phụ thuộc trực tiếp vào nhau.

Việc tìm hiểu cơ chế hoạt động, chức năng, cũng như cách cài đặt và sử dụng RabbitMQ không chỉ giúp sinh viên nắm bắt được nguyên lý vận hành của các hệ thống truyền thông điệp, mà còn trang bị nền tảng kiến thức quan trọng để xây dựng các hệ thống microservices, event-driven hay ứng dụng realtime.

RabbitMQ phù hợp cho những bài toán cần độ tin cậy cao, linh hoạt trong xử lý thông điệp, và là một lựa chọn vững chắc trong số các hệ thống message queue hiện nay.

-----

