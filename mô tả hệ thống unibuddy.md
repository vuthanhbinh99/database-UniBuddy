# Mô Tả Hệ Thống UniBuddy

> Tài liệu này mô tả UniBuddy bằng ngôn ngữ dễ hiểu cho người dùng, dựa trên chức năng thực tế đã có trong mã nguồn (backend, ứng dụng di động) và cấu trúc cơ sở dữ liệu của hệ thống.

## UniBuddy là gì?

UniBuddy là **người bạn đồng hành học tập dành cho sinh viên**. Ứng dụng giúp bạn gom tất cả việc học vào một chỗ: lịch học, lịch thi, deadline bài tập, điểm số, ghi chú, thẻ ghi nhớ (flashcard), nhóm học tập và tài liệu chia sẻ. Bên cạnh đó còn có một **trợ lý AI** để hỏi đáp về việc học của chính bạn.

Hệ thống gồm hai phần:
- **Ứng dụng cho người dùng** (chạy trên điện thoại, viết bằng Flutter): nơi sinh viên và người quản trị sử dụng hằng ngày.
- **Máy chủ xử lý** (backend): nơi lưu trữ dữ liệu, kiểm tra quy tắc và kết nối với các dịch vụ như AI, gửi thông báo, lưu file.

## Ai sử dụng hệ thống?

Hệ thống có 3 nhóm người dùng, mỗi nhóm nhìn thấy màn hình và chức năng khác nhau:

### 1. Sinh viên
Đây là người dùng chính. Sinh viên có thể tự đăng ký tài khoản bằng email hoặc đăng nhập nhanh bằng tài khoản Google. Sau khi đăng nhập, sinh viên được dùng toàn bộ các tính năng học tập.

### 2. Quản trị viên nội dung (Admin)
Người phụ trách **duyệt nội dung và quản lý dữ liệu chung của trường**. Admin xử lý các báo cáo tài liệu vi phạm và quản lý danh mục trường học cùng quy chế điểm.

### 3. Quản trị viên hệ thống (Super Admin)
Người có quyền cao nhất, phụ trách **vận hành toàn hệ thống**: tạo và phân quyền tài khoản, khóa/mở tài khoản, gửi thông báo toàn hệ thống, xem nhật ký hoạt động và nhật ký lỗi, theo dõi dung lượng lưu trữ.

---

## Những gì sinh viên làm được

### Tài khoản và đăng nhập
- Đăng ký tài khoản mới bằng email, hoặc đăng nhập bằng Google.
- Quên mật khẩu thì lấy lại qua email: hệ thống gửi mã xác thực, bạn nhập mã rồi đặt mật khẩu mới.
- Đăng nhập được trên nhiều thiết bị cùng lúc (điện thoại, web). Bạn có thể xem danh sách các phiên đang đăng nhập và **đăng xuất từ xa** một thiết bị bất kỳ.
- Cập nhật thông tin cá nhân và ảnh đại diện.
- Gửi góp ý (feedback) cho đội ngũ phát triển ngay trong ứng dụng.

### Quản lý học phần theo học kỳ
- Tạo các **học kỳ** của riêng bạn, mỗi học kỳ có ngày bắt đầu – kết thúc.
- Trong mỗi học kỳ, thêm các **môn học** kèm số tín chỉ.
- Xem chi tiết và chỉnh sửa từng môn.

### Thời khóa biểu và lịch thi
- Thêm **lịch học** cho từng môn: thứ mấy, tiết bắt đầu, số tiết, phòng học, khoảng ngày áp dụng.
- Khi thêm lịch bị **trùng giờ** với môn khác, hệ thống sẽ cảnh báo để bạn tránh xếp nhầm.
- Thêm **lịch thi** cho môn học.
- Có thể **nhập lịch học từ file** (ví dụ file Excel thời khóa biểu): hệ thống xem trước dữ liệu, có AI hỗ trợ gợi ý cách ghép cột cho đúng, rồi bạn xác nhận để nhập hàng loạt.

