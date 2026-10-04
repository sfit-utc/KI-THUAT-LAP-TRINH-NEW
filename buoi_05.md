# Buổi 5 – Hàm
## Môn: Kỹ Thuật Lập Trình C

---

## Mục tiêu buổi học

Sau buổi này sinh viên có thể:
- Hiểu vì sao cần chia chương trình thành các hàm
- Khai báo, định nghĩa và gọi hàm
- Phân biệt truyền tham trị và truyền mảng vào hàm
- Hiểu phạm vi biến (cục bộ / toàn cục)
- Viết hàm xử lý mảng và struct

---

## 1. Tại sao cần hàm?

Khi chương trình dài, ta chia thành các khối nhỏ, mỗi khối làm **một việc**. Mỗi khối đó là một **hàm**.

Lợi ích:
- Tái sử dụng: viết một lần, gọi nhiều lần
- Dễ đọc, dễ sửa lỗi
- Chia việc khi làm nhóm

Thực ra bạn đã dùng hàm từ buổi 1: `printf`, `scanf`, `main` đều là hàm.

---

## 2. Cú pháp hàm

```c
kieu_tra_ve ten_ham(kieu1 thamso1, kieu2 thamso2) {
    // thân hàm
    return gia_tri;
}
```

Ví dụ: hàm tính tổng hai số

```c
#include <stdio.h>

int tong(int a, int b) {
    return a + b;
}

int main() {
    int x = 5, y = 7;
    int kq = tong(x, y);
    printf("Tong = %d\n", kq);
    return 0;
}
```

Kết quả:
```
Tong = 12
```

<details>
<summary>Giải thích từng phần</summary>

- `int` đứng trước tên hàm: hàm trả về một số nguyên.
- `a`, `b`: **tham số hình thức** – biến tạm nhận giá trị khi gọi hàm.
- `x`, `y` trong `tong(x, y)`: **đối số** – giá trị thực truyền vào.
- `return a + b;` trả kết quả về nơi gọi và kết thúc hàm ngay lập tức.

</details>

<details>
<summary>Giải thích kỹ: chuyện gì xảy ra khi gọi <code>tong(x, y)</code>?</summary>

Theo từng bước:
1. `main` đang chạy, gặp `tong(x, y)` → **tạm dừng** `main`.
2. Chương trình tạo ra các biến `a`, `b` cho hàm `tong` và **sao chép** giá trị: `a = 5`, `b = 7`.
3. Thân hàm chạy, tính `a + b = 12`.
4. `return 12;` kết thúc hàm, các biến `a`, `b` bị huỷ.
5. Giá trị 12 quay về đúng chỗ gọi hàm, nên `kq = tong(x, y)` thành `kq = 12`.
6. `main` chạy tiếp.

Tư duy quan trọng: **hàm giống một cái máy** – bỏ nguyên liệu vào (tham số), máy xử lý, trả sản phẩm ra (`return`). Mỗi hàm chỉ nên làm **một việc**, và tên hàm nên là động từ cho biết việc đó (`tinhTong`, `timMax`, `nhapMang`).

Có thể gọi hàm ở nhiều nơi, kể cả làm đối số cho hàm khác: `printf("%d", tong(tong(1, 2), 3));`.

</details>

### Hàm không trả về giá trị – `void`

```c
void chaoMung(char ten[]) {
    printf("Xin chao, %s!\n", ten);
}
```

Hàm `void` không cần `return` (hoặc chỉ viết `return;`).

---

## 3. Khai báo nguyên mẫu (prototype)

C đọc file từ trên xuống. Nếu gọi hàm trước khi định nghĩa sẽ báo lỗi/cảnh báo. Giải pháp: khai báo nguyên mẫu ở đầu file.

```c
#include <stdio.h>

int tong(int a, int b);      // nguyên mẫu – nhớ dấu ;

int main() {
    printf("%d\n", tong(2, 3));
    return 0;
}

int tong(int a, int b) {     // định nghĩa đặt sau main
    return a + b;
}
```

---

## 4. Truyền tham trị

