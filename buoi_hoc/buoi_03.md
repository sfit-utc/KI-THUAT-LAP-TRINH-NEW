# Buổi 3 – Vòng Lặp
## Môn: Kỹ Thuật Lập Trình C

---

## Mục tiêu buổi học

Sau buổi này sinh viên có thể:
- Hiểu khái niệm vòng lặp và khi nào cần dùng
- Sử dụng được `for`, `while`, `do-while`
- Biết dùng `break` và `continue` đúng chỗ
- Viết được vòng lặp lồng nhau đơn giản

---

## 1. Tại sao cần vòng lặp?

Giả sử in ra "Xin chao" 1000 lần – không thể viết 1000 dòng `printf`.
Vòng lặp cho phép **lặp lại một đoạn code nhiều lần** mà không cần viết lại.

```c
// Không có vòng lặp – phải viết 5 lần
printf("Xin chao!\n");
printf("Xin chao!\n");
printf("Xin chao!\n");
printf("Xin chao!\n");
printf("Xin chao!\n");

// Có vòng lặp – gọn hơn rất nhiều
for (int i = 1; i <= 5; i++) {
    printf("Xin chao!\n");
}
```

---

## 2. Vòng lặp for

Dùng khi **biết trước số lần lặp**.

### Cú pháp

```c
for (khoi_tao; dieu_kien; buoc_nhay) {
    // code lặp lại
}
```

- `khoi_tao`: chạy một lần duy nhất lúc bắt đầu
- `dieu_kien`: kiểm tra trước mỗi lần lặp, sai thì dừng
- `buoc_nhay`: chạy sau mỗi lần lặp xong

### Ví dụ cơ bản

```c
for (int i = 1; i <= 5; i++) {
    printf("Lan lap thu %d\n", i);
}
```

Kết quả:
```
Lan lap thu 1
Lan lap thu 2
Lan lap thu 3
Lan lap thu 4
Lan lap thu 5
```

<details>
<summary>Tracing – theo dõi từng bước vòng lặp</summary>

```
i = 1  -> 1 <= 5 đúng -> in "Lan lap thu 1" -> i++ -> i = 2
i = 2  -> 2 <= 5 đúng -> in "Lan lap thu 2" -> i++ -> i = 3
i = 3  -> 3 <= 5 đúng -> in "Lan lap thu 3" -> i++ -> i = 4
i = 4  -> 4 <= 5 đúng -> in "Lan lap thu 4" -> i++ -> i = 5
i = 5  -> 5 <= 5 đúng -> in "Lan lap thu 5" -> i++ -> i = 6
i = 6  -> 6 <= 5 sai  -> DỪNG
```

Kỹ năng tracing như trên rất quan trọng để tự kiểm tra code đúng hay sai.

</details>

### Ví dụ tính tổng

```c
#include <stdio.h>

int main() {
    int n;
    printf("Nhap n: ");
    scanf("%d", &n);

    int tong = 0;
    for (int i = 1; i <= n; i++) {
        tong += i;       // tong = tong + i
    }

    printf("Tong 1 + 2 + ... + %d = %d\n", n, tong);
    return 0;
}
```

Kết quả với n = 5:
```
Tong 1 + 2 + ... + 5 = 15
```

<details>
<summary>Các biến thể phổ biến của vòng for</summary>

Đếm ngược:
```c
for (int i = 10; i >= 1; i--) {
    printf("%d ", i);
}
// Kết quả: 10 9 8 7 6 5 4 3 2 1
```

Bước nhảy khác 1:
```c
for (int i = 0; i <= 20; i += 5) {
    printf("%d ", i);
}
// Kết quả: 0 5 10 15 20
```

Chỉ in số chẵn:
```c
for (int i = 2; i <= 10; i += 2) {
    printf("%d ", i);
}
// Kết quả: 2 4 6 8 10
```

</details>

---

## 3. Vòng lặp while

Dùng khi **chưa biết trước số lần lặp**, lặp chừng nào điều kiện còn đúng.

### Cú pháp

```c
while (dieu_kien) {
    // code lặp lại
}
```

Kiểm tra điều kiện **trước** khi vào vòng lặp. Nếu điều kiện sai ngay từ đầu thì không chạy lần nào.

### Ví dụ cơ bản

```c
#include <stdio.h>

int main() {
    int i = 1;
    while (i <= 5) {
        printf("Lan lap thu %d\n", i);
        i++;             // nhớ tăng i, không thì vòng lặp vô tận!
    }
    return 0;
}
```

### Ví dụ thực tế – nhập đến khi hợp lệ

