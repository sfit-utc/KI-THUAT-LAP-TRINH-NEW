# Buổi 2 – Toán Tử và Câu Lệnh Rẽ Nhánh
## Môn: Kỹ Thuật Lập Trình C

---

## Mục tiêu buổi học

Sau buổi này sinh viên có thể:
- Sử dụng thành thạo các toán tử trong C
- Hiểu và vận dụng câu lệnh `if`, `if/else`, `if/else if`, `switch`
- Kết hợp điều kiện để giải các bài toán phân loại thực tế

---

## 1. Toán Tử

### 1.1 Toán tử số học

| Toán tử | Ý nghĩa | Ví dụ | Kết quả |
|---|---|---|---|
| `+` | Cộng | `5 + 3` | `8` |
| `-` | Trừ | `5 - 3` | `2` |
| `*` | Nhân | `5 * 3` | `15` |
| `/` | Chia | `5 / 3` | `1` (chia nguyên!) |
| `%` | Chia lấy dư | `5 % 3` | `2` |

<details>
<summary>Lưu ý quan trọng về phép chia</summary>

Khi cả hai toán hạng đều là `int`, phép `/` sẽ cho kết quả là số nguyên (bỏ phần thập phân).

```c
int a = 5, b = 3;
printf("%d\n", a / b);      // in ra 1, không phải 1.666

// Muốn ra số thực, phải ép kiểu:
printf("%.2f\n", (float)a / b);   // in ra 1.67
```

`(float)a` gọi là **ép kiểu (cast)** – chuyển tạm thời `a` thành float để phép chia ra số thực.

Phép `%` (chia lấy dư) chỉ dùng được với số nguyên:
```c
int a = 10;
printf("%d\n", a % 3);   // 10 = 3*3 + 1, dư 1
printf("%d\n", a % 2);   // 10 = 2*5 + 0, dư 0 -> số chẵn
printf("%d\n", a % 5);   // 10 = 5*2 + 0, dư 0
```

Ứng dụng phổ biến của `%`:
- Kiểm tra số chẵn/lẻ: `n % 2 == 0`
- Kiểm tra chia hết: `n % k == 0`
- Lấy chữ số cuối: `n % 10`

</details>

### 1.2 Toán tử gán

| Toán tử | Ý nghĩa | Tương đương |
|---|---|---|
| `=` | Gán | `x = 5` |
| `+=` | Cộng rồi gán | `x += 3` tức `x = x + 3` |
| `-=` | Trừ rồi gán | `x -= 3` tức `x = x - 3` |
| `*=` | Nhân rồi gán | `x *= 3` tức `x = x * 3` |
| `/=` | Chia rồi gán | `x /= 3` tức `x = x / 3` |
| `%=` | Lấy dư rồi gán | `x %= 3` tức `x = x % 3` |

```c
int x = 10;
x += 5;     // x = 15
x -= 3;     // x = 12
x *= 2;     // x = 24
x /= 4;     // x = 6
```

### 1.3 Toán tử tăng giảm

```c
int x = 5;
x++;    // x = 6  (tăng 1)
x--;    // x = 5  (giảm 1)
```

<details>
<summary>Khác nhau giữa x++ và ++x</summary>

`x++` (hậu tố): dùng giá trị trước, rồi mới tăng.
`++x` (tiền tố): tăng trước, rồi mới dùng giá trị.

```c
int a = 5;
int b = a++;    // b = 5, a = 6  (lấy giá trị 5 xong mới tăng a)
int c = ++a;    // a = 7, c = 7  (tăng a trước rồi gán cho c)
```

Trong hầu hết các trường hợp thông thường (không lồng vào biểu thức), `x++` và `++x` cho kết quả giống nhau.
Chỉ cần cẩn thận khi dùng trong biểu thức phức tạp.

</details>

### 1.4 Toán tử so sánh

Kết quả luôn là `1` (đúng) hoặc `0` (sai).

| Toán tử | Ý nghĩa | Ví dụ | Kết quả |
|---|---|---|---|
| `==` | Bằng | `5 == 5` | `1` (đúng) |
| `!=` | Khác | `5 != 3` | `1` (đúng) |
| `>` | Lớn hơn | `5 > 3` | `1` (đúng) |
| `<` | Nhỏ hơn | `5 < 3` | `0` (sai) |
| `>=` | Lớn hơn hoặc bằng | `5 >= 5` | `1` (đúng) |
| `<=` | Nhỏ hơn hoặc bằng | `5 <= 4` | `0` (sai) |

> Chú ý: `==` là so sánh, `=` là gán. Nhầm lẫn giữa hai cái này là lỗi rất phổ biến.

### 1.5 Toán tử logic

