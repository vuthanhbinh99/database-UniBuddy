# Tài Liệu Quy Tắc Nghiệp Vụ Hệ Thống QLBTSV (Business Rules Document)

## Tổng Quan
Tài liệu này định nghĩa các quy tắc nghiệp vụ (Business Rules - BR) áp dụng cho hệ thống Quản lý Bài tập và Học tập của Sinh viên (QLBTSV). Bộ quy tắc này trực tiếp điều hướng logic xử lý tại tầng Backend và ràng buộc dữ liệu tại tầng Database dựa trên cấu trúc database 25 bảng hiện tại.

---

## MODULE 1: XÁC THỰC, PHÂN QUYỀN & TÀI KHOẢN

### BR-AUTH-01: Ràng buộc Trạng thái Tài khoản bị khóa
- **Mô tả:** Đảm bảo người dùng bị khóa không thể thực hiện bất kỳ hành động nào nhằm phá hoại hoặc khai thác hệ thống.
- **Logic xử lý:** Khi trường `TRANG_THAI` trong bảng `nguoi_dung` chuyển sang giá trị `'BI_KHOA'`:
  1. Ngay lập tức vô hiệu hóa (thu hồi) toàn bộ các mã token bằng cách cập nhật `THOI_GIAN_THU_HOI = NOW()` cho tất cả các phiên tương ứng trong bảng `phien_dang_nhap`.
  2. Từ chối mọi yêu cầu cấp mới Access Token / Refresh Token từ thiết bị của tài khoản này.
- **Bảng ảnh hưởng:** `nguoi_dung`, `phien_dang_nhap`.

### BR-AUTH-02: Duy trì Phiên đăng nhập và FCM Token độc nhất
- **Mô tả:** Cho phép sinh viên đăng nhập trên nhiều thiết bị (Web, Mobile) nhưng tránh việc đẩy trùng thông báo (Push Notification).
- **Logic xử lý:** Một `MA_NGUOI_DUNG` có thể sở hữu nhiều bản ghi hoạt động trong bảng `phien_dang_nhap`. Tuy nhiên, nếu một thiết bị mới đăng nhập và gửi lên một `FCM_TOKEN` đã tồn tại ở một phiên khác, hệ thống phải cập nhật `FCM_TOKEN` của phiên cũ thành `NULL` trước khi gán cho phiên mới.
- **Bảng ảnh hưởng:** `phien_dang_nhap`.

---

## MODULE 2: QUẢN LÝ HỌC PHẦN, TIẾN ĐỘ & TÍNH ĐIỂM

### BR-EDU-01: Ràng buộc duy nhất theo phân cấp Học kỳ - Môn học
- **Mô tả:** Sinh viên không được phép tạo các bản ghi trùng lặp gây nhiễu dữ liệu học tập cá nhân.
- **Logic xử lý:** - `TEN_HOC_KY` phải là duy nhất đối với từng tài khoản sinh viên cụ thể trong bảng `hoc_ky`.
  - `MA_MON` và `TEN_MON` phải là duy nhất trong cùng một `MA_HOC_KY` thuộc bảng `mon_hoc`.
  - Thời gian học kỳ bắt buộc phải thỏa mãn điều kiện logic: `NGAY_BAT_DAU` < `NGAY_KET_THUC`.
- **Bảng ảnh hưởng:** `hoc_ky`, `mon_hoc`.

### BR-EDU-02: Ràng buộc Tổng trọng số Điểm thành phần
- **Mô tả:** Đảm bảo cấu trúc điểm số của một môn học thiết lập chính xác theo quy chế đào tạo.
- **Logic xử lý:** Đối với mỗi môn học (`MA_MON_HOC`), tổng tất cả các giá trị `TRONG_SO` của các thành phần điểm con trong bảng `thanh_phan_diem` bắt buộc phải bằng chính xác $100\%$ (hoặc $1.00$). Hệ thống sẽ chặn hành động lưu điểm và cảnh báo nếu tổng trọng số lớn hơn hoặc nhỏ hơn $100\%$.
- **Bảng ảnh hưởng:** `thanh_phan_diem`.

