# Cấu Trúc Table Database QLBTSV

## Tổng Quan

- Database: `QLBTSV`
- Tổng số bảng: `25`
- Tên bảng và tên cột dùng tiếng Việt không dấu.

## Danh Sách Table

```text
1. vai_tro
2. nguoi_dung
3. truong_hoc
4. ho_so_sinh_vien
5. phien_dang_nhap
6. thang_diem
7. chi_tiet_thang_diem
8. quy_che_hoc_luc
9. hoc_ky
10. mon_hoc
11. thanh_phan_diem
12. lich_hoc
13. lich_thi
14. deadline
15. nhac_nho
16. ghi_chu
17. bo_flashcard
18. flashcard
19. nhom_hoc_tap
20. thanh_vien_nhom
21. cong_viec_nhom
22. tai_lieu
23. bao_cao_tai_lieu
24. thong_bao
25. nhat_ky_he_thong
```

## Cấu Trúc Từng Table

## 1. `vai_tro`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_vai_tro` | `smallserial` | PK | Mã vai trò |
| `ma_code` | `varchar(50)` | NOT NULL, UNIQUE | Mã code vai trò |
| `ten_vai_tro` | `varchar(100)` | NOT NULL | Tên vai trò |

## 2. `nguoi_dung`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_nguoi_dung` | `uuid` | PK | Mã người dùng |
| `ma_vai_tro` | `smallint` | NOT NULL, FK -> `vai_tro.ma_vai_tro` | Vai trò chính của người dùng |
| `email` | `varchar(255)` | NOT NULL, UNIQUE | Email đăng nhập |
| `mat_khau_hash` | `varchar(255)` | NOT NULL | Mật khẩu đã mã hóa |
| `ho_ten` | `varchar(150)` | NOT NULL | Họ tên người dùng |
| `so_dien_thoai` | `varchar(20)` | NULL | Số điện thoại |
| `anh_dai_dien_url` | `text` | NULL | Đường dẫn ảnh đại diện |
| `trang_thai` | `varchar(30)` | NOT NULL | Trạng thái tài khoản |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `cap_nhat_luc` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 3. `truong_hoc`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_truong` | `uuid` | PK | Mã trường |
| `ma_truong_code` | `varchar(50)` | NOT NULL, UNIQUE | Mã code trường |
| `ten_truong` | `varchar(255)` | NOT NULL | Tên trường |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `cap_nhat_luc` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 4. `ho_so_sinh_vien`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_nguoi_dung` | `uuid` | PK, FK -> `nguoi_dung.ma_nguoi_dung` | Mã người dùng của sinh viên |
| `ma_truong` | `uuid` | FK -> `truong_hoc.ma_truong`, NULL | Trường của sinh viên |
| `ma_sinh_vien` | `varchar(50)` | NOT NULL | Mã sinh viên |
| `nganh_hoc` | `varchar(150)` | NULL | Ngành học |
| `khoa_hoc` | `varchar(50)` | NULL | Khóa học |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `cap_nhat_luc` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`ma_truong`, `ma_sinh_vien`).

## 5. `phien_dang_nhap`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_phien` | `uuid` | PK | Mã phiên đăng nhập |
| `ma_nguoi_dung` | `uuid` | NOT NULL, FK -> `nguoi_dung.ma_nguoi_dung` | Người dùng sở hữu phiên |
| `refresh_token_hash` | `varchar(255)` | NOT NULL, UNIQUE | Refresh token đã hash |
| `fcm_token` | `text` | UNIQUE nếu khác NULL | Token gửi thông báo FCM |
| `loai_thiet_bi` | `varchar(50)` | NULL | Loại thiết bị |
| `ip_address` | `inet` | NULL | Địa chỉ IP |
| `user_agent` | `text` | NULL | Thông tin trình duyệt/thiết bị |
| `het_han_luc` | `timestamptz` | NOT NULL | Thời điểm hết hạn |
| `bi_thu_hoi_luc` | `timestamptz` | NULL | Thời điểm bị thu hồi |
| `lan_hoat_dong_cuoi` | `timestamptz` | NOT NULL | Lần hoạt động cuối |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |

## 6. `thang_diem`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_thang_diem` | `uuid` | PK | Mã thang điểm |
| `ma_truong` | `uuid` | NOT NULL, FK -> `truong_hoc.ma_truong` | Trường áp dụng |
| `ten_thang_diem` | `varchar(100)` | NOT NULL | Tên thang điểm |
| `diem_min` | `numeric(4,2)` | NOT NULL | Điểm thấp nhất |
| `diem_max` | `numeric(4,2)` | NOT NULL | Điểm cao nhất |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `cap_nhat_luc` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`ma_truong`, `ten_thang_diem`).

