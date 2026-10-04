# Buổi 4 – Mảng 1 Chiều và Mảng 2 Chiều
## Môn: Kỹ Thuật Lập Trình C

---

## Mục tiêu buổi học

Sau buổi này sinh viên có thể:
- Khai báo và sử dụng mảng 1 chiều, 2 chiều
- Nhập và xuất dữ liệu mảng bằng vòng lặp
- Thực hiện các thao tác cơ bản: tìm max/min, tính tổng, đếm, tìm kiếm

---

## 1. Mảng 1 Chiều

### 1.1 Mảng là gì?

Mảng là tập hợp nhiều biến **cùng kiểu**, được lưu liền nhau trong bộ nhớ, truy cập bằng **chỉ số** (index).

Thay vì khai báo 5 biến riêng lẻ:
```c
int diem1, diem2, diem3, diem4, diem5;
```

Dùng mảng:
```c
int diem[5];   // mảng 5 phần tử kiểu int
```

### 1.2 Khai báo mảng

```c
int    a[10];         // mảng 10 số nguyên
float  b[5];          // mảng 5 số thực
char   ten[50];       // mảng 50 ký tự (chuỗi)

// Khai báo và khởi tạo ngay
int c[5] = {10, 20, 30, 40, 50};
int d[]  = {1, 2, 3};   // tự suy ra kích thước = 3
```

> Quan trọng: chỉ số mảng bắt đầu từ **0**, không phải 1.
> Mảng `a[5]` có các phần tử: `a[0]`, `a[1]`, `a[2]`, `a[3]`, `a[4]`.

<details>
<summary>Hình dung mảng trong bộ nhớ</summary>

```
int a[5] = {10, 20, 30, 40, 50};

Chỉ số:  [0]  [1]  [2]  [3]  [4]
Giá trị:  10   20   30   40   50
```

Truy cập:
```c
printf("%d\n", a[0]);    // 10
printf("%d\n", a[2]);    // 30
a[3] = 99;               // đổi a[3] thành 99
```

Lỗi phổ biến – truy cập ngoài phạm vi:
```c
int a[5];
a[5] = 10;   // LỖI! chỉ số hợp lệ là 0..4, a[5] không tồn tại
```
C không báo lỗi khi biên dịch nhưng chương trình sẽ hoạt động sai hoặc crash.

</details>

### 1.3 Nhập và xuất mảng

```c
#include <stdio.h>

int main() {
    int n;
    printf("Nhap so phan tu: ");
    scanf("%d", &n);

    int a[100];   // khai báo mảng kích thước tối đa

    // Nhập mảng
    for (int i = 0; i < n; i++) {
        printf("a[%d] = ", i);
        scanf("%d", &a[i]);
    }

    // Xuất mảng
    printf("Mang vua nhap: ");
    for (int i = 0; i < n; i++) {
        printf("%d ", a[i]);
    }
    printf("\n");

    return 0;
}
```

Kết quả mẫu:
```
Nhap so phan tu: 5
a[0] = 3
a[1] = 7
a[2] = 1
a[3] = 9
a[4] = 4
Mang vua nhap: 3 7 1 9 4
```

<details>
<summary>Tại sao khai báo int a[100] mà không phải int a[n]?</summary>

Trong C chuẩn (C89/C90), kích thước mảng phải là **hằng số** biết trước lúc biên dịch.
Cách thông dụng nhất là khai báo mảng với kích thước tối đa (ví dụ 100 hay 1000), rồi chỉ dùng `n` phần tử đầu.

```c
int a[100];   // ô nhớ cho 100 phần tử, nhưng chỉ dùng n phần tử
```

C99 trở đi hỗ trợ VLA (Variable Length Array) cho phép `int a[n]`, nhưng không phải compiler nào cũng hỗ trợ tốt và không nên dùng trong thực tế.

</details>

### 1.4 Các thao tác cơ bản

#### Tính tổng và trung bình

```c
int tong = 0;
for (int i = 0; i < n; i++) {
    tong += a[i];
}
float tb = (float)tong / n;
printf("Tong = %d, Trung binh = %.2f\n", tong, tb);
```

#### Tìm max và min

```c
int max = a[0], min = a[0];
for (int i = 1; i < n; i++) {
    if (a[i] > max) max = a[i];
    if (a[i] < min) min = a[i];
}
printf("Max = %d, Min = %d\n", max, min);
```

> Khởi tạo `max = a[0]` (phần tử đầu tiên), không phải `max = 0`.
> Nếu tất cả phần tử đều âm mà khởi tạo `max = 0` thì kết quả sẽ sai.

#### Đếm phần tử thoả điều kiện

```c
// Đếm số phần tử chẵn
int dem = 0;
for (int i = 0; i < n; i++) {
    if (a[i] % 2 == 0) dem++;
}
printf("So phan tu chan: %d\n", dem);
```

#### Tìm kiếm tuyến tính