```c
#include <stdio.h>

int main() {
    int diem;

    printf("Nhap diem (0 - 10): ");
    scanf("%d", &diem);

    while (diem < 0 || diem > 10) {
        printf("Diem khong hop le! Nhap lai: ");
        scanf("%d", &diem);
    }

    printf("Diem hop le: %d\n", diem);
    return 0;
}
```

<details>
<summary>So sánh for và while</summary>

Về mặt kỹ thuật, `for` và `while` đều có thể thay thế cho nhau. Thông thường:

Dùng `for` khi biết rõ số lần lặp:
```c
for (int i = 1; i <= 10; i++) { ... }
```

Dùng `while` khi số lần lặp phụ thuộc vào dữ liệu hoặc điều kiện phức tạp:
```c
while (chua_tim_thay) {
    tim_kiem();
}
```

Chuyển đổi tương đương:
```c
// for
for (int i = 0; i < n; i++) { code; }

// while tương đương
int i = 0;
while (i < n) { code; i++; }
```

</details>

---

## 4. Vòng lặp do-while

Giống `while` nhưng kiểm tra điều kiện **sau** khi chạy. Đảm bảo vòng lặp chạy **ít nhất một lần**.

### Cú pháp

```c
do {
    // code lặp lại
} while (dieu_kien);
```

### Ví dụ – menu lặp lại

```c
#include <stdio.h>

int main() {
    int chon;

    do {
        printf("\n=== MENU ===\n");
        printf("1. Chuc nang A\n");
        printf("2. Chuc nang B\n");
        printf("0. Thoat\n");
        printf("Chon: ");
        scanf("%d", &chon);

        switch (chon) {
            case 1: printf("Ban chon chuc nang A\n"); break;
            case 2: printf("Ban chon chuc nang B\n"); break;
            case 0: printf("Tam biet!\n"); break;
            default: printf("Lua chon khong hop le\n");
        }

    } while (chon != 0);

    return 0;
}
```

<details>
<summary>Khi nào dùng do-while?</summary>

Dùng `do-while` khi bắt buộc phải chạy ít nhất một lần trước khi kiểm tra.

Ví dụ điển hình: hiển thị menu, sau đó hỏi người dùng có tiếp tục không.
Không thể dùng `while` thông thường vì chưa biết người dùng chọn gì trước khi hiển thị menu.

```c
// while thông thường – phải đặt điều kiện ban đầu giả tạo
int chon = -1;
while (chon != 0) { ... }

// do-while – tự nhiên hơn
do { ... } while (chon != 0);
```

</details>

---

## 5. break và continue

### break – thoát vòng lặp ngay lập tức

```c
for (int i = 1; i <= 10; i++) {
    if (i == 5) {
        break;           // dừng vòng lặp khi i = 5
    }
    printf("%d ", i);
}
// Kết quả: 1 2 3 4
```

### continue – bỏ qua lần lặp hiện tại, chuyển sang lần tiếp theo

```c
for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
        continue;        // bỏ qua khi i chẵn
    }
    printf("%d ", i);
}
// Kết quả: 1 3 5 7 9
```

<details>
<summary>Ví dụ kết hợp break – tìm số đầu tiên chia hết cho 7 trong khoảng 1-100</summary>

```c
#include <stdio.h>

int main() {
    for (int i = 1; i <= 100; i++) {
        if (i % 7 == 0) {
            printf("So dau tien chia het cho 7: %d\n", i);
            break;
        }
    }
    return 0;
}
// Kết quả: So dau tien chia het cho 7: 7
```

</details>

---

## 6. Vòng lặp lồng nhau

Vòng lặp bên trong chạy **hết một chu kỳ** cho mỗi lần lặp của vòng ngoài.

```c
for (int i = 1; i <= 3; i++) {
    for (int j = 1; j <= 3; j++) {
        printf("(%d,%d) ", i, j);
    }
    printf("\n");
}
```

Kết quả:
```
(1,1) (1,2) (1,3)
(2,1) (2,2) (2,3)
(3,1) (3,2) (3,3)
```

### Ví dụ – in bảng cửu chương

```c
#include <stdio.h>

int main() {
    int n;
    printf("In bang cuu chuong so: ");
    scanf("%d", &n);

    for (int i = 1; i <= 10; i++) {
        printf("%d x %2d = %2d\n", n, i, n * i);
    }

    return 0;
}
```

Kết quả với n = 3:
```
3 x  1 =  3
3 x  2 =  6
3 x  3 =  9
...
3 x 10 = 30
```

<details>
<summary>Vẽ hình tam giác bằng vòng lặp lồng nhau</summary>

```c
#include <stdio.h>

int main() {
    int n = 5;

    // Tam giác sao
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= i; j++) {
            printf("* ");
        }
        printf("\n");
    }

    return 0;
}
```

