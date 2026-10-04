# Buổi 6 – Luyện Tập Buổi 1 → 5
## Môn: Kỹ Thuật Lập Trình C

---

## Mục tiêu buổi học

- Ôn lại và củng cố kiến thức buổi 1 (biến, kiểu dữ liệu, nhập xuất, struct)
- Ôn lại và củng cố kiến thức buổi 2 (toán tử, if/else, switch)
- Ôn lại vòng lặp (buổi 3), mảng 1 chiều và 2 chiều (buổi 4), hàm (buổi 5)
- Tự lực giải bài tập, nhận ra và sửa lỗi

---

## Ôn Tập Nhanh

### Checklist trước khi làm bài

Kiểm tra lại các điểm sau trước khi bắt đầu:

| Kiến thức | Cần nhớ |
|---|---|
| Khai báo biến | `int x;` / `float y = 1.5f;` / `char c = 'A';` |
| Nhập | `scanf("%d", &x);` – nhớ dấu `&` |
| Xuất | `printf("%d\n", x);` – đúng format |
| Chia lấy dư | `n % 2` kiểm tra chẵn/lẻ |
| Ép kiểu | `(float)a / b` để ra số thực |
| if/else if | Điều kiện lớn hơn đặt trước |
| switch | Không quên `break` |
| Struct | Truy cập trường bằng dấu `.` |
| Vòng lặp | `for` khi biết số lần, `while` khi chưa biết; đừng quên cập nhật biến điều kiện |
| Mảng | Chỉ số từ `0` đến `n-1`, vòng lặp `i < n` |
| Mảng 2 chiều | Vòng ngoài là hàng `i`, vòng trong là cột `j` |
| Hàm | Khai báo kiểu trả về, tham số; mảng truyền vào hàm kèm kích thước `n` |

---

## Bài Tập

Làm theo thứ tự. Không xem gợi ý trước khi tự thử ít nhất 10 phút.

---

### Bài 1 – Đổi nhiệt độ

Nhập vào nhiệt độ theo Celsius. Tính và in ra nhiệt độ theo Fahrenheit và Kelvin.

Công thức:
```
F = C * 9 / 5 + 32
K = C + 273.15
```

Ví dụ:
```
Nhap nhiet do (Celsius): 100
Fahrenheit: 212.00
Kelvin    : 373.15
```

<details>
<summary>Gợi ý</summary>

```c
#include <stdio.h>

int main() {
    float C, F, K;

    printf("Nhap nhiet do (Celsius): ");
    scanf("%f", &C);

    F = C * 9.0f / 5.0f + 32;
    K = C + 273.15f;

    printf("Fahrenheit: %.2f\n", F);
    printf("Kelvin    : %.2f\n", K);

    return 0;
}
```

Lưu ý: viết `9.0f / 5.0f` thay vì `9 / 5` vì `9 / 5` trong C cho kết quả là `1` (chia nguyên).

</details>

---

### Bài 2 – Tính tiền taxi

Cước taxi tính như sau:
- 1 km đầu: 15,000 đồng
- Từ km thứ 2 đến km thứ 10: 12,000 đồng/km
- Từ km thứ 11 trở đi: 11,000 đồng/km

Nhập số km. In ra tổng tiền cần trả.

Ví dụ:
```
Nhap so km: 15
Tong tien: 178000 dong
```

<details>
<summary>Giải thích cách tính</summary>

15 km:
- 1 km đầu: 15,000
- 9 km tiếp (km 2–10): 9 * 12,000 = 108,000
- 5 km còn lại (km 11–15): 5 * 11,000 = 55,000
- Tổng: 15,000 + 108,000 + 55,000 = 178,000

</details>

<details>
<summary>Gợi ý code</summary>

```c
#include <stdio.h>

int main() {
    float km;
    long tien = 0;

    printf("Nhap so km: ");
    scanf("%f", &km);

    if (km <= 1) {
        tien = 15000;
    } else if (km <= 10) {
        tien = 15000 + (long)((km - 1) * 12000);
    } else {
        tien = 15000 + 9 * 12000 + (long)((km - 10) * 11000);
    }

    printf("Tong tien: %ld dong\n", tien);
    return 0;
}
```

</details>

---

### Bài 3 – Phân loại tam giác

Nhập vào 3 cạnh `a`, `b`, `c` (kiểu float).

Yêu cầu:
1. Kiểm tra 3 cạnh có tạo thành tam giác không (tổng 2 cạnh bất kỳ phải lớn hơn cạnh còn lại)
2. Nếu là tam giác, phân loại:
   - Đều: `a == b == c`
   - Cân: chỉ có 2 cạnh bằng nhau
   - Vuông: `a² + b² == c²` (hoặc hoán vị – cạnh dài nhất là cạnh huyền)
   - Thường: không thoả mãn điều kiện nào trên

Ví dụ:
```
Nhap a: 3
Nhap b: 4
Nhap c: 5
Tam giac VUONG
```

<details>
<summary>Gợi ý</summary>

