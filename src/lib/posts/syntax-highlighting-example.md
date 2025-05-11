---
title: "Trao đổi thông tin"
date: "2025-01-05"
updated: "2025-01-05"
categories:
  - "POST"
  - "RabbitMQ"
  - "JSON-RPC"
  - "Unbutu"
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




# Bài tập 2: Xây dựng hệ thống sử dụng dịch vụ RPC (JSON-RPC)

## 1. Mô tả

Hệ thống này sử dụng dịch vụ **JSON-RPC** để thực hiện các phép tính đơn giản như cộng, trừ, nhân, chia giữa hai số. Server được xây dựng bằng Node.js sử dụng thư viện `express` và `jsonrpc-lite`.

## 2. Cài đặt môi trường

```bash
npm init -y
npm install express body-parser jsonrpc-lite
```
## 3. Mã Nguồn
```bash
const express = require('express');
const bodyParser = require('body-parser');
const jsonrpc = require('jsonrpc-lite');

const app = express();
app.use(bodyParser.json());

app.post('/rpc', (req, res) => {
  const parsed = jsonrpc.parseObject(req.body);

  if (parsed.type === 'request') {
    const { method, params, id } = parsed.payload;

    let result;

    switch (method) {
      case 'add':
        result = params.a + params.b;
        break;
      case 'subtract':
        result = params.a - params.b;
        break;
      case 'multiply':
        result = params.a * params.b;
        break;
      case 'divide':
        if (params.b === 0) {
          return res.json(jsonrpc.error(id, jsonrpc.JsonRpcError.invalidParams("Không thể chia cho 0")));
        }
        result = params.a / params.b;
        break;
      default:
        return res.json(jsonrpc.error(id, jsonrpc.JsonRpcError.methodNotFound()));
    }

    return res.json(jsonrpc.success(id, result));
  }

  res.json(jsonrpc.error(null, jsonrpc.JsonRpcError.invalidRequest()));
});

app.listen(3000, () => {
  console.log('🟢 JSON-RPC server đang chạy tại http://localhost:3000/rpc');
});
```

## 4. Kiểm thử hệ thống
**Gửi yêu cầu bằng curl:**
```
curl -X POST http://localhost:3000/rpc \

  -H "Content-Type: application/json" \

  -d '{"jsonrpc":"2.0","method":"add","params":{"a":5,"b":3},"id":1}'
```
**Kết quả trả về:**
```
{
  "jsonrpc": "2.0",

  "result": 8,

  "id": 1
}

```
**Kết luận**
Hệ thống đã triển khai thành công một dịch vụ RPC sử dụng định dạng JSON-RPC. Người dùng có thể gửi yêu cầu từ xa đến server để thực hiện các phép toán mà không cần biết chi tiết bên trong.


# Bài tập 3: Tìm hiểu các thư viện RPC ngoài xmlrpc và demo một thư viện sử dụng JSON

## 1. Giới thiệu

RPC (Remote Procedure Call) là kỹ thuật cho phép chương trình gọi hàm từ xa trên một máy chủ thông qua giao thức mạng. Ngoài `xmlrpc`, còn nhiều thư viện RPC khác sử dụng các định dạng hiện đại như **JSON** hoặc **gRPC**.

Trong bài này, ta sẽ:
- Liệt kê các thư viện hỗ trợ RPC (đặc biệt với định dạng JSON).
- Phân tích thư viện `jsonrpc-lite` (Node.js).
- Xây dựng một demo nhỏ sử dụng thư viện này.

---

## 2. Một số thư viện RPC phổ biến ngoài `xmlrpc`

| Tên thư viện       | Ngôn ngữ      | Định dạng hỗ trợ | Ghi chú ngắn                                      |
|--------------------|---------------|------------------|--------------------------------------------------|
| `jsonrpc-lite`     | JavaScript    | JSON-RPC 2.0     | Nhẹ, dễ tích hợp với Express                     |
| `json-rpc`         | Python        | JSON-RPC 2.0     | Dễ dùng, hỗ trợ server và client                 |
| `grpc`             | Đa ngôn ngữ   | Protocol Buffers | Hiệu suất cao, phức tạp hơn, sử dụng .proto file|
| `msgpack-rpc`      | Python, C++   | MsgPack          | Hiệu quả cao, dùng trong các ứng dụng game hoặc IoT |
| `simplejsonrpc`    | Python        | JSON-RPC         | Tối giản, dễ triển khai                          |

---

## 3. Phân tích thư viện `jsonrpc-lite`

### a. Tổng quan

- **Tên**: `jsonrpc-lite`
- **NPM**: https://www.npmjs.com/package/jsonrpc-lite
- **Hỗ trợ chuẩn**: JSON-RPC 2.0
- **Cài đặt**: `npm install jsonrpc-lite`
- **Ưu điểm**:
  - Nhẹ, không phụ thuộc nặng.
  - Hỗ trợ tạo và phân tích các message JSON-RPC rất nhanh.
  - Dễ kết hợp với các framework như Express hoặc Fastify.

---

## 4. Demo hệ thống RPC sử dụng `jsonrpc-lite`

