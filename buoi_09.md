# Buổi 9 – Luyện Tập Buổi 7 + 8
## Môn: Kỹ Thuật Lập Trình C

---

## Mục tiêu buổi học

- Củng cố con trỏ, truyền địa chỉ vào hàm, con trỏ và mảng
- Củng cố nhập xuất tệp
- Củng cố xử lý chuỗi bằng con trỏ và `<string.h>`
- Kết hợp nhiều kỹ thuật trong cùng một bài

---

## Ôn Tập Nhanh

| Kiến thức | Cần nhớ |
|---|---|
| Con trỏ | `int *p = &a;` – `*p` là giá trị, `p` là địa chỉ |
| Hàm đổi biến | Tham số `int *x`, gọi bằng `&a` |
| Mảng & con trỏ | `a[i]` ≡ `*(a + i)` |
| Struct | `p->truong` khi có con trỏ struct |
| Tệp | `fopen` → kiểm tra `NULL` → `fscanf/fprintf` → `fclose` |
| Chuỗi | Kết thúc `'\0'`, so sánh bằng `strcmp`, nhập dòng bằng `fgets` |

---

## Bài Tập

Làm theo thứ tự. Tự thử ít nhất 10 phút trước khi xem gợi ý.

---

### Bài 1 – Con trỏ cơ bản

Khai báo `int a = 10, b = 20`. Dùng con trỏ `p`:
- In `a` thông qua `p`, sau đó cho `p` trỏ sang `b` và in `b`
- Tăng `a` thêm 5 thông qua con trỏ
- In ra địa chỉ của `a` và `b`

---

### Bài 2 – Hàm với con trỏ

Viết hàm `void thongKe(int a[], int n, int *tong, int *soAm, int *soDuong)`.
Nhập mảng, gọi hàm và in 3 kết quả.


---

### Bài 3 – Đảo mảng bằng con trỏ

Viết hàm `void daoMang(int *a, int n)` dùng hai con trỏ `đầu` và `cuối` để đảo ngược mảng tại chỗ.


---

### Bài 4 – Ghi và đọc tệp số

1. Nhập `n` và `n` số nguyên, ghi vào `so.txt`
2. Đọc lại từ `so.txt`, in ra màn hình
3. Ghi các số **chẵn** sang tệp `chan.txt`

---

### Bài 5 – Đếm trong tệp văn bản

Tạo sẵn tệp `vanban.txt` có vài dòng chữ. Viết chương trình đọc từng ký tự (`fgetc`) và đếm:
- Tổng số ký tự
- Số dòng
- Số nguyên âm


---

### Bài 6 – Chuỗi: tần suất ký tự

Nhập một chuỗi chữ thường. In ra số lần xuất hiện của mỗi chữ cái có trong chuỗi.


---

### Bài 7 – Chuỗi: tách và đảo từ

Nhập một câu. In ra câu với **thứ tự các từ bị đảo ngược**.
Ví dụ: `"toi yeu lap trinh"` → `"trinh lap yeu toi"`.


---

### Bài 8 – Quản lý sinh viên từ tệp (bài khó)

Tệp `sv.txt` có dạng: mỗi dòng `Ten Diem` (tên không có khoảng trắng).

```
An 8.5
Binh 6.0
Chi 9.0
```

Viết chương trình:
1. Đọc danh sách vào mảng struct `SinhVien`
2. In danh sách, in điểm trung bình lớp
3. Sắp xếp giảm dần theo điểm và ghi ra `sv_sapxep.txt`
4. Nhập tên cần tìm (dùng `strcmp`), in điểm nếu tìm thấy

---

## Tổng Kết Buổi 9

| Nội dung đã ôn | Áp dụng trong bài |
|---|---|
| Con trỏ cơ bản | Bài 1 |
| Con trỏ làm tham số | Bài 2, 3 |
| Nhập xuất tệp | Bài 4, 5, 8 |
| Xử lý chuỗi | Bài 6, 7, 8 |
| Struct + mảng + tệp | Bài 8 |

---

Buổi tiếp theo: Buổi 10 – Cấp phát động – Mảng 1 chiều