Mặc định C truyền **bản sao** của giá trị vào hàm. Thay đổi tham số bên trong hàm **không** ảnh hưởng biến bên ngoài.

```c
void tang(int x) {
    x = x + 1;
}

int main() {
    int a = 5;
    tang(a);
    printf("%d\n", a);    // vẫn in 5
    return 0;
}
```

<details>
<summary>Muốn hàm sửa được biến bên ngoài thì sao?</summary>

Dùng **con trỏ** (sẽ học ở buổi 7). Ví dụ trước: `void tang(int *x) { *x = *x + 1; }` và gọi `tang(&a);`.

Hiện tại chỉ cần nhớ: hàm nhận số bình thường → làm việc trên bản sao.

</details>

<details>
<summary>Giải thích kỹ: khi nào dùng hàm trả về giá trị, khi nào dùng void?</summary>

| Tình huống | Chọn |
|---|---|
| Hàm **tính ra một kết quả** (tổng, max, kiểm tra đúng/sai) | Trả về giá trị: `int`, `float`... |
| Hàm **làm một việc** rồi thôi (in, nhập vào mảng) | `void` |
| Hàm kiểm tra "có/không" | Trả `int`: `1` là đúng, `0` là sai (đặt tên bắt đầu bằng `la...`, `co...`) |

Một hàm chỉ `return` được **một** giá trị. Muốn trả nhiều kết quả thì dùng con trỏ (buổi 7) hoặc struct.

</details>

---

## 5. Truyền mảng vào hàm

Mảng khi truyền vào hàm **không bị sao chép** – hàm làm việc trực tiếp trên mảng gốc. Vì vậy phải truyền kèm kích thước `n`.

```c
#include <stdio.h>

void nhapMang(int a[], int n) {
    for (int i = 0; i < n; i++) {
        printf("a[%d] = ", i);
        scanf("%d", &a[i]);
    }
}

void xuatMang(int a[], int n) {
    for (int i = 0; i < n; i++)
        printf("%d ", a[i]);
    printf("\n");
}

int timMax(int a[], int n) {
    int max = a[0];
    for (int i = 1; i < n; i++)
        if (a[i] > max) max = a[i];
    return max;
}

int main() {
    int a[100], n;
    printf("Nhap n: ");
    scanf("%d", &n);

    nhapMang(a, n);
    xuatMang(a, n);
    printf("Max = %d\n", timMax(a, n));
    return 0;
}
```

<details>
<summary>Tại sao hàm sửa được mảng gốc?</summary>

Tên mảng thực chất là **địa chỉ của phần tử đầu tiên**. Khi truyền `a` vào hàm, ta truyền địa chỉ đó, nên hàm đọc/ghi thẳng vào vùng nhớ của mảng gốc. Đây là kiến thức nền để học con trỏ ở buổi 7.

</details>

<details>
<summary>Giải thích kỹ: theo dõi chương trình mảng ở trên</summary>

Giả sử nhập `n = 3`, mảng `{4, 9, 2}`.

- `nhapMang(a, n)`: `a` là địa chỉ mảng gốc, vòng `for` ghi trực tiếp vào `a[0..2]` → mảng ở `main` có dữ liệu ngay sau khi hàm kết thúc, **không cần return**.
- `xuatMang(a, n)`: chỉ đọc nên in `4 9 2`.
- `timMax(a, n)`: `max` lấy `a[0] = 4`, vòng lặp bắt đầu từ `i = 1` (vì phần tử 0 đã được lấy), gặp 9 > 4 → `max = 9`; gặp 2 → giữ nguyên; trả về 9.

Vì sao phải truyền `n`? Bên trong hàm, `a` chỉ là một địa chỉ, **không còn thông tin độ dài**. Hàm không thể tự biết mảng có mấy phần tử.

Vì sao viết `int a[]` mà không ghi kích thước? Trình biên dịch bỏ qua kích thước đó; `int a[]` và `int *a` hoàn toàn giống nhau trong tham số hàm.

</details>

### Mảng 2 chiều

Với mảng 2 chiều, bắt buộc phải ghi rõ số cột:

```c
void xuatMaTran(int a[][100], int m, int n) {
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++)
            printf("%d ", a[i][j]);
        printf("\n");
    }
}
```

