# CHƯƠNG 3. THIẾT KẾ

## 3.1 MÔ HÌNH DỮ LIỆU

### 3.1.3 Mô hình mức vật lý

Mô hình mức vật lý của hệ thống UniBuddy được xây dựng trên hệ quản trị cơ sở dữ liệu PostgreSQL với cơ sở dữ liệu `QLBTSV`. Mô hình gồm 26 bảng, sử dụng khóa chính dạng `UUID`, `INT` hoặc `BIGINT` tùy đặc điểm dữ liệu, đồng thời dùng các ràng buộc khóa ngoại, `UNIQUE`, `NOT NULL`, `CHECK` và các kiểu `ENUM` để bảo đảm tính toàn vẹn dữ liệu.

Các ký hiệu trong bảng được sử dụng theo mẫu: `K` là khóa chính, `U` là thuộc tính duy nhất (unique), `M` là thuộc tính bắt buộc nhập.

#### 3.1.3.1 Sơ đồ logic dữ liệu (mô hình quan hệ)

Sơ đồ logic dữ liệu được biểu diễn dưới dạng lược đồ quan hệ. Trong mỗi quan hệ, thuộc tính khóa chính được <u><b>in đậm và gạch chân</b></u>, thuộc tính khóa ngoại được <i>in nghiêng</i>. Nếu một thuộc tính vừa là khóa chính vừa là khóa ngoại thì thuộc tính đó được kết hợp cả hai cách trình bày.

- vai_tro (<u><b>MA_VAI_TRO</b></u>, MA_CODE, TEN_VAI_TRO)
- nguoi_dung (<u><b>MA_NGUOI_DUNG</b></u>, <i>MA_VAI_TRO</i>, EMAIL, MAT_KHAU, HO_TEN, SO_DIEN_THOAI, ANH_DAI_DIEN, TRANG_THAI, THOI_GIAN_TAO, THOI_GIAN_CAP_NHAT, SO_GIO_NHAC_DEADLINE, NHAN_THONG_BAO_DAY)
- truong_hoc (<u><b>MA_TRUONG</b></u>, MA_TRUONG_CODE, TEN_TRUONG, THOI_GIAN_TAO, THOI_GIAN_CAP_NHAT)
- ho_so_sinh_vien (<u><b><i>MA_NGUOI_DUNG</i></b></u>, <i>MA_TRUONG</i>, MA_SINH_VIEN, NGANH_HOC, KHOA_HOC, THOI_GIAN_TAO, THOI_GIAN_CAP_NHAT)
- phien_dang_nhap (<u><b>MA_PHIEN</b></u>, <i>MA_NGUOI_DUNG</i>, REFRESH_TOKEN, FCM_TOKEN, LOAI_THIET_BI, IP_ADDRESS, USER_AGENT, THOI_GIAN_HET_HAN, THOI_GIAN_THU_HOI, LAN_HOAT_DONG_CUOI, THOI_GIAN_TAO)
- thang_diem (<u><b>MA_THANG_DIEM</b></u>, <i>MA_TRUONG</i>, TEN_THANG_DIEM, DIEM_THAP_NHAT, DIEM_CAO_NHAT, THOI_GIAN_TAO, THOI_GIAN_CAP_NHAT)
- chi_tiet_thang_diem (<u><b>MA_CHI_TIET</b></u>, <i>MA_THANG_DIEM</i>, DIEM_CHU, DIEM_HE_4, DIEM_THAP_NHAT, DIEM_CAO_NHAT)
- quy_che_hoc_luc (<u><b>MA_QUY_CHE</b></u>, <i>MA_TRUONG</i>, TEN_XEP_LOAI, GPA_TOI_THIEU, GPA_TOI_DA)
- hoc_ky (<u><b>MA_HOC_KY</b></u>, <i>MA_NGUOI_DUNG</i>, TEN_HOC_KY, NGAY_BAT_DAU, NGAY_KET_THUC, THOI_GIAN_TAO, THOI_GIAN_CAP_NHAT)
- mon_hoc (<u><b>MA_MON_HOC</b></u>, <i>MA_HOC_KY</i>, MA_MON, TEN_MON, SO_TIN_CHI, THOI_GIAN_TAO, THOI_GIAN_CAP_NHAT)
- thanh_phan_diem (<u><b>MA_THANH_PHAN</b></u>, <i>MA_MON_HOC</i>, TEN_THANH_PHAN, TRONG_SO, DIEM, THOI_GIAN_TAO, THOI_GIAN_CAP_NHAT)
- lich_hoc (<u><b>MA_LICH_HOC</b></u>, <i>MA_MON_HOC</i>, THU, TIET_BAT_DAU, SO_TIET, PHONG_HOC, NGAY_BAT_DAU, NGAY_KET_THUC)
- lich_thi (<u><b>MA_LICH_THI</b></u>, <i>MA_MON_HOC</i>, LOAI_THI, THOI_GIAN_THI, PHONG_THI)
- deadline (<u><b>MA_DEADLINE</b></u>, <i>MA_MON_HOC</i>, TIEU_DE, MO_TA, HAN_NOP, TRANG_THAI, THOI_GIAN_TAO, THOI_GIAN_CAP_NHAT)
- nhac_nho (<u><b>MA_NHAC_NHO</b></u>, <i>MA_NGUOI_DUNG</i>, <i>MA_DEADLINE</i>, <i>MA_LICH_THI</i>, THOI_GIAN_NHAC, THOI_GIAN_DA_GUI, THOI_GIAN_TAO)
- ghi_chu (<u><b>MA_GHI_CHU</b></u>, <i>MA_NGUOI_DUNG</i>, <i>MA_MON_HOC</i>, TIEU_DE, NOI_DUNG, THOI_GIAN_TAO, THOI_GIAN_CAP_NHAT)
- bo_flashcard (<u><b>MA_BO</b></u>, <i>MA_NGUOI_DUNG</i>, <i>MA_MON_HOC</i>, TEN_BO, THOI_GIAN_TAO, THOI_GIAN_CAP_NHAT)
- flashcard (<u><b>MA_FLASHCARD</b></u>, <i>MA_BO</i>, MAT_TRUOC, MAT_SAU, SO_LAN_ON, DIEM_GHI_NHO, THOI_GIAN_LAN_ON_CUOI, THOI_GIAN_LAN_ON_TIEP_THEO, THOI_GIAN_TAO, THOI_GIAN_CAP_NHAT)
- nhom_hoc_tap (<u><b>MA_NHOM</b></u>, <i>NGUOI_TAO</i>, <i>MA_TRUONG</i>, MA_MON, TEN_NHOM, MA_THAM_GIA, LINK_NHOM_CHAT, THOI_GIAN_TAO, THOI_GIAN_CAP_NHAT)
- thanh_vien_nhom (<u><b><i>MA_NHOM</i></b></u>, <u><b><i>MA_NGUOI_DUNG</i></b></u>, VAI_TRO_TRONG_NHOM, THOI_GIAN_THAM_GIA)
- cong_viec_nhom (<u><b>MA_CONG_VIEC</b></u>, <i>MA_NHOM</i>, <i>NGUOI_DUOC_GIAO</i>, TIEU_DE, MO_TA, TRANG_THAI, HAN_HOAN_THANH, THOI_GIAN_TAO, THOI_GIAN_CAP_NHAT)
- binh_luan_cong_viec (<u><b>MA_BINH_LUAN</b></u>, <i>MA_CONG_VIEC</i>, <i>MA_NGUOI_DUNG</i>, NOI_DUNG, THOI_GIAN_TAO)
- tai_lieu (<u><b>MA_TAI_LIEU</b></u>, <i>NGUOI_TAI_LEN</i>, <i>MA_MON_HOC</i>, <i>MA_NHOM</i>, <i>MA_GHI_CHU</i>, DUONG_DAN_LUU_TRU, TEN_FILE, LOAI_FILE, DUNG_LUONG, CHE_DO_HIEN_THI, TRANG_THAI, THOI_GIAN_TAO, THOI_GIAN_CAP_NHAT)
- bao_cao_tai_lieu (<u><b>MA_BAO_CAO</b></u>, <i>MA_TAI_LIEU</i>, <i>NGUOI_BAO_CAO</i>, LY_DO, TRANG_THAI, <i>NGUOI_KIEM_DUYET</i>, KET_QUA_KIEM_DUYET, THOI_GIAN_KIEM_DUYET, THOI_GIAN_TAO)
- thong_bao (<u><b>MA_THONG_BAO</b></u>, <i>MA_NGUOI_NHAN</i>, <i>NGUOI_TAO</i>, TIEU_DE, NOI_DUNG, LOAI_THONG_BAO, THOI_GIAN_DA_GUI, THOI_GIAN_DA_DOC, THOI_GIAN_TAO)
- nhat_ky_he_thong (<u><b>MA_NHAT_KY</b></u>, <i>NGUOI_THUC_HIEN</i>, MUC_DO, HANH_DONG, BANG_TAC_DONG, MA_BAN_GHI, NOI_DUNG, DU_LIEU_JSON, THOI_GIAN)