```c
#include <stdio.h>

int main() {
    float a, b, c;

    printf("Nhap a: "); scanf("%f", &a);
    printf("Nhap b: "); scanf("%f", &b);
    printf("Nhap c: "); scanf("%f", &c);

    if (a + b <= c || a + c <= b || b + c <= a) {
        printf("Khong phai tam giac\n");
        return 0;
    }

    if (a == b && b == c) {
        printf("Tam giac DEU\n");
    } else if (a == b || b == c || a == c) {
        printf("Tam giac CAN\n");
    } else {
        // Tìm cạnh lớn nhất để kiểm tra vuông
        float max = a;
        if (b > max) max = b;
        if (c > max) max = c;

        float tong;
        if (max == a) tong = b*b + c*c;
        else if (max == b) tong = a*a + c*c;
        else tong = a*a + b*b;

        if (max * max == tong)
            printf("Tam giac VUONG\n");
        else
            printf("Tam giac THUONG\n");
    }

    return 0;
}
```

Lưu ý: so sánh số thực bằng `==` đôi khi không chính xác do sai số máy tính. Trong bài học này tạm chấp nhận.

</details>

---

### Bài 4 – Struct sinh viên và xếp loại

Định nghĩa struct `SinhVien` gồm: họ tên, mã SV, điểm lý thuyết (hệ số 3), điểm thực hành (hệ số 2).

Nhập thông tin 1 sinh viên. Tính điểm trung bình theo hệ số:
```
DTB = (lyThuyet * 3 + thucHanh * 2) / 5
```

In ra thông tin và xếp loại:
- DTB >= 9.0: Xuất sắc
- DTB >= 8.0: Giỏi
- DTB >= 6.5: Khá
- DTB >= 5.0: Trung bình
- DTB < 5.0: Yếu

Ví dụ:
```
Nhap ho ten: Nguyen Van A
Nhap ma SV : SV001
Nhap diem ly thuyet (0-10): 8.5
Nhap diem thuc hanh (0-10): 7.0

--- Ket qua ---
Ho ten  : Nguyen Van A
Ma SV   : SV001
DTB     : 7.90
Xep loai: Kha
```

<details>
<summary>Gợi ý</summary>

```c
#include <stdio.h>

typedef struct {
    char hoTen[100];
    char maSV[20];
    float lyThuyet;
    float thucHanh;
} SinhVien;

int main() {
    SinhVien sv;

    printf("Nhap ho ten: ");
    scanf(" %[^\n]", sv.hoTen);   // đọc cả chuỗi có khoảng trắng

    printf("Nhap ma SV : ");
    scanf("%s", sv.maSV);

    printf("Nhap diem ly thuyet (0-10): ");
    scanf("%f", &sv.lyThuyet);

    printf("Nhap diem thuc hanh (0-10): ");
    scanf("%f", &sv.thucHanh);

    float dtb = (sv.lyThuyet * 3 + sv.thucHanh * 2) / 5.0f;

    printf("\n--- Ket qua ---\n");
    printf("Ho ten  : %s\n", sv.hoTen);
    printf("Ma SV   : %s\n", sv.maSV);
    printf("DTB     : %.2f\n", dtb);

    printf("Xep loai: ");
    if (dtb >= 9.0f)       printf("Xuat sac\n");
    else if (dtb >= 8.0f)  printf("Gioi\n");
    else if (dtb >= 6.5f)  printf("Kha\n");
    else if (dtb >= 5.0f)  printf("Trung binh\n");
    else                   printf("Yeu\n");

    return 0;
}
```

Lưu ý về `scanf(" %[^\n]", sv.hoTen)`:
- `%[^\n]` đọc tất cả ký tự cho đến khi gặp Enter, dùng khi họ tên có khoảng trắng
- Dấu cách trước `%` để bỏ qua ký tự Enter còn sót từ lần `scanf` trước

</details>

---

### Bài 5 – Máy tính với menu

Viết chương trình hiển thị menu:
```
=== MAY TINH DON GIAN ===
1. Cong
2. Tru
3. Nhan
4. Chia
0. Thoat
Chon chuc nang:
```

Nhập lựa chọn và 2 số, thực hiện phép tính tương ứng.
Dùng `switch` để xử lý lựa chọn.

<details>
<summary>Gợi ý</summary>

```c
#include <stdio.h>

int main() {
    int chon;
    float a, b;

    printf("=== MAY TINH DON GIAN ===\n");
    printf("1. Cong\n");
    printf("2. Tru\n");
    printf("3. Nhan\n");
    printf("4. Chia\n");
    printf("0. Thoat\n");
    printf("Chon chuc nang: ");
    scanf("%d", &chon);

    if (chon == 0) {
        printf("Tam biet!\n");
        return 0;
    }

    printf("Nhap a: "); scanf("%f", &a);
    printf("Nhap b: "); scanf("%f", &b);

    switch (chon) {
        case 1:
            printf("Ket qua: %.2f\n", a + b);
            break;
        case 2:
            printf("Ket qua: %.2f\n", a - b);
            break;
        case 3:
            printf("Ket qua: %.2f\n", a * b);
            break;
        case 4:
            if (b == 0)
                printf("Loi: Khong chia duoc cho 0\n");
            else
                printf("Ket qua: %.2f\n", a / b);
            break;
        default:
            printf("Lua chon khong hop le\n");
    }

    return 0;
}
```

