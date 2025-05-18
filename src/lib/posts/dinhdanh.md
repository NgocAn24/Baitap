---
title: "Bai 4 _ Định danh "
date: "2025-10-26"
updated: "2025-10-26"
categories:
  - "ungdungphantan"
  - "dinhdanh"
  - "thuattoan"
coverImage: "https://www.matbao.ws/wp-content/uploads/blogs/2022/02/image3-10.jpg"
coverWidth: 16
coverHeight: 9
excerpt: cre Nhom 20.
---

# Quy trình duyệt web và Thuật toán Chord: Giải thích kỹ thuật chi tiết

## 1. Quy trình duyệt một trang web 

Khi bạn nhập một địa chỉ trang web như `www.xxx.com` vào trình duyệt, nhiều bước kỹ thuật phức tạp diễn ra phía sau để trình duyệt có thể tải và hiển thị trang web này. Dưới đây là tóm tắt các bước chính liên quan đến việc tra cứu DNS, resolving DNS và caching.

### 1.1 Tra cứu DNS (DNS Lookup)

Trình duyệt cần tìm địa chỉ IP thực sự của máy chủ lưu trang web. Ban đầu, nó sẽ kiểm tra bộ nhớ cache DNS trên máy tính của bạn để xem đã có địa chỉ IP tương ứng với tên miền này chưa. Nếu có, nó sẽ dùng luôn mà không phải yêu cầu thêm.

Nếu chưa, trình duyệt sẽ gửi yêu cầu DNS đến máy chủ DNS của nhà cung cấp dịch vụ Internet (ISP) hoặc những máy chủ DNS mà máy tính được cấu hình để sử dụng.

### 1.2 Resolving DNS

Quá trình resolving bắt đầu khi máy chủ DNS nhận truy vấn. Nếu máy chủ này không biết IP của tên miền, nó sẽ hỏi đến các máy chủ DNS cấp cao hơn theo thứ tự:

- Máy chủ DNS gốc (root DNS server)
- Máy chủ DNS cho miền cấp cao (TLD server, ví dụ `.com`)
- Máy chủ DNS chịu trách nhiệm cho tên miền cụ thể (authoritative DNS server)

Khi đến máy chủ DNS chịu trách nhiệm cuối cùng, nó sẽ trả về địa chỉ IP chính xác của `www.xxx.com`.

### 1.3 Caching

Kết quả địa chỉ IP trả về được lưu trong cache của hệ thống và trình duyệt để giảm thời gian tra cứu khi truy cập lần sau. Máy chủ DNS cũng lưu cache để tối ưu các truy vấn tiếp theo từ nhiều người dùng khác nhau.

### Tài liệu tham khảo:

> "DNS and BIND" by Cricket Liu and Paul Albitz  
> [How DNS Works - HowStuffWorks](https://computer.howstuffworks.com/dns.htm)  

---

## 2. Thuật toán Chord trong mạng phân tán P2P

Chord là một thuật toán được sử dụng để phân phối và tìm kiếm dữ liệu trong các mạng phân tán ngang hàng (P2P). Nó tổ chức các nút mạng thành một vòng ID và dùng hàm băm để định vị tài nguyên một cách nhanh chóng và hiệu quả.

### 2.1 Mô tả thuật toán cơ bản

Các nút trong mạng được gán các ID dựa trên giá trị băm (ví dụ: SHA-1), được sắp xếp theo vòng tròn (chẳng hạn 0 đến 2^160 - 1). Mỗi tài nguyên (ví dụ, khóa/dữ liệu) cũng được ánh xạ bằng hàm băm sang một giá trị ID. Để lưu hoặc truy xuất dữ liệu, thuật toán tìm nút “successor” trong vòng có ID lớn nhất không nhỏ hơn ID của tài nguyên.

### 2.2 Ví dụ code Python minh họa thuật toán Chord

```python
import hashlib

class ChordNode:
    def __init__(self, id):
        self.id = id
        self.successor = None
        self.predecessor = None
        self.data = {}

    def find_successor(self, id):
        if self.successor and self.predecessor:
            if self.id < id <= self.successor.id:
                return self.successor
            else:
                return self.successor.find_successor(id)
        return self

    def join(self, node):
        if node:
            self.successor = node.find_successor(self.id)
            self.successor.predecessor = self
        else:
            self.successor = self
            self.predecessor = self

    def store(self, key, value):
        node_id = self.hash_key(key)
        successor = self.find_successor(node_id)
        successor.data[key] = value

    @staticmethod
    def hash_key(key):
        return int(hashlib.sha1(key.encode()).hexdigest(), 16) % 2**160

def test_chord():
    node1 = ChordNode(1)
    node2 = ChordNode(2)
    node3 = ChordNode(3)

    node1.join(None)  
    node2.join(node1)  
    node3.join(node1) 

    node1.store("key1", "value1")
    node2.store("key2", "value2")
    node3.store("key3", "value3")

    assert node1.data["key1"] == "value1"
    assert node2.data["key2"] == "value2"
    assert node3.data["key3"] == "value3"

    print("Tất cả các test case đã thành công!")

if __name__ == "__main__":
    test_chord()
```

#### Kết quả chạy

##### Giải thích kết quả
Khởi tạo các nút: Ba nút node1, node2, và node3 được tạo ra với các ID khác nhau (1, 2, 3).

**Tham gia mạng:**

- node1 tham gia mạng đầu tiên (không có nút nào khác).

- node2 và node3 tham gia qua node1, do đó node1 trở thành nút successor cho cả hai.

**Lưu trữ dữ liệu:**

- node1 lưu trữ key1 với giá trị value1.

- node2 lưu trữ key2 với giá trị value2.

- node3 lưu trữ key3 với giá trị value3.

**Kiểm tra dữ liệu:**

Các assert kiểm tra xem dữ liệu đã được lưu trữ đúng tại các nút tương ứng hay không. Nếu tất cả các assert đều đúng, chương trình sẽ in ra thông báo thành công.