#### 3.1.3.2 Loại thực thể

##### Loại thực thể `vai_tro`

Mô tả: Loại thực thể `vai_tro` gồm các vai trò được sử dụng để phân quyền trong hệ thống.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_VAI_TRO` | `INT` | x | x | x | Mã vai trò (KHÓA CHÍNH) |
| `MA_CODE` | `varchar(50)` |  | x | x | Mã code vai trò |
| `TEN_VAI_TRO` | `varchar(100)` |  |  | x | Tên vai trò |

##### Loại thực thể `nguoi_dung`

Mô tả: Loại thực thể `nguoi_dung` gồm thông tin tài khoản của người dùng sử dụng hệ thống UniBuddy.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_NGUOI_DUNG` | `uuid` | x | x | x | Mã người dùng (KHÓA CHÍNH) |
| `MA_VAI_TRO` | `INT` |  |  | x | Vai trò chính của người dùng - khóa ngoại tham chiếu `vai_tro.MA_VAI_TRO` |
| `EMAIL` | `varchar(255)` |  | x | x | Email đăng nhập |
| `MAT_KHAU` | `varchar(255)` |  |  | x | Mật khẩu đã mã hóa |
| `HO_TEN` | `varchar(150)` |  |  | x | Họ tên người dùng |
| `SO_DIEN_THOAI` | `varchar(20)` |  |  |  | Số điện thoại |
| `ANH_DAI_DIEN` | `text` |  |  |  | Đường dẫn ảnh đại diện |
| `TRANG_THAI` | `enum_trang_thai_nguoi_dung` |  |  | x | Trạng thái tài khoản |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |
| `SO_GIO_NHAC_DEADLINE` | `smallint` |  |  |  | Giờ nhắc deadline |
| `NHAN_THONG_BAO_DAY` | `boolean` |  |  | x | Cho phép người dùng tắt hoặc mở để nhận thông báo |

##### Loại thực thể `truong_hoc`

Mô tả: Loại thực thể `truong_hoc` gồm thông tin các trường học được sinh viên khai báo trong hệ thống.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_TRUONG` | `INT` | x | x | x | Mã trường (KHÓA CHÍNH) |
| `MA_TRUONG_CODE` | `varchar(50)` |  | x | x | Mã code trường |
| `TEN_TRUONG` | `varchar(255)` |  |  | x | Tên trường |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Loại thực thể `ho_so_sinh_vien`

Mô tả: Loại thực thể `ho_so_sinh_vien` gồm hồ sơ học tập mở rộng của người dùng có vai trò sinh viên.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_NGUOI_DUNG` | `uuid` | x | x | x | Mã người dùng của sinh viên - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` (KHÓA CHÍNH) |
| `MA_TRUONG` | `INT` |  |  |  | Trường của sinh viên - khóa ngoại tham chiếu `truong_hoc.MA_TRUONG` |
| `MA_SINH_VIEN` | `varchar(50)` |  |  | x | Mã sinh viên |
| `NGANH_HOC` | `varchar(150)` |  |  |  | Ngành học |
| `KHOA_HOC` | `varchar(50)` |  |  |  | Khóa học |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

Ghi chú: Ràng buộc bổ sung: UNIQUE (`MA_TRUONG`, `MA_SINH_VIEN`).

##### Loại thực thể `phien_dang_nhap`

Mô tả: Loại thực thể `phien_dang_nhap` gồm thông tin phiên đăng nhập, refresh token và thiết bị của người dùng.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_PHIEN` | `uuid` | x | x | x | Mã phiên đăng nhập (KHÓA CHÍNH) |
| `MA_NGUOI_DUNG` | `uuid` |  |  | x | Người dùng sở hữu phiên - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `REFRESH_TOKEN` | `text` |  | x | x | Refresh token |
| `FCM_TOKEN` | `text` |  | x |  | Token gửi thông báo FCM |
| `LOAI_THIET_BI` | `varchar(50)` |  |  |  | Loại thiết bị |
| `IP_ADDRESS` | `inet` |  |  |  | Địa chỉ IP |
| `USER_AGENT` | `text` |  |  |  | Thông tin trình duyệt/thiết bị |
| `THOI_GIAN_HET_HAN` | `timestamptz` |  |  | x | Thời điểm hết hạn |
| `THOI_GIAN_THU_HOI` | `timestamptz` |  |  |  | Thời điểm bị thu hồi |
| `LAN_HOAT_DONG_CUOI` | `timestamptz` |  |  | x | Lần hoạt động cuối |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |

##### Loại thực thể `thang_diem`

Mô tả: Loại thực thể `thang_diem` gồm các thang điểm được áp dụng theo từng trường học.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_THANG_DIEM` | `INT` | x | x | x | Mã thang điểm (KHÓA CHÍNH) |
| `MA_TRUONG` | `INT` |  |  | x | Trường áp dụng - khóa ngoại tham chiếu `truong_hoc.MA_TRUONG` |
| `TEN_THANG_DIEM` | `varchar(100)` |  |  | x | Tên thang điểm |
| `DIEM_THAP_NHAT` | `numeric(4,2)` |  |  | x | Điểm thấp nhất |
| `DIEM_CAO_NHAT` | `numeric(4,2)` |  |  | x | Điểm cao nhất |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

Ghi chú: Ràng buộc bổ sung: UNIQUE (`MA_TRUONG`, `TEN_THANG_DIEM`).

##### Loại thực thể `chi_tiet_thang_diem`

Mô tả: Loại thực thể `chi_tiet_thang_diem` gồm các mức quy đổi điểm chi tiết thuộc một thang điểm.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_CHI_TIET` | `INT` | x | x | x | Mã chi tiết thang điểm (KHÓA CHÍNH) |
| `MA_THANG_DIEM` | `INT` |  |  | x | Thang điểm cha - khóa ngoại tham chiếu `thang_diem.MA_THANG_DIEM` |
| `DIEM_CHU` | `varchar(10)` |  |  | x | Điểm chữ |
| `DIEM_HE_4` | `numeric(3,2)` |  |  | x | Điểm hệ 4 |
| `DIEM_THAP_NHAT` | `numeric(4,2)` |  |  | x | Điểm tối thiểu |
| `DIEM_CAO_NHAT` | `numeric(4,2)` |  |  | x | Điểm tối đa |

Ghi chú: Ràng buộc bổ sung: UNIQUE (`MA_THANG_DIEM`, `DIEM_CHU`).

##### Loại thực thể `quy_che_hoc_luc`

Mô tả: Loại thực thể `quy_che_hoc_luc` gồm quy định xếp loại học lực theo khoảng GPA của từng trường.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_QUY_CHE` | `INT` | x | x | x | Mã quy chế (KHÓA CHÍNH) |
| `MA_TRUONG` | `INT` |  |  | x | Trường áp dụng - khóa ngoại tham chiếu `truong_hoc.MA_TRUONG` |
| `TEN_XEP_LOAI` | `varchar(100)` |  |  | x | Tên xếp loại học lực |
| `GPA_TOI_THIEU` | `numeric(3,2)` |  |  | x | GPA tối thiểu |
| `GPA_TOI_DA` | `numeric(3,2)` |  |  | x | GPA tối đa |

Ghi chú: Ràng buộc bổ sung: UNIQUE (`MA_TRUONG`, `TEN_XEP_LOAI`).

##### Loại thực thể `hoc_ky`

Mô tả: Loại thực thể `hoc_ky` gồm các học kỳ do sinh viên quản lý trong tài khoản của mình.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_HOC_KY` | `uuid` | x | x | x | Mã học kỳ (KHÓA CHÍNH) |
| `MA_NGUOI_DUNG` | `uuid` |  |  | x | Tài khoản sinh viên sở hữu học kỳ - khóa ngoại tham chiếu `ho_so_sinh_vien.MA_NGUOI_DUNG` |
| `TEN_HOC_KY` | `varchar(100)` |  |  | x | Tên học kỳ |
| `NGAY_BAT_DAU` | `date` |  |  |  | Ngày bắt đầu |
| `NGAY_KET_THUC` | `date` |  |  |  | Ngày kết thúc |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

Ghi chú: Ràng buộc bổ sung: UNIQUE (`MA_NGUOI_DUNG`, `TEN_HOC_KY`).

##### Loại thực thể `mon_hoc`

Mô tả: Loại thực thể `mon_hoc` gồm thông tin môn học thuộc từng học kỳ của sinh viên.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_MON_HOC` | `uuid` | x | x | x | Mã môn học (KHÓA CHÍNH) |
| `MA_HOC_KY` | `uuid` |  |  | x | Học kỳ chứa môn học - khóa ngoại tham chiếu `hoc_ky.MA_HOC_KY` |
| `MA_MON` | `varchar(50)` |  |  |  | Mã môn |
| `TEN_MON` | `varchar(255)` |  |  | x | Tên môn học |
| `SO_TIN_CHI` | `smallint` |  |  | x | Số tín chỉ |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

Ghi chú: Ràng buộc bổ sung: UNIQUE (`MA_HOC_KY`, `MA_MON`).

##### Loại thực thể `thanh_phan_diem`

Mô tả: Loại thực thể `thanh_phan_diem` gồm các thành phần điểm và trọng số của từng môn học.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_THANH_PHAN` | `INT` | x | x | x | Mã thành phần điểm (KHÓA CHÍNH) |
| `MA_MON_HOC` | `uuid` |  |  | x | Môn học liên kết - khóa ngoại tham chiếu `mon_hoc.MA_MON_HOC` |
| `TEN_THANH_PHAN` | `varchar(100)` |  |  | x | Tên thành phần điểm |
| `TRONG_SO` | `numeric(5,2)` |  |  | x | Trọng số điểm |
| `DIEM` | `numeric(4,2)` |  |  |  | Điểm đạt được |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

