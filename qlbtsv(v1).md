# Cấu Trúc Table Database QUẢN LÝ BÀI TẬP CHO SINH VIÊN

## Tổng Quan

- Database: `QLBTSV`
- Tổng số bảng: `26`
- Tên bảng dùng tiếng Việt không dấu.
- Các trường trạng thái và phân loại được chuẩn hóa bằng kiểu dữ liệu `ENUM` của PostgreSQL.
- Thêm các contraint, ràng buộc, index cho database hệ thống

## Danh Sách Các PostgreSQL ENUM Định Nghĩa Trước

các kiểu dữ liệu ENUM :

1. `enum_trang_thai_nguoi_dung` (`HOAT_DONG`, `BI_KHOA`, `CHUA_XAC_THUC`)
2. `enum_trang_thai_deadline` (`CHUA_LAM`, `DANG_LAM`, `HOAN_THANH`, `TRE_HAN`)
3. `enum_vai_tro_nhom` (`TRUONG_NHOM`, `THANH_VIEN`)
4. `enum_trang_thai_cong_viec` (`CHUA_BAT_DAU`, `DANG_THUC_HIEN`, `HOAN_THANH`, `TRE_HAN`)
5. `enum_che_do_hien_thi` (`CONG_KHAI`, `RIENG_TU`, `CHIA_SE_NHOM`)
6. `enum_trang_thai_tai_lieu` (`KHA_DUNG`, `DA_XOA`, `CHO_KIEM_DUYET`)
7. `enum_trang_thai_bao_cao` (`CHO_XU_LY`, `DA_DUYET`, `DA_TU_CHOI`)
8. `enum_loai_thong_bao` (`DEADLINE`, `HE_THONG`, `NHOM_HOC_TAP`, `NHAC_NHO`)
9. `enum_muc_do_log` (`INFO`, `WARNING`, `ERROR`, `CRITICAL`)

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
22. binh_luan_cong_viec
23. tai_lieu
24. bao_cao_tai_lieu
25. thong_bao
26. nhat_ky_he_thong
```

## Cấu Trúc Từng Table

## 1. `vai_tro`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_VAI_TRO` | `INT` | PK | Mã vai trò |
| `MA_CODE` | `varchar(50)` | NOT NULL, UNIQUE | Mã code vai trò |
| `TEN_VAI_TRO` | `varchar(100)` | NOT NULL | Tên vai trò |

## 2. `nguoi_dung`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_NGUOI_DUNG` | `uuid` | PK | Mã người dùng |
| `MA_VAI_TRO` | `INT` | NOT NULL, FK -> `vai_tro.MA_VAI_TRO` | Vai trò chính của người dùng |
| `EMAIL` | `varchar(255)` | NOT NULL, UNIQUE | Email đăng nhập |
| `MAT_KHAU` | `varchar(255)` | NOT NULL | Mật khẩu đã mã hóa |
| `HO_TEN` | `varchar(150)` | NOT NULL | Họ tên người dùng |
| `SO_DIEN_THOAI` | `varchar(20)` | NULL | Số điện thoại |
| `ANH_DAI_DIEN` | `text` | NULL | Đường dẫn ảnh đại diện |
| `TRANG_THAI` | `enum_trang_thai_nguoi_dung` | NOT NULL | Trạng thái tài khoản |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |
| `SO_GIO_NHAC_DEADLINE` | ` smallint` | NULL | Giờ nhắc deadline|
| `NHAN_THONG_BAO_DAY` | ` boolean` | NOT NULL | Cho phép người dùng tắt hoặc mở để nhận thông báo|

## 3. `truong_hoc`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_TRUONG` | `INT` | PK | Mã trường |
| `MA_TRUONG_CODE` | `varchar(50)` | NOT NULL, UNIQUE | Mã code trường |
| `TEN_TRUONG` | `varchar(255)` | NOT NULL | Tên trường |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 4. `ho_so_sinh_vien`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_NGUOI_DUNG` | `uuid` | PK, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Mã người dùng của sinh viên |
| `MA_TRUONG` | `INT` | FK -> `truong_hoc.MA_TRUONG`, NULL | Trường của sinh viên |
| `MA_SINH_VIEN` | `varchar(50)` | NOT NULL | Mã sinh viên |
| `NGANH_HOC` | `varchar(150)` | NULL | Ngành học |
| `KHOA_HOC` | `varchar(50)` | NULL | Khóa học |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`MA_TRUONG`, `MA_SINH_VIEN`).

## 5. `phien_dang_nhap`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_PHIEN` | `uuid` | PK | Mã phiên đăng nhập |
| `MA_NGUOI_DUNG` | `uuid` | NOT NULL, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Người dùng sở hữu phiên |
| `REFRESH_TOKEN` | `text` | NOT NULL, UNIQUE | Refresh token |
| `FCM_TOKEN` | `text` | UNIQUE nếu khác NULL | Token gửi thông báo FCM |
| `LOAI_THIET_BI` | `varchar(50)` | NULL | Loại thiết bị |
| `IP_ADDRESS` | `inet` | NULL | Địa chỉ IP |
| `USER_AGENT` | `text` | NULL | Thông tin trình duyệt/thiết bị |
| `THOI_GIAN_HET_HAN` | `timestamptz` | NOT NULL | Thời điểm hết hạn |
| `THOI_GIAN_THU_HOI` | `timestamptz` | NULL | Thời điểm bị thu hồi |
| `LAN_HOAT_DONG_CUOI` | `timestamptz` | NOT NULL | Lần hoạt động cuối |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |

## 6. `thang_diem`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_THANG_DIEM` | `INT` | PK | Mã thang điểm |
| `MA_TRUONG` | `INT` | NOT NULL, FK -> `truong_hoc.MA_TRUONG` | Trường áp dụng |
| `TEN_THANG_DIEM` | `varchar(100)` | NOT NULL | Tên thang điểm |
| `DIEM_THAP_NHAT` | `numeric(4,2)` | NOT NULL | Điểm thấp nhất |
| `DIEM_CAO_NHAT` | `numeric(4,2)` | NOT NULL | Điểm cao nhất |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`MA_TRUONG`, `TEN_THANG_DIEM`).

## 7. `chi_tiet_thang_diem`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_CHI_TIET` | `INT` | PK | Mã chi tiết thang điểm |
| `MA_THANG_DIEM` | `INT` | NOT NULL, FK -> `thang_diem.MA_THANG_DIEM` | Thang điểm cha |
| `DIEM_CHU` | `varchar(10)` | NOT NULL | Điểm chữ |
| `DIEM_HE_4` | `numeric(3,2)` | NOT NULL | Điểm hệ 4 |
| `DIEM_THAP_NHAT` | `numeric(4,2)` | NOT NULL | Điểm tối thiểu |
| `DIEM_CAO_NHAT` | `numeric(4,2)` | NOT NULL | Điểm tối đa |

Ràng buộc bổ sung: UNIQUE (`MA_THANG_DIEM`, `DIEM_CHU`).

## 8. `quy_che_hoc_luc`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_QUY_CHE` | `INT` | PK | Mã quy chế |
| `MA_TRUONG` | `INT` | NOT NULL, FK -> `truong_hoc.MA_TRUONG` | Trường áp dụng |
| `TEN_XEP_LOAI` | `varchar(100)` | NOT NULL | Tên xếp loại học lực |
| `GPA_TOI_THIEU` | `numeric(3,2)` | NOT NULL | GPA tối thiểu |
| `GPA_TOI_DA` | `numeric(3,2)` | NOT NULL | GPA tối đa |

Ràng buộc bổ sung: UNIQUE (`MA_TRUONG`, `TEN_XEP_LOAI`).

## 9. `hoc_ky`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_HOC_KY` | `uuid` | PK | Mã học kỳ |
| `MA_NGUOI_DUNG` | `uuid` | NOT NULL, FK -> `ho_so_sinh_vien.MA_NGUOI_DUNG` | Tài khoản sinh viên sở hữu học kỳ |
| `TEN_HOC_KY` | `varchar(100)` | NOT NULL | Tên học kỳ |
| `NGAY_BAT_DAU` | `date` | NULL | Ngày bắt đầu |
| `NGAY_KET_THUC` | `date` | NULL | Ngày kết thúc |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`MA_NGUOI_DUNG`, `TEN_HOC_KY`).

## 10. `mon_hoc`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_MON_HOC` | `uuid` | PK | Mã môn học |
| `MA_HOC_KY` | `uuid` | NOT NULL, FK -> `hoc_ky.MA_HOC_KY` | Học kỳ chứa môn học |
| `MA_MON` | `varchar(50)` | NULL | Mã môn |
| `TEN_MON` | `varchar(255)` | NOT NULL | Tên môn học |
| `SO_TIN_CHI` | `smallint` | NOT NULL | Số tín chỉ |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`MA_HOC_KY`, `MA_MON`).

## 11. `thanh_phan_diem`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_THANH_PHAN` | `INT` | PK | Mã thành phần điểm |
| `MA_MON_HOC` | `uuid` | NOT NULL, FK -> `mon_hoc.MA_MON_HOC` | Môn học liên kết |
| `TEN_THANH_PHAN` | `varchar(100)` | NOT NULL | Tên thành phần điểm |
| `TRONG_SO` | `numeric(5,2)` | NOT NULL | Trọng số điểm |
| `DIEM` | `numeric(4,2)` | NULL | Điểm đạt được |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`MA_MON_HOC`, `TEN_THANH_PHAN`).

## 12. `lich_hoc`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_LICH_HOC` | `uuid` | PK | Mã lịch học |
| `MA_MON_HOC` | `uuid` | NOT NULL, FK -> `mon_hoc.MA_MON_HOC` | Môn học |
| `THU` | `smallint` | NOT NULL | Thứ trong tuần |
| `TIET_BAT_DAU` | `smallint` | NOT NULL | Tiết bắt đầu |
| `SO_TIET` | `smallint` | NOT NULL | Số tiết |
| `PHONG_HOC` | `varchar(100)` | NULL | Phòng học |
| `NGAY_BAT_DAU` | `date` | NULL | Ngày bắt đầu |
| `NGAY_KET_THUC` | `date` | NULL | Ngày kết thúc |

## 13. `lich_thi`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_LICH_THI` | `uuid` | PK | Mã lịch thi |
| `MA_MON_HOC` | `uuid` | NOT NULL, FK -> `mon_hoc.MA_MON_HOC` | Môn học |
| `LOAI_THI` | `varchar(100)` | NULL | Loại thi |
| `THOI_GIAN_THI` | `timestamptz` | NOT NULL | Thời gian thi |
| `PHONG_THI` | `varchar(100)` | NULL | Phòng thi |