```c
int x;
printf("Tim gia tri: ");
scanf("%d", &x);

int viTri = -1;   // -1 nghĩa là chưa tìm thấy
for (int i = 0; i < n; i++) {
    if (a[i] == x) {
        viTri = i;
        break;
    }
}

if (viTri != -1)
    printf("Tim thay %d tai vi tri %d\n", x, viTri);
else
    printf("Khong tim thay %d\n", x);
```

<details>
<summary>Ví dụ hoàn chỉnh – tổng hợp các thao tác trên mảng</summary>

```c
#include <stdio.h>

int main() {
    int n;
    printf("Nhap n: ");
    scanf("%d", &n);

    int a[100];
    for (int i = 0; i < n; i++) {
        printf("a[%d] = ", i);
        scanf("%d", &a[i]);
    }

    // Tổng và trung bình
    int tong = 0;
    for (int i = 0; i < n; i++) tong += a[i];
    printf("Tong        : %d\n", tong);
    printf("Trung binh  : %.2f\n", (float)tong / n);

    // Max và min
    int max = a[0], min = a[0];
    for (int i = 1; i < n; i++) {
        if (a[i] > max) max = a[i];
        if (a[i] < min) min = a[i];
    }
    printf("Max         : %d\n", max);
    printf("Min         : %d\n", min);

    // Đếm số chẵn
    int dem = 0;
    for (int i = 0; i < n; i++)
        if (a[i] % 2 == 0) dem++;
    printf("So chan      : %d\n", dem);

    return 0;
}
```

</details>

---

## 2. Chuỗi Ký Tự (String)

Chuỗi trong C là mảng `char`, kết thúc bằng ký tự `'\0'` (null terminator).

```c
char ten[50];

printf("Nhap ten: ");
scanf("%s", ten);           // đọc 1 từ (dừng khi gặp khoảng trắng)

// Hoặc đọc cả câu có khoảng trắng:
scanf(" %[^\n]", ten);

printf("Xin chao, %s!\n", ten);
```