### BR-EDU-03: Công thức Tính Điểm Tổng kết Môn học (Hệ 10)
- **Mô tả:** Tự động tính toán điểm tổng kết môn học dựa trên các đầu điểm thành phần hiện có.
- **Logic xử lý:** Điểm tổng kết hệ 10 của một môn học ($Điểm_{TK}$) được tính toán theo công thức:
$$Điểm_{TK} = \sum_{i=1}^{n} (DIEM_i \times TRONG\_SO_i)$$
  Trong đó, nếu một `thanh_phan_diem` chưa có điểm (`DIEM` là `NULL`), hệ thống sẽ mặc định tính đầu điểm đó bằng $0$ hoặc bỏ qua tùy theo thiết lập trạng thái hoãn thi của sinh viên.
- **Bảng ảnh hưởng:** `thanh_phan_diem`, `mon_hoc`.

### BR-EDU-04: Tự động quy đổi Điểm chữ, Điểm hệ 4 và Xếp loại Học lực
- **Mô tả:** Đồng bộ hóa điểm số theo đúng quy chế riêng biệt của từng trường đại học.
- **Logic xử lý:** 1. Sau khi tính toán được Điểm tổng kết hệ 10 ($Điểm_{TK}$), hệ thống truy vấn vào bảng `chi_tiet_thang_diem` dựa trên `MA_THANG_DIEM` của trường học tương ứng. Tìm bản ghi thỏa mãn ràng buộc: $DIEM\_THAP\_NHAT \le Điểm_{TK} \le DIEM\_CAO\_NHAT$. Từ đó tự động gán giá trị cho `DIEM_CHU` và `DIEM_HE_4`.
  2. Điểm Trung bình Học kỳ (GPA) được tính tự động:
$$GPA = \frac{\sum_{j=1}^{m} (DIEM\_HE\_4_j \times SO\_TIN\_CHI_j)}{\sum_{j=1}^{m} SO\_TIN\_CHI_j}$$
  3. Giá trị $GPA$ cuối cùng được đối chiếu trực tiếp với bảng `quy_che_hoc_luc` của trường để cập nhật trạng thái xếp loại (`TEN_XEP_LOAI`) tự động cho sinh viên.
- **Bảng ảnh hưởng:** `truong_hoc`, `thang_diem`, `chi_tiet_thang_diem`, `quy_che_hoc_luc`, `mon_hoc`.

---

## MODULE 3: LỊCH TRÌNH, DEADLINE & NHẮC NHỞ TỰ ĐỘNG

### BR-SCH-01: Kiểm tra xung đột Thời khóa biểu (Trùng lịch học)
- **Mô tả:** Ngăn chặn việc sinh viên đăng ký hoặc thêm nhầm lịch học bị trùng thời gian.
- **Logic xử lý:** Khi người dùng thêm một bản ghi vào bảng `lich_hoc`, hệ thống phải kiểm tra xem có tồn tại lịch học nào khác trùng lặp các yếu tố sau hay không:
  - Giống nhau về `THU`.
  - Có sự giao thoa về khoảng tiết học: Khoảng thời gian từ `TIET_BAT_DAU` đến (`TIET_BAT_DAU` + `SO_TIET`) của bản ghi mới chồng lấn lên bản ghi cũ.
  - Khoảng thời gian hiệu lực (`NGAY_BAT_DAU` đến `NGAY_KET_THUC`) có giao nhau.
  - Nếu phát hiện trùng lặp, hệ thống phải đưa ra cảnh báo cho người dùng (tuy nhiên vẫn cho phép lưu nếu đó là lịch học bù đặc biệt).
- **Bảng ảnh hưởng:** `lich_hoc`.

### BR-SCH-02: Tự động cập nhật Trạng thái Deadline trễ hạn
- **Mô tả:** Hệ thống tự động quét và đánh giá tiến độ bài tập dựa theo mốc thời gian thực tế.
- **Logic xử lý:** Thiết lập một tác vụ ngầm định kỳ (Cron Job) chạy mỗi 15 phút một lần trên toàn hệ thống:
  - Tìm kiếm toàn bộ các bản ghi trong bảng `deadline` và bảng `cong_viec_nhom` có `HAN_NOP` hoặc `HAN_HOAN_THANH` nhỏ hơn thời gian hiện tại (`NOW()`).
  - Nếu `TRANG_THAI` của các bản ghi này đang ở dạng `'CHUA_LAM'` hoặc `'DANG_LAM'` (đối với deadline) hoặc khác `'HOAN_THANH'` (đối với công việc nhóm), hệ thống tự động chuyển trạng thái của chúng sang `'TRE_HAN'`.
