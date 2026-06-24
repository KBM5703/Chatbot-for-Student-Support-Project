# BKute n8n Chatbot

BKute là website chatbot hỗ trợ giải đáp thắc mắc cho sinh viên Bách Khoa. Dự án sử dụng Flask để xây dựng giao diện web, SQLite để lưu tài khoản người dùng, và tích hợp widget `@n8n/chat` để gửi câu hỏi đến workflow n8n.

## Chức năng chính

- Đăng ký và đăng nhập bằng mã số sinh viên.
- Hiển thị giao diện chatbot BKute tích hợp n8n.
- Cập nhật tên hiển thị và ảnh đại diện.
- Gửi góp ý cho hệ thống chatbot.
- Tài khoản admin có MSSV `2111464` có thể xem danh sách góp ý tại `/admin/feedback`.

## Công nghệ sử dụng

- Python, Flask, Flask-SQLAlchemy
- SQLite
- HTML, CSS, JavaScript
- n8n workflow
- `@n8n/chat`

## Cấu trúc thư mục

```text
BKute_n8n/
|-- app.py
|-- model.py
|-- package.json
|-- package-lock.json
|-- feedback.csv
|-- instance/
|   `-- database.db
|-- static/
|   |-- style.css
|   |-- uploads/
|   `-- ...
|-- templates/
|   |-- login.html
|   |-- register.html
|   |-- dashboard.html
|   |-- admin_feedback.html
|   `-- profile.html
`-- Báo Cáo ĐA2.pdf
```

## Yêu cầu trước khi chạy

Cài đặt sẵn:

- Python 3.10 hoặc mới hơn
- Node.js và npm nếu muốn cài hoặc chạy n8n bằng npm
- n8n đang chạy ở máy local hoặc một n8n webhook public tương đương

Trong file `templates/dashboard.html`, chatbot đang trỏ đến webhook:

```text
http://localhost:5678/webhook/c736eac9-607a-42f6-9ea8-259ddb4a9b20/chat
```

Nếu workflow n8n của bạn có webhook khác, hãy sửa giá trị `webhookUrl` trong `templates/dashboard.html`.

## Cài đặt Python dependencies

Mở terminal tại thư mục dự án:

```powershell
cd BKute_n8n
```

Tạo môi trường ảo:

```powershell
python -m venv venv
```

Kích hoạt môi trường ảo trên Windows PowerShell:

```powershell
.\venv\Scripts\Activate.ps1
```

Cài các thư viện cần thiết:

```powershell
python -m pip install --upgrade pip
pip install Flask Flask-SQLAlchemy Werkzeug
```

## Cài đặt package Node

Dự án đã có `package.json` với dependency `@n8n/chat`. Nếu cần cài lại `node_modules`, chạy:

```powershell
npm install
```

Lưu ý: giao diện hiện đang import `@n8n/chat` trực tiếp từ CDN trong `dashboard.html`, nên Flask app vẫn có thể chạy nếu không chỉnh sửa phần frontend.

## Chạy n8n local

Cài n8n bằng npm nếu máy chưa có:

```powershell
npm install -g n8n
```

Khởi động n8n:

```powershell
n8n start
```

Sau đó mở:

```text
http://localhost:5678
```

Trong n8n, cần chuẩn bị workflow chatbot có endpoint chat/webhook tương ứng với `webhookUrl` trong `templates/dashboard.html`. Nếu tạo workflow mới, copy webhook URL mới và cập nhật lại trong file `dashboard.html`.

## Chạy website Flask

Từ thư mục `BKute_n8n`, chạy:

```powershell
python app.py
```

Khi Flask khởi động thành công, mở trình duyệt tại:

```text
http://127.0.0.1:5000
```

Hoặc:

```text
http://localhost:5000
```

## Cách sử dụng

1. Truy cập trang đăng nhập tại `/`.
2. Chọn đăng ký tài khoản mới tại `/register`.
3. Đăng nhập bằng MSSV và mật khẩu vừa tạo.
4. Sau khi đăng nhập, trang dashboard sẽ hiển thị chatbot BKute.
5. Người dùng có thể chat với BKute, cập nhật thông tin cá nhân hoặc gửi góp ý.
6. Để xem góp ý với quyền admin, đăng ký hoặc đăng nhập bằng MSSV `2111464`, sau đó truy cập nút xem góp ý hoặc vào `/admin/feedback`.

## Lưu ý khi chạy

- Flask mặc định chạy ở port `5000`.
- n8n mặc định chạy ở port `5678`.
- Cần bật n8n workflow trước khi thử chat, nếu không widget chat sẽ không nhận được phản hồi.
- Dữ liệu tài khoản được lưu trong SQLite tại `instance/database.db`.
- Góp ý người dùng được lưu trong file `feedback.csv`.
- Ảnh đại diện upload được lưu trong `static/uploads/`.

## Lỗi thường gặp

### Chatbot không phản hồi

Kiểm tra:

- n8n đã chạy chưa.
- Workflow chatbot đã được bật chưa.
- `webhookUrl` trong `templates/dashboard.html` có đúng với webhook hiện tại không.
- Nếu dùng webhook public, đảm bảo URL có thể truy cập từ trình duyệt.

### Không xem được trang góp ý admin

Chỉ tài khoản có MSSV `2111464` được truy cập `/admin/feedback`. Nếu dùng MSSV khác, hệ thống sẽ trả về lỗi không có quyền truy cập.

### Lỗi thiếu thư viện Python

Chạy lại:

```powershell
pip install Flask Flask-SQLAlchemy Werkzeug
```

## Kết quả đề tài

Theo nội dung trong `Báo Cáo ĐA2.pdf`, hệ thống đã được triển khai thử nghiệm sau khi deploy website bằng Render. Nhóm bắt đầu thử nghiệm bằng cách đăng ký tài khoản admin với MSSV, sau đó cho người dùng khác tạo tài khoản, đăng nhập, trải nghiệm chatbot và gửi góp ý.

Các kết quả chính đạt được:

- Website có giao diện đăng nhập và đăng ký tài khoản cho người dùng.
- Người dùng sau khi đăng nhập có thể truy cập giao diện chính và trò chuyện với BKute.
- Tài khoản thường không hiển thị chức năng xem đánh giá của admin.
- Tài khoản admin có thể xem danh sách góp ý từ người dùng thử nghiệm.
- Người dùng có thể cập nhật tên và ảnh đại diện cá nhân.
- Người dùng có thể gửi góp ý, báo lỗi hoặc đề xuất phát triển thông qua nút góp ý trên giao diện.
- Chatbot BKute bước đầu hỗ trợ trả lời các câu hỏi liên quan đến thông tin trường, nội quy, học vụ và các thắc mắc thường gặp.

Phần đánh giá trong báo cáo cho thấy chatbot đã bước đầu đạt hiệu quả trong việc cung cấp thông tin liên quan đến trường học. Nội dung phản hồi được nhận xét là khá chính xác và có mức độ tương đồng cao với các nguồn thông tin chính thống đã công khai. Tuy nhiên, hệ thống vẫn còn một số hạn chế như khung chat còn nhỏ, vị trí chưa thật thuận tiện, tốc độ phản hồi còn chậm do dùng công cụ embedding và mô hình xử lý ngôn ngữ trên nền tảng miễn phí của Ollama, đồng thời tính năng hiện tại vẫn còn cơ bản.

Hướng phát triển tiếp theo là cải thiện giao diện, tối ưu hiệu năng xử lý, mở rộng thêm các tính năng thông minh và nâng cao trải nghiệm người dùng để BKute trở thành một trợ lý ảo thân thiện, hiệu quả hơn cho sinh viên.