### Deadline bài tập
- Tạo **deadline** cho từng môn: tiêu đề, mô tả, hạn nộp.
- Đánh dấu trạng thái: chưa làm, đang làm, hoàn thành.
- Deadline nào **quá hạn mà chưa hoàn thành** sẽ tự động chuyển sang trạng thái "trễ hạn".

### Nhắc nhở tự động
- Khi bạn tạo deadline hoặc lịch thi, hệ thống tự cài sẵn các **lời nhắc trước hạn** (ví dụ nhắc trước 24 giờ và trước 3 giờ).
- Bạn cũng có thể tự đặt thêm lời nhắc theo ý mình, miễn là thời điểm nhắc nằm trước hạn nộp/giờ thi.

### Điểm số và tính GPA
- Với mỗi môn, thiết lập các **thành phần điểm** (ví dụ: giữa kỳ, cuối kỳ, chuyên cần) kèm **trọng số**. Tổng trọng số của một môn phải đủ 100%.
- Nhập điểm từng thành phần, hệ thống **tự tính điểm tổng kết** của môn.
- Hệ thống **tự quy đổi** điểm số sang điểm chữ và điểm hệ 4 theo thang điểm của trường, rồi **tính GPA** học kỳ và **xếp loại học lực**.
- Có thể **nhập điểm từ file**, xem trước rồi xác nhận.
- Có tính năng **dự đoán GPA** (project GPA) để bạn ước lượng kết quả.
- Có **tư vấn AI về điểm số**: gợi ý cho bạn cần đạt bao nhiêu để đạt mục tiêu.

### Ghi chú
- Tạo **ghi chú** học tập, có thể gắn với một môn học cụ thể.
- **Đính kèm tài liệu/file** vào ghi chú.
- Xem, sửa, xóa ghi chú.

### Flashcard (thẻ ghi nhớ) và học thông minh
- Tạo các **bộ flashcard**, mỗi thẻ có mặt trước – mặt sau, có thể gắn với một môn.
- Nhiều cách tạo thẻ nhanh:
  - Tạo thủ công.
  - **Nhập từ file**.
  - **Nhờ AI sinh thẻ tự động** từ nội dung, kể cả sinh thẻ dạng câu hỏi tự luận từ file tài liệu.
- Khi ôn tập, hệ thống áp dụng **thuật toán lặp lại giãn cách (spaced repetition)**: thẻ nào bạn nhớ kém sẽ được nhắc ôn lại sớm hơn, thẻ nhớ tốt thì giãn ra xa hơn. Sau mỗi lượt ôn, số lần ôn và lịch ôn tiếp theo được cập nhật tự động.
- Xem **thống kê** tiến độ ôn tập.

### Nhóm học tập và bảng công việc (Kanban)
- Tạo **nhóm học tập** hoặc **tham gia nhóm** bằng mã tham gia. Mỗi nhóm gắn với một trường và có link nhóm chat (Zalo/Messenger/Discord).
- Người tạo nhóm mặc định là **trưởng nhóm**.
- Nhóm có một **bảng công việc kiểu Kanban**:
  - **Trưởng nhóm** được: sửa thông tin nhóm, quản lý thành viên, tạo công việc và **giao việc** cho thành viên.
  - **Thành viên** được: xem danh sách công việc và cập nhật trạng thái công việc mình được giao.
  - Mỗi công việc có thể **bình luận** để trao đổi.

### Tài liệu chia sẻ
- Tải tài liệu lên (lưu trên đám mây), đặt **chế độ hiển thị**:
  - **Riêng tư**: chỉ mình bạn xem.
  - **Chia sẻ nhóm**: chỉ thành viên trong nhóm liên quan mới xem/tải được.
  - **Công khai**: mọi người dùng hợp lệ đều xem và tải được.
- Có tính năng **tóm tắt nội dung tài liệu bằng AI**.
- Xem dung lượng lưu trữ đã dùng.