- **Bảng ảnh hưởng:** `deadline`, `cong_viec_nhom`.

### BR-SCH-03: Ràng buộc Thời gian đặt Nhắc nhở
- **Mô tả:** Đảm bảo tính hợp lệ về mặt thời gian của các thông báo nhắc nhở tự động.
- **Logic xử lý:** Khi sinh viên tạo một bản ghi nhắc nhở học tập trong bảng `nhac_nho`:
  - Giá trị `THOI_GIAN_NHAC` bắt buộc phải lớn hơn thời gian hiện tại (`NOW()`).
  - Nếu nhắc nhở liên kết với một deadline (`MA_DEADLINE`), `THOI_GIAN_NHAC` phải nhỏ hơn hoặc bằng `HAN_NOP`.
  - Nếu nhắc nhở liên kết với một lịch thi (`MA_LICH_THI`), `THOI_GIAN_NHAC` phải nhỏ hơn hoặc bằng `THOI_GIAN_THI`.
- **Bảng ảnh hưởng:** `nhac_nho`, `deadline`, `lich_thi`.

---

## MODULE 4: TƯƠNG TÁC NHÓM HỌC TẬP & PHÂN CHIA CÔNG VIỆC

### BR-GROUP-01: Quyền hạn đặc quyền của Trưởng nhóm
- **Mô tả:** Phân định rõ chức năng điều hành nhóm nhằm đảm bảo tính tổ chức khi làm bài tập lớn.
- **Logic xử lý:** Hệ thống thực hiện kiểm tra quyền (Authorization Check) tại tầng Backend Service trước khi xử lý các hành động sau:
  - Chỉ thành viên có `VAI_TRO_TRONG_NHOM` = `'TRUONG_NHOM'` trong bảng `thanh_vien_nhom` mới có quyền chỉnh sửa thông tin nhóm, xóa thành viên, tạo `cong_viec_nhom` và phân bổ công việc cho các thành viên khác (`NGUOI_DUOC_GIAO`).
  - Các thành viên có vai trò `'THANH_VIEN'` chỉ có quyền xem danh sách và cập nhật trường `TRANG_THAI` của các `cong_viec_nhom` mà mình được phân công đảm nhiệm.
- **Bảng ảnh hưởng:** `nhom_hoc_tap`, `thanh_vien_nhom`, `cong_viec_nhom`.

### BR-GROUP-02: Tự động chỉ định Trưởng nhóm khi khởi tạo
- **Mô tả:** Đảm bảo nhóm học tập luôn có người điều hành ngay khi được thành lập.
- **Logic xử lý:** Khi một sinh viên thực hiện hành động tạo nhóm mới (`nhom_hoc_tap`), hệ thống xử lý dưới dạng một Transaction bảo toàn:
  1. Tạo bản ghi mới trong bảng `nhom_hoc_tap`, lưu thông tin sinh viên đó vào cột `NGUOI_TAI_LEN` / `NGUOI_TAO`.
  2. Tự động chèn một bản ghi liên kết vào bảng `thanh_vien_nhom` với `MA_NGUOI_DUNG` của sinh viên đó và thiết lập cứng trường `VAI_TRO_TRONG_NHOM` thành `'TRUONG_NHOM'`.
- **Bảng ảnh hưởng:** `nhom_hoc_tap`, `thanh_vien_nhom`.

---

## MODULE 5: CHIA SẺ TÀI LIỆU & CƠ CHẾ KIỂM DUYỆT CỘNG ĐỒNG

