# Buổi 14 – Luyện Đề Thi Thử
## Môn: Kỹ Thuật Lập Trình C

---

## Mục tiêu buổi học

- Làm quen cấu trúc và áp lực thời gian của đề thi
- Luyện kỹ năng đọc đề, chia nhỏ bài toán, viết chương trình hoàn chỉnh
- Tự kiểm tra, tìm lỗi bằng test case
- Chuẩn bị cho buổi thi thử tổng kết

---

## 1. Chiến Lược Làm Bài

| Bước | Việc cần làm | Thời gian gợi ý |
|---|---|---|
| 1 | Đọc **toàn bộ** đề, đánh dấu bài dễ | 5 phút |
| 2 | Làm bài dễ trước, lấy điểm chắc | |
| 3 | Với mỗi bài: xác định **vào – ra – xử lý** | |
| 4 | Viết khung chương trình, chia hàm | |
| 5 | Chạy thử với test mẫu và test biên | |
| 6 | Kiểm tra lại bộ nhớ (`free`), đóng tệp (`fclose`) | 5 phút cuối |

### Mẹo
- Chạy được còn hơn hoàn hảo: bài chưa xong vẫn nộp phần đã chạy.
- Viết từng hàm, test từng hàm, không viết hết rồi mới chạy.
- Đặt tên biến rõ nghĩa, có tên hàm đúng yêu cầu đề.
- Test biên: `n = 0`, `n = 1`, mảng toàn số âm, chuỗi rỗng, không tìm thấy.

### Khung chương trình mẫu

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// 1. struct / hằng số

// 2. khai báo nguyên mẫu hàm

int main() {
    // 3. nhập → gọi hàm → in kết quả
    return 0;
}

// 4. định nghĩa các hàm
```

---

## 2. Đề Luyện Tập 1 (90 phút)

### Câu 1 (2 điểm) – Cơ bản
Nhập số nguyên dương `n`. Kiểm tra `n` có phải số nguyên tố không; in ra tổng các chữ số của `n` và số đảo ngược của `n`.

### Câu 2 (3 điểm) – Mảng động
Nhập `n` rồi nhập mảng `n` số nguyên (cấp phát động).
a) In mảng.
b) Tìm giá trị lớn thứ hai (khác giá trị lớn nhất).
c) Xoá các phần tử trùng lặp, chỉ giữ lần xuất hiện đầu tiên. In mảng kết quả.
d) Giải phóng bộ nhớ.

### Câu 3 (3 điểm) – Struct + tệp
Tệp `sanpham.txt` gồm `n` dòng, mỗi dòng: `MaSP TenSP SoLuong DonGia` (không có khoảng trắng trong mã và tên).
a) Đọc vào mảng struct động.
b) In bảng sản phẩm; tính tổng giá trị kho `Σ SoLuong × DonGia`.
c) In sản phẩm có giá trị lớn nhất.
d) Ghi các sản phẩm có `SoLuong < 5` sang tệp `hethang.txt`.

### Câu 4 (2 điểm) – Chuỗi
Nhập một câu. Chuẩn hoá: bỏ khoảng trắng thừa, viết hoa chữ cái đầu mỗi từ. In ra số từ và từ dài nhất.

---

## 3. Đề Luyện Tập 2 (90 phút)

### Câu 1 (2 điểm)
Viết hàm đệ quy tính `S(n) = 1 + 1/2 + 1/3 + … + 1/n` và hàm đệ quy tính `C(n, k) = C(n-1, k-1) + C(n-1, k)` (biết `C(n,0) = C(n,n) = 1`).

### Câu 2 (3 điểm) – Ma trận động
Nhập ma trận `m x n` (cấp phát động).
a) In ma trận.
b) Tìm hàng có tổng lớn nhất, cột có tổng nhỏ nhất.
c) Tạo ma trận mới bằng cách xoá hàng và cột chứa phần tử lớn nhất (ma trận `(m-1) x (n-1)`). In ma trận mới.
d) Giải phóng bộ nhớ đúng cách.

### Câu 3 (3 điểm) – Struct + menu
Quản lý `HocSinh` gồm: mã, họ tên (có dấu cách), điểm TB. Viết menu:
1) Thêm học sinh  2) Xoá theo mã  3) Tìm theo tên  4) Sắp xếp giảm dần theo điểm  5) Lưu tệp  0) Thoát.

### Câu 4 (2 điểm) – Con trỏ và chuỗi
Viết hàm `void xoaKyTu(char *s, char c)` xoá mọi ký tự `c` khỏi chuỗi `s` **chỉ dùng con trỏ**. Viết hàm `int demTuXuatHien(char *s, char *tu)` đếm số lần chuỗi `tu` xuất hiện trong `s`.

---

## 4. Danh Sách Tự Kiểm Tra Trước Khi Nộp

- [ ] Chương trình **biên dịch không lỗi**, không cảnh báo quan trọng
- [ ] Mọi `scanf` có `&` (trừ mảng/chuỗi)
- [ ] Mọi `malloc` được kiểm tra NULL và có `free`
- [ ] Mọi `fopen` được kiểm tra NULL và có `fclose`
- [ ] Mảng không bị truy cập ngoài phạm vi `0..n-1`
- [ ] Chuỗi đủ chỗ cho `'\0'`
- [ ] Đã test với dữ liệu biên

---

## 5. Lỗi Thường Gặp Khi Thi

| Lỗi | Hậu quả | Phòng tránh |
|---|---|---|
| Không đọc kỹ đề | Làm sai yêu cầu | Gạch chân từ khoá: *tăng/giảm*, *chẵn/lẻ*, *đầu tiên/tất cả* |
| Dành quá nhiều thời gian cho 1 bài | Thiếu thời gian bài khác | Giới hạn thời gian mỗi bài |
| Quên `free`, `fclose` | Mất điểm | Viết `free` ngay khi viết `malloc` |
| Không test biên | Sai ở trường hợp đặc biệt | Luôn thử `n = 0`, `n = 1` |
| Viết một hàm `main` dài | Khó tìm lỗi | Chia thành các hàm nhỏ |

---

Buổi tiếp theo: Buổi 15 – Thi thử tổng kết