## 14. `deadline`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_DEADLINE` | `uuid` | PK | Mã deadline |
| `MA_MON_HOC` | `uuid` | NOT NULL, FK -> `mon_hoc.MA_MON_HOC` | Môn học |
| `TIEU_DE` | `varchar(255)` | NOT NULL | Tiêu đề deadline |
| `MO_TA` | `text` | NULL | Mô tả |
| `HAN_NOP` | `timestamptz` | NOT NULL | Hạn nộp |
| `TRANG_THAI` | `enum_trang_thai_deadline` | NOT NULL | Trạng thái công việc |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 15. `nhac_nho`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_NHAC_NHO` | `uuid` | PK | Mã nhắc nhở |
| `MA_NGUOI_DUNG` | `uuid` | NOT NULL, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Người nhận nhắc nhở |
| `MA_DEADLINE` | `uuid` | FK -> `deadline.MA_DEADLINE`, NULL | Deadline cần nhắc |
| `MA_LICH_THI` | `uuid` | FK -> `lich_thi.MA_LICH_THI`, NULL | Lịch thi cần nhắc |
| `THOI_GIAN_NHAC` | `timestamptz` | NOT NULL | Thời gian nhắc |
| `THOI_GIAN_DA_GUI` | `timestamptz` | NULL | Thời điểm đã gửi |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |

## 16. `ghi_chu`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_GHI_CHU` | `uuid` | PK | Mã ghi chú |
| `MA_NGUOI_DUNG` | `uuid` | NOT NULL, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Người tạo ghi chú |
| `MA_MON_HOC` | `uuid` | FK -> `mon_hoc.MA_MON_HOC`, NULL | Môn học liên quan |
| `TIEU_DE` | `varchar(255)` | NOT NULL | Tiêu đề ghi chú |
| `NOI_DUNG` | `text` | NULL | Nội dung ghi chú |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 17. `bo_flashcard`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_BO` | `uuid` | PK | Mã bộ flashcard |
| `MA_NGUOI_DUNG` | `uuid` | NOT NULL, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Người tạo bộ |
| `MA_MON_HOC` | `uuid` | FK -> `mon_hoc.MA_MON_HOC`, NULL | Môn học liên quan |
| `TEN_BO` | `varchar(255)` | NOT NULL | Tên bộ flashcard |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 18. `flashcard`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_FLASHCARD` | `uuid` | PK | Mã flashcard |
| `MA_BO` | `uuid` | NOT NULL, FK -> `bo_flashcard.MA_BO` | Bộ flashcard |
| `MAT_TRUOC` | `text` | NOT NULL | Nội dung mặt trước |
| `MAT_SAU` | `text` | NOT NULL | Nội dung mặt sau |
| `SO_LAN_ON` | `integer` | NOT NULL | Số lần đã ôn |
| `DIEM_GHI_NHO` | `numeric(5,2)` | NOT NULL | Điểm ghi nhớ |
| `THOI_GIAN_LAN_ON_CUOI` | `timestamptz` | NULL | Lần ôn gần nhất |
| `THOI_GIAN_LAN_ON_TIEP_THEO` | `timestamptz` | NULL | Lần ôn tiếp theo |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 19. `nhom_hoc_tap`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_NHOM` | `uuid` | PK | Mã nhóm học tập |
| `NGUOI_TAO` | `uuid` | NOT NULL, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Người tạo nhóm |
| `MA_TRUONG` | `int` | NOT NULL, FK -> `truong_hoc.MA_TRUONG` | Mã trường học|
| `MA_MON` | `varchar(50)` | NULL | Mã môn học của trường|
| `TEN_NHOM` | `varchar(255)` | NOT NULL | Tên nhóm |
| `MA_THAM_GIA` | `varchar(30)` | NOT NULL, UNIQUE | Mã tham gia nhóm |
| `LINK_NHOM_CHAT` | `text` | NOT NULL | Đường link nhóm chat Zalo/Messenger/Discord |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 20. `thanh_vien_nhom`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_NHOM` | `uuid` | PK, FK -> `nhom_hoc_tap.MA_NHOM` | Nhóm học tập |
| `MA_NGUOI_DUNG` | `uuid` | PK, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Thành viên nhóm |
| `VAI_TRO_TRONG_NHOM` | `enum_vai_tro_nhom` | NOT NULL | Vai trò điều hành nhóm |
| `THOI_GIAN_THAM_GIA` | `timestamptz` | NOT NULL | Thời điểm tham gia |

Khóa chính: (`MA_NHOM`, `MA_NGUOI_DUNG`).

## 21. `cong_viec_nhom`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_CONG_VIEC` | `uuid` | PK | Mã công việc nhóm |
| `MA_NHOM` | `uuid` | NOT NULL, FK -> `nhom_hoc_tap.MA_NHOM` | Nhóm học tập |
| `NGUOI_DUOC_GIAO` | `uuid` | FK -> `nguoi_dung.MA_NGUOI_DUNG`, NULL | Người được giao |
| `TIEU_DE` | `varchar(255)` | NOT NULL | Tiêu đề công việc |
| `MO_TA` | `text` | NULL | Mô tả công việc |
| `TRANG_THAI` | `enum_trang_thai_cong_viec` | NOT NULL | Trạng thái công việc |
| `HAN_HOAN_THANH` | `timestamptz` | NULL | Hạn hoàn thành |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 22. `binh_luan_cong_viec`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả|
|---|---|---|---|
| `MA_BINH_LUAN` | `uuid` | PK | Mã bình luận |
| `MA_CONG_VIEC` | `uuid` | NOT NULL, FK -> `cong_viec_nhom.MA_CONG_VIEC` | Công việc được bình luận |
| `MA_NGUOI_DUNG` | `uuid` | NOT NULL, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Người viết bình luận |
| `NOI_DUNG` | `text` | NOT NULL | Nội dung bình luận |
| `THOI_GIAN_tao` | `timestamptz` | NOT NULL | Thời điểm tạo bình luận | 

## 23. `tai_lieu`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_TAI_LIEU` | `uuid` | PK | Mã tài liệu |
| `NGUOI_TAI_LEN` | `uuid` | NOT NULL, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Người tải lên |
| `MA_MON_HOC` | `uuid` | FK -> `mon_hoc.MA_MON_HOC`, NULL | Môn học liên quan |
| `MA_NHOM` | `uuid` | FK -> `nhom_hoc_tap.MA_NHOM`, NULL | Nhóm liên quan |
| `MA_GHI_CHU` | `uuid` | FK -> `ghi_chu.MA_GHI_CHU`, NULL | Ghi chú liên quan |
| `DUONG_DAN_LUU_TRU` | `text` | NOT NULL, UNIQUE | Đường dẫn lưu trữ Firebase Storage |
| `TEN_FILE` | `varchar(255)` | NOT NULL | Tên file |
| `LOAI_FILE` | `varchar(100)` | NULL | Loại file |
| `DUNG_LUONG` | `bigint` | NULL | Dung lượng file |
| `CHE_DO_HIEN_THI` | `enum_che_do_hien_thi` | NOT NULL | Chế độ hiển thị chia sẻ |
| `TRANG_THAI` | `enum_trang_thai_tai_lieu` | NOT NULL | Trạng thái kiểm duyệt file |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 24. `bao_cao_tai_lieu`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_BAO_CAO` | `uuid` | PK | Mã báo cáo tài liệu |
| `MA_TAI_LIEU` | `uuid` | NOT NULL, FK -> `tai_lieu.MA_TAI_LIEU` | Tài liệu bị báo cáo |
| `NGUOI_BAO_CAO` | `uuid` | NOT NULL, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Người báo cáo |
| `LY_DO` | `text` | NOT NULL | Lý do báo cáo |
| `TRANG_THAI` | `enum_trang_thai_bao_cao` | NOT NULL | Tiến độ xử lý report |
| `NGUOI_KIEM_DUYET` | `uuid` | FK -> `nguoi_dung.MA_NGUOI_DUNG`, NULL | Người kiểm duyệt |
| `KET_QUA_KIEM_DUYET` | `text` | NULL | Kết quả kiểm duyệt |
| `THOI_GIAN_KIEM_DUYET` | `timestamptz` | NULL | Thời điểm kiểm duyệt |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |

Ràng buộc bổ sung: UNIQUE (`MA_TAI_LIEU`, `NGUOI_BAO_CAO`).

## 25. `thong_bao`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_THONG_BAO` | `uuid` | PK | Mã thông báo |
| `MA_NGUOI_NHAN` | `uuid` | NOT NULL, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Người nhận thông báo |
| `NGUOI_TAO` | `uuid` | FK -> `nguoi_dung.MA_NGUOI_DUNG`, NULL | Người tạo thông báo |
| `TIEU_DE` | `varchar(255)` | NOT NULL | Tiêu đề thông báo |
| `NOI_DUNG` | `text` | NOT NULL | Nội dung thông báo |
| `LOAI_THONG_BAO` | `enum_loai_thong_bao` | NOT NULL | Phân loại thông báo |
| `THOI_GIAN_DA_GUI` | `timestamptz` | NULL | Thời điểm đã gửi |
| `THOI_GIAN_DA_DOC` | `timestamptz` | NULL | Thời điểm đã đọc |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |

## 26. `nhat_ky_he_thong`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_NHAT_KY` | `BIGINT` | PK | Mã nhật ký |
| `NGUOI_THUC_HIEN` | `uuid` | FK -> `nguoi_dung.MA_NGUOI_DUNG`, NULL | Người thực hiện hành động |
| `MUC_DO` | `enum_muc_do_log` | NOT NULL | Mức độ nghiêm trọng của log |
| `HANH_DONG` | `varchar(100)` | NOT NULL | Hành động thực hiện |
| `BANG_TAC_DONG` | `varchar(100)` | NULL | Bảng bị tác động |
| `MA_BAN_GHI` | `varchar(100)` | NULL | Mã bản ghi bị ảnh hưởng |
| `NOI_DUNG` | `text` | NULL | Nội dung log chi tiết |
| `DU_LIEU_JSON` | `jsonb` | NULL | Dữ liệu log bổ sung |
| `THOI_GIAN` | `timestamptz` | NOT NULL | Thời điểm ghi log |

## Danh Sách Quan Hệ Khóa Ngoại