<details>
<summary>Giải thích kỹ: vì sao mảng 2 chiều phải ghi số cột?</summary>

Mảng 2 chiều lưu trong bộ nhớ thành **một dãy liên tục**, hàng này nối hàng kia. Để tìm `a[i][j]`, máy tính cần tính `i * số_cột + j`. Nếu không biết số cột thì không thể tính được vị trí → trình biên dịch bắt buộc bạn ghi `a[][100]`. Số hàng thì không cần vì không tham gia công thức.

</details>

---

## 6. Truyền struct vào hàm

```c
typedef struct {
    char ten[50];
    float diem;
} SinhVien;

void xuatSV(SinhVien sv) {
    printf("%s - %.1f\n", sv.ten, sv.diem);
}

SinhVien nhapSV() {
    SinhVien sv;
    printf("Ten : "); scanf(" %[^\n]", sv.ten);
    printf("Diem: "); scanf("%f", &sv.diem);
    return sv;                  // hàm có thể trả về struct
}
```

---

## 7. Phạm vi biến

| Loại | Khai báo ở đâu | Sống đến khi nào |
|---|---|---|
| Biến cục bộ | Trong hàm | Hết hàm |
| Biến toàn cục | Ngoài mọi hàm | Hết chương trình |

```c
int dem = 0;          // toàn cục

void tangDem() {
    int tam = 1;      // cục bộ – chỉ dùng trong tangDem
    dem += tam;
}
```

> Hạn chế dùng biến toàn cục: dễ gây lỗi khó tìm. Ưu tiên truyền tham số.

---

## 8. Tóm Tắt Nhanh

```
Hàm có giá trị trả về  ->  int f(int a, int b) { return a + b; }
Hàm không trả về       ->  void in(int a) { printf("%d", a); }
Nguyên mẫu             ->  int f(int a, int b);
Truyền mảng            ->  void xuat(int a[], int n);   // kèm n
Truyền số              ->  làm việc trên bản sao
```

---

## Bài Tập Buổi 5

### Bài 1 – Dễ
Viết các hàm: `int binhPhuong(int x)`, `int max2(int a, int b)`, `int laChan(int n)` (trả về 1 nếu chẵn, 0 nếu lẻ). Gọi chúng trong `main`.

---

### Bài 2 – Trung bình
Viết hàm `int laNguyenTo(int n)` và `int tongChuSo(int n)`.
Nhập `n`, in ra các số nguyên tố từ 1 đến `n` và tổng các chữ số của `n`.


---

### Bài 3 – Hàm xử lý mảng
Viết các hàm cho mảng số nguyên: `nhapMang`, `xuatMang`, `tinhTong`, `demAm`, `timViTri(a, n, x)` (trả về vị trí đầu tiên của `x`, hoặc `-1` nếu không có).

---

### Bài 4 – Struct và hàm
Định nghĩa struct `PhanSo` (tử, mẫu). Viết hàm `nhapPS`, `xuatPS`, `congPS`, `nhanPS` (trả về `PhanSo`) và hàm rút gọn phân số (dùng ước chung lớn nhất).


---

### Bài 5 – Ma trận
Viết hàm nhập, xuất ma trận `m x n` và hàm `tongDuongCheo` cho ma trận vuông. Gọi trong `main`.

---

## Lỗi Thường Gặp

| Lỗi | Nguyên nhân | Cách sửa |
|---|---|---|
| Gọi hàm bị lỗi "implicit declaration" | Gọi trước khi định nghĩa | Thêm nguyên mẫu ở đầu file |
| Hàm không trả về đúng | Thiếu `return` | Thêm `return` đúng kiểu |
| Thay đổi biến nhưng không đổi | Truyền tham trị | Dùng con trỏ (buổi 7) |
| Hàm mảng chạy sai | Quên truyền `n` | Luôn truyền kèm kích thước |
| Lỗi mảng 2 chiều | Thiếu số cột trong tham số | Ghi `a[][COT]` |

---

Buổi tiếp theo: Buổi 6 – Luyện tập buổi 1 → 5