<details>
<summary>Các hàm xử lý chuỗi thông dụng (cần #include string.h)</summary>

```c
#include <string.h>

char s1[50] = "Hello";
char s2[50] = "World";

strlen(s1)          // độ dài chuỗi = 5
strcpy(s1, s2)      // sao chép s2 vào s1
strcat(s1, s2)      // nối s2 vào sau s1
strcmp(s1, s2)      // so sánh: 0 nếu bằng nhau, khác 0 nếu khác
```

Ví dụ:
```c
char ho[50], ten[50], hoTen[100];

printf("Nhap ho: ");  scanf("%s", ho);
printf("Nhap ten: "); scanf("%s", ten);

strcpy(hoTen, ho);
strcat(hoTen, " ");
strcat(hoTen, ten);

printf("Ho ten day du: %s\n", hoTen);
printf("Do dai       : %d ky tu\n", (int)strlen(hoTen));
```

</details>

---

## 3. Mảng 2 Chiều

### 3.1 Khai báo

Mảng 2 chiều như một **bảng** có hàng và cột.

```c
int a[3][4];         // 3 hàng, 4 cột
float b[10][10];

// Khai báo và khởi tạo
int c[2][3] = {
    {1, 2, 3},       // hàng 0
    {4, 5, 6}        // hàng 1
};
```

<details>
<summary>Hình dung mảng 2 chiều</summary>

```
int a[3][4]:

         cột 0  cột 1  cột 2  cột 3
hàng 0:  a[0][0] a[0][1] a[0][2] a[0][3]
hàng 1:  a[1][0] a[1][1] a[1][2] a[1][3]
hàng 2:  a[2][0] a[2][1] a[2][2] a[2][3]
```

Truy cập: `a[hàng][cột]`

</details>

### 3.2 Nhập và xuất mảng 2 chiều

Dùng 2 vòng lặp lồng nhau: vòng ngoài duyệt hàng, vòng trong duyệt cột.

```c
#include <stdio.h>

int main() {
    int m, n;
    printf("Nhap so hang: "); scanf("%d", &m);
    printf("Nhap so cot : "); scanf("%d", &n);

    int a[50][50];

    // Nhập
    printf("Nhap mang:\n");
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            printf("a[%d][%d] = ", i, j);
            scanf("%d", &a[i][j]);
        }
    }

    // Xuất
    printf("Mang vua nhap:\n");
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            printf("%4d", a[i][j]);   // %4d: mỗi số chiếm 4 ký tự, căn phải
        }
        printf("\n");
    }

    return 0;
}
```

Kết quả mẫu với ma trận 2x3:
```
Mang vua nhap:
   1   2   3
   4   5   6
```

### 3.3 Các thao tác trên mảng 2 chiều

#### Tính tổng toàn bộ mảng

```c
int tong = 0;
for (int i = 0; i < m; i++)
    for (int j = 0; j < n; j++)
        tong += a[i][j];
printf("Tong toan bo: %d\n", tong);
```

#### Tính tổng từng hàng

```c
for (int i = 0; i < m; i++) {
    int tongHang = 0;
    for (int j = 0; j < n; j++)
        tongHang += a[i][j];
    printf("Tong hang %d: %d\n", i, tongHang);
}
```

#### Tính tổng từng cột

```c
for (int j = 0; j < n; j++) {
    int tongCot = 0;
    for (int i = 0; i < m; i++)
        tongCot += a[i][j];
    printf("Tong cot %d: %d\n", j, tongCot);
}
```

#### Tính tổng đường chéo chính (ma trận vuông m == n)

```c
int tongCheo = 0;
for (int i = 0; i < m; i++)
    tongCheo += a[i][i];   // đường chéo chính: i == j
printf("Tong duong cheo chinh: %d\n", tongCheo);
```

<details>
<summary>Ví dụ hoàn chỉnh – thao tác ma trận</summary>

```c
#include <stdio.h>

int main() {
    int m, n;
    printf("Nhap so hang va cot: ");
    scanf("%d %d", &m, &n);

    int a[50][50];
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++) {
            printf("a[%d][%d] = ", i, j);
            scanf("%d", &a[i][j]);
        }

    // In mảng
    printf("\nMang:\n");
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++)
            printf("%4d", a[i][j]);
        printf("\n");
    }

    // Tổng toàn bộ
    int tong = 0;
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++)
            tong += a[i][j];
    printf("Tong toan bo: %d\n", tong);

    // Tìm max
    int max = a[0][0];
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++)
            if (a[i][j] > max) max = a[i][j];
    printf("Max: %d\n", max);

    return 0;
}
```

</details>

---

## Bài Tập Buổi 4

### Bài 1 – Mảng 1 chiều cơ bản
Nhập mảng `n` phần tử số nguyên. In ra:
- Các phần tử theo thứ tự ngược lại
- Tổng các phần tử dương
- Số lượng phần tử âm

---

### Bài 2 – Tìm kiếm và đếm
Nhập mảng `n` phần tử. Nhập thêm giá trị `x`.
- Kiểm tra `x` có trong mảng không
- Nếu có, in ra tất cả các vị trí xuất hiện của `x`
- In ra số lần `x` xuất hiện

---

### Bài 3 – Struct kết hợp mảng
Định nghĩa struct `SinhVien` gồm: tên, điểm (float).
Nhập mảng `n` sinh viên. In ra:
- Danh sách tất cả sinh viên và điểm
- Sinh viên có điểm cao nhất
- Số sinh viên đạt (điểm >= 5)

<details>
<summary>Gợi ý khai báo</summary>

```c
typedef struct {
    char ten[50];
    float diem;
} SinhVien;

SinhVien ds[100];   // mảng struct

// Nhập
for (int i = 0; i < n; i++) {
    printf("Ten SV %d: ", i + 1);
    scanf(" %[^\n]", ds[i].ten);
    printf("Diem     : ");
    scanf("%f", &ds[i].diem);
}
```

</details>

---

### Bài 4 – Mảng 2 chiều cơ bản
Nhập ma trận `m x n`. Tính và in ra:
- Tổng mỗi hàng
- Tổng mỗi cột
- Phần tử lớn nhất của toàn bảng (kèm theo vị trí hàng, cột)

---

### Bài 5 – Ma trận vuông
Nhập ma trận vuông `n x n`. Tính:
- Tổng đường chéo chính (các phần tử `a[i][i]`)
- Tổng đường chéo phụ (các phần tử `a[i][n-1-i]`)
- Kiểm tra ma trận có đối xứng không (ma trận đối xứng khi `a[i][j] == a[j][i]`)

<details>
<summary>Gợi ý kiểm tra đối xứng</summary>

```c
int doiXung = 1;   // giả sử đối xứng
for (int i = 0; i < n && doiXung; i++)
    for (int j = 0; j < n && doiXung; j++)
        if (a[i][j] != a[j][i])
            doiXung = 0;

if (doiXung) printf("Ma tran doi xung\n");
else         printf("Ma tran khong doi xung\n");
```

</details>

---

## Lỗi Thường Gặp

| Lỗi | Nguyên nhân | Cách sửa |
|---|---|---|
| Chỉ số vượt quá kích thước | Dùng `i <= n` thay vì `i < n` | Vòng lặp phải là `i < n` |
| Khởi tạo max/min bằng 0 | Mảng toàn số âm thì max sẽ sai | Khởi tạo bằng `a[0]` |
| Nhập mảng 2 chiều sai thứ tự | Nhầm `a[j][i]` và `a[i][j]` | Vòng ngoài là hàng `i`, vòng trong là cột `j` |
| Mảng struct không nhập được tên | Dùng `scanf("%s")` bỏ qua khoảng trắng | Dùng `scanf(" %[^\n]", ...)` |

---

Buổi tiếp theo: Buổi 5 – Hàm
