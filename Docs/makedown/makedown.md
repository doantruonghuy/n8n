Markdown là gì?
Markdown là một ngôn ngữ đánh dấu đơn giản, sử dụng các ký hiệu dễ đọc để định dạng văn bản thô thành HTML (hoặc các định dạng khác). Mục tiêu của Markdown là cho phép bạn viết bằng một định dạng dễ đọc và dễ viết, sau đó chuyển đổi nó sang HTML. Nó được sử dụng rộng rãi trên GitHub, các trình soạn thảo tài liệu, diễn đàn, và nhiều nền tảng khác.

Các cú pháp Markdown cơ bản
Dưới đây là những cú pháp Markdown phổ biến nhất mà bạn sẽ thường xuyên sử dụng:

1. Tiêu đề (Headings)
Sử dụng dấu thăng (#) để tạo tiêu đề. Số lượng dấu thăng tương ứng với cấp độ tiêu đề (từ H1 đến H6).

Markdown

# Tiêu đề cấp 1 (H1)
## Tiêu đề cấp 2 (H2)
### Tiêu đề cấp 3 (H3)
#### Tiêu đề cấp 4 (H4)
##### Tiêu đề cấp 5 (H5)
###### Tiêu đề cấp 6 (H6)
Kết quả:

Tiêu đề cấp 1 (H1)
Tiêu đề cấp 2 (H2)
Tiêu đề cấp 3 (H3)
Tiêu đề cấp 4 (H4)
Tiêu đề cấp 5 (H5)
Tiêu đề cấp 6 (H6)
2. In đậm và In nghiêng (Bold and Italic)
In đậm: Sử dụng hai dấu hoa thị (**) hoặc hai dấu gạch dưới (__) bao quanh văn bản.

In nghiêng: Sử dụng một dấu hoa thị (*) hoặc một dấu gạch dưới (_) bao quanh văn bản.

Markdown

Đây là văn bản **in đậm**.
Đây là văn bản __in đậm__.

Đây là văn bản *in nghiêng*.
Đây là văn bản _in nghiêng_.

Đây là văn bản ***in đậm và in nghiêng***.
Kết quả:

Đây là văn bản in đậm.
Đây là văn bản in đậm.

Đây là văn bản in nghiêng.
Đây là văn bản in nghiêng.

Đây là văn bản in đậm và in nghiêng.

3. Danh sách (Lists)
Danh sách không có thứ tự (Unordered list): Sử dụng dấu hoa thị (*), dấu cộng (+) hoặc dấu gạch ngang (-) theo sau là một khoảng trắng.

Danh sách có thứ tự (Ordered list): Sử dụng số theo sau là dấu chấm (.) và một khoảng trắng.

Markdown

* Mục 1
* Mục 2
  * Mục con của Mục 2
  * Mục con khác

- Mục A
- Mục B

1. Bước một
2. Bước hai
3. Bước ba
   1. Bước con 3.1
   2. Bước con 3.2
Kết quả:

Mục 1

Mục 2

Mục con của Mục 2

Mục con khác

Mục A

Mục B

Bước một

Bước hai

Bước ba

Bước con 3.1

Bước con 3.2

4. Liên kết (Links)
Sử dụng dấu ngoặc vuông [] cho văn bản hiển thị và dấu ngoặc đơn () cho URL.

Markdown

Đây là một [liên kết đến Google](https://www.google.com).
Kết quả:

Đây là một liên kết đến Google.

5. Hình ảnh (Images)
Cú pháp tương tự liên kết, nhưng có thêm dấu chấm than (!) ở phía trước. Văn bản trong dấu ngoặc vuông là văn bản thay thế (alt text) nếu hình ảnh không tải được.

Markdown

![Mô tả hình ảnh](https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png "Tiêu đề hình ảnh")
Kết quả:

6. Khối mã (Code Blocks) và Mã nội tuyến (Inline Code)
Mã nội tuyến: Sử dụng dấu huyền (``) bao quanh văn bản.

Khối mã: Sử dụng ba dấu huyền (```) bao quanh đoạn mã. Bạn có thể thêm tên ngôn ngữ sau dấu huyền đầu tiên để làm nổi bật cú pháp.

Markdown

`Đây là mã nội tuyến`.

```python
def hello_world():
    print("Hello, Markdown!")
JavaScript

console.log("Đây là một ví dụ JavaScript.");

**Kết quả:**

`Đây là mã nội tuyến`.

```python
def hello_world():
    print("Hello, Markdown!")
JavaScript

console.log("Đây là một ví dụ JavaScript.");
7. Trích dẫn (Blockquotes)
Sử dụng dấu lớn hơn (>) ở đầu mỗi dòng trích dẫn.

Markdown

> "Markdown là một công cụ tuyệt vời cho việc viết lách."
> — Một người hâm mộ Markdown
Kết quả:

"Markdown là một công cụ tuyệt vời cho việc viết lách."
— Một người hâm mộ Markdown

8. Đường kẻ ngang (Horizontal Rules)
Sử dụng ba hoặc nhiều hơn các dấu hoa thị (***), dấu gạch ngang (---), hoặc dấu gạch dưới (___) trên một dòng riêng biệt.

Markdown

Đây là một đoạn văn bản.

---

Đây là một đoạn văn bản khác sau đường kẻ ngang.
Kết quả:

Đây là một đoạn văn bản.

Đây là một đoạn văn bản khác sau đường kẻ ngang.

Tại sao nên sử dụng Markdown?
Đơn giản và dễ học: Cú pháp trực quan, dễ nhớ và không yêu cầu kiến thức phức tạp về HTML hay CSS.

Tập trung vào nội dung: Bạn có thể viết mà không bị phân tâm bởi các tùy chọn định dạng phức tạp như trong các trình soạn thảo văn bản truyền thống.

Định dạng nhất quán: Giúp duy trì sự nhất quán về định dạng trên các nền tảng khác nhau.

Linh hoạt: Có thể chuyển đổi sang nhiều định dạng khác nhau như HTML, PDF, v.v.

Tương thích rộng rãi: Được hỗ trợ bởi rất nhiều ứng dụng, nền tảng, và dịch vụ.

Thực hành
Cách tốt nhất để học Markdown là thực hành! Bạn có thể thử viết một vài ghi chú hoặc tài liệu đơn giản bằng Markdown. Nhiều trình soạn thảo văn bản hiện đại (như VS Code, Typora, Obsidian) và các công cụ trực tuyến (như Dillinger.io, StackEdit.io) đều hỗ trợ xem trước Markdown theo thời gian thực, giúp bạn dễ dàng kiểm tra kết quả.

Nếu có bất kỳ cú pháp nào bạn muốn tìm hiểu thêm hoặc có câu hỏi cụ thể, cứ hỏi nhé!