## 7. `chi_tiet_thang_diem`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_chi_tiet` | `uuid` | PK | Mã chi tiết thang điểm |
| `ma_thang_diem` | `uuid` | NOT NULL, FK -> `thang_diem.ma_thang_diem` | Thang điểm cha |
| `diem_chu` | `varchar(10)` | NOT NULL | Điểm chữ |
| `diem_he_4` | `numeric(3,2)` | NOT NULL | Điểm hệ 4 |
| `diem_min` | `numeric(4,2)` | NOT NULL | Điểm tối thiểu |
| `diem_max` | `numeric(4,2)` | NOT NULL | Điểm tối đa |

Ràng buộc bổ sung: UNIQUE (`ma_thang_diem`, `diem_chu`).

## 8. `quy_che_hoc_luc`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_quy_che` | `uuid` | PK | Mã quy chế |
| `ma_truong` | `uuid` | NOT NULL, FK -> `truong_hoc.ma_truong` | Trường áp dụng |
| `ten_xep_loai` | `varchar(100)` | NOT NULL | Tên xếp loại học lực |
| `gpa_min` | `numeric(3,2)` | NOT NULL | GPA tối thiểu |
| `gpa_max` | `numeric(3,2)` | NOT NULL | GPA tối đa |

Ràng buộc bổ sung: UNIQUE (`ma_truong`, `ten_xep_loai`).

## 9. `hoc_ky`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_hoc_ky` | `uuid` | PK | Mã học kỳ |
| `ma_sinh_vien` | `uuid` | NOT NULL, FK -> `ho_so_sinh_vien.ma_nguoi_dung` | Sinh viên sở hữu học kỳ |
| `ten_hoc_ky` | `varchar(100)` | NOT NULL | Tên học kỳ |
| `ngay_bat_dau` | `date` | NULL | Ngày bắt đầu |
| `ngay_ket_thuc` | `date` | NULL | Ngày kết thúc |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `cap_nhat_luc` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`ma_sinh_vien`, `ten_hoc_ky`).

## 10. `mon_hoc`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_mon_hoc` | `uuid` | PK | Mã môn học |
| `ma_hoc_ky` | `uuid` | NOT NULL, FK -> `hoc_ky.ma_hoc_ky` | Học kỳ chứa môn học |
| `ma_mon` | `varchar(50)` | NULL | Mã môn |
| `ten_mon` | `varchar(255)` | NOT NULL | Tên môn học |
| `so_tin_chi` | `smallint` | NOT NULL | Số tín chỉ |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `cap_nhat_luc` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`ma_hoc_ky`, `ma_mon`).

## 11. `thanh_phan_diem`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_thanh_phan` | `uuid` | PK | Mã thành phần điểm |
| `ma_mon_hoc` | `uuid` | NOT NULL, FK -> `mon_hoc.ma_mon_hoc` | Môn học |
| `ten_thanh_phan` | `varchar(100)` | NOT NULL | Tên thành phần điểm |
| `trong_so` | `numeric(5,2)` | NOT NULL | Trọng số điểm |
| `diem` | `numeric(4,2)` | NULL | Điểm đạt được |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `cap_nhat_luc` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`ma_mon_hoc`, `ten_thanh_phan`).

## 12. `lich_hoc`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_lich_hoc` | `uuid` | PK | Mã lịch học |
| `ma_mon_hoc` | `uuid` | NOT NULL, FK -> `mon_hoc.ma_mon_hoc` | Môn học |
| `thu` | `smallint` | NOT NULL | Thứ trong tuần |
| `tiet_bat_dau` | `smallint` | NOT NULL | Tiết bắt đầu |
| `so_tiet` | `smallint` | NOT NULL | Số tiết |
| `phong_hoc` | `varchar(100)` | NULL | Phòng học |
| `ngay_bat_dau` | `date` | NULL | Ngày bắt đầu |
| `ngay_ket_thuc` | `date` | NULL | Ngày kết thúc |

## 13. `lich_thi`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_lich_thi` | `uuid` | PK | Mã lịch thi |
| `ma_mon_hoc` | `uuid` | NOT NULL, FK -> `mon_hoc.ma_mon_hoc` | Môn học |
| `loai_thi` | `varchar(100)` | NULL | Loại thi |
| `thoi_gian_thi` | `timestamptz` | NOT NULL | Thời gian thi |
| `phong_thi` | `varchar(100)` | NULL | Phòng thi |