### Trợ lý AI học tập
- Sinh viên có thể **trò chuyện với trợ lý AI** ngay trong ứng dụng.
- Trợ lý hiểu và trả lời dựa trên chính dữ liệu học tập của bạn, xoay quanh các mảng: **điểm số, deadline, flashcard, nhóm học tập** và các nội dung học tập khác. Nếu câu hỏi nằm ngoài phạm vi, trợ lý sẽ báo cho bạn biết.

### Chế độ tập trung (Focus Mode)
- Có **đồng hồ hẹn giờ kiểu Pomodoro** giúp bạn tập trung học theo từng phiên.

### Thông báo
- Nhận **thông báo** về deadline, nhắc nhở, hoạt động nhóm và thông báo từ hệ thống.
- Đánh dấu đã đọc từng thông báo hoặc đánh dấu đã đọc tất cả.
- Thông báo đẩy (push notification) được gửi tới thiết bị.

---

## Những gì Quản trị viên nội dung (Admin) làm được

- **Duyệt tài liệu bị báo cáo**: khi một tài liệu bị nhiều sinh viên báo cáo vi phạm, hệ thống tự ẩn tài liệu và chuyển sang trạng thái "chờ kiểm duyệt". Admin xem danh sách báo cáo, sau đó **duyệt** (chấp nhận) hoặc **từ chối** báo cáo.
- **Quản lý danh mục trường học**: thêm, sửa, xóa, xem thông tin các trường.
- **Quản lý thang điểm và quy chế học lực** của trường: cập nhật thang điểm quy đổi và các mức xếp loại học lực (dùng để hệ thống tính điểm chữ, điểm hệ 4 và GPA cho sinh viên).

---

## Những gì Quản trị viên hệ thống (Super Admin) làm được

- **Quản lý tài khoản người dùng**: xem danh sách và chi tiết người dùng, **tạo tài khoản quản trị**, **đổi vai trò** và **khóa/mở tài khoản**.
  - Khi một tài khoản bị khóa, hệ thống **thu hồi ngay toàn bộ phiên đăng nhập** của tài khoản đó và từ chối cấp token mới — người bị khóa lập tức không dùng được nữa.
- **Gửi thông báo toàn hệ thống** tới sinh viên hoặc theo nhóm vai trò.
- **Xem nhật ký hoạt động** (audit log): ghi lại ai đã làm gì trên hệ thống.
- **Xem nhật ký lỗi** (error log) và chi tiết từng lỗi để xử lý sự cố.
- **Theo dõi dung lượng lưu trữ** của hệ thống.

---

## Vài điểm nổi bật về cách hệ thống bảo vệ dữ liệu

- **Kiểm soát trùng lặp**: mỗi email chỉ dùng một tài khoản; trong cùng một học kỳ không có hai môn trùng mã; trong cùng một môn không có hai thành phần điểm trùng tên; một người không báo cáo cùng một tài liệu nhiều lần.
- **Kiểm tra tính hợp lệ của số liệu**: điểm, trọng số, số tín chỉ, khoảng thời gian... đều phải nằm trong giới hạn hợp lý.
- **Phân quyền rõ ràng**: mỗi vai trò chỉ truy cập được đúng phần chức năng của mình; nhiều thao tác quan trọng được kiểm tra quyền ngay tại máy chủ trước khi thực hiện.
- **Tự động hóa theo thời gian**: hệ thống tự cập nhật deadline trễ hạn, tự gửi nhắc nhở đến hạn, và tự ẩn tài liệu khi bị báo cáo quá nhiều.

---

## Tóm tắt

UniBuddy giúp sinh viên **chủ động quản lý toàn bộ việc học** — từ lịch học, deadline, điểm số, đến ôn tập bằng flashcard và làm việc nhóm — với sự hỗ trợ của **AI** và các **nhắc nhở tự động**. Đội ngũ quản trị đảm bảo nội dung lành mạnh và hệ thống vận hành ổn định. Tất cả gói gọn trong một ứng dụng di động duy nhất.