| Toán tử | Ý nghĩa | Mô tả |
|---|---|---|
| `&&` | Và (AND) | Đúng khi cả hai vế đều đúng |
| `\|\|` | Hoặc (OR) | Đúng khi ít nhất một vế đúng |
| `!` | Phủ định (NOT) | Đảo ngược kết quả |

```c
int a = 5, b = 10;

printf("%d\n", a > 0 && b > 0);   // 1 (cả hai đều đúng)
printf("%d\n", a > 0 && b < 0);   // 0 (b < 0 sai)
printf("%d\n", a < 0 || b > 0);   // 1 (b > 0 đúng)
printf("%d\n", !(a > 0));          // 0 (a > 0 đúng, phủ định thành sai)
```

<details>
<summary>Ví dụ thực tế tổng hợp toán tử</summary>

```c
#include <stdio.h>

int main() {
    int a, b;
    printf("Nhap a: "); scanf("%d", &a);
    printf("Nhap b: "); scanf("%d", &b);

    printf("Tong      : %d\n", a + b);
    printf("Hieu      : %d\n", a - b);
    printf("Tich      : %d\n", a * b);
    printf("Thuong    : %.2f\n", (float)a / b);
    printf("Phan du   : %d\n", a % b);
    printf("a chan    : %d\n", a % 2 == 0);   // 1 nếu chẵn, 0 nếu lẻ
    printf("a > b     : %d\n", a > b);
    printf("a >= 0 va b >= 0: %d\n", a >= 0 && b >= 0);

    return 0;
}
```

</details>

---

## 2. Câu Lệnh Rẽ Nhánh

### 2.1 if đơn giản

Dùng khi chỉ cần làm gì đó khi điều kiện đúng, không có trường hợp sai.

```c
if (dieu_kien) {
    // code chạy khi điều kiện đúng
}
```

Ví dụ:
```c
int diem = 8;
if (diem >= 5) {
    printf("Qua mon!\n");
}
```

<details>
<summary>Khi nào có thể bỏ dấu ngoặc nhọn { }</summary>

Nếu chỉ có 1 câu lệnh, có thể bỏ `{ }`:
```c
if (diem >= 5)
    printf("Qua mon!\n");
```

Tuy nhiên, nên luôn giữ `{ }` để tránh nhầm lẫn khi thêm code sau này:
```c
// Dễ nhầm: tưởng cả 2 dòng đều trong if, nhưng không phải
if (diem >= 5)
    printf("Qua mon!\n");
    printf("Chuc mung!\n");   // dòng này luôn chạy, không phụ thuộc if
```

</details>

### 2.2 if / else

Dùng khi có 2 trường hợp: đúng hoặc sai.

```c
if (dieu_kien) {
    // chạy khi đúng
} else {
    // chạy khi sai
}
```

Ví dụ:
```c
int diem = 4;
if (diem >= 5) {
    printf("Qua mon!\n");
} else {
    printf("Hoc lai!\n");
}
```

### 2.3 if / else if / else

Dùng khi có nhiều trường hợp.

```c
if (dieu_kien_1) {
    // trường hợp 1
} else if (dieu_kien_2) {
    // trường hợp 2
} else if (dieu_kien_3) {
    // trường hợp 3
} else {
    // không thoả mãn trường hợp nào
}
```

Ví dụ – xếp loại học sinh:
```c
#include <stdio.h>

int main() {
    float diem;
    printf("Nhap diem: ");
    scanf("%f", &diem);

    if (diem >= 9.0f) {
        printf("Xuat sac\n");
    } else if (diem >= 8.0f) {
        printf("Gioi\n");
    } else if (diem >= 6.5f) {
        printf("Kha\n");
    } else if (diem >= 5.0f) {
        printf("Trung binh\n");
    } else {
        printf("Yeu\n");
    }

    return 0;
}
```

<details>
<summary>Tại sao thứ tự điều kiện quan trọng?</summary>

C kiểm tra điều kiện từ trên xuống, gặp cái đúng đầu tiên thì chạy và thoát luôn, không kiểm tra tiếp.

Ví dụ sai – thứ tự ngược:
```c
if (diem >= 5.0f) {
    printf("Trung binh\n");   // diem = 9 vẫn vào đây! vì 9 >= 5
} else if (diem >= 6.5f) {
    printf("Kha\n");          // không bao giờ đến đây
} else if (diem >= 8.0f) {
    printf("Gioi\n");         // không bao giờ đến đây
}
```

Quy tắc: với điều kiện so sánh `>=`, luôn đặt điều kiện **lớn hơn trước**.

</details>

### 2.4 Toán tử ba ngôi (ternary)

Cách viết ngắn gọn cho if/else đơn giản.

```c
// Cú pháp:
ket_qua = (dieu_kien) ? gia_tri_neu_dung : gia_tri_neu_sai;
```