Ghi chú: Ràng buộc bổ sung: UNIQUE (`MA_MON_HOC`, `TEN_THANH_PHAN`).

##### Loại thực thể `lich_hoc`

Mô tả: Loại thực thể `lich_hoc` gồm lịch học theo môn, tiết học, phòng học và khoảng thời gian học.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_LICH_HOC` | `uuid` | x | x | x | Mã lịch học (KHÓA CHÍNH) |
| `MA_MON_HOC` | `uuid` |  |  | x | Môn học - khóa ngoại tham chiếu `mon_hoc.MA_MON_HOC` |
| `THU` | `smallint` |  |  | x | Thứ trong tuần |
| `TIET_BAT_DAU` | `smallint` |  |  | x | Tiết bắt đầu |
| `SO_TIET` | `smallint` |  |  | x | Số tiết |
| `PHONG_HOC` | `varchar(100)` |  |  |  | Phòng học |
| `NGAY_BAT_DAU` | `date` |  |  |  | Ngày bắt đầu |
| `NGAY_KET_THUC` | `date` |  |  |  | Ngày kết thúc |

##### Loại thực thể `lich_thi`

Mô tả: Loại thực thể `lich_thi` gồm lịch thi của các môn học cần theo dõi và nhắc nhở.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_LICH_THI` | `uuid` | x | x | x | Mã lịch thi (KHÓA CHÍNH) |
| `MA_MON_HOC` | `uuid` |  |  | x | Môn học - khóa ngoại tham chiếu `mon_hoc.MA_MON_HOC` |
| `LOAI_THI` | `varchar(100)` |  |  |  | Loại thi |
| `THOI_GIAN_THI` | `timestamptz` |  |  | x | Thời gian thi |
| `PHONG_THI` | `varchar(100)` |  |  |  | Phòng thi |

##### Loại thực thể `deadline`

Mô tả: Loại thực thể `deadline` gồm các hạn nộp bài, bài tập hoặc công việc học tập theo môn học.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_DEADLINE` | `uuid` | x | x | x | Mã deadline (KHÓA CHÍNH) |
| `MA_MON_HOC` | `uuid` |  |  | x | Môn học - khóa ngoại tham chiếu `mon_hoc.MA_MON_HOC` |
| `TIEU_DE` | `varchar(255)` |  |  | x | Tiêu đề deadline |
| `MO_TA` | `text` |  |  |  | Mô tả |
| `HAN_NOP` | `timestamptz` |  |  | x | Hạn nộp |
| `TRANG_THAI` | `enum_trang_thai_deadline` |  |  | x | Trạng thái công việc |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Loại thực thể `nhac_nho`

Mô tả: Loại thực thể `nhac_nho` gồm các nhắc nhở gắn với deadline hoặc lịch thi của người dùng.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_NHAC_NHO` | `uuid` | x | x | x | Mã nhắc nhở (KHÓA CHÍNH) |
| `MA_NGUOI_DUNG` | `uuid` |  |  | x | Người nhận nhắc nhở - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `MA_DEADLINE` | `uuid` |  |  |  | Deadline cần nhắc - khóa ngoại tham chiếu `deadline.MA_DEADLINE` |
| `MA_LICH_THI` | `uuid` |  |  |  | Lịch thi cần nhắc - khóa ngoại tham chiếu `lich_thi.MA_LICH_THI` |
| `THOI_GIAN_NHAC` | `timestamptz` |  |  | x | Thời gian nhắc |
| `THOI_GIAN_DA_GUI` | `timestamptz` |  |  |  | Thời điểm đã gửi |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |

##### Loại thực thể `ghi_chu`

Mô tả: Loại thực thể `ghi_chu` gồm ghi chú học tập do người dùng tạo và có thể gắn với môn học.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_GHI_CHU` | `uuid` | x | x | x | Mã ghi chú (KHÓA CHÍNH) |
| `MA_NGUOI_DUNG` | `uuid` |  |  | x | Người tạo ghi chú - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `MA_MON_HOC` | `uuid` |  |  |  | Môn học liên quan - khóa ngoại tham chiếu `mon_hoc.MA_MON_HOC` |
| `TIEU_DE` | `varchar(255)` |  |  | x | Tiêu đề ghi chú |
| `NOI_DUNG` | `text` |  |  |  | Nội dung ghi chú |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Loại thực thể `bo_flashcard`

Mô tả: Loại thực thể `bo_flashcard` gồm các bộ flashcard phục vụ ôn tập của người dùng.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_BO` | `uuid` | x | x | x | Mã bộ flashcard (KHÓA CHÍNH) |
| `MA_NGUOI_DUNG` | `uuid` |  |  | x | Người tạo bộ - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `MA_MON_HOC` | `uuid` |  |  |  | Môn học liên quan - khóa ngoại tham chiếu `mon_hoc.MA_MON_HOC` |
| `TEN_BO` | `varchar(255)` |  |  | x | Tên bộ flashcard |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Loại thực thể `flashcard`

Mô tả: Loại thực thể `flashcard` gồm từng thẻ flashcard và thông tin phục vụ cơ chế ôn tập lặp lại.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_FLASHCARD` | `uuid` | x | x | x | Mã flashcard (KHÓA CHÍNH) |
| `MA_BO` | `uuid` |  |  | x | Bộ flashcard - khóa ngoại tham chiếu `bo_flashcard.MA_BO` |
| `MAT_TRUOC` | `text` |  |  | x | Nội dung mặt trước |
| `MAT_SAU` | `text` |  |  | x | Nội dung mặt sau |
| `SO_LAN_ON` | `integer` |  |  | x | Số lần đã ôn |
| `DIEM_GHI_NHO` | `numeric(5,2)` |  |  | x | Điểm ghi nhớ |
| `THOI_GIAN_LAN_ON_CUOI` | `timestamptz` |  |  |  | Lần ôn gần nhất |
| `THOI_GIAN_LAN_ON_TIEP_THEO` | `timestamptz` |  |  |  | Lần ôn tiếp theo |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Loại thực thể `nhom_hoc_tap`

Mô tả: Loại thực thể `nhom_hoc_tap` gồm các nhóm học tập do người dùng tạo trong phạm vi trường hoặc môn học.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_NHOM` | `uuid` | x | x | x | Mã nhóm học tập (KHÓA CHÍNH) |
| `NGUOI_TAO` | `uuid` |  |  | x | Người tạo nhóm - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `MA_TRUONG` | `int` |  |  | x | Mã trường học - khóa ngoại tham chiếu `truong_hoc.MA_TRUONG` |
| `MA_MON` | `varchar(50)` |  |  |  | Mã môn học của trường |
| `TEN_NHOM` | `varchar(255)` |  |  | x | Tên nhóm |
| `MA_THAM_GIA` | `varchar(30)` |  | x | x | Mã tham gia nhóm |
| `LINK_NHOM_CHAT` | `text` |  |  | x | Đường link nhóm chat Zalo/Messenger/Discord |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Loại thực thể `thanh_vien_nhom`

Mô tả: Loại thực thể `thanh_vien_nhom` gồm danh sách thành viên và vai trò của từng người trong nhóm học tập.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_NHOM` | `uuid` | x | x | x | Nhóm học tập - khóa ngoại tham chiếu `nhom_hoc_tap.MA_NHOM` (KHÓA CHÍNH) |
| `MA_NGUOI_DUNG` | `uuid` | x | x | x | Thành viên nhóm - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` (KHÓA CHÍNH) |
| `VAI_TRO_TRONG_NHOM` | `enum_vai_tro_nhom` |  |  | x | Vai trò điều hành nhóm |
| `THOI_GIAN_THAM_GIA` | `timestamptz` |  |  | x | Thời điểm tham gia |

Ghi chú: Khóa chính: (`MA_NHOM`, `MA_NGUOI_DUNG`).

##### Loại thực thể `cong_viec_nhom`