Kết quả:
```
*
* *
* * *
* * * *
* * * * *
```

Tracing: khi i = 3, vòng j chạy từ 1 đến 3 → in 3 dấu `*`.

</details>

---

## Bài Tập Buổi 3

### Bài 1 – Dễ
Nhập `n`. In ra tất cả các số từ 1 đến n.
Sau đó in ra tổng của chúng.

---

### Bài 2 – Dễ
Nhập `n`. In ra bảng cửu chương của `n` (từ 1 đến 10).

---

### Bài 3 – Trung bình
Nhập `n`. Tính và in ra:
- Tổng các số chẵn từ 1 đến n
- Tổng các số lẻ từ 1 đến n

<details>
<summary>Gợi ý</summary>

```c
int tongChan = 0, tongLe = 0;
for (int i = 1; i <= n; i++) {
    if (i % 2 == 0) tongChan += i;
    else            tongLe   += i;
}
```

</details>

---

### Bài 4 – Trung bình
Nhập `n` số nguyên từ bàn phím (nhập từng số một). Tìm và in ra số lớn nhất, số nhỏ nhất, và trung bình cộng.

<details>
<summary>Gợi ý</summary>

```c
int x, max, min;
float tong = 0;

printf("Nhap so thu 1: ");
scanf("%d", &x);
max = min = x;
tong = x;

for (int i = 2; i <= n; i++) {
    printf("Nhap so thu %d: ", i);
    scanf("%d", &x);
    tong += x;
    if (x > max) max = x;
    if (x < min) min = x;
}
```

Nhập số đầu tiên ra ngoài vòng lặp để khởi tạo `max` và `min` với giá trị thực tế.

</details>

---

### Bài 5 – Trung bình
Nhập `n`. Kiểm tra `n` có phải số nguyên tố không.

Số nguyên tố là số lớn hơn 1, chỉ chia hết cho 1 và chính nó.

<details>
<summary>Gợi ý</summary>

```c
int laNguyenTo = 1;   // giả sử là số nguyên tố

if (n <= 1) {
    laNguyenTo = 0;
} else {
    for (int i = 2; i * i <= n; i++) {    // chỉ cần kiểm tra đến căn(n)
        if (n % i == 0) {
            laNguyenTo = 0;
            break;
        }
    }
}

if (laNguyenTo) printf("%d la so nguyen to\n", n);
else            printf("%d khong phai so nguyen to\n", n);
```

Tại sao chỉ kiểm tra đến `căn(n)`? Nếu `n` chia hết cho `i > căn(n)` thì `n/i < căn(n)` đã được kiểm tra trước đó rồi.

</details>

---

### Bài 6 – Khá
Nhập `n`. In ra tất cả số nguyên tố từ 2 đến n.

Gợi ý: đặt vòng lặp ngoài duyệt từ 2 đến n, vòng lặp trong kiểm tra từng số có phải nguyên tố không.

---

### Bài 7 – Khá
In ra tam giác sao có `n` hàng như sau (ví dụ n = 5):

```
* * * * *
* * * *
* * *
* *
*
```

---

### Bài 8 – Khó (nâng cao)
Nhập `n`. Tính số Fibonacci thứ `n`.

Dãy Fibonacci: 1, 1, 2, 3, 5, 8, 13, 21, ...
Quy tắc: `F(n) = F(n-1) + F(n-2)`, với `F(1) = F(2) = 1`.

<details>
<summary>Gợi ý</summary>

```c
int a = 1, b = 1, c;

if (n == 1 || n == 2) {
    printf("F(%d) = 1\n", n);
} else {
    for (int i = 3; i <= n; i++) {
        c = a + b;
        a = b;
        b = c;
    }
    printf("F(%d) = %d\n", n, b);
}
```

</details>

---

## Lỗi Thường Gặp

| Lỗi | Nguyên nhân | Cách sửa |
|---|---|---|
| Vòng lặp chạy mãi không dừng | Quên tăng biến đếm trong `while` | Thêm `i++` hoặc `i--` trong vòng lặp |
| Lặp nhiều hơn hoặc ít hơn 1 lần | Dùng `<` thay vì `<=` hoặc ngược lại | Trace thủ công với n nhỏ để kiểm tra |
| Biến `i` dùng ngoài vòng `for` | `int i` khai báo trong `for` chỉ dùng trong phạm vi đó | Khai báo `i` trước vòng `for` |
| Vòng lặp lồng break nhầm | `break` chỉ thoát vòng lặp trong cùng | Dùng cờ (flag) để thoát vòng ngoài |

---

Buổi tiếp theo: Buổi 4 – Mảng 1 chiều và Mảng 2 chiều