## 14. `deadline`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_deadline` | `uuid` | PK | Mã deadline |
| `ma_mon_hoc` | `uuid` | NOT NULL, FK -> `mon_hoc.ma_mon_hoc` | Môn học |
| `tieu_de` | `varchar(255)` | NOT NULL | Tiêu đề deadline |
| `mo_ta` | `text` | NULL | Mô tả |
| `han_nop` | `timestamptz` | NOT NULL | Hạn nộp |
| `trang_thai` | `varchar(30)` | NOT NULL | Trạng thái |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `cap_nhat_luc` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 15. `nhac_nho`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_nhac_nho` | `uuid` | PK | Mã nhắc nhở |
| `ma_nguoi_dung` | `uuid` | NOT NULL, FK -> `nguoi_dung.ma_nguoi_dung` | Người nhận nhắc nhở |
| `ma_deadline` | `uuid` | FK -> `deadline.ma_deadline`, NULL | Deadline cần nhắc |
| `ma_lich_thi` | `uuid` | FK -> `lich_thi.ma_lich_thi`, NULL | Lịch thi cần nhắc |
| `thoi_gian_nhac` | `timestamptz` | NOT NULL | Thời gian nhắc |
| `da_gui_luc` | `timestamptz` | NULL | Thời điểm đã gửi |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |

## 16. `ghi_chu`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_ghi_chu` | `uuid` | PK | Mã ghi chú |
| `ma_nguoi_dung` | `uuid` | NOT NULL, FK -> `nguoi_dung.ma_nguoi_dung` | Người tạo ghi chú |
| `ma_mon_hoc` | `uuid` | FK -> `mon_hoc.ma_mon_hoc`, NULL | Môn học liên quan |
| `tieu_de` | `varchar(255)` | NOT NULL | Tiêu đề ghi chú |
| `noi_dung` | `text` | NULL | Nội dung ghi chú |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `cap_nhat_luc` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 17. `bo_flashcard`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_bo` | `uuid` | PK | Mã bộ flashcard |
| `ma_nguoi_dung` | `uuid` | NOT NULL, FK -> `nguoi_dung.ma_nguoi_dung` | Người tạo bộ |
| `ma_mon_hoc` | `uuid` | FK -> `mon_hoc.ma_mon_hoc`, NULL | Môn học liên quan |
| `ten_bo` | `varchar(255)` | NOT NULL | Tên bộ flashcard |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `cap_nhat_luc` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 18. `flashcard`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_flashcard` | `uuid` | PK | Mã flashcard |
| `ma_bo` | `uuid` | NOT NULL, FK -> `bo_flashcard.ma_bo` | Bộ flashcard |
| `mat_truoc` | `text` | NOT NULL | Nội dung mặt trước |
| `mat_sau` | `text` | NOT NULL | Nội dung mặt sau |
| `so_lan_on` | `integer` | NOT NULL | Số lần đã ôn |
| `diem_ghi_nho` | `numeric(5,2)` | NOT NULL | Điểm ghi nhớ |
| `lan_on_cuoi_luc` | `timestamptz` | NULL | Lần ôn gần nhất |
| `lan_on_tiep_theo_luc` | `timestamptz` | NULL | Lần ôn tiếp theo |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `cap_nhat_luc` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 19. `nhom_hoc_tap`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_nhom` | `uuid` | PK | Mã nhóm học tập |
| `nguoi_tao` | `uuid` | NOT NULL, FK -> `nguoi_dung.ma_nguoi_dung` | Người tạo nhóm |
| `ten_nhom` | `varchar(255)` | NOT NULL | Tên nhóm |
| `ma_tham_gia` | `varchar(30)` | NOT NULL, UNIQUE | Mã tham gia nhóm |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `cap_nhat_luc` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 20. `thanh_vien_nhom`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_nhom` | `uuid` | PK, FK -> `nhom_hoc_tap.ma_nhom` | Nhóm học tập |
| `ma_nguoi_dung` | `uuid` | PK, FK -> `nguoi_dung.ma_nguoi_dung` | Thành viên nhóm |
| `vai_tro_trong_nhom` | `varchar(30)` | NOT NULL | Vai trò trong nhóm |
| `tham_gia_luc` | `timestamptz` | NOT NULL | Thời điểm tham gia |

Khóa chính: (`ma_nhom`, `ma_nguoi_dung`).