Ví dụ:
```c
int a = 10, b = 20;
int max = (a > b) ? a : b;
printf("So lon hon: %d\n", max);   // 20

// Tương đương với:
int max2;
if (a > b) max2 = a;
else max2 = b;
```

### 2.5 switch / case

Dùng khi so sánh một biến với nhiều giá trị cụ thể. Thay thế chuỗi `if/else if` khi so sánh bằng `==`.

```c
switch (bien) {
    case gia_tri_1:
        // code
        break;
    case gia_tri_2:
        // code
        break;
    default:
        // không khớp case nào
}
```

Ví dụ – in tên ngày trong tuần:
```c
#include <stdio.h>

int main() {
    int ngay;
    printf("Nhap so ngay (1-7): ");
    scanf("%d", &ngay);

    switch (ngay) {
        case 1: printf("Chu nhat\n");  break;
        case 2: printf("Thu hai\n");   break;
        case 3: printf("Thu ba\n");    break;
        case 4: printf("Thu tu\n");    break;
        case 5: printf("Thu nam\n");   break;
        case 6: printf("Thu sau\n");   break;
        case 7: printf("Thu bay\n");   break;
        default: printf("Sai! Chi nhap 1 den 7\n");
    }

    return 0;
}
```

<details>
<summary>Tại sao phải có break trong switch?</summary>

Nếu không có `break`, chương trình sẽ **tiếp tục chạy xuống case tiếp theo** (gọi là fall-through), cho đến khi gặp `break` hoặc hết switch.

```c
int x = 2;
switch (x) {
    case 1: printf("Mot\n");
    case 2: printf("Hai\n");   // chạy vào đây
    case 3: printf("Ba\n");    // fall-through! cũng chạy luôn
    case 4: printf("Bon\n");   // fall-through! cũng chạy luôn
    default: printf("Khac\n"); // fall-through! cũng chạy luôn
}
// Kết quả: Hai, Ba, Bon, Khac - in ra cả 4 dòng!
```

Thêm `break` để dừng:
```c
switch (x) {
    case 2:
        printf("Hai\n");
        break;   // thoát switch ngay
    case 3:
        printf("Ba\n");
        break;
}
// Kết quả: chỉ in "Hai"
```

Đôi khi người ta dùng fall-through có chủ đích:
```c
switch (thang) {
    case 1: case 3: case 5: case 7:
    case 8: case 10: case 12:
        printf("31 ngay\n");
        break;
    case 4: case 6: case 9: case 11:
        printf("30 ngay\n");
        break;
    case 2:
        printf("28 hoac 29 ngay\n");
        break;
}
```

</details>

<details>
<summary>Khi nào dùng switch, khi nào dùng if/else?</summary>

Dùng `switch` khi:
- So sánh một biến với nhiều giá trị cụ thể (số nguyên hoặc char)
- Code gọn và dễ đọc hơn

Dùng `if/else` khi:
- So sánh phạm vi (>, <, >=, <=)
- Điều kiện phức tạp (dùng &&, ||)
- So sánh số thực (float, double)

```c
// Dùng switch được:
switch (diem_chu) {   // 'A', 'B', 'C', 'D'
    case 'A': ...
}

// Phải dùng if/else:
if (diem >= 8.0f) {   // so sánh phạm vi với số thực
    ...
}
```

</details>

---

## 3. Ví Dụ Tổng Hợp

### Bài toán: Giải phương trình bậc 1 – ax + b = 0

```c
#include <stdio.h>

int main() {
    float a, b;

    printf("Nhap a: "); scanf("%f", &a);
    printf("Nhap b: "); scanf("%f", &b);

    if (a == 0) {
        if (b == 0) {
            printf("Phuong trinh vo so nghiem\n");
        } else {
            printf("Phuong trinh vo nghiem\n");
        }
    } else {
        float x = -b / a;
        printf("Nghiem: x = %.2f\n", x);
    }

    return 0;
}
```

<details>
<summary>Giải thích logic bài toán</summary>

Phương trình ax + b = 0:
- Nếu a = 0 và b = 0 → 0 = 0 → đúng với mọi x → vô số nghiệm
- Nếu a = 0 và b != 0 → b = 0 → vô lý → vô nghiệm
- Nếu a != 0 → x = -b/a → một nghiệm duy nhất

Lưu ý: so sánh số thực với 0 bằng `==` có thể không chính xác do sai số floating-point. Trong bài học này tạm chấp nhận.

</details>

---

## Bài Tập Buổi 2

### Bài 1 – Dễ
Nhập vào số nguyên `n`. Kiểm tra và in ra:
- `n` là số chẵn hay lẻ
- `n` là số dương, âm hay bằng 0

---

### Bài 2 – Trung bình
Nhập vào 3 số nguyên `a`, `b`, `c`. In ra số lớn nhất trong 3 số.

<details>
<summary>Gợi ý</summary>

