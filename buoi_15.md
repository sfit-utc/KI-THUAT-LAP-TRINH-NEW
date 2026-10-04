# Buổi 15 – Thi Thử Tổng Kết
## Môn: Kỹ Thuật Lập Trình C

---

## Thông Tin Buổi Thi

| Mục | Nội dung |
|---|---|
| Thời gian | 120 phút |
| Hình thức | Viết chương trình C trên máy, tài liệu đóng |
| Thang điểm | 10 |
| Phạm vi | Buổi 1 → 14 |
| Nộp bài | 1 file `.c` cho mỗi câu (`cau1.c`, `cau2.c`, …) |

### Cơ cấu điểm

| Câu | Nội dung | Điểm |
|---|---|---|
| 1 | Cơ bản: vòng lặp, rẽ nhánh, hàm | 2 |
| 2 | Mảng 1 chiều / 2 chiều (động) | 2.5 |
| 3 | Chuỗi và con trỏ | 2 |
| 4 | Struct + tệp + cấp phát động | 3.5 |

### Quy định
- Không dùng điện thoại, không trao đổi bài.
- Bài biên dịch lỗi được **0 điểm** câu đó – hãy biên dịch thường xuyên.
- Chấm theo test case ẩn: cần đúng định dạng xuất như đề yêu cầu.
- Rò rỉ bộ nhớ, quên đóng tệp bị trừ điểm.

---

## Ôn Tập Nhanh Trước Khi Thi (10 phút)

| Chủ đề | Nhớ |
|---|---|
| Nhập xuất | `scanf("%d", &x)`, `%f`, `%c`, `%s` – đúng format |
| Rẽ nhánh / lặp | `if/else`, `switch` + `break`, `for/while/do-while` |
| Mảng | Chỉ số `0..n-1`; max/min khởi tạo bằng `a[0]` |
| Hàm | Mảng truyền kèm `n`; đổi biến cần con trỏ |
| Con trỏ | `*p` giá trị, `&a` địa chỉ, `p->x` |
| Chuỗi | `'\0'`, `strcmp`, `fgets` / `" %[^\n]"` |
| Tệp | `fopen` → NULL? → `fscanf/fprintf` → `fclose` |
| Cấp phát | `malloc/calloc/realloc/free`, kiểm tra NULL |
| Ma trận động | Cấp từng hàng; free từng hàng rồi free con trỏ |
| Đệ quy | Điều kiện dừng + bước thu nhỏ bài toán |

---

## ĐỀ THI THỬ

### Câu 1 (2 điểm) – Cơ bản

Viết chương trình nhập số nguyên dương `n` (nhập lại cho đến khi hợp lệ, dùng `do-while`). Thực hiện bằng các hàm riêng:
a) `int laHoanHao(int n)` – kiểm tra số hoàn hảo (tổng ước thực sự bằng chính nó, ví dụ 6, 28).
b) In ra tất cả số hoàn hảo nhỏ hơn hoặc bằng `n`.
c) `long long tongLuyThua(int n)` – tính `1 + 2² + 3³ … + nⁿ` (dùng vòng lặp, không dùng `pow`) hoặc đệ quy.

---

### Câu 2 (2.5 điểm) – Mảng động

Nhập `m`, `n` rồi nhập ma trận số nguyên `m x n` (cấp phát động).
a) In ma trận (mỗi phần tử rộng 5 ký tự).
b) Tìm và in các phần tử là số nguyên tố cùng vị trí.
c) Tìm hàng có nhiều số chẵn nhất.
d) Sắp xếp mỗi hàng tăng dần, in lại ma trận.
e) Giải phóng bộ nhớ.

Ví dụ:
```
Nhap m, n: 2 3
1 2 3
4 5 6
Phan tu nguyen to: a[0][1]=2  a[0][2]=3  a[1][1]=5
Hang nhieu so chan nhat: 1
```

---

### Câu 3 (2 điểm) – Chuỗi và con trỏ

Nhập một đoạn văn bản (có khoảng trắng).
a) Viết hàm `void chuanHoa(char *s)`: xoá khoảng trắng thừa, viết hoa chữ cái đầu mỗi từ.
b) Viết hàm `int demTu(char *s)` và `char* tuDaiNhat(char *s, int *doDai)`.
c) Viết hàm đệ quy `int doiXung(char *s, int dau, int cuoi)` kiểm tra chuỗi (không phân biệt hoa/thường) có đối xứng không.

---

### Câu 4 (3.5 điểm) – Struct + Tệp + Cấp phát động

Tệp `nhanvien.txt` có dạng:
```
4
NV01 Nguyen_Van_An 5000000 2.5
NV02 Tran_Thi_Binh 4500000 3.0
NV03 Le_Minh_Chau 6000000 2.0
NV04 Pham_Duc_Dung 5500000 3.5
```
Dòng 1 là số nhân viên; mỗi dòng sau: mã, họ tên (dùng `_` thay khoảng trắng), lương cơ bản, hệ số lương.

Định nghĩa struct `NhanVien` và thực hiện:
a) Đọc tệp vào **mảng struct cấp phát động**. Báo lỗi nếu không mở được tệp.
b) Tính lương = lương cơ bản × hệ số; in bảng có tiêu đề, thẳng hàng.
c) Tìm và in nhân viên lương cao nhất, tổng quỹ lương và lương trung bình.
d) Sắp xếp giảm dần theo lương, ghi ra tệp `ketqua.txt`.
e) Nhập mã nhân viên cần xoá, xoá khỏi mảng (thu nhỏ bằng `realloc`), in lại danh sách.
f) Giải phóng bộ nhớ và đóng tệp.

---

## Tiêu Chí Chấm Điểm

| Tiêu chí | Tỉ trọng |
|---|---|
| Chương trình chạy đúng kết quả | 60% |
| Chia hàm hợp lý, đặt tên rõ ràng | 15% |
| Xử lý trường hợp biên, kiểm tra lỗi (NULL, tệp) | 15% |
| Quản lý bộ nhớ (`free`, `fclose`) | 10% |

---

## Sau Buổi Thi

1. Tự rà soát bài làm, ghi lại các lỗi đã mắc.
2. Làm lại các câu chưa hoàn thành.
3. Xem lại phần *Lỗi Thường Gặp* của các buổi tương ứng với lỗi của bạn.

Chúc các bạn thi tốt!