## 21. `cong_viec_nhom`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_cong_viec` | `uuid` | PK | Mã công việc nhóm |
| `ma_nhom` | `uuid` | NOT NULL, FK -> `nhom_hoc_tap.ma_nhom` | Nhóm học tập |
| `nguoi_duoc_giao` | `uuid` | FK -> `nguoi_dung.ma_nguoi_dung`, NULL | Người được giao |
| `tieu_de` | `varchar(255)` | NOT NULL | Tiêu đề công việc |
| `mo_ta` | `text` | NULL | Mô tả công việc |
| `trang_thai` | `varchar(30)` | NOT NULL | Trạng thái công việc |
| `han_hoan_thanh` | `timestamptz` | NULL | Hạn hoàn thành |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `cap_nhat_luc` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 22. `tai_lieu`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_tai_lieu` | `uuid` | PK | Mã tài liệu |
| `nguoi_tai_len` | `uuid` | NOT NULL, FK -> `nguoi_dung.ma_nguoi_dung` | Người tải lên |
| `ma_mon_hoc` | `uuid` | FK -> `mon_hoc.ma_mon_hoc`, NULL | Môn học liên quan |
| `ma_nhom` | `uuid` | FK -> `nhom_hoc_tap.ma_nhom`, NULL | Nhóm liên quan |
| `ma_ghi_chu` | `uuid` | FK -> `ghi_chu.ma_ghi_chu`, NULL | Ghi chú liên quan |
| `duong_dan_luu_tru` | `text` | NOT NULL, UNIQUE | Đường dẫn lưu trữ Firebase Storage |
| `ten_file` | `varchar(255)` | NOT NULL | Tên file |
| `loai_file` | `varchar(100)` | NULL | Loại file |
| `dung_luong` | `bigint` | NULL | Dung lượng file |
| `che_do_hien_thi` | `varchar(30)` | NOT NULL | Chế độ hiển thị |
| `trang_thai` | `varchar(30)` | NOT NULL | Trạng thái tài liệu |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `cap_nhat_luc` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 23. `bao_cao_tai_lieu`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_bao_cao` | `uuid` | PK | Mã báo cáo tài liệu |
| `ma_tai_lieu` | `uuid` | NOT NULL, FK -> `tai_lieu.ma_tai_lieu` | Tài liệu bị báo cáo |
| `nguoi_bao_cao` | `uuid` | NOT NULL, FK -> `nguoi_dung.ma_nguoi_dung` | Người báo cáo |
| `ly_do` | `text` | NOT NULL | Lý do báo cáo |
| `trang_thai` | `varchar(30)` | NOT NULL | Trạng thái xử lý |
| `nguoi_kiem_duyet` | `uuid` | FK -> `nguoi_dung.ma_nguoi_dung`, NULL | Người kiểm duyệt |
| `ket_qua_kiem_duyet` | `text` | NULL | Kết quả kiểm duyệt |
| `kiem_duyet_luc` | `timestamptz` | NULL | Thời điểm kiểm duyệt |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |

Ràng buộc bổ sung: UNIQUE (`ma_tai_lieu`, `nguoi_bao_cao`).

## 24. `thong_bao`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_thong_bao` | `uuid` | PK | Mã thông báo |
| `ma_nguoi_nhan` | `uuid` | NOT NULL, FK -> `nguoi_dung.ma_nguoi_dung` | Người nhận thông báo |
| `nguoi_tao` | `uuid` | FK -> `nguoi_dung.ma_nguoi_dung`, NULL | Người tạo thông báo |
| `tieu_de` | `varchar(255)` | NOT NULL | Tiêu đề thông báo |
| `noi_dung` | `text` | NOT NULL | Nội dung thông báo |
| `loai_thong_bao` | `varchar(50)` | NOT NULL | Loại thông báo |
| `da_gui_luc` | `timestamptz` | NULL | Thời điểm đã gửi |
| `da_doc_luc` | `timestamptz` | NULL | Thời điểm đã đọc |
| `tao_luc` | `timestamptz` | NOT NULL | Thời điểm tạo |

## 25. `nhat_ky_he_thong`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `ma_nhat_ky` | `uuid` | PK | Mã nhật ký |
| `nguoi_thuc_hien` | `uuid` | FK -> `nguoi_dung.ma_nguoi_dung`, NULL | Người thực hiện hành động |
| `muc_do` | `varchar(30)` | NOT NULL | Mức độ log |
| `hanh_dong` | `varchar(100)` | NOT NULL | Hành động |
| `bang_tac_dong` | `varchar(100)` | NULL | Bảng bị tác động |
| `ma_ban_ghi` | `uuid` | NULL | Mã bản ghi bị tác động |
| `noi_dung` | `text` | NULL | Nội dung log |
| `du_lieu_json` | `jsonb` | NULL | Dữ liệu bổ sung |
| `thoi_gian` | `timestamptz` | NOT NULL | Thời điểm ghi log |

