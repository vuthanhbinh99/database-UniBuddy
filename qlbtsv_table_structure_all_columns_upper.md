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
| `TRANG_THAI` | `varchar(30)` | NOT NULL | Trạng thái tài khoản |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

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
| `REFRESH_TOKEN` | `varchar(255)` | NOT NULL, UNIQUE | Refresh token đã hash |
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
| `MA_THANG_DIEM` | `uuid` | PK | Mã thang điểm |
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
| `MA_CHI_TIET` | `uuid` | PK | Mã chi tiết thang điểm |
| `MA_THANG_DIEM` | `uuid` | NOT NULL, FK -> `thang_diem.MA_THANG_DIEM` | Thang điểm cha |
| `DIEM_CHU` | `varchar(10)` | NOT NULL | Điểm chữ |
| `DIEM_HE_4` | `numeric(3,2)` | NOT NULL | Điểm hệ 4 |
| `DIEM_THAP_NHAT` | `numeric(4,2)` | NOT NULL | Điểm tối thiểu |
| `DIEM_CAO_NHAT` | `numeric(4,2)` | NOT NULL | Điểm tối đa |

Ràng buộc bổ sung: UNIQUE (`MA_THANG_DIEM`, `DIEM_CHU`).

## 8. `quy_che_hoc_luc`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_QUY_CHE` | `uuid` | PK | Mã quy chế |
| `MA_TRUONG` | `INT` | NOT NULL, FK -> `truong_hoc.MA_TRUONG` | Trường áp dụng |
| `TEN_XEP_LOAI` | `varchar(100)` | NOT NULL | Tên xếp loại học lực |
| `GPA_TOI_THIEU` | `numeric(3,2)` | NOT NULL | GPA tối thiểu |
| `GPA_TOI_DA` | `numeric(3,2)` | NOT NULL | GPA tối đa |

Ràng buộc bổ sung: UNIQUE (`MA_TRUONG`, `TEN_XEP_LOAI`).

## 9. `hoc_ky`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_HOC_KY` | `uuid` | PK | Mã học kỳ |
| `MA_SINH_VIEN` | `uuid` | NOT NULL, FK -> `ho_so_sinh_vien.MA_NGUOI_DUNG` | Sinh viên sở hữu học kỳ |
| `TEN_HOC_KY` | `varchar(100)` | NOT NULL | Tên học kỳ |
| `NGAY_BAT_DAU` | `date` | NULL | Ngày bắt đầu |
| `NGAY_KET_THUC` | `date` | NULL | Ngày kết thúc |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`MA_SINH_VIEN`, `TEN_HOC_KY`).

## 10. `mon_hoc`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_MON_HOC` | `INT` | PK | Mã môn học |
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
| `MA_THANH_PHAN` | `uuid` | PK | Mã thành phần điểm |
| `MA_MON_HOC` | `INT` | NOT NULL, FK -> `mon_hoc.MA_MON_HOC` | Môn học |
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
| `MA_MON_HOC` | `INT` | NOT NULL, FK -> `mon_hoc.MA_MON_HOC` | Môn học |
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
| `MA_MON_HOC` | `INT` | NOT NULL, FK -> `mon_hoc.MA_MON_HOC` | Môn học |
| `LOAI_THI` | `varchar(100)` | NULL | Loại thi |
| `THOI_GIAN_THI` | `timestamptz` | NOT NULL | Thời gian thi |
| `PHONG_THI` | `varchar(100)` | NULL | Phòng thi |

