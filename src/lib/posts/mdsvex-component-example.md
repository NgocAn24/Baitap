---
title: "Tiến trình và luồng"
date: "2023-04-28"
updated: "2023-04-28"
categories:
  - "Phancung"
  - "thread"
  - "process"
  - "Chatgpt"
coverImage: "https://images.viblo.asia/0aac891f-f99f-47ae-94a9-c38135c72256.png"
coverWidth: 16
coverHeight: 9
excerpt: Bài tập môn Hệ thống phân tán – Tổng hợp kiến thức và thực hành
---

<script>
	import Callout from '$lib/components/Callout.svelte';
</script>

*Bài tập môn Hệ thống phân tán – Tổng hợp kiến thức và thực hành*

---

## 1. Đánh giá hiệu năng máy tính cá nhân

Dựa vào kiến thức bài học về tiến trình và luồng, em đã kiểm tra cấu hình của máy tính cá nhân như sau:
![Web và ứng dụng di động](https://st.quantrimang.com/photos/image/2024/11/22/kiem-tra-hieu-suat-may-tinh-1.png)


- **CPU**: AMD Ryzen 7700 – 8 nhân 12 luồng
- **GPU**: AMD RX6800 – 6GB GDDR6
- **RAM**: 16GB DDR4 – Bus 3200MHz

### Giải thích hiệu năng:
- **CPU nhiều luồng:** 
CPU (bộ xử lý trung tâm) có nhiều luồng (threads) đồng nghĩa với việc nó có thể xử lý nhiều tác vụ cùng lúc hiệu quả hơn. Điều này rất quan trọng khi chạy nhiều ứng dụng cùng lúc hoặc xử lý các tác vụ phức tạp như lập trình, dựng video, hoặc làm việc với máy ảo. Nhiều luồng sẽ giúp giảm độ trễ và tăng khả năng phản hồi của hệ thống.
- **GPU rời:** 
GPU rời (card đồ họa riêng biệt, không tích hợp trong CPU) cung cấp sức mạnh xử lý đồ họa cao hơn, rất hữu ích cho thiết kế đồ họa, chỉnh sửa video, chơi game hoặc dựng hình 3D. Ngoài ra, GPU cũng hỗ trợ tăng tốc tính toán cho các ứng dụng trí tuệ nhân tạo (AI) và học máy (Machine Learning – ML), đặc biệt là trong việc huấn luyện và chạy mô hình ở mức cơ bản đến trung cấp.
- **RAM 16GB:** 
Dung lượng RAM lớn (16GB) giúp hệ thống có đủ bộ nhớ để vận hành mượt mà nhiều ứng dụng cùng lúc, đặc biệt là các phần mềm nặng như trình IDE, trình duyệt với nhiều tab, công cụ đồ họa, và phần mềm máy ảo (như VirtualBox, Docker). RAM lớn giúp giảm tình trạng chậm do hệ thống phải sử dụng bộ nhớ ảo từ ổ đĩa.
---

## 2. Các bài toán phổ biến dùng đa luồng hoặc đa tiến trình

Trong thế giới phần mềm hiện đại, hiệu năng luôn là yếu tố then chốt. Để tối ưu hiệu suất, nhiều hệ thống sử dụng **đa luồng ** hoặc **đa tiến trình ** nhằm khai thác triệt để tài nguyên phần cứng, đặc biệt là CPU đa nhân. Nhưng nên dùng kỹ thuật nào cho bài toán nào?. Vậy ttrong bài viết này, chúng ta sẽ khám phá sự khác biệt giữa tiến trình và luồng một cách chi tiết.


### 📌 Đa Luồng và Đa Tiến Trình Là Gì?
![Web và ứng dụng di động](https://nguyentruonglong.net/images/MultithreadingvsMultiprocessing.png)
- **Đa luồng**: Nhiều luồng (threads) cùng chạy trong một tiến trình, chia sẻ bộ nhớ chung. Phù hợp với các tác vụ nhẹ, phụ thuộc lẫn nhau, hoặc cần tốc độ giao tiếp nội bộ nhanh.
- **Đa tiến trình**: Mỗi tiến trình là một không gian bộ nhớ riêng. Phù hợp với tác vụ nặng, độc lập, hoặc khi cần độ cách ly cao giữa các phần.

Hãy cùng điểm qua **12 bài toán phổ biến** thường áp dụng đa luồng hoặc đa tiến trình, cùng với ví dụ ứng dụng thực tế.


| STT | Bài toán / Ứng dụng                          | Dùng đa luồng / đa tiến trình | Ứng dụng                             |
|-----|----------------------------------------------|-------------------------------|---------------------------------------------|
| 1   | Web Server xử lý nhiều request       | Đa tiến trình / đa luồng      | Apache, Nginx                               |
| 2   | Game Engine                                  | Đa luồng                      | Unity, Unreal Engine                        |
| 3   | Phân tích dữ liệu lớn              | Đa tiến trình                 | Hadoop, Spark                               |
| 4   | Rendering video                              | Đa luồng                      | Adobe Premiere, Blender                     |
| 5   | Download nhiều file song song            | Đa luồng                      | Trình duyệt, IDM                            |
| 6   | AI/ML training model                         | Đa tiến trình                 | TensorFlow, PyTorch                         |
| 7   | Anti-virus scan                              | Đa tiến trình                 | Avast, Kaspersky                            |
| 8   | Chat server                                   | Đa luồng                      | WhatsApp server, Discord backend            |
| 9   | Compiler (biên dịch nhiều file)     | Đa tiến trình                 | GCC, Clang                                  |
| 10  | Simulation vật lý                        | Đa luồng                      | NASA, game vật lý                           |
| 11  | Database server                              | Đa tiến trình / đa luồng      | MySQL, PostgreSQL                           |
| 12  | Virtual Machine / Docker                     | Đa tiến trình                 | VMware, Docker                              |

---

## 3. Khi nào dùng Thread, khi nào dùng Process, khi nào dùng cả hai

Tiến trình và luồng là các thành phần cơ bản trong hệ điều hành. Tiến trình là một chương trình đang được thực thi trong khi luồng là một phần của tiến trình. Luồng cho phép một chương trình thực hiện nhiều tác vụ cùng lúc, như tải xuống tệp trong khi bạn duyệt trang web hoặc chạy hoạt ảnh trong khi xử lý đầu vào của người dùng. Một tiến trình có thể bao gồm nhiều luồng. Trong bài viết này, chúng ta sẽ khám phá sự khác biệt giữa tiến trình và luồng một cách chi tiết.

### Quy trình là gì?
Tiến trình về cơ bản là các chương trình được phân phối từ trạng thái sẵn sàng và được lên lịch trong CPU để thực thi. **PCB** (Khối điều khiển tiến trình) giữ ngữ cảnh của tiến trình. Một tiến trình có thể tạo ra các tiến trình khác được gọi là Tiến trình con. Tiến trình mất nhiều thời gian hơn để kết thúc và nó bị cô lập có nghĩa là nó không chia sẻ bộ nhớ với bất kỳ tiến trình nào khác. Tiến trình có thể có các trạng thái sau: mới, sẵn sàng, đang chạy, đang chờ, đã kết thúc và bị treo.

Đọc thêm về Giới thiệu về Quản lý quy trình.

### Thread là gì?
Luồng thường được gọi là "quy trình nhẹ" vì chúng chia sẻ một số tính năng của quy trình nhưng nhỏ hơn và nhanh hơn. Mỗi luồng luôn là một phần của một quy trình cụ thể. Một luồng có ba trạng thái: Đang chạy, Sẵn sàng và Bị chặn.

Một luồng mất ít thời gian hơn để kết thúc so với một tiến trình nhưng không giống như tiến trình, các luồng không cô lập.

### Ví dụ:
Hãy tưởng tượng một trình xử lý văn bản hoạt động với hai tác vụ riêng biệt diễn ra cùng lúc. Một tác vụ tập trung vào việc tương tác với người dùng, như phản hồi khi nhập hoặc cuộn, trong khi tác vụ kia hoạt động ở chế độ nền để điều chỉnh định dạng của toàn bộ tài liệu.

Ví dụ, nếu bạn xóa một câu trên trang 1, tác vụ tập trung vào người dùng sẽ ngay lập tức yêu cầu tác vụ nền định dạng lại toàn bộ cuốn sách. Trong khi tác vụ nền đang bận định dạng lại, tác vụ tập trung vào người dùng vẫn tiếp tục xử lý các hành động đơn giản như cho phép bạn cuộn qua trang 1 hoặc nhấp vào các mục.

Khi bạn sử dụng trình duyệt web, các luồng sẽ hoạt động ẩn để xử lý nhiều tác vụ khác nhau cùng lúc.
Ví dụ:
- Một luồng đang tải nội dung trang web (văn bản, hình ảnh, video).
- Một luồng khác đang phản hồi các hành động của bạn như cuộn, nhấp hoặc nhập.
- Một luồng riêng biệt có thể đang chạy JavaScript để làm cho trang web tương tác.

Đa nhiệm này làm cho trình duyệt mượt mà và phản hồi nhanh. Ví dụ, bạn có thể cuộn qua một trang hoặc nhập vào thanh tìm kiếm trong khi phần còn lại của trang vẫn đang tải. Nếu luồng không được sử dụng, trình duyệt sẽ đóng băng và đợi một tác vụ hoàn tất trước khi bắt đầu tác vụ khác. Luồng đảm bảo mọi thứ diễn ra nhanh chóng và liền mạch.

<p align="right" width="80%">
    <img width="10%" src="https://media.geeksforgeeks.org/wp-content/uploads/20190522155604/Untitled-Diagram-361.png"> 
</p>

### Sự khác biệt giữa Process và Thread
Bảng dưới đây thể hiện sự khác biệt giữa tiến trình và luồng.

| Tiêu chí                     | Tiến Trình (Process)                                    | Luồng (Thread)                                    |
|------------------------------|---------------------------------------------------------|---------------------------------------------------|
| **Khái niệm**                | Tiến trình có nghĩa là một chương trình đang được thực thi. | Luồng có nghĩa là một phân đoạn của một quá trình. |
| **Thời gian kết thúc**        | Một tiến trình mất nhiều thời gian hơn để kết thúc.    | Một luồng mất ít thời gian hơn để kết thúc.       |
| **Thời gian tạo ra**          | Phải mất nhiều thời gian hơn để tạo ra.                | Mất ít thời gian hơn để tạo ra.                   |
| **Chuyển đổi ngữ cảnh**       | Việc chuyển đổi ngữ cảnh cũng mất nhiều thời gian hơn. | Chuyển đổi ngữ cảnh mất ít thời gian hơn.         |
| **Giao tiếp giữa các đơn vị**| Tiến trình kém hiệu quả hơn về mặt giao tiếp.           | Luồng có hiệu quả hơn về mặt giao tiếp.           |
| **Đa nhiệm**                  | Đa lập trình bao gồm các khái niệm về đa tiến trình.    | Chúng ta không cần nhiều chương trình hoạt động cho nhiều luồng vì một tiến trình duy nhất bao gồm nhiều luồng. |
| **Bộ nhớ**                    | Mỗi tiến trình chạy trong bộ nhớ riêng của nó.          | Các luồng chia sẻ bộ nhớ.                         |
| **Trọng lượng**               | Một tiến trình nặng hơn so với một luồng.              | Luồng rất nhẹ vì mỗi luồng trong một tiến trình đều chia sẻ mã, dữ liệu và tài nguyên. |
| **Chuyển đổi**                | Chuyển đổi quy trình sử dụng giao diện trong hệ điều hành. | Chuyển đổi luồng có thể không cần sự tham gia của hệ điều hành. |
| **Ảnh hưởng khi bị chặn**     | Nếu một tiến trình bị chặn thì nó sẽ không ảnh hưởng đến việc thực thi các tiến trình khác. | Nếu một luồng cấp người dùng bị chặn, thì tất cả các luồng cấp người dùng khác cũng bị chặn. |
| **Ngữ cảnh**                  | Mỗi tiến trình có Khối điều khiển tiến trình, Ngăn xếp và Không gian địa chỉ riêng. | Luồng có PCB cha, Khối điều khiển luồng riêng, Ngăn xếp và không gian Địa chỉ chung. |
| **Sự thay đổi**               | Những thay đổi ở tiến trình cha không ảnh hưởng tới tiến trình con. | Vì tất cả các luồng của cùng một tiến trình chia sẻ không gian địa chỉ và các tài nguyên khác nên bất kỳ thay đổi nào đối với luồng chính đều có thể ảnh hưởng đến hành vi của các luồng khác của tiến trình. |
| **Lệnh gọi hệ thống**         | Có một lệnh gọi hệ thống liên quan đến việc này.        | Không cần lệnh gọi hệ thống nào cả, nó được tạo ra bằng cách sử dụng API. |
| **Chia sẻ dữ liệu**           | Mỗi tiến trình không chia sẻ dữ liệu với nhau.          | Các luồng chia sẻ dữ liệu với nhau.               |

### Ưu điểm của quy trình
- Các quy trình hoạt động độc lập trong bộ nhớ riêng, đảm bảo không bị can thiệp và bảo mật tốt hơn.
- Các tài nguyên như CPU ​​và bộ nhớ được phân bổ hiệu quả để tối ưu hóa hiệu suất.
- Các quy trình có thể được ưu tiên để đảm bảo các nhiệm vụ quan trọng nhận được đủ nguồn lực cần thiết.

### Nhược điểm của quy trình
- Việc chuyển đổi thường xuyên giữa các quy trình có thể làm chậm hệ thống và giảm tốc độ.
- Quản lý tài nguyên không đúng cách có thể gây ra tình trạng bế tắc khi các quy trình ngừng hoạt động và cản trở tiến độ.
- Có quá nhiều tiến trình có thể khiến bảng tiến trình chiếm nhiều bộ nhớ. Điều này cũng có thể khiến việc tìm kiếm hoặc cập nhật bảng chậm hơn, có thể làm giảm hiệu suất hệ thống.

### Ưu điểm của luồng
- Khi có nhiều công việc tính toán và nhập/xuất (I/O), các luồng sẽ giúp các tác vụ chạy cùng lúc, giúp ứng dụng chạy nhanh hơn.
- Một lợi thế khác của việc sử dụng luồng là vì chúng nhẹ hơn tiến trình nên việc tạo và hủy chúng dễ dàng hơn (tức là nhanh hơn) so với tiến trình.
- Nhiều ứng dụng cần xử lý nhiều tác vụ khác nhau cùng lúc. Ví dụ, trình duyệt web có thể tải trang web, phát video và cho phép bạn cuộn tất cả cùng một lúc. Luồng giúp thực hiện điều này bằng cách chia các tác vụ này thành các phần nhỏ hơn có thể chạy cùng nhau.

### Nhược điểm của luồng
- Các luồng trong cùng một tiến trình không hoàn toàn độc lập như các tiến trình riêng biệt. Chúng chia sẻ cùng một không gian bộ nhớ bao gồm các biến toàn cục. Điều này có nghĩa là một luồng có thể vô tình thay đổi hoặc thậm chí xóa dữ liệu của luồng khác vì không có sự bảo vệ nào giữa chúng.
- Các luồng cũng chia sẻ tài nguyên như tệp. Ví dụ – nếu một luồng đóng tệp trong khi luồng khác vẫn đang sử dụng tệp đó, điều này có thể gây ra lỗi hoặc hành vi không mong muốn.
- Nếu tạo quá nhiều luồng, chúng có thể làm chậm hệ thống hoặc khiến hệ thống hết bộ nhớ.

### Phần kết luận
Từ thảo luận trên, chúng ta có thể kết luận rằng **tiến trình** là chương trình đang thực thi mất nhiều thời gian hơn để tạo cũng như kết thúc trong khi **luồng** là thành phần của tiến trình mất ít thời gian hơn để tạo cũng như kết thúc. Tiến trình có PCB trong khi luồng có PCB luồng riêng của chúng cùng với PCB của tiến trình cha.

## 4. ChatGPT training trên Distributed System
##### ChatGPT: bản chất ChatGPT hoạt động như thế nào?

Source: https://phanxuanphucnd.github.io/

ChatGPT là một Large Language Model (LLM) mới nhất của OpenAI và cho thấy được sự cải thiện đáng kể với mô hình tiền nhiệm của nó GPT-3. Tương tự như nhiều LLMs, ChatGPT có khả năng sinh văn bản (text) theo nhiều phong cách khác nhau và cho nhiều mục đích khác nhau, nhưng ChatGPT cho thấy được khả năng về độ chính xác, chi tiết và mạch lạc hơn rất đáng kể. ChatGPT đang là xu hướng, nó như đại diện cho thế hệ tiếp theo của LLMs, tập trung mạnh vào sự tương tác trong hội thoại (interative conversations).

OpenAI họ đã sử dụng kết hợp cả Supervised Learning (học giám sát) và Reinforment Learning (học tăng cường) để tinh chỉnh (fine-tune) ChatGPT,** nhưng Reinforcement Learning mới là thành phần làm cho ChatGPT trở nên độc đáo hơn tất cả**. Kỹ thuật cụ thể được gọi là** Reinforcement Learning from Human Feedback (RLHF)**, tức là học tăng cường từ những dữ liệu feedback của con người, sử dụng vòng lặp feedback của con người trong quá trình huấn luyện để giảm thiểu việc đưa ra các phản hồi có hại, không trung thực hay là sai lệch tri thức.

### Khái niệm Capability và Alignment
![Web và ứng dụng di động](https://images.viblo.asia/4ef9b71a-2f9d-41b7-a4ac-e63d8238ef90.png)
Đầu tiền chúng ta cần hiểu 2 khái niệm Capability và Alignment trong machine learning. Khái niệm Capability đề cập tới khả năng của một model để thực hiện một hoặc nhiều task cụ thể. Khả năng của một model cụ thể được đánh giá bởi khả năng tối ưu objective-function (hàm tối ưu/ hàm mục tiêu) của nó tốt như thế nào?

  Ví dụ như, một model được thiết kế để dự đoán giá của thị trường chứng khoán phải có một hàm mục tiêu nào đó đo được độ chính xác các dự đoán của model. Nếu model có khả năng dự đoán chính xác sự biến động giá chứng khoán theo thời gian, thì được xem là có khả năng capability cao cho task này.

Mặt khác, Alignment liên quan tới những gì mà chúng ta thực sự muốn model làm so với **những gì mà model đang được huấn luyện để làm**.

Các LLMs chẳng hạn như GPT3, LLaMA, BLOOM, PaLM, ... được huấn luyện trên một lượng rất lớn dữ liệu text từ internet và có khả năng sinh văn bản giống như con người, nhưng không phải lúc nào LLMs cũng sinh ra được đầu ra như kỳ vọng của con người hoặc có tính chính xác. Trong thực tế, hàm mục tiêu của chúng là phân phối xác suất trên các chuỗi từ (word sequences) hoặc trên chuỗi token (token sequences) mà cho phép dự đoán chính xác từ tiếp theo trong chuỗi
#### Capability (Năng lực)
- Khả năng của một mô hình thực hiện một hoặc nhiều tác vụ cụ thể.
- Được đánh giá dựa trên việc mô hình tối ưu **hàm mục tiêu (objective function)** như thế nào.
- **Ví dụ:** Dự đoán giá chứng khoán chính xác -> capability cao trong tác vụ đó.

#### Alignment (Căn chỉnh)
- Mức độ phù hợp giữa **hành vi của mô hình** và **ý định thực tế của con người**.
- Câu hỏi đặt ra: *"Hàm mục tiêu mà mô hình được huấn luyện có thực sự phản ánh điều con người muốn không?"*
- **Ví dụ sai lệch (misalignment):**
  - Dùng log loss để huấn luyện model phân loại chim, nhưng người dùng lại muốn accuracy cao.
  - Mặc dù log loss thấp → model có capability cao, nhưng accuracy thấp → alignment kém.

---

### Hệ thống phân tán trong huấn luyện

Huấn luyện một mô hình ngôn ngữ lớn như ChatGPT (dựa trên GPT-3, GPT-4) đòi hỏi sử dụng hàng ngàn GPU hoạt động đồng thời. Vì vậy, OpenAI và các tổ chức phát triển LLM phải áp dụng nhiều chiến lược **phân tán (distributed)** để tăng hiệu quả, đảm bảo khả năng mở rộng và sử dụng tài nguyên tối ưu.

##### a. Song song hóa dữ liệu (Data Parallelism)

- Mỗi bản sao của mô hình được đặt trên một GPU khác nhau.
- Tập dữ liệu huấn luyện được chia nhỏ thành các batch, mỗi GPU nhận một batch khác nhau.
- Mỗi GPU thực hiện lan truyền xuôi (forward pass) và lan truyền ngược (backward pass) trên batch của nó.
- Sau khi tính gradient, các GPU đồng bộ hóa trọng số với nhau (thường qua **AllReduce**).

> 📌 Ví dụ: Nếu có 8 GPU, mỗi GPU sẽ xử lý 1/8 tập dữ liệu trong một vòng lặp, sau đó tổng hợp kết quả lại để cập nhật mô hình.

##### b. Song song hóa mô hình (Model Parallelism)

- Mô hình quá lớn để chứa trên một GPU đơn lẻ → chia các lớp của mô hình hoặc từng phần của layer cho nhiều GPU khác nhau.
- Các GPU trao đổi dữ liệu trung gian (activations) khi lan truyền xuôi hoặc ngược.

> 📌 Ví dụ: Layer 1 → GPU A, Layer 2 → GPU B, Layer 3 → GPU C…

##### c. Pipeline Parallelism (Dòng xử lý song song)

- Mô hình được chia thành các đoạn (stage), mỗi đoạn chạy trên một GPU.
- Các mini-batch được xếp theo pipeline (ống chuyền) → GPU nào cũng luôn bận rộn, không bị chờ đợi quá nhiều.

> 📌 Ưu điểm: Giảm độ trễ (latency) so với Model Parallelism thuần túy.

##### d. Huấn luyện kết hợp (Hybrid Parallelism)

- Kết hợp **Data + Model + Pipeline Parallelism** để tận dụng tối đa tài nguyên.
- Được sử dụng trong các mô hình khổng lồ như GPT-3 (175B parameters) hoặc GPT-4.

> 📌 Ví dụ: Tập dữ liệu chia theo Data Parallelism, mô hình chia theo Model Parallelism, và các tầng được xử lý theo pipeline.

##### e. Zero Redundancy Optimizer (ZeRO)

- Được giới thiệu bởi Microsoft trong DeepSpeed.
- Phân tán thông tin về optimizer, gradients và trạng thái mô hình → giảm mức sử dụng bộ nhớ trên mỗi GPU → có thể huấn luyện mô hình lớn hơn nhiều lần.

> 📌 GPT-3 có thể không dùng ZeRO, nhưng GPT-4 hoặc các mô hình hiện đại sau này đều tận dụng kỹ thuật này.

---

### 3. Tối ưu hóa hiệu suất huấn luyện

Huấn luyện các mô hình ngôn ngữ lớn (LLMs) như GPT-3, GPT-4, LLaMA, PaLM,… đòi hỏi rất nhiều tài nguyên và thời gian. Vì vậy, việc **tối ưu hiệu suất huấn luyện** là cực kỳ quan trọng để giảm chi phí, tăng tốc độ và đạt hiệu quả tối đa từ phần cứng. Các chiến lược dưới đây thường được sử dụng:

---

##### a. Mixed Precision Training (Huấn luyện với độ chính xác hỗn hợp)

- Sử dụng **FP16 (half-precision)** hoặc **bfloat16** thay cho **FP32 (single-precision)**.
- Ưu điểm:
  - Giảm bộ nhớ GPU tiêu thụ.
  - Tăng tốc độ xử lý vì phần cứng như NVIDIA A100 có hỗ trợ phần cứng FP16.
  - Cho phép huấn luyện mô hình lớn hơn trên cùng GPU.

> 📌 Thư viện hỗ trợ: `NVIDIA Apex`, `DeepSpeed`, `Transformers` của Hugging Face (`Trainer` hỗ trợ `fp16=True`)

---

##### b. Gradient Accumulation

- Khi batch size lớn không vừa vào bộ nhớ GPU, chia thành nhiều mini-batch nhỏ và tích lũy gradient qua nhiều vòng rồi mới cập nhật trọng số.
- Ưu điểm:
  - Giữ được hiệu quả của batch size lớn.
  - Tối ưu hiệu quả sử dụng GPU hạn chế bộ nhớ.

---

##### c. Gradient Checkpointing

- Thay vì lưu toàn bộ intermediate activations trong quá trình forward, chỉ lưu một phần và tính toán lại khi cần trong backward.
- Ưu điểm:
  - Giảm lượng bộ nhớ dùng để lưu trữ activations.
  - Cho phép huấn luyện mô hình sâu hơn trên GPU hạn chế bộ nhớ.

---

##### d. Efficient Optimizers (Tối ưu hóa hiệu quả)

- Dùng các optimizer nhẹ và tối ưu bộ nhớ:
  - **AdamW**, **8-bit Adam** (from `bitsandbytes`)
  - **ZeRO Optimizer** từ DeepSpeed (đã trình bày ở phần trước)

---

##### e. Caching & Prefetching

- Tải trước và lưu cache dữ liệu huấn luyện vào bộ nhớ để tránh tình trạng chờ I/O từ ổ cứng.
- Dùng **dataloader.prefetch_factor**, **num_workers > 0** để tăng tốc độ cung cấp dữ liệu.

---

##### f. Distributed Training Optimization

- Sử dụng thư viện hỗ trợ huấn luyện phân tán hiệu quả:
  - `DeepSpeed`
  - `FairScale`
  - `FSDP (Fully Sharded Data Parallel)` từ PyTorch
  - `Megatron-LM`
  - `Accelerate` từ Hugging Face

---

##### g. Dynamic Batching

- Gộp các input có độ dài gần nhau để tránh lãng phí tài nguyên do padding.
- Dùng `BucketIterator`, `Dynamic Padding`, hoặc tokenizer của Hugging Face với `pad_to_multiple_of`.

---

##### h. Profiling & Monitoring

- Dùng các công cụ như:
  - **NVIDIA Nsight Systems / Nsight Compute**
  - **TensorBoard**
  - **WandB**, **Weights & Biases**
  - **PyTorch Profiler**
- Mục đích: theo dõi bộ nhớ, tốc độ

### Tài liệu tham khảo

- [Efficient Training for Transformers – Hugging Face](https://huggingface.co/course/chapter6)
- [Mixed Precision Training – NVIDIA](https://docs.nvidia.com/deeplearning/performance/mixed-precision-training/index.html)
- [Gradient Checkpointing in PyTorch](https://pytorch.org/docs/stable/checkpoint.html)
- [Accelerate by Hugging Face](https://huggingface.co/docs/accelerate/index)