## Danh Sách Quan Hệ Khóa Ngoại

| Table con | Khóa ngoại | Table cha | Quan hệ |
|---|---|---|---|
| `nguoi_dung` | `ma_vai_tro` | `vai_tro.ma_vai_tro` | N-1 |
| `ho_so_sinh_vien` | `ma_nguoi_dung` | `nguoi_dung.ma_nguoi_dung` | 1-1 |
| `ho_so_sinh_vien` | `ma_truong` | `truong_hoc.ma_truong` | N-1 |
| `phien_dang_nhap` | `ma_nguoi_dung` | `nguoi_dung.ma_nguoi_dung` | N-1 |
| `thang_diem` | `ma_truong` | `truong_hoc.ma_truong` | N-1 |
| `chi_tiet_thang_diem` | `ma_thang_diem` | `thang_diem.ma_thang_diem` | N-1 |
| `quy_che_hoc_luc` | `ma_truong` | `truong_hoc.ma_truong` | N-1 |
| `hoc_ky` | `ma_sinh_vien` | `ho_so_sinh_vien.ma_nguoi_dung` | N-1 |
| `mon_hoc` | `ma_hoc_ky` | `hoc_ky.ma_hoc_ky` | N-1 |
| `thanh_phan_diem` | `ma_mon_hoc` | `mon_hoc.ma_mon_hoc` | N-1 |
| `lich_hoc` | `ma_mon_hoc` | `mon_hoc.ma_mon_hoc` | N-1 |
| `lich_thi` | `ma_mon_hoc` | `mon_hoc.ma_mon_hoc` | N-1 |
| `deadline` | `ma_mon_hoc` | `mon_hoc.ma_mon_hoc` | N-1 |
| `nhac_nho` | `ma_nguoi_dung` | `nguoi_dung.ma_nguoi_dung` | N-1 |
| `nhac_nho` | `ma_deadline` | `deadline.ma_deadline` | N-1 |
| `nhac_nho` | `ma_lich_thi` | `lich_thi.ma_lich_thi` | N-1 |
| `ghi_chu` | `ma_nguoi_dung` | `nguoi_dung.ma_nguoi_dung` | N-1 |
| `ghi_chu` | `ma_mon_hoc` | `mon_hoc.ma_mon_hoc` | N-1 |
| `bo_flashcard` | `ma_nguoi_dung` | `nguoi_dung.ma_nguoi_dung` | N-1 |
| `bo_flashcard` | `ma_mon_hoc` | `mon_hoc.ma_mon_hoc` | N-1 |
| `flashcard` | `ma_bo` | `bo_flashcard.ma_bo` | N-1 |
| `nhom_hoc_tap` | `nguoi_tao` | `nguoi_dung.ma_nguoi_dung` | N-1 |
| `thanh_vien_nhom` | `ma_nhom` | `nhom_hoc_tap.ma_nhom` | N-1 |
| `thanh_vien_nhom` | `ma_nguoi_dung` | `nguoi_dung.ma_nguoi_dung` | N-1 |
| `cong_viec_nhom` | `ma_nhom` | `nhom_hoc_tap.ma_nhom` | N-1 |
| `cong_viec_nhom` | `nguoi_duoc_giao` | `nguoi_dung.ma_nguoi_dung` | N-1 |
| `tai_lieu` | `nguoi_tai_len` | `nguoi_dung.ma_nguoi_dung` | N-1 |
| `tai_lieu` | `ma_mon_hoc` | `mon_hoc.ma_mon_hoc` | N-1 |
| `tai_lieu` | `ma_nhom` | `nhom_hoc_tap.ma_nhom` | N-1 |
| `tai_lieu` | `ma_ghi_chu` | `ghi_chu.ma_ghi_chu` | N-1 |
| `bao_cao_tai_lieu` | `ma_tai_lieu` | `tai_lieu.ma_tai_lieu` | N-1 |
| `bao_cao_tai_lieu` | `nguoi_bao_cao` | `nguoi_dung.ma_nguoi_dung` | N-1 |
| `bao_cao_tai_lieu` | `nguoi_kiem_duyet` | `nguoi_dung.ma_nguoi_dung` | N-1 |
| `thong_bao` | `ma_nguoi_nhan` | `nguoi_dung.ma_nguoi_dung` | N-1 |
| `thong_bao` | `nguoi_tao` | `nguoi_dung.ma_nguoi_dung` | N-1 |
| `nhat_ky_he_thong` | `nguoi_thuc_hien` | `nguoi_dung.ma_nguoi_dung` | N-1 |