</details>

---

### Bài 6 – Tổng hợp (bài khó)

Định nghĩa struct `HoaDon` gồm: tên khách hàng, số lượng sản phẩm (`soLuong`, kiểu int), đơn giá (`donGia`, kiểu float).

Nhập thông tin 1 hoá đơn. Tính tiền thanh toán theo quy tắc:
- Mua dưới 5 sản phẩm: không giảm
- Mua từ 5 đến 9 sản phẩm: giảm 5%
- Mua từ 10 đến 19 sản phẩm: giảm 10%
- Mua từ 20 sản phẩm trở lên: giảm 20%

In ra hoá đơn gồm: tên khách, số lượng, đơn giá, % giảm, tiền gốc, tiền được giảm, tiền phải trả.

Ví dụ:
```
Nhap ten khach hang: Tran Thi B
Nhap so luong     : 12
Nhap don gia      : 50000

====== HOA DON ======
Khach hang : Tran Thi B
So luong   : 12
Don gia    : 50000.00
Tien goc   : 600000.00
Giam gia   : 10%
Tien giam  : 60000.00
Thanh toan : 540000.00
```

<details>
<summary>Gợi ý</summary>

```c
#include <stdio.h>

typedef struct {
    char tenKhach[100];
    int soLuong;
    float donGia;
} HoaDon;

int main() {
    HoaDon hd;

    printf("Nhap ten khach hang: ");
    scanf(" %[^\n]", hd.tenKhach);

    printf("Nhap so luong     : ");
    scanf("%d", &hd.soLuong);

    printf("Nhap don gia      : ");
    scanf("%f", &hd.donGia);

    float tienGoc = hd.soLuong * hd.donGia;

    int phanTramGiam;
    if (hd.soLuong >= 20)      phanTramGiam = 20;
    else if (hd.soLuong >= 10) phanTramGiam = 10;
    else if (hd.soLuong >= 5)  phanTramGiam = 5;
    else                       phanTramGiam = 0;

    float tienGiam  = tienGoc * phanTramGiam / 100.0f;
    float thanhToan = tienGoc - tienGiam;

    printf("\n====== HOA DON ======\n");
    printf("Khach hang : %s\n", hd.tenKhach);
    printf("So luong   : %d\n", hd.soLuong);
    printf("Don gia    : %.2f\n", hd.donGia);
    printf("Tien goc   : %.2f\n", tienGoc);
    printf("Giam gia   : %d%%\n", phanTramGiam);
    printf("Tien giam  : %.2f\n", tienGiam);
    printf("Thanh toan : %.2f\n", thanhToan);

    return 0;
}
```

</details>

---

### Bài 7 – Vòng lặp: số nguyên tố

Nhập `n`. In ra tất cả số nguyên tố nhỏ hơn hoặc bằng `n` và đếm xem có bao nhiêu số.


---

### Bài 8 – Mảng 1 chiều: sắp xếp và chèn

Nhập mảng `n` phần tử số nguyên.
- Sắp xếp mảng tăng dần (dùng sắp xếp nổi bọt hoặc chọn)
- In ra mảng sau khi sắp xếp, phần tử lớn nhất và lớn thứ hai


---

### Bài 9 – Mảng 2 chiều: ma trận chuyển vị

Nhập ma trận `m x n`. Tạo và in ra ma trận chuyển vị (`n x m`) với `b[j][i] = a[i][j]`.

---

### Bài 10 – Hàm: tổng hợp

Viết các hàm sau và gọi trong `main`:
- `void nhapMang(int a[], int n)`
- `void xuatMang(int a[], int n)`
- `int timMax(int a[], int n)`
- `int demChan(int a[], int n)`
- `int laNguyenTo(int k)`

Trong `main` nhập mảng, in mảng, in ra max, số phần tử chẵn và các số nguyên tố trong mảng.


---

## Tổng Kết Buổi 6

| Nội dung đã ôn | Áp dụng trong bài |
|---|---|
| Biến, kiểu dữ liệu, nhập xuất | Tất cả các bài |
| Ép kiểu float | Bài 1, 2 |
| if / else if / else | Bài 2, 3, 4, 6 |
| switch | Bài 5 |
| Struct | Bài 4, 6 |
| Toán tử logic `&&`, `\|\|` | Bài 3 |
| Vòng lặp | Bài 7, 8 |
| Mảng 1 chiều, 2 chiều | Bài 8, 9, 10 |
| Hàm | Bài 10 |

---

Buổi tiếp theo: Buổi 7 – Con trỏ và Nhập xuất tệp