### BR-DOC-01: Ràng buộc Quyền truy cập hiển thị file Tài liệu
- **Mô tả:** Kiểm soát tính an toàn dữ liệu và quyền riêng tư của tài liệu học tập được tải lên hệ thống cloud.
- **Logic xử lý:** Khi một người dùng yêu cầu truy cập hoặc tải về một file dữ liệu (`MA_TAI_LIEU`):
  - Nếu `CHE_DO_HIEN_THI` = `'RIENG_TU'`: Chỉ cho phép duy nhất `NGUOI_TAI_LEN` truy cập.
  - Nếu `CHE_DO_HIEN_THI` = `'CHIA_SE_NHOM'`: Hệ thống kiểm tra xem người dùng hiện tại có nằm trong bảng `thanh_vien_nhom` của `MA_NHOM` liên kết với tài liệu đó hay không. Nếu không nằm trong nhóm, hệ thống chặn quyền truy cập.
  - Nếu `CHE_DO_HIEN_THI` = `'CONG_KHAI'`: Cho phép tất cả người dùng hệ thống có trạng thái tài khoản hợp lệ được phép xem và tải về.
- **Bảng ảnh hưởng:** `tai_lieu`, `thanh_vien_nhom`.

### BR-DOC-02: Tự động ẩn tài liệu cộng đồng khi nhận quá số lượt Báo cáo xấu (Report)
- **Mô tả:** Giảm tải công việc kiểm duyệt thủ công và ngăn chặn nội dung độc hại phát tán nhanh chóng.
- **Logic xử lý:** - Mỗi khi có sinh viên tạo một bản ghi báo cáo vi phạm mới trong bảng `bao_cao_tai_lieu`, hệ thống sẽ đếm tổng số lượng bản ghi báo cáo chưa xử lý (`TRANG_THAI` = `'CHO_XU_LY'`) thuộc về `MA_TAI_LIEU` đó.
  - Ngưỡng giới hạn tự động: Nếu tổng số lượt báo cáo xấu đạt từ **5 lượt trở lên**, hệ thống tự động cập nhật trường `TRANG_THAI` của tài liệu đó trong bảng `tai_lieu` từ `'KHA_DUNG'` sang `'CHO_KIEM_DUYET'`.
  - Khi tài liệu chuyển sang trạng thái chờ duyệt, nó sẽ ngay lập tức bị ẩn khỏi toàn bộ các danh sách tìm kiếm công khai cho đến khi Quản trị viên (`NGUOI_KIEM_DUYET`) đưa ra quyết định xử lý cuối cùng.
- **Bảng ảnh hưởng:** `tai_lieu`, `bao_cao_tai_lieu`.

---

## MODULE 6: THUẬT TOÁN FLASHCARD HỌC THÔNG MINH

### BR-CARD-01: Cập nhật Tiến độ Ôn tập Flashcard theo Thuật toán Giãn cách (Spaced Repetition)
- **Mô tả:** Tính toán chu kỳ phân phối thời gian lặp lại giúp tối ưu khả năng ghi nhớ kiến thức của sinh viên.
- **Logic xử lý:** Khi sinh viên hoàn thành một lượt lật ôn tập một thẻ flashcard, dựa vào đánh giá độ khó dễ của họ, hệ thống tự động cập nhật lại các trường dữ liệu sau:
  - Tăng trường số lần ôn tập: `SO_LAN_ON = SO_LAN_ON + 1`.
  - Cập nhật thời điểm vừa học: `THOI_GIAN_LAN_ON_CUOI = NOW()`.
  - Tính toán khoảng cách chu kỳ lặp lại tiếp theo: Trường `THOI_GIAN_LAN_ON_TIEP_THEO` sẽ bằng `NOW() + (\Delta t)`. Trong đó khoảng thời gian giãn cách $\Delta t$ được tối ưu hóa dựa trên thang điểm ghi nhớ (`DIEM_GHI_NHO`) của thẻ học. Thẻ có điểm ghi nhớ càng thấp (càng khó nhớ) sẽ có mốc thời gian ôn tập tiếp theo ngắn hơn (Ví dụ: sau 1 ngày), ngược lại thẻ dễ nhớ sẽ giãn cách xa hơn (Ví dụ: sau 7 ngày).
- **Bảng ảnh hưởng:** `flashcard`.