| Table con | Khóa ngoại | Table cha | Quan hệ |
|---|---|---|---|
| `nguoi_dung` | `MA_VAI_TRO` | `vai_tro.MA_VAI_TRO` | N-1 |
| `ho_so_sinh_vien` | `MA_NGUOI_DUNG` | `nguoi_dung.MA_NGUOI_DUNG` | 1-1 |
| `ho_so_sinh_vien` | `MA_TRUONG` | `truong_hoc.MA_TRUONG` | N-1 |
| `phien_dang_nhap` | `MA_NGUOI_DUNG` | `nguoi_dung.MA_NGUOI_DUNG` | N-1 |
| `thang_diem` | `MA_TRUONG` | `truong_hoc.MA_TRUONG` | N-1 |
| `chi_tiet_thang_diem` | `MA_THANG_DIEM` | `thang_diem.MA_THANG_DIEM` | N-1 |
| `quy_che_hoc_luc` | `MA_TRUONG` | `truong_hoc.MA_TRUONG` | N-1 |
| `hoc_ky` | `MA_NGUOI_DUNG` | `ho_so_sinh_vien.MA_NGUOI_DUNG` | N-1 |
| `mon_hoc` | `MA_HOC_KY` | `hoc_ky.MA_HOC_KY` | N-1 |
| `thanh_phan_diem` | `MA_MON_HOC` | `mon_hoc.MA_MON_HOC` | N-1 |
| `lich_hoc` | `MA_MON_HOC` | `mon_hoc.MA_MON_HOC` | N-1 |
| `lich_thi` | `MA_MON_HOC` | `mon_hoc.MA_MON_HOC` | N-1 |
| `deadline` | `MA_MON_HOC` | `mon_hoc.MA_MON_HOC` | N-1 |
| `nhac_nho` | `MA_NGUOI_DUNG` | `nguoi_dung.MA_NGUOI_DUNG` | N-1 |
| `nhac_nho` | `MA_DEADLINE` | `deadline.MA_DEADLINE` | N-1 |
| `nhac_nho` | `MA_LICH_THI` | `lich_thi.MA_LICH_THI` | N-1 |
| `ghi_chu` | `MA_NGUOI_DUNG` | `nguoi_dung.MA_NGUOI_DUNG` | N-1 |
| `ghi_chu` | `MA_MON_HOC` | `mon_hoc.MA_MON_HOC` | N-1 |
| `bo_flashcard` | `MA_NGUOI_DUNG` | `nguoi_dung.MA_NGUOI_DUNG` | N-1 |
| `bo_flashcard` | `MA_MON_HOC` | `mon_hoc.MA_MON_HOC` | N-1 |
| `flashcard` | `MA_BO` | `bo_flashcard.MA_BO` | N-1 |
| `nhom_hoc_tap` | `NGUOI_TAO` | `nguoi_dung.MA_NGUOI_DUNG` | N-1 |
| `thanh_vien_nhom` | `MA_NHOM` | `nhom_hoc_tap.MA_NHOM` | N-1 |
| `thanh_vien_nhom` | `MA_NGUOI_DUNG` | `nguoi_dung.MA_NGUOI_DUNG` | N-1 |
| `cong_viec_nhom` | `MA_NHOM` | `nhom_hoc_tap.MA_NHOM` | N-1 |
| `cong_viec_nhom` | `NGUOI_DUOC_GIAO` | `nguoi_dung.MA_NGUOI_DUNG` | N-1 |
| `binh_luan_cong_viec` | `MA_CONG_VIEC` | `cong_viec.MA_CONG_VIEC` | N-1 |
| `binh_luan_cong_viec` | `MA_NGUOI_DUNG` | `nguoi_dung.MA_NGUOI_DUNG` | N-1 |
| `tai_lieu` | `NGUOI_TAI_LEN` | `nguoi_dung.MA_NGUOI_DUNG` | N-1 |
| `tai_lieu` | `MA_MON_HOC` | `mon_hoc.MA_MON_HOC` | N-1 |
| `tai_lieu` | `MA_NHOM` | `nhom_hoc_tap.MA_NHOM` | N-1 |
| `tai_lieu` | `MA_GHI_CHU` | `ghi_chu.MA_GHI_CHU` | N-1 |
| `bao_cao_tai_lieu` | `MA_TAI_LIEU` | `tai_lieu.MA_TAI_LIEU` | N-1 |
| `bao_cao_tai_lieu` | `NGUOI_BAO_CAO` | `nguoi_dung.MA_NGUOI_DUNG` | N-1 |
| `bao_cao_tai_lieu` | `NGUOI_KIEM_DUYET` | `nguoi_dung.MA_NGUOI_DUNG` | N-1 |
| `thong_bao` | `MA_NGUOI_NHAN` | `nguoi_dung.MA_NGUOI_DUNG` | N-1 |
| `thong_bao` | `NGUOI_TAO` | `nguoi_dung.MA_NGUOI_DUNG` | N-1 |
| `nhat_ky_he_thong` | `NGUOI_THUC_HIEN` | `nguoi_dung.MA_NGUOI_DUNG` | N-1 |

## Ràng Buộc Bổ Sung