Cách 1: dùng if/else lồng nhau
```c
if (a >= b && a >= c)
    printf("Max: %d\n", a);
else if (b >= a && b >= c)
    printf("Max: %d\n", b);
else
    printf("Max: %d\n", c);
```

Cách 2: dùng biến tạm
```c
int max = a;
if (b > max) max = b;
if (c > max) max = c;
printf("Max: %d\n", max);
```

</details>

---

### Bài 3 – Trung bình
Viết chương trình nhập điểm (thang 10). In ra xếp loại:
- 9.0 – 10: Xuất sắc
- 8.0 – 8.9: Giỏi
- 6.5 – 7.9: Khá
- 5.0 – 6.4: Trung bình
- Dưới 5.0: Yếu

Kiểm tra thêm: nếu điểm < 0 hoặc > 10 thì in `"Diem khong hop le"`.

---

### Bài 4 – Khá
Viết chương trình máy tính đơn giản:
- Nhập 2 số thực `a`, `b` và một ký tự toán tử `op` (`+`, `-`, `*`, `/`)
- Tính và in ra kết quả
- Nếu `op` là `/` và `b = 0` thì in `"Khong chia duoc cho 0"`
- Nếu `op` không hợp lệ thì in `"Toan tu khong hop le"`

Gợi ý: dùng `switch` để xử lý toán tử.

<details>
<summary>Gợi ý cách nhập ký tự toán tử</summary>

```c
char op;
printf("Nhap toan tu (+, -, *, /): ");
scanf(" %c", &op);   // chú ý dấu cách trước %c để bỏ qua ký tự trắng thừa
```

</details>

---

### Bài 5 – Khá (nâng cao)
Nhập vào năm. Kiểm tra năm đó có phải năm nhuận không.

Quy tắc năm nhuận:
- Chia hết cho 400, HOẶC
- Chia hết cho 4 nhưng KHÔNG chia hết cho 100

<details>
<summary>Gợi ý</summary>

```c
if (nam % 400 == 0 || (nam % 4 == 0 && nam % 100 != 0))
    printf("Nam nhuan\n");
else
    printf("Khong phai nam nhuan\n");
```

</details>

---

### Bài 6 – Struct kết hợp rẽ nhánh
Định nghĩa struct `SanPham` gồm: tên sản phẩm (`ten`), giá gốc (`gia`, kiểu float), số lượng tồn kho (`soLuong`, kiểu int).

Nhập thông tin 1 sản phẩm. Sau đó:
- Nếu `soLuong == 0`: in `"Hang het"`
- Nếu `soLuong < 10`: in `"Sắp hết hàng"` và in giá gốc
- Nếu `soLuong >= 10`: tính giá sau khi giảm 15% và in ra

<details>
<summary>Gợi ý</summary>

```c
#include <stdio.h>

typedef struct {
    char ten[100];
    float gia;
    int soLuong;
} SanPham;

int main() {
    SanPham sp;

    printf("Nhap ten san pham: ");
    scanf("%s", sp.ten);

    printf("Nhap gia goc: ");
    scanf("%f", &sp.gia);

    printf("Nhap so luong ton kho: ");
    scanf("%d", &sp.soLuong);

    printf("\n--- Ket qua ---\n");
    printf("San pham : %s\n", sp.ten);

    if (sp.soLuong == 0) {
        printf("Trang thai: Hang het\n");
    } else if (sp.soLuong < 10) {
        printf("Trang thai: Sap het hang\n");
        printf("Gia       : %.2f\n", sp.gia);
    } else {
        float giaGiam = sp.gia * 0.85f;   // giảm 15%
        printf("Trang thai: Con hang\n");
        printf("Gia goc   : %.2f\n", sp.gia);
        printf("Gia sau giam 15%%: %.2f\n", giaGiam);
    }

    return 0;
}
```

Lưu ý: trong `printf`, muốn in dấu `%` thì phải viết `%%`.

</details>

---

## Lỗi Thường Gặp

| Lỗi | Nguyên nhân | Cách sửa |
|---|---|---|
| Dùng `=` thay vì `==` trong điều kiện | Nhầm gán với so sánh | Kiểm tra kỹ `if (x == 5)` |
| Thiếu `break` trong switch | Chương trình chạy qua các case tiếp theo | Thêm `break` sau mỗi case |
| Thứ tự điều kiện sai trong if/else if | Case lớn đặt sau case nhỏ | Đặt điều kiện lớn hơn trước |
| Phép chia nguyên ra kết quả sai | Quên ép kiểu float | Dùng `(float)a / b` |
| So sánh nhầm chuỗi bằng `==` | `==` không so sánh được chuỗi | Dùng `strcmp()` (học sau) |

---

Buổi tiếp theo: Buổi 3 – Vòng lặp