### 4.1 Cài đặt

```bash
npm init -y
npm install express body-parser jsonrpc-lite
```

### 4.2 Mã nguồn: 

** Thư viện sử dụng **
```
const express = require('express');
const bodyParser = require('body-parser');
const jsonrpc = require('jsonrpc-lite');
```
express: Web framework giúp tạo máy chủ HTTP nhanh chóng.

body-parser: Middleware giúp phân tích nội dung JSON từ các yêu cầu POST.

jsonrpc-lite: Thư viện giúp tạo và phân tích các gói JSON-RPC (phiên bản nhẹ, chuẩn 2.0).

📌 2. Cấu hình ứng dụng
```
const app = express();
app.use(bodyParser.json());
Tạo ứng dụng Express.
```
Dùng middleware bodyParser.json() để đọc nội dung JSON từ request body.

📌 3. API xử lý JSON-RPC
```
app.post('/rpc', (req, res) => {
  const parsed = jsonrpc.parseObject(req.body);
Endpoint /rpc nhận các request dạng POST.
```
parseObject() dùng để đọc và xác thực yêu cầu JSON-RPC gửi lên.

📌 4. Kiểm tra và xử lý method
```
if (parsed.type === 'request') {
  const { method, params, id } = parsed.payload;

  let result;

  switch (method) {
    case 'add':        // cộng
    case 'subtract':   // trừ
    case 'multiply':   // nhân
    case 'divide':     // chia
```
Kiểm tra xem đây có phải là yêu cầu (request) hợp lệ không.

Tùy vào giá trị method, thực hiện các phép tính toán tương ứng.

Các toán hạng truyền vào từ client nằm trong params.a và params.b.

📌 5. Xử lý lỗi chia cho 0
```
if (params.b === 0) {
  return res.json(jsonrpc.error(id, jsonrpc.JsonRpcError.invalidParams("Không thể chia cho 0")));
}
```
Nếu phép chia và mẫu số bằng 0, phản hồi lỗi với thông báo "Không thể chia cho 0".

📌 6. Phản hồi thành công hoặc lỗi
```
return res.json(jsonrpc.success(id, result));
```
Nếu thành công: trả kết quả với ID kèm theo.

Nếu method không hỗ trợ: trả lỗi methodNotFound.

Nếu yêu cầu không hợp lệ: trả lỗi invalidRequest.

📌 7. Khởi động server
```
app.listen(3000, () => {
  console.log('🟢 Server JSON-RPC chạy tại http://localhost:3000/rpc');
});
```
Máy chủ lắng nghe ở cổng 3000 và thông báo khi đã sẵn sàng.

✅ Tóm tắt
Đây là một máy chủ JSON-RPC 2.0 đơn giản, hỗ trợ 4 phép toán: cộng, trừ, nhân, chia.

Kiểm tra và xử lý lỗi cơ bản (chia cho 0, method không tồn tại, request sai định dạng).

Có thể tương tác từ client bằng cách gửi JSON qua POST tới /rpc.



### 4.3 Gửi yêu cầu thử nghiệm

#### Ví dụ với `curl`

```bash
curl -X POST http://localhost:3000/rpc \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"add","params":{"a":10,"b":5},"id":1}'
```

#### Kết quả:

```json
{
  "jsonrpc": "2.0",
  "result": 15,
  "id": 1
}
```

---

## 5. Kết luận

Qua bài này, ta đã:
- Tìm hiểu thêm về các thư viện RPC ngoài xmlrpc, đặc biệt là những thư viện hỗ trợ JSON như jsonrpc-lite, giúp hiểu rõ hơn về sự phát triển của các phương thức gọi thủ tục từ xa (RPC) trong môi trường web hiện đại.
- Phân tích cách hoạt động và tích hợp của jsonrpc-lite với Node.js, với ưu điểm là khả năng tích hợp dễ dàng và hiệu suất cao trong việc trao đổi dữ liệu qua JSON, một chuẩn phổ biến trong lập trình web.
- Xây dựng một demo RPC nhỏ với các phép toán cơ bản, giúp minh họa cách thức hoạt động của thư viện jsonrpc-lite và cách ứng dụng nó vào thực tế trong các dự án Node.js.
-Thư viện jsonrpc-lite phù hợp với các ứng dụng web nhẹ, đơn giản, dễ tích hợp và mở rộng trong các hệ thống hiện đại, đặc biệt là khi cần xử lý dữ liệu một cách nhanh chóng và hiệu quả mà không cần quá nhiều cấu hình phức tạp.

Nhìn chung, jsonrpc-lite là một công cụ tuyệt vời cho các ứng dụng cần giao tiếp qua RPC, đặc biệt trong các hệ thống yêu cầu nhẹ nhàng, dễ mở rộng và dễ bảo trì.

---

## 6. Tài liệu tham khảo

- [https://www.jsonrpc.org/specification](https://www.jsonrpc.org/specification)
- [https://www.npmjs.com/package/jsonrpc-lite](https://www.npmjs.com/package/jsonrpc-lite)
- [https://github.com/ethereum/json-rpc-spec](https://github.com/ethereum/json-rpc-spec)