Mô tả: Loại thực thể `cong_viec_nhom` gồm các công việc được tạo và phân công trong nhóm học tập.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_CONG_VIEC` | `uuid` | x | x | x | Mã công việc nhóm (KHÓA CHÍNH) |
| `MA_NHOM` | `uuid` |  |  | x | Nhóm học tập - khóa ngoại tham chiếu `nhom_hoc_tap.MA_NHOM` |
| `NGUOI_DUOC_GIAO` | `uuid` |  |  |  | Người được giao - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `TIEU_DE` | `varchar(255)` |  |  | x | Tiêu đề công việc |
| `MO_TA` | `text` |  |  |  | Mô tả công việc |
| `TRANG_THAI` | `enum_trang_thai_cong_viec` |  |  | x | Trạng thái công việc |
| `HAN_HOAN_THANH` | `timestamptz` |  |  |  | Hạn hoàn thành |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Loại thực thể `binh_luan_cong_viec`

Mô tả: Loại thực thể `binh_luan_cong_viec` gồm bình luận trao đổi trong từng công việc nhóm.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_BINH_LUAN` | `uuid` | x | x | x | Mã bình luận (KHÓA CHÍNH) |
| `MA_CONG_VIEC` | `uuid` |  |  | x | Công việc được bình luận - khóa ngoại tham chiếu `cong_viec_nhom.MA_CONG_VIEC` |
| `MA_NGUOI_DUNG` | `uuid` |  |  | x | Người viết bình luận - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `NOI_DUNG` | `text` |  |  | x | Nội dung bình luận |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo bình luận |

##### Loại thực thể `tai_lieu`

Mô tả: Loại thực thể `tai_lieu` gồm metadata của tài liệu được tải lên và chia sẻ trong hệ thống.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_TAI_LIEU` | `uuid` | x | x | x | Mã tài liệu (KHÓA CHÍNH) |
| `NGUOI_TAI_LEN` | `uuid` |  |  | x | Người tải lên - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `MA_MON_HOC` | `uuid` |  |  |  | Môn học liên quan - khóa ngoại tham chiếu `mon_hoc.MA_MON_HOC` |
| `MA_NHOM` | `uuid` |  |  |  | Nhóm liên quan - khóa ngoại tham chiếu `nhom_hoc_tap.MA_NHOM` |
| `MA_GHI_CHU` | `uuid` |  |  |  | Ghi chú liên quan - khóa ngoại tham chiếu `ghi_chu.MA_GHI_CHU` |
| `DUONG_DAN_LUU_TRU` | `text` |  | x | x | Đường dẫn lưu trữ Firebase Storage |
| `TEN_FILE` | `varchar(255)` |  |  | x | Tên file |
| `LOAI_FILE` | `varchar(100)` |  |  |  | Loại file |
| `DUNG_LUONG` | `bigint` |  |  |  | Dung lượng file |
| `CHE_DO_HIEN_THI` | `enum_che_do_hien_thi` |  |  | x | Chế độ hiển thị chia sẻ |
| `TRANG_THAI` | `enum_trang_thai_tai_lieu` |  |  | x | Trạng thái kiểm duyệt file |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Loại thực thể `bao_cao_tai_lieu`

Mô tả: Loại thực thể `bao_cao_tai_lieu` gồm các báo cáo vi phạm đối với tài liệu do người dùng gửi.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_BAO_CAO` | `uuid` | x | x | x | Mã báo cáo tài liệu (KHÓA CHÍNH) |
| `MA_TAI_LIEU` | `uuid` |  |  | x | Tài liệu bị báo cáo - khóa ngoại tham chiếu `tai_lieu.MA_TAI_LIEU` |
| `NGUOI_BAO_CAO` | `uuid` |  |  | x | Người báo cáo - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `LY_DO` | `text` |  |  | x | Lý do báo cáo |
| `TRANG_THAI` | `enum_trang_thai_bao_cao` |  |  | x | Tiến độ xử lý report |
| `NGUOI_KIEM_DUYET` | `uuid` |  |  |  | Người kiểm duyệt - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `KET_QUA_KIEM_DUYET` | `text` |  |  |  | Kết quả kiểm duyệt |
| `THOI_GIAN_KIEM_DUYET` | `timestamptz` |  |  |  | Thời điểm kiểm duyệt |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |

Ghi chú: Ràng buộc bổ sung: UNIQUE (`MA_TAI_LIEU`, `NGUOI_BAO_CAO`).

##### Loại thực thể `thong_bao`

Mô tả: Loại thực thể `thong_bao` gồm thông báo hệ thống, deadline, nhóm học tập và nhắc nhở gửi tới người dùng.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_THONG_BAO` | `uuid` | x | x | x | Mã thông báo (KHÓA CHÍNH) |
| `MA_NGUOI_NHAN` | `uuid` |  |  | x | Người nhận thông báo - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `NGUOI_TAO` | `uuid` |  |  |  | Người tạo thông báo - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `TIEU_DE` | `varchar(255)` |  |  | x | Tiêu đề thông báo |
| `NOI_DUNG` | `text` |  |  | x | Nội dung thông báo |
| `LOAI_THONG_BAO` | `enum_loai_thong_bao` |  |  | x | Phân loại thông báo |
| `THOI_GIAN_DA_GUI` | `timestamptz` |  |  |  | Thời điểm đã gửi |
| `THOI_GIAN_DA_DOC` | `timestamptz` |  |  |  | Thời điểm đã đọc |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |

##### Loại thực thể `nhat_ky_he_thong`

Mô tả: Loại thực thể `nhat_ky_he_thong` gồm nhật ký thao tác và sự kiện phục vụ theo dõi hoạt động hệ thống.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_NHAT_KY` | `BIGINT` | x | x | x | Mã nhật ký (KHÓA CHÍNH) |
| `NGUOI_THUC_HIEN` | `uuid` |  |  |  | Người thực hiện hành động - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `MUC_DO` | `enum_muc_do_log` |  |  | x | Mức độ nghiêm trọng của log |
| `HANH_DONG` | `varchar(100)` |  |  | x | Hành động thực hiện |
| `BANG_TAC_DONG` | `varchar(100)` |  |  |  | Bảng bị tác động |
| `MA_BAN_GHI` | `varchar(100)` |  |  |  | Mã bản ghi bị ảnh hưởng |
| `NOI_DUNG` | `text` |  |  |  | Nội dung log chi tiết |
| `DU_LIEU_JSON` | `jsonb` |  |  |  | Dữ liệu log bổ sung |
| `THOI_GIAN` | `timestamptz` |  |  | x | Thời điểm ghi log |

#### 3.1.3.3 Mô tả các ràng buộc nghiệp vụ

Các ràng buộc nghiệp vụ chính của hệ thống UniBuddy được mô tả theo góc nhìn người dùng như sau:

- Mỗi email chỉ được dùng để đăng ký một tài khoản duy nhất. Người dùng không thể tạo nhiều tài khoản bằng cùng một email.
- Khi đăng ký, mật khẩu phải đủ an toàn theo yêu cầu của hệ thống. Nếu sinh viên chọn trường và nhập mã trường, hai thông tin này phải khớp với cùng một trường có trong hệ thống.
- Mỗi tài khoản luôn có một trạng thái rõ ràng như đang hoạt động, bị khóa, chưa xác thực hoặc đang chờ đổi mật khẩu. Tài khoản bị khóa không được tiếp tục đăng nhập hoặc sử dụng các phiên đăng nhập cũ.
- Tài khoản dùng mật khẩu tạm phải đổi mật khẩu khi đăng nhập lần đầu. Mật khẩu tạm chỉ có hiệu lực trong 24 giờ; sau thời gian này, người dùng phải được cấp lại mật khẩu tạm mới.
- Người quản trị không được tự khóa hoặc tự mở khóa chính tài khoản của mình. Việc cấp mật khẩu tạm chỉ áp dụng cho tài khoản quản trị phù hợp.
- Mỗi lần đăng nhập được xem là một phiên riêng. Phiên đăng nhập hết hạn, bị thu hồi hoặc thuộc tài khoản đã bị khóa sẽ không được tiếp tục sử dụng.
- Một mã sinh viên không được trùng trong cùng một trường. Nhờ đó, hồ sơ của từng sinh viên trong mỗi trường luôn được xác định rõ ràng.
- Mỗi trường học phải có mã trường và tên trường rõ ràng; mã trường không được trùng giữa các trường.
- Mỗi trường chỉ có một tên thang điểm, một quy tắc quy đổi điểm và một mức xếp loại học lực tương ứng. Danh sách mức điểm không được để trống, các khoảng điểm hoặc khoảng GPA phải hợp lý và không được chồng lấn nhau.
- Một sinh viên không thể tạo hai học kỳ trùng tên trong cùng tài khoản. Nếu học kỳ có ngày bắt đầu và ngày kết thúc, ngày bắt đầu phải diễn ra trước hoặc bằng ngày kết thúc.
- Trong cùng một học kỳ, sinh viên không thể tạo hai môn học có cùng mã môn hoặc cùng tên môn. Số tín chỉ của môn học phải lớn hơn 0.
- Khi xóa học kỳ hoặc môn học đã có dữ liệu liên quan, hệ thống phải yêu cầu người dùng xác nhận rõ ràng trước khi xóa.
- Trong một môn học, phải có ít nhất một thành phần điểm khi cấu hình trọng số. Tên thành phần điểm không được để trống hoặc trùng nhau, trọng số từng thành phần phải hợp lệ, điểm nhập vào phải nằm trong thang điểm cho phép, và tổng trọng số phải bằng 100%.
- Lịch học phải có thứ, tiết bắt đầu và số tiết hợp lệ. Một lịch học không được vượt quá số tiết cho phép trong ngày, ngày bắt đầu không được sau ngày kết thúc, và hệ thống không cho phép một sinh viên có hai lịch học bị trùng thời gian.
- Lịch thi phải có thời gian thi rõ ràng để hệ thống có thể theo dõi và gửi nhắc nhở đúng lúc.
- Deadline luôn có trạng thái cụ thể như chưa làm, đang làm, hoàn thành hoặc trễ hạn. Khi đã quá hạn mà chưa hoàn thành, hệ thống có thể tự chuyển deadline sang trạng thái trễ hạn.
- Mỗi nhắc nhở chỉ gắn với một việc chính: hoặc một deadline, hoặc một lịch thi. Người dùng không thể tạo nhắc nhở sau thời điểm sự kiện đã diễn ra.
- Nhóm học tập chỉ được tạo cho môn học đã có trong thời khóa biểu của sinh viên. Tên nhóm không được trùng trong cùng phạm vi môn học và trường học.
- Trong một nhóm học tập, một người chỉ được xuất hiện một lần và vai trò của từng người phải rõ ràng. Mỗi nhóm có một trưởng nhóm để quản lý thành viên và phân công công việc.
- Người dùng chỉ có thể tham gia nhóm bằng mã mời hợp lệ và không thể tham gia lại nhóm mà mình đã là thành viên. Trưởng nhóm không được tự rời nhóm khi chưa chuyển quyền hoặc xóa nhóm.
- Khi xóa nhóm học tập, chỉ trưởng nhóm được thực hiện và phải xác nhận bằng mật khẩu. Các công việc, bình luận và thành viên liên quan đến nhóm sẽ được xử lý cùng lúc để tránh dữ liệu còn sót.
- Công việc nhóm luôn có trạng thái rõ ràng như chưa bắt đầu, đang thực hiện, hoàn thành hoặc trễ hạn. Chỉ trưởng nhóm được tạo, xóa hoặc phân công công việc; người được giao phải là thành viên của chính nhóm đó.
- Hạn hoàn thành của công việc nhóm, nếu có, không được nằm trong quá khứ. Người dùng không được tự chuyển công việc sang trạng thái trễ hạn; trạng thái này do hệ thống xác định khi đến hạn.
- Chỉ thành viên của nhóm mới được xem, cập nhật trạng thái hoặc bình luận vào công việc nhóm. Người dùng chỉ được thu hồi bình luận do chính mình viết.
- Ghi chú, tài liệu và bộ flashcard gắn với môn học chỉ được gắn vào môn học thuộc đúng người dùng đó. Người dùng chỉ được sửa hoặc xóa dữ liệu học tập do chính mình tạo.
- Ghi chú phải có tiêu đề rõ ràng, nội dung và tệp đính kèm không được vượt quá giới hạn hệ thống. Tệp đính kèm phải có định dạng được hỗ trợ và đường dẫn tải xuống không được trùng với tệp đã có.
- Mỗi tài liệu tải lên được hệ thống ghi nhận thành một bản duy nhất. Tài liệu phải có tệp hoặc đường dẫn tải xuống hợp lệ, dung lượng phù hợp, định dạng được hỗ trợ và trạng thái rõ ràng như khả dụng, đã xóa hoặc chờ kiểm duyệt.
- Mỗi tài liệu chỉ thuộc một ngữ cảnh chính, chẳng hạn một môn học, một nhóm học tập hoặc một ghi chú. Người dùng chỉ được xóa tài liệu do mình tải lên, và tài liệu bị xóa sẽ không còn được xem như tài liệu khả dụng.
- Một người chỉ được báo cáo cùng một tài liệu một lần. Tài liệu bị báo cáo phải còn tồn tại, lý do báo cáo phải đủ rõ ràng, và báo cáo luôn có trạng thái xử lý như chờ xử lý, đã duyệt hoặc đã từ chối.
- Bộ flashcard phải có tên rõ ràng. Mỗi thẻ flashcard phải có nội dung mặt trước và mặt sau; với thẻ trắc nghiệm, phải có từ 2 đến 6 lựa chọn và đáp án đúng phải nằm trong các lựa chọn đó.
- Flashcard không được có số lần ôn, thời gian phản hồi hoặc điểm ghi nhớ là giá trị âm. Sau mỗi lần ôn, hệ thống cập nhật lần ôn gần nhất và gợi ý thời điểm ôn tiếp theo.
- Thông báo luôn có người nhận và loại thông báo rõ ràng, ví dụ thông báo deadline, thông báo hệ thống, thông báo nhóm học tập hoặc nhắc nhở. Nếu thông báo đã được đọc, thời điểm đọc không được trước thời điểm gửi.
- Khi gửi thông báo hệ thống, người quản trị phải chọn rõ phạm vi người nhận như toàn bộ người dùng, một số vai trò hoặc danh sách người dùng cụ thể. Tiêu đề và nội dung thông báo không được để trống hoặc vượt quá giới hạn hệ thống.
- Nhật ký hệ thống ghi lại các thao tác quan trọng với mức độ rõ ràng như thông tin, cảnh báo, lỗi hoặc lỗi nghiêm trọng để hỗ trợ theo dõi và xử lý sự cố.

#### 3.1.3.4 Mô tả các bảng dữ liệu

Phần này mô tả trực tiếp các bảng dữ liệu ở mức cài đặt vật lý trên PostgreSQL. Kiểu dữ liệu được giữ theo kiểu SQL để thuận tiện khi triển khai cơ sở dữ liệu.

##### Bảng `vai_tro`

Mô tả: Bảng `vai_tro` lưu trữ các vai trò được sử dụng để phân quyền trong hệ thống.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_VAI_TRO` | `INT` | x | x | x | Mã vai trò (KHÓA CHÍNH) |
| `MA_CODE` | `varchar(50)` |  | x | x | Mã code vai trò |
| `TEN_VAI_TRO` | `varchar(100)` |  |  | x | Tên vai trò |

##### Bảng `nguoi_dung`

Mô tả: Bảng `nguoi_dung` lưu trữ thông tin tài khoản của người dùng sử dụng hệ thống UniBuddy.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_NGUOI_DUNG` | `uuid` | x | x | x | Mã người dùng (KHÓA CHÍNH) |
| `MA_VAI_TRO` | `INT` |  |  | x | Vai trò chính của người dùng - khóa ngoại tham chiếu `vai_tro.MA_VAI_TRO` |
| `EMAIL` | `varchar(255)` |  | x | x | Email đăng nhập |
| `MAT_KHAU` | `varchar(255)` |  |  | x | Mật khẩu đã mã hóa |
| `HO_TEN` | `varchar(150)` |  |  | x | Họ tên người dùng |
| `SO_DIEN_THOAI` | `varchar(20)` |  |  |  | Số điện thoại |
| `ANH_DAI_DIEN` | `text` |  |  |  | Đường dẫn ảnh đại diện |
| `TRANG_THAI` | `enum_trang_thai_nguoi_dung` |  |  | x | Trạng thái tài khoản |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |
| `SO_GIO_NHAC_DEADLINE` | `smallint` |  |  |  | Giờ nhắc deadline |
| `NHAN_THONG_BAO_DAY` | `boolean` |  |  | x | Cho phép người dùng tắt hoặc mở để nhận thông báo |

##### Bảng `truong_hoc`

Mô tả: Bảng `truong_hoc` lưu trữ thông tin các trường học được sinh viên khai báo trong hệ thống.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_TRUONG` | `INT` | x | x | x | Mã trường (KHÓA CHÍNH) |
| `MA_TRUONG_CODE` | `varchar(50)` |  | x | x | Mã code trường |
| `TEN_TRUONG` | `varchar(255)` |  |  | x | Tên trường |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Bảng `ho_so_sinh_vien`

Mô tả: Bảng `ho_so_sinh_vien` lưu trữ hồ sơ học tập mở rộng của người dùng có vai trò sinh viên.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_NGUOI_DUNG` | `uuid` | x | x | x | Mã người dùng của sinh viên - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` (KHÓA CHÍNH) |
| `MA_TRUONG` | `INT` |  |  |  | Trường của sinh viên - khóa ngoại tham chiếu `truong_hoc.MA_TRUONG` |
| `MA_SINH_VIEN` | `varchar(50)` |  |  | x | Mã sinh viên |
| `NGANH_HOC` | `varchar(150)` |  |  |  | Ngành học |
| `KHOA_HOC` | `varchar(50)` |  |  |  | Khóa học |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`MA_TRUONG`, `MA_SINH_VIEN`).

##### Bảng `phien_dang_nhap`

Mô tả: Bảng `phien_dang_nhap` lưu trữ thông tin phiên đăng nhập, refresh token và thiết bị của người dùng.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_PHIEN` | `uuid` | x | x | x | Mã phiên đăng nhập (KHÓA CHÍNH) |
| `MA_NGUOI_DUNG` | `uuid` |  |  | x | Người dùng sở hữu phiên - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `REFRESH_TOKEN` | `text` |  | x | x | Refresh token |
| `FCM_TOKEN` | `text` |  | x |  | Token gửi thông báo FCM |
| `LOAI_THIET_BI` | `varchar(50)` |  |  |  | Loại thiết bị |
| `IP_ADDRESS` | `inet` |  |  |  | Địa chỉ IP |
| `USER_AGENT` | `text` |  |  |  | Thông tin trình duyệt/thiết bị |
| `THOI_GIAN_HET_HAN` | `timestamptz` |  |  | x | Thời điểm hết hạn |
| `THOI_GIAN_THU_HOI` | `timestamptz` |  |  |  | Thời điểm bị thu hồi |
| `LAN_HOAT_DONG_CUOI` | `timestamptz` |  |  | x | Lần hoạt động cuối |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |

##### Bảng `thang_diem`

Mô tả: Bảng `thang_diem` lưu trữ các thang điểm được áp dụng theo từng trường học.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_THANG_DIEM` | `INT` | x | x | x | Mã thang điểm (KHÓA CHÍNH) |
| `MA_TRUONG` | `INT` |  |  | x | Trường áp dụng - khóa ngoại tham chiếu `truong_hoc.MA_TRUONG` |
| `TEN_THANG_DIEM` | `varchar(100)` |  |  | x | Tên thang điểm |
| `DIEM_THAP_NHAT` | `numeric(4,2)` |  |  | x | Điểm thấp nhất |
| `DIEM_CAO_NHAT` | `numeric(4,2)` |  |  | x | Điểm cao nhất |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`MA_TRUONG`, `TEN_THANG_DIEM`).

##### Bảng `chi_tiet_thang_diem`

Mô tả: Bảng `chi_tiet_thang_diem` lưu trữ các mức quy đổi điểm chi tiết thuộc một thang điểm.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_CHI_TIET` | `INT` | x | x | x | Mã chi tiết thang điểm (KHÓA CHÍNH) |
| `MA_THANG_DIEM` | `INT` |  |  | x | Thang điểm cha - khóa ngoại tham chiếu `thang_diem.MA_THANG_DIEM` |
| `DIEM_CHU` | `varchar(10)` |  |  | x | Điểm chữ |
| `DIEM_HE_4` | `numeric(3,2)` |  |  | x | Điểm hệ 4 |
| `DIEM_THAP_NHAT` | `numeric(4,2)` |  |  | x | Điểm tối thiểu |
| `DIEM_CAO_NHAT` | `numeric(4,2)` |  |  | x | Điểm tối đa |

Ràng buộc bổ sung: UNIQUE (`MA_THANG_DIEM`, `DIEM_CHU`).

##### Bảng `quy_che_hoc_luc`

Mô tả: Bảng `quy_che_hoc_luc` lưu trữ quy định xếp loại học lực theo khoảng GPA của từng trường.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_QUY_CHE` | `INT` | x | x | x | Mã quy chế (KHÓA CHÍNH) |
| `MA_TRUONG` | `INT` |  |  | x | Trường áp dụng - khóa ngoại tham chiếu `truong_hoc.MA_TRUONG` |
| `TEN_XEP_LOAI` | `varchar(100)` |  |  | x | Tên xếp loại học lực |
| `GPA_TOI_THIEU` | `numeric(3,2)` |  |  | x | GPA tối thiểu |
| `GPA_TOI_DA` | `numeric(3,2)` |  |  | x | GPA tối đa |

Ràng buộc bổ sung: UNIQUE (`MA_TRUONG`, `TEN_XEP_LOAI`).

##### Bảng `hoc_ky`

Mô tả: Bảng `hoc_ky` lưu trữ các học kỳ do sinh viên quản lý trong tài khoản của mình.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_HOC_KY` | `uuid` | x | x | x | Mã học kỳ (KHÓA CHÍNH) |
| `MA_NGUOI_DUNG` | `uuid` |  |  | x | Tài khoản sinh viên sở hữu học kỳ - khóa ngoại tham chiếu `ho_so_sinh_vien.MA_NGUOI_DUNG` |
| `TEN_HOC_KY` | `varchar(100)` |  |  | x | Tên học kỳ |
| `NGAY_BAT_DAU` | `date` |  |  |  | Ngày bắt đầu |
| `NGAY_KET_THUC` | `date` |  |  |  | Ngày kết thúc |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`MA_NGUOI_DUNG`, `TEN_HOC_KY`).

##### Bảng `mon_hoc`

Mô tả: Bảng `mon_hoc` lưu trữ thông tin môn học thuộc từng học kỳ của sinh viên.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_MON_HOC` | `uuid` | x | x | x | Mã môn học (KHÓA CHÍNH) |
| `MA_HOC_KY` | `uuid` |  |  | x | Học kỳ chứa môn học - khóa ngoại tham chiếu `hoc_ky.MA_HOC_KY` |
| `MA_MON` | `varchar(50)` |  |  |  | Mã môn |
| `TEN_MON` | `varchar(255)` |  |  | x | Tên môn học |
| `SO_TIN_CHI` | `smallint` |  |  | x | Số tín chỉ |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`MA_HOC_KY`, `MA_MON`).

##### Bảng `thanh_phan_diem`

Mô tả: Bảng `thanh_phan_diem` lưu trữ các thành phần điểm và trọng số của từng môn học.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_THANH_PHAN` | `INT` | x | x | x | Mã thành phần điểm (KHÓA CHÍNH) |
| `MA_MON_HOC` | `uuid` |  |  | x | Môn học liên kết - khóa ngoại tham chiếu `mon_hoc.MA_MON_HOC` |
| `TEN_THANH_PHAN` | `varchar(100)` |  |  | x | Tên thành phần điểm |
| `TRONG_SO` | `numeric(5,2)` |  |  | x | Trọng số điểm |
| `DIEM` | `numeric(4,2)` |  |  |  | Điểm đạt được |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

Ràng buộc bổ sung: UNIQUE (`MA_MON_HOC`, `TEN_THANH_PHAN`).

##### Bảng `lich_hoc`

Mô tả: Bảng `lich_hoc` lưu trữ lịch học theo môn, tiết học, phòng học và khoảng thời gian học.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_LICH_HOC` | `uuid` | x | x | x | Mã lịch học (KHÓA CHÍNH) |
| `MA_MON_HOC` | `uuid` |  |  | x | Môn học - khóa ngoại tham chiếu `mon_hoc.MA_MON_HOC` |
| `THU` | `smallint` |  |  | x | Thứ trong tuần |
| `TIET_BAT_DAU` | `smallint` |  |  | x | Tiết bắt đầu |
| `SO_TIET` | `smallint` |  |  | x | Số tiết |
| `PHONG_HOC` | `varchar(100)` |  |  |  | Phòng học |
| `NGAY_BAT_DAU` | `date` |  |  |  | Ngày bắt đầu |
| `NGAY_KET_THUC` | `date` |  |  |  | Ngày kết thúc |

##### Bảng `lich_thi`

Mô tả: Bảng `lich_thi` lưu trữ lịch thi của các môn học cần theo dõi và nhắc nhở.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_LICH_THI` | `uuid` | x | x | x | Mã lịch thi (KHÓA CHÍNH) |
| `MA_MON_HOC` | `uuid` |  |  | x | Môn học - khóa ngoại tham chiếu `mon_hoc.MA_MON_HOC` |
| `LOAI_THI` | `varchar(100)` |  |  |  | Loại thi |
| `THOI_GIAN_THI` | `timestamptz` |  |  | x | Thời gian thi |
| `PHONG_THI` | `varchar(100)` |  |  |  | Phòng thi |

##### Bảng `deadline`

Mô tả: Bảng `deadline` lưu trữ các hạn nộp bài, bài tập hoặc công việc học tập theo môn học.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_DEADLINE` | `uuid` | x | x | x | Mã deadline (KHÓA CHÍNH) |
| `MA_MON_HOC` | `uuid` |  |  | x | Môn học - khóa ngoại tham chiếu `mon_hoc.MA_MON_HOC` |
| `TIEU_DE` | `varchar(255)` |  |  | x | Tiêu đề deadline |
| `MO_TA` | `text` |  |  |  | Mô tả |
| `HAN_NOP` | `timestamptz` |  |  | x | Hạn nộp |
| `TRANG_THAI` | `enum_trang_thai_deadline` |  |  | x | Trạng thái công việc |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Bảng `nhac_nho`

Mô tả: Bảng `nhac_nho` lưu trữ các nhắc nhở gắn với deadline hoặc lịch thi của người dùng.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_NHAC_NHO` | `uuid` | x | x | x | Mã nhắc nhở (KHÓA CHÍNH) |
| `MA_NGUOI_DUNG` | `uuid` |  |  | x | Người nhận nhắc nhở - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `MA_DEADLINE` | `uuid` |  |  |  | Deadline cần nhắc - khóa ngoại tham chiếu `deadline.MA_DEADLINE` |
| `MA_LICH_THI` | `uuid` |  |  |  | Lịch thi cần nhắc - khóa ngoại tham chiếu `lich_thi.MA_LICH_THI` |
| `THOI_GIAN_NHAC` | `timestamptz` |  |  | x | Thời gian nhắc |
| `THOI_GIAN_DA_GUI` | `timestamptz` |  |  |  | Thời điểm đã gửi |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |

##### Bảng `ghi_chu`

Mô tả: Bảng `ghi_chu` lưu trữ ghi chú học tập do người dùng tạo và có thể gắn với môn học.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_GHI_CHU` | `uuid` | x | x | x | Mã ghi chú (KHÓA CHÍNH) |
| `MA_NGUOI_DUNG` | `uuid` |  |  | x | Người tạo ghi chú - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `MA_MON_HOC` | `uuid` |  |  |  | Môn học liên quan - khóa ngoại tham chiếu `mon_hoc.MA_MON_HOC` |
| `TIEU_DE` | `varchar(255)` |  |  | x | Tiêu đề ghi chú |
| `NOI_DUNG` | `text` |  |  |  | Nội dung ghi chú |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Bảng `bo_flashcard`

Mô tả: Bảng `bo_flashcard` lưu trữ các bộ flashcard phục vụ ôn tập của người dùng.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_BO` | `uuid` | x | x | x | Mã bộ flashcard (KHÓA CHÍNH) |
| `MA_NGUOI_DUNG` | `uuid` |  |  | x | Người tạo bộ - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `MA_MON_HOC` | `uuid` |  |  |  | Môn học liên quan - khóa ngoại tham chiếu `mon_hoc.MA_MON_HOC` |
| `TEN_BO` | `varchar(255)` |  |  | x | Tên bộ flashcard |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Bảng `flashcard`

Mô tả: Bảng `flashcard` lưu trữ từng thẻ flashcard và thông tin phục vụ cơ chế ôn tập lặp lại.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_FLASHCARD` | `uuid` | x | x | x | Mã flashcard (KHÓA CHÍNH) |
| `MA_BO` | `uuid` |  |  | x | Bộ flashcard - khóa ngoại tham chiếu `bo_flashcard.MA_BO` |
| `MAT_TRUOC` | `text` |  |  | x | Nội dung mặt trước |
| `MAT_SAU` | `text` |  |  | x | Nội dung mặt sau |
| `SO_LAN_ON` | `integer` |  |  | x | Số lần đã ôn |
| `DIEM_GHI_NHO` | `numeric(5,2)` |  |  | x | Điểm ghi nhớ |
| `THOI_GIAN_LAN_ON_CUOI` | `timestamptz` |  |  |  | Lần ôn gần nhất |
| `THOI_GIAN_LAN_ON_TIEP_THEO` | `timestamptz` |  |  |  | Lần ôn tiếp theo |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Bảng `nhom_hoc_tap`

Mô tả: Bảng `nhom_hoc_tap` lưu trữ các nhóm học tập do người dùng tạo trong phạm vi trường hoặc môn học.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_NHOM` | `uuid` | x | x | x | Mã nhóm học tập (KHÓA CHÍNH) |
| `NGUOI_TAO` | `uuid` |  |  | x | Người tạo nhóm - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `MA_TRUONG` | `int` |  |  | x | Mã trường học - khóa ngoại tham chiếu `truong_hoc.MA_TRUONG` |
| `MA_MON` | `varchar(50)` |  |  |  | Mã môn học của trường |
| `TEN_NHOM` | `varchar(255)` |  |  | x | Tên nhóm |
| `MA_THAM_GIA` | `varchar(30)` |  | x | x | Mã tham gia nhóm |
| `LINK_NHOM_CHAT` | `text` |  |  | x | Đường link nhóm chat Zalo/Messenger/Discord |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Bảng `thanh_vien_nhom`

Mô tả: Bảng `thanh_vien_nhom` lưu trữ danh sách thành viên và vai trò của từng người trong nhóm học tập.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_NHOM` | `uuid` | x | x | x | Nhóm học tập - khóa ngoại tham chiếu `nhom_hoc_tap.MA_NHOM` (KHÓA CHÍNH) |
| `MA_NGUOI_DUNG` | `uuid` | x | x | x | Thành viên nhóm - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` (KHÓA CHÍNH) |
| `VAI_TRO_TRONG_NHOM` | `enum_vai_tro_nhom` |  |  | x | Vai trò điều hành nhóm |
| `THOI_GIAN_THAM_GIA` | `timestamptz` |  |  | x | Thời điểm tham gia |

Ràng buộc bổ sung: Khóa chính: (`MA_NHOM`, `MA_NGUOI_DUNG`).

##### Bảng `cong_viec_nhom`

Mô tả: Bảng `cong_viec_nhom` lưu trữ các công việc được tạo và phân công trong nhóm học tập.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_CONG_VIEC` | `uuid` | x | x | x | Mã công việc nhóm (KHÓA CHÍNH) |
| `MA_NHOM` | `uuid` |  |  | x | Nhóm học tập - khóa ngoại tham chiếu `nhom_hoc_tap.MA_NHOM` |
| `NGUOI_DUOC_GIAO` | `uuid` |  |  |  | Người được giao - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `TIEU_DE` | `varchar(255)` |  |  | x | Tiêu đề công việc |
| `MO_TA` | `text` |  |  |  | Mô tả công việc |
| `TRANG_THAI` | `enum_trang_thai_cong_viec` |  |  | x | Trạng thái công việc |
| `HAN_HOAN_THANH` | `timestamptz` |  |  |  | Hạn hoàn thành |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Bảng `binh_luan_cong_viec`

Mô tả: Bảng `binh_luan_cong_viec` lưu trữ bình luận trao đổi trong từng công việc nhóm.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_BINH_LUAN` | `uuid` | x | x | x | Mã bình luận (KHÓA CHÍNH) |
| `MA_CONG_VIEC` | `uuid` |  |  | x | Công việc được bình luận - khóa ngoại tham chiếu `cong_viec_nhom.MA_CONG_VIEC` |
| `MA_NGUOI_DUNG` | `uuid` |  |  | x | Người viết bình luận - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `NOI_DUNG` | `text` |  |  | x | Nội dung bình luận |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo bình luận |

##### Bảng `tai_lieu`

Mô tả: Bảng `tai_lieu` lưu trữ metadata của tài liệu được tải lên và chia sẻ trong hệ thống.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_TAI_LIEU` | `uuid` | x | x | x | Mã tài liệu (KHÓA CHÍNH) |
| `NGUOI_TAI_LEN` | `uuid` |  |  | x | Người tải lên - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `MA_MON_HOC` | `uuid` |  |  |  | Môn học liên quan - khóa ngoại tham chiếu `mon_hoc.MA_MON_HOC` |
| `MA_NHOM` | `uuid` |  |  |  | Nhóm liên quan - khóa ngoại tham chiếu `nhom_hoc_tap.MA_NHOM` |
| `MA_GHI_CHU` | `uuid` |  |  |  | Ghi chú liên quan - khóa ngoại tham chiếu `ghi_chu.MA_GHI_CHU` |
| `DUONG_DAN_LUU_TRU` | `text` |  | x | x | Đường dẫn lưu trữ Firebase Storage |
| `TEN_FILE` | `varchar(255)` |  |  | x | Tên file |
| `LOAI_FILE` | `varchar(100)` |  |  |  | Loại file |
| `DUNG_LUONG` | `bigint` |  |  |  | Dung lượng file |
| `CHE_DO_HIEN_THI` | `enum_che_do_hien_thi` |  |  | x | Chế độ hiển thị chia sẻ |
| `TRANG_THAI` | `enum_trang_thai_tai_lieu` |  |  | x | Trạng thái kiểm duyệt file |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |
| `THOI_GIAN_CAP_NHAT` | `timestamptz` |  |  | x | Thời điểm cập nhật |

##### Bảng `bao_cao_tai_lieu`

Mô tả: Bảng `bao_cao_tai_lieu` lưu trữ các báo cáo vi phạm đối với tài liệu do người dùng gửi.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_BAO_CAO` | `uuid` | x | x | x | Mã báo cáo tài liệu (KHÓA CHÍNH) |
| `MA_TAI_LIEU` | `uuid` |  |  | x | Tài liệu bị báo cáo - khóa ngoại tham chiếu `tai_lieu.MA_TAI_LIEU` |
| `NGUOI_BAO_CAO` | `uuid` |  |  | x | Người báo cáo - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `LY_DO` | `text` |  |  | x | Lý do báo cáo |
| `TRANG_THAI` | `enum_trang_thai_bao_cao` |  |  | x | Tiến độ xử lý report |
| `NGUOI_KIEM_DUYET` | `uuid` |  |  |  | Người kiểm duyệt - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `KET_QUA_KIEM_DUYET` | `text` |  |  |  | Kết quả kiểm duyệt |
| `THOI_GIAN_KIEM_DUYET` | `timestamptz` |  |  |  | Thời điểm kiểm duyệt |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |

Ràng buộc bổ sung: UNIQUE (`MA_TAI_LIEU`, `NGUOI_BAO_CAO`).

##### Bảng `thong_bao`