| Nhóm/Bảng | Ràng buộc | Mục đích |
|---|---|---|
| `nguoi_dung` | `EMAIL` phải UNIQUE và NOT NULL | Không cho trùng tài khoản đăng nhập |
| `nguoi_dung` | `TRANG_THAI` chỉ nhận giá trị trong `enum_trang_thai_nguoi_dung` | Kiểm soát trạng thái tài khoản |
| `nguoi_dung` | Khi `TRANG_THAI = 'BI_KHOA'`, các phiên trong `phien_dang_nhap` phải được thu hồi | Đảm bảo tài khoản bị khóa không còn dùng token cũ |
| `phien_dang_nhap` | `REFRESH_TOKEN` phải UNIQUE và NOT NULL | Mỗi phiên đăng nhập có một refresh token riêng |
| `phien_dang_nhap` | `FCM_TOKEN` nên UNIQUE nếu khác NULL | Tránh một thiết bị nhận trùng push notification |
| `phien_dang_nhap` | `THOI_GIAN_HET_HAN` phải lớn hơn `THOI_GIAN_TAO` | Không tạo phiên hết hạn ngay khi vừa đăng nhập |
| `ho_so_sinh_vien` | UNIQUE (`MA_TRUONG`, `MA_SINH_VIEN`) | Một mã sinh viên không bị trùng trong cùng trường |
| `thang_diem` | UNIQUE (`MA_TRUONG`, `TEN_THANG_DIEM`) | Mỗi trường không có hai thang điểm trùng tên |
| `thang_diem` | `DIEM_THAP_NHAT < DIEM_CAO_NHAT` | Đảm bảo khoảng điểm hợp lệ |
| `chi_tiet_thang_diem` | UNIQUE (`MA_THANG_DIEM`, `DIEM_CHU`) | Một thang điểm không có hai dòng điểm chữ trùng nhau |
| `chi_tiet_thang_diem` | `DIEM_THAP_NHAT <= DIEM_CAO_NHAT` | Đảm bảo khoảng quy đổi điểm hợp lệ |
| `quy_che_hoc_luc` | UNIQUE (`MA_TRUONG`, `TEN_XEP_LOAI`) | Mỗi trường không có hai mức xếp loại trùng tên |
| `quy_che_hoc_luc` | `GPA_TOI_THIEU <= GPA_TOI_DA` | Đảm bảo khoảng GPA hợp lệ |
| `hoc_ky` | UNIQUE (`MA_NGUOI_DUNG`, `TEN_HOC_KY`) | Một sinh viên không có hai học kỳ trùng tên |
| `hoc_ky` | `NGAY_BAT_DAU <= NGAY_KET_THUC` nếu cả hai khác NULL | Đảm bảo thời gian học kỳ hợp lệ |
| `mon_hoc` | UNIQUE (`MA_HOC_KY`, `MA_MON`) | Một học kỳ không có hai môn trùng mã |
| `mon_hoc` | `SO_TIN_CHI > 0` | Số tín chỉ phải hợp lệ |
| `thanh_phan_diem` | UNIQUE (`MA_MON_HOC`, `TEN_THANH_PHAN`) | Một môn không có hai thành phần điểm trùng tên |
| `thanh_phan_diem` | `TRONG_SO > 0 AND TRONG_SO <= 100` | Trọng số từng thành phần phải hợp lệ |
| `thanh_phan_diem` | `DIEM` nằm trong khoảng `0..10` nếu khác NULL | Điểm thành phần theo thang 10 |
| `thanh_phan_diem` | Tổng `TRONG_SO` của một `MA_MON_HOC` nên bằng 100 | Đảm bảo tính điểm tổng kết chính xác |
| `lich_hoc` | `THU` nằm trong khoảng `2..8` | Chuẩn hóa thứ trong tuần |
| `lich_hoc` | `TIET_BAT_DAU > 0` và `SO_TIET > 0` | Đảm bảo tiết học hợp lệ |
| `lich_hoc` | Không cho trùng lịch học của cùng sinh viên trong cùng thời điểm | Tránh xung đột thời khóa biểu |
| `lich_thi` | `THOI_GIAN_THI` phải là thời điểm hợp lệ | Phục vụ nhắc lịch thi |
| `deadline` | `TRANG_THAI` chỉ nhận giá trị trong `enum_trang_thai_deadline` | Chuẩn hóa trạng thái deadline |
| `deadline` | Tự chuyển sang trạng thái trễ hạn khi quá `HAN_NOP` mà chưa hoàn thành | Phục vụ quy tắc deadline |
| `nhac_nho` | Chỉ một trong hai cột `MA_DEADLINE`, `MA_LICH_THI` được dùng cho một bản ghi | Một nhắc nhở gắn với một đối tượng chính |
| `nhac_nho` | `THOI_GIAN_NHAC` phải trước hoặc bằng thời gian sự kiện cần nhắc | Không tạo nhắc nhở sau hạn |
| `thanh_vien_nhom` | PK (`MA_NHOM`, `MA_NGUOI_DUNG`) | Một người chỉ xuất hiện một lần trong cùng nhóm |
| `thanh_vien_nhom` | `VAI_TRO_TRONG_NHOM` chỉ nhận giá trị trong `enum_vai_tro_nhom` | Chuẩn hóa vai trò nhóm |
| `thanh_vien_nhom` | Nên có tối đa một `TRUONG_NHOM` trong mỗi `MA_NHOM` | Đảm bảo quyền điều hành nhóm rõ ràng |
| `cong_viec_nhom` | `TRANG_THAI` chỉ nhận giá trị trong `enum_trang_thai_cong_viec` | Chuẩn hóa trạng thái Kanban |
| `cong_viec_nhom` | `NGUOI_DUOC_GIAO` nên là thành viên của cùng `MA_NHOM` | Không giao task cho người ngoài nhóm |
| `tai_lieu` | `DUONG_DAN_LUU_TRU` phải UNIQUE và NOT NULL | Một file lưu trữ có một bản ghi metadata |
| `tai_lieu` | `DUNG_LUONG >= 0` nếu khác NULL | Dung lượng file không âm |
| `tai_lieu` | `CHE_DO_HIEN_THI` chỉ nhận giá trị trong `enum_che_do_hien_thi` | Chuẩn hóa quyền hiển thị |
| `tai_lieu` | `TRANG_THAI` chỉ nhận giá trị trong `enum_trang_thai_tai_lieu` | Chuẩn hóa trạng thái tài liệu |
| `tai_lieu` | Một tài liệu nên gắn tối đa một ngữ cảnh chính: môn học, nhóm hoặc ghi chú | Tránh mơ hồ nơi tài liệu được sử dụng |
| `bao_cao_tai_lieu` | UNIQUE (`MA_TAI_LIEU`, `NGUOI_BAO_CAO`) | Một người không report cùng một tài liệu nhiều lần |
| `bao_cao_tai_lieu` | `TRANG_THAI` chỉ nhận giá trị trong `enum_trang_thai_bao_cao` | Chuẩn hóa tiến độ xử lý report |
| `bao_cao_tai_lieu` | Tài liệu có nhiều report hợp lệ có thể tự chuyển sang trạng thái chờ kiểm duyệt/ẩn | Phục vụ kiểm duyệt cộng đồng |
| `flashcard` | `SO_LAN_ON >= 0` | Số lần ôn không âm |
| `flashcard` | `DIEM_GHI_NHO >= 0` | Điểm ghi nhớ hợp lệ |
| `flashcard` | Sau mỗi lần ôn, cập nhật `THOI_GIAN_LAN_ON_CUOI` và `THOI_GIAN_LAN_ON_TIEP_THEO` | Phục vụ spaced repetition |
| `thong_bao` | `LOAI_THONG_BAO` chỉ nhận giá trị trong `enum_loai_thong_bao` | Chuẩn hóa loại thông báo |
| `thong_bao` | `THOI_GIAN_DA_DOC >= THOI_GIAN_DA_GUI` nếu cả hai khác NULL | Đảm bảo thời gian đọc hợp lệ |
| `nhat_ky_he_thong` | `MUC_DO` chỉ nhận giá trị trong `enum_muc_do_log` | Chuẩn hóa mức độ log |