## 14. `deadline`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_DEADLINE` | `uuid` | PK | Mã deadline |
| `MA_MON_HOC` | `INT` | NOT NULL, FK -> `mon_hoc.MA_MON_HOC` | Môn học |
| `TIEU_DE` | `varchar(255)` | NOT NULL | Tiêu đề deadline |
| `MO_TA` | `text` | NULL | Mô tả |
| `HAN_NOP` | `timestamptz` | NOT NULL | Hạn nộp |
| `TRANG_THAI` | `varchar(30)` | NOT NULL | Trạng thái |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_TAO_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

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
| `MA_MON_HOC` | `INT` | FK -> `mon_hoc.MA_MON_HOC`, NULL | Môn học liên quan |
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
| `TEN_NHOM` | `varchar(255)` | NOT NULL | Tên nhóm |
| `MA_THAM_GIA` | `varchar(30)` | NOT NULL, UNIQUE | Mã tham gia nhóm |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 20. `thanh_vien_nhom`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_NHOM` | `uuid` | PK, FK -> `nhom_hoc_tap.MA_NHOM` | Nhóm học tập |
| `MA_NGUOI_DUNG` | `uuid` | PK, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Thành viên nhóm |
| `VAI_TRO_TRONG_NHOM` | `varchar(30)` | NOT NULL | Vai trò trong nhóm |
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
| `TRANG_THAI` | `varchar(30)` | NOT NULL | Trạng thái công việc |
| `HAN_HOAN_THANH` | `timestamptz` | NULL | Hạn hoàn thành |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 22. `tai_lieu`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_TAI_LIEU` | `uuid` | PK | Mã tài liệu |
| `NGUOI_TAI_LEN` | `uuid` | NOT NULL, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Người tải lên |
| `MA_MON_HOC` | `INT` | FK -> `mon_hoc.MA_MON_HOC`, NULL | Môn học liên quan |
| `MA_NHOM` | `uuid` | FK -> `nhom_hoc_tap.MA_NHOM`, NULL | Nhóm liên quan |
| `MA_GHI_CHU` | `uuid` | FK -> `ghi_chu.MA_GHI_CHU`, NULL | Ghi chú liên quan |
| `DUONG_DAN_LUU_TRU` | `text` | NOT NULL, UNIQUE | Đường dẫn lưu trữ Firebase Storage |
| `TEN_FILE` | `varchar(255)` | NOT NULL | Tên file |
| `LOAI_FILE` | `varchar(100)` | NULL | Loại file |
| `DUNG_LUONG` | `bigint` | NULL | Dung lượng file |
| `CHE_DO_HIEN_THI` | `varchar(30)` | NOT NULL | Chế độ hiển thị |
| `TRANG_THAI` | `varchar(30)` | NOT NULL | Trạng thái tài liệu |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` | NOT NULL | Thời điểm cập nhật |

## 23. `bao_cao_tai_lieu`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_BAO_CAO` | `uuid` | PK | Mã báo cáo tài liệu |
| `MA_TAI_LIEU` | `uuid` | NOT NULL, FK -> `tai_lieu.MA_TAI_LIEU` | Tài liệu bị báo cáo |
| `NGUOI_BAO_CAO` | `uuid` | NOT NULL, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Người báo cáo |
| `LY_DO` | `text` | NOT NULL | Lý do báo cáo |
| `TRANG_THAI` | `varchar(30)` | NOT NULL | Trạng thái xử lý |
| `NGUOI_KIEM_DUYET` | `uuid` | FK -> `nguoi_dung.MA_NGUOI_DUNG`, NULL | Người kiểm duyệt |
| `KET_QUA_KIEM_DUYET` | `text` | NULL | Kết quả kiểm duyệt |
| `THOI_GIAN_KIEM_DUYET` | `timestamptz` | NULL | Thời điểm kiểm duyệt |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |

Ràng buộc bổ sung: UNIQUE (`MA_TAI_LIEU`, `NGUOI_BAO_CAO`).

## 24. `thong_bao`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_THONG_BAO` | `uuid` | PK | Mã thông báo |
| `MA_NGUOI_NHAN` | `uuid` | NOT NULL, FK -> `nguoi_dung.MA_NGUOI_DUNG` | Người nhận thông báo |
| `NGUOI_TAO` | `uuid` | FK -> `nguoi_dung.MA_NGUOI_DUNG`, NULL | Người tạo thông báo |
| `TIEU_DE` | `varchar(255)` | NOT NULL | Tiêu đề thông báo |
| `NOI_DUNG` | `text` | NOT NULL | Nội dung thông báo |
| `LOAI_THONG_BAO` | `varchar(50)` | NOT NULL | Loại thông báo |
| `THOI_GIAN_DA_GUI` | `timestamptz` | NULL | Thời điểm đã gửi |
| `THOI_GIAN_DA_DOC` | `timestamptz` | NULL | Thời điểm đã đọc |
| `THOI_GIAN_TAO` | `timestamptz` | NOT NULL | Thời điểm tạo |

## 25. `nhat_ky_he_thong`

| Cột | Kiểu dữ liệu | Ràng buộc | Mô tả |
|---|---|---|---|
| `MA_NHAT_KY` | `uuid` | PK | Mã nhật ký |
| `NGUOI_THUC_HIEN` | `uuid` | FK -> `nguoi_dung.MA_NGUOI_DUNG`, NULL | Người thực hiện hành động |
| `MUC_DO` | `varchar(30)` | NOT NULL | Mức độ log |
| `HANH_DONG` | `varchar(100)` | NOT NULL | Hành động |
| `BANG_TAC_DONG` | `varchar(100)` | NULL | Bảng bị tác động |
| `MA_BAN_GHI` | `uuid` | NULL | Mã bản ghi bị tác động |
| `NOI_DUNG` | `text` | NULL | Nội dung log |
| `DU_LIEU_JSON` | `jsonb` | NULL | Dữ liệu bổ sung |
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
| `hoc_ky` | `MA_SINH_VIEN` | `ho_so_sinh_vien.MA_NGUOI_DUNG` | N-1 |
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