Mô tả: Bảng `thong_bao` lưu trữ thông báo hệ thống, deadline, nhóm học tập và nhắc nhở gửi tới người dùng.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_THONG_BAO` | `uuid` | x | x | x | Mã thông báo (KHÓA CHÍNH) |
| `MA_NGUOI_NHAN` | `uuid` |  |  | x | Người nhận thông báo - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `NGUOI_TAO` | `uuid` |  |  |  | Người tạo thông báo - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `TIEU_DE` | `varchar(255)` |  |  | x | Tiêu đề thông báo |
| `NOI_DUNG` | `text` |  |  | x | Nội dung thông báo |
| `LOAI_THONG_BAO` | `enum_loai_thong_bao` |  |  | x | Phân loại thông báo |
| `THOI_GIAN_DA_GUI` | `timestamptz` |  |  |  | Thời điểm đã gửi |
| `THOI_GIAN_DA_DOC` | `timestamptz` |  |  |  | Thời điểm đã đọc |
| `THOI_GIAN_TAO` | `timestamptz` |  |  | x | Thời điểm tạo |

##### Bảng `nhat_ky_he_thong`

Mô tả: Bảng `nhat_ky_he_thong` lưu trữ nhật ký thao tác và sự kiện phục vụ theo dõi hoạt động hệ thống.

| Thuộc tính | Kiểu | K | U | M | Diễn giải |
|---|---|---|---|---|---|
| `MA_NHAT_KY` | `BIGINT` | x | x | x | Mã nhật ký (KHÓA CHÍNH) |
| `NGUOI_THUC_HIEN` | `uuid` |  |  |  | Người thực hiện hành động - khóa ngoại tham chiếu `nguoi_dung.MA_NGUOI_DUNG` |
| `MUC_DO` | `enum_muc_do_log` |  |  | x | Mức độ nghiêm trọng của log |
| `HANH_DONG` | `varchar(100)` |  |  | x | Hành động thực hiện |
| `BANG_TAC_DONG` | `varchar(100)` |  |  |  | Bảng bị tác động |
| `MA_BAN_GHI` | `varchar(100)` |  |  |  | Mã bản ghi bị ảnh hưởng |
| `NOI_DUNG` | `text` |  |  |  | Nội dung log chi tiết |
| `DU_LIEU_JSON` | `jsonb` |  |  |  | Dữ liệu log bổ sung |
| `THOI_GIAN` | `timestamptz` |  |  | x | Thời điểm ghi log |

#### 3.1.3.5 Quan hệ khóa ngoại và chỉ mục đề xuất

Các quan hệ khóa ngoại chính trong mô hình vật lý gồm:

- ``nguoi_dung`` sử dụng ``MA_VAI_TRO`` tham chiếu đến ``vai_tro.MA_VAI_TRO`` theo quan hệ N-1.
- ``ho_so_sinh_vien`` sử dụng ``MA_NGUOI_DUNG`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ 1-1.
- ``ho_so_sinh_vien`` sử dụng ``MA_TRUONG`` tham chiếu đến ``truong_hoc.MA_TRUONG`` theo quan hệ N-1.
- ``phien_dang_nhap`` sử dụng ``MA_NGUOI_DUNG`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ N-1.
- ``thang_diem`` sử dụng ``MA_TRUONG`` tham chiếu đến ``truong_hoc.MA_TRUONG`` theo quan hệ N-1.
- ``chi_tiet_thang_diem`` sử dụng ``MA_THANG_DIEM`` tham chiếu đến ``thang_diem.MA_THANG_DIEM`` theo quan hệ N-1.
- ``quy_che_hoc_luc`` sử dụng ``MA_TRUONG`` tham chiếu đến ``truong_hoc.MA_TRUONG`` theo quan hệ N-1.
- ``hoc_ky`` sử dụng ``MA_NGUOI_DUNG`` tham chiếu đến ``ho_so_sinh_vien.MA_NGUOI_DUNG`` theo quan hệ N-1.
- ``mon_hoc`` sử dụng ``MA_HOC_KY`` tham chiếu đến ``hoc_ky.MA_HOC_KY`` theo quan hệ N-1.
- ``thanh_phan_diem`` sử dụng ``MA_MON_HOC`` tham chiếu đến ``mon_hoc.MA_MON_HOC`` theo quan hệ N-1.
- ``lich_hoc`` sử dụng ``MA_MON_HOC`` tham chiếu đến ``mon_hoc.MA_MON_HOC`` theo quan hệ N-1.
- ``lich_thi`` sử dụng ``MA_MON_HOC`` tham chiếu đến ``mon_hoc.MA_MON_HOC`` theo quan hệ N-1.
- ``deadline`` sử dụng ``MA_MON_HOC`` tham chiếu đến ``mon_hoc.MA_MON_HOC`` theo quan hệ N-1.
- ``nhac_nho`` sử dụng ``MA_NGUOI_DUNG`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ N-1.
- ``nhac_nho`` sử dụng ``MA_DEADLINE`` tham chiếu đến ``deadline.MA_DEADLINE`` theo quan hệ N-1.
- ``nhac_nho`` sử dụng ``MA_LICH_THI`` tham chiếu đến ``lich_thi.MA_LICH_THI`` theo quan hệ N-1.
- ``ghi_chu`` sử dụng ``MA_NGUOI_DUNG`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ N-1.
- ``ghi_chu`` sử dụng ``MA_MON_HOC`` tham chiếu đến ``mon_hoc.MA_MON_HOC`` theo quan hệ N-1.
- ``bo_flashcard`` sử dụng ``MA_NGUOI_DUNG`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ N-1.
- ``bo_flashcard`` sử dụng ``MA_MON_HOC`` tham chiếu đến ``mon_hoc.MA_MON_HOC`` theo quan hệ N-1.
- ``flashcard`` sử dụng ``MA_BO`` tham chiếu đến ``bo_flashcard.MA_BO`` theo quan hệ N-1.
- ``nhom_hoc_tap`` sử dụng ``NGUOI_TAO`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ N-1.
- ``thanh_vien_nhom`` sử dụng ``MA_NHOM`` tham chiếu đến ``nhom_hoc_tap.MA_NHOM`` theo quan hệ N-1.
- ``thanh_vien_nhom`` sử dụng ``MA_NGUOI_DUNG`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ N-1.
- ``cong_viec_nhom`` sử dụng ``MA_NHOM`` tham chiếu đến ``nhom_hoc_tap.MA_NHOM`` theo quan hệ N-1.
- ``cong_viec_nhom`` sử dụng ``NGUOI_DUOC_GIAO`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ N-1.
- ``binh_luan_cong_viec`` sử dụng ``MA_CONG_VIEC`` tham chiếu đến ``cong_viec_nhom.MA_CONG_VIEC`` theo quan hệ N-1.
- ``binh_luan_cong_viec`` sử dụng ``MA_NGUOI_DUNG`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ N-1.
- ``tai_lieu`` sử dụng ``NGUOI_TAI_LEN`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ N-1.
- ``tai_lieu`` sử dụng ``MA_MON_HOC`` tham chiếu đến ``mon_hoc.MA_MON_HOC`` theo quan hệ N-1.
- ``tai_lieu`` sử dụng ``MA_NHOM`` tham chiếu đến ``nhom_hoc_tap.MA_NHOM`` theo quan hệ N-1.
- ``tai_lieu`` sử dụng ``MA_GHI_CHU`` tham chiếu đến ``ghi_chu.MA_GHI_CHU`` theo quan hệ N-1.
- ``bao_cao_tai_lieu`` sử dụng ``MA_TAI_LIEU`` tham chiếu đến ``tai_lieu.MA_TAI_LIEU`` theo quan hệ N-1.
- ``bao_cao_tai_lieu`` sử dụng ``NGUOI_BAO_CAO`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ N-1.
- ``bao_cao_tai_lieu`` sử dụng ``NGUOI_KIEM_DUYET`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ N-1.
- ``thong_bao`` sử dụng ``MA_NGUOI_NHAN`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ N-1.
- ``thong_bao`` sử dụng ``NGUOI_TAO`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ N-1.
- ``nhat_ky_he_thong`` sử dụng ``NGUOI_THUC_HIEN`` tham chiếu đến ``nguoi_dung.MA_NGUOI_DUNG`` theo quan hệ N-1.

Các index đề xuất nhằm tối ưu tra cứu người dùng, phiên đăng nhập, học kỳ, môn học, deadline, nhắc nhở, nhóm học tập, tài liệu, thông báo và nhật ký hệ thống. Khi triển khai, các index này nên được tạo theo đúng nhu cầu truy vấn thường xuyên của từng chức năng để bảo đảm tốc độ xử lý và tránh dư thừa chỉ mục.

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
| `idx_lich_hoc_thu_tiet` | `lich_hoc` | (`THU`, `TIET_BAT_DAU`, `SO_TIET`) | Hỗ trợ kiểm tra trùng thời khóa biểu (XEM LẠI CÓ THỂ TỐI ƯU BẰNG KIỂU DỮ LIỆU RANGE KẾT HỢP VỚI INDEX GiST VÀ RÀNG BUỘC EXCLUDE) |
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