## Index Đề Xuất

| Tên index đề xuất | Bảng | Cột / điều kiện | Mục đích |
|---|---|---|---|
| `idx_nguoi_dung_vai_tro_trang_thai` | `nguoi_dung` | (`MA_VAI_TRO`, `TRANG_THAI`) | Lọc user theo role và trạng thái |
| `idx_phien_dang_nhap_user` | `phien_dang_nhap` | (`MA_NGUOI_DUNG`) | Lấy danh sách phiên của một người dùng |
| `idx_phien_dang_nhap_active` | `phien_dang_nhap` | (`MA_NGUOI_DUNG`, `THOI_GIAN_HET_HAN`) WHERE `THOI_GIAN_THU_HOI IS NULL` | Tìm phiên còn hoạt động |
| `uq_phien_dang_nhap_fcm_token` | `phien_dang_nhap` | (`FCM_TOKEN`) WHERE `FCM_TOKEN IS NOT NULL` | Đảm bảo FCM token không bị gán trùng |
| `idx_ho_so_sinh_vien_truong` | `ho_so_sinh_vien` | (`MA_TRUONG`) | Lọc sinh viên theo trường |
| `idx_hoc_ky_sinh_vien` | `hoc_ky` | (`MA_NGUOI_DUNG`) | Lấy học kỳ theo sinh viên |
| `idx_mon_hoc_hoc_ky` | `mon_hoc` | (`MA_HOC_KY`) | Lấy môn học theo học kỳ |
| `idx_thanh_phan_diem_mon` | `thanh_phan_diem` | (`MA_MON_HOC`) | Tính điểm/GPA theo môn |
| `idx_lich_hoc_mon` | `lich_hoc` | (`MA_MON_HOC`) | Lấy lịch học của môn |
| `idx_lich_hoc_thu_tiet` | `lich_hoc` | (`THU`, `TIET_BAT_DAU`, `SO_TIET`) | Hỗ trợ kiểm tra trùng thời khóa biểu (XEM LẠI CÓ THỂ TỐI ƯU BẰNG KIỂU DỮ LIỆU RANGE KẾT HỢP VỚI INDEX GiST VÀ RÀNG BUỘC EXCLUDE)|
| `idx_lich_thi_mon_time` | `lich_thi` | (`MA_MON_HOC`, `THOI_GIAN_THI`) | Lấy lịch thi và nhắc lịch |
| `idx_deadline_mon` | `deadline` | (`MA_MON_HOC`) | Lấy deadline theo môn |
| `idx_deadline_han_nop` | `deadline` | (`HAN_NOP`) | Tìm deadline sắp đến/quá hạn |
| `idx_deadline_trang_thai` | `deadline` | (`TRANG_THAI`) | Lọc deadline theo trạng thái |
| `idx_nhac_nho_user_time` | `nhac_nho` | (`MA_NGUOI_DUNG`, `THOI_GIAN_NHAC`) | Lấy nhắc nhở của người dùng |
| `idx_nhac_nho_chua_gui` | `nhac_nho` | (`THOI_GIAN_NHAC`) WHERE `THOI_GIAN_DA_GUI IS NULL` | Worker tìm nhắc nhở cần gửi |
| `idx_ghi_chu_user` | `ghi_chu` | (`MA_NGUOI_DUNG`) | Lấy ghi chú theo người dùng |
| `idx_ghi_chu_mon` | `ghi_chu` | (`MA_MON_HOC`) | Lấy ghi chú theo môn học |
| `idx_bo_flashcard_user` | `bo_flashcard` | (`MA_NGUOI_DUNG`) | Lấy bộ flashcard theo người dùng |
| `idx_bo_flashcard_mon` | `bo_flashcard` | (`MA_MON_HOC`) | Lấy bộ flashcard theo môn |
| `idx_flashcard_bo` | `flashcard` | (`MA_BO`) | Lấy flashcard trong một bộ |
| `idx_flashcard_on_tiep_theo` | `flashcard` | (`THOI_GIAN_LAN_ON_TIEP_THEO`) | Tìm flashcard cần ôn |
| `idx_nhom_hoc_tap_nguoi_tao` | `nhom_hoc_tap` | (`NGUOI_TAO`) | Lấy nhóm do người dùng tạo |
| `idx_thanh_vien_nhom_user` | `thanh_vien_nhom` | (`MA_NGUOI_DUNG`) | Lấy nhóm mà người dùng tham gia |
| `idx_thanh_vien_nhom_role` | `thanh_vien_nhom` | (`MA_NHOM`, `VAI_TRO_TRONG_NHOM`) | Tìm trưởng nhóm/thành viên |
| `idx_cong_viec_nhom_group_status` | `cong_viec_nhom` | (`MA_NHOM`, `TRANG_THAI`) | Lọc task theo nhóm và cột Kanban |
| `idx_cong_viec_nhom_assignee` | `cong_viec_nhom` | (`NGUOI_DUOC_GIAO`) | Lấy task được giao cho một người |
| `idx_binh_luan_cong_viec_task_item` | `binh_luan_cong_viec` | (MA_CONG_VIEC, THOI_GIAN_TAO) | lấy danh sách bình luận của một task theo thời gian |
| `idx_binh_luan_cong_viec_user` | `binh_luan_cong_viec` | (MA_NGUOI_DUNG) | Xem bình luận do một người dùng tạo |
| `idx_binh_luan_cong_viec_link_chat` | `binh_luan_cong_viec` | (LINK_NHOM_CHAT) WHERE LINK_NHOM_CHAT IS NOT NULL | Hỗ trợ lọc nhóm đã cấu hình link nhóm chat |
| `idx_tai_lieu_owner` | `tai_lieu` | (`NGUOI_TAI_LEN`) | Lấy tài liệu do người dùng tải lên |
| `idx_tai_lieu_context` | `tai_lieu` | (`MA_MON_HOC`, `MA_NHOM`, `MA_GHI_CHU`) | Lấy tài liệu theo ngữ cảnh |
| `idx_tai_lieu_trang_thai` | `tai_lieu` | (`TRANG_THAI`) | Lọc tài liệu theo trạng thái |
| `idx_bao_cao_tai_lieu_status` | `bao_cao_tai_lieu` | (`TRANG_THAI`) | Admin lọc report cần xử lý |
| `idx_bao_cao_tai_lieu_file` | `bao_cao_tai_lieu` | (`MA_TAI_LIEU`) | Đếm số report của một tài liệu |
| `idx_thong_bao_user_read` | `thong_bao` | (`MA_NGUOI_NHAN`, `THOI_GIAN_DA_DOC`) | Lấy thông báo chưa đọc/đã đọc |
| `idx_thong_bao_user_created` | `thong_bao` | (`MA_NGUOI_NHAN`, `THOI_GIAN_TAO`) | Sắp xếp lịch sử thông báo |
| `idx_nhat_ky_time` | `nhat_ky_he_thong` | (`THOI_GIAN`) | Xem nhật ký theo thời gian |
| `idx_nhat_ky_actor` | `nhat_ky_he_thong` | (`NGUOI_THUC_HIEN`) | Xem lịch sử thao tác của một người dùng |

## Ghi Chú Triển Khai Constraint

| Ràng buộc | Cách triển khai đề xuất |
|---|---|
| Tổng trọng số điểm của một môn bằng 100 | Có thể kiểm tra ở backend service hoặc dùng PostgreSQL trigger |
| Không trùng lịch học của cùng sinh viên | Nên kiểm tra ở backend service trước khi insert/update; có thể bổ sung trigger nếu cần |
| Chỉ một trưởng nhóm trong một nhóm | Có thể dùng partial unique index trên `thanh_vien_nhom(MA_NHOM)` khi `VAI_TRO_TRONG_NHOM = 'TRUONG_NHOM'` |
| Người được giao task phải là thành viên nhóm | Có thể kiểm tra ở backend service hoặc dùng composite FK nếu muốn chặt hơn |
| Tài liệu chỉ gắn một ngữ cảnh chính | Có thể dùng CHECK với `num_nonnulls(MA_MON_HOC, MA_NHOM, MA_GHI_CHU) <= 1` |
| Nhắc nhở chỉ gắn với một deadline hoặc một lịch thi | Có thể dùng CHECK với `num_nonnulls(MA_DEADLINE, MA_LICH_THI) = 1` |
