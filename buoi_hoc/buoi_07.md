# Buổi 7 – Con Trỏ và Nhập Xuất Tệp
## Môn: Kỹ Thuật Lập Trình C

---

## Mục tiêu buổi học

Sau buổi này sinh viên có thể:
- Hiểu địa chỉ bộ nhớ và con trỏ là gì
- Khai báo, gán, giải tham chiếu con trỏ
- Dùng con trỏ làm tham số hàm (hàm đổi được biến bên ngoài)
- Mối quan hệ giữa con trỏ và mảng
- Đọc/ghi tệp văn bản với `fopen`, `fprintf`, `fscanf`, `fclose`

---

## 1. Con Trỏ

### 1.1 Địa chỉ và con trỏ

Mỗi biến nằm ở một **địa chỉ** trong bộ nhớ. Toán tử `&` lấy địa chỉ của biến (bạn đã dùng trong `scanf`).

**Con trỏ** là biến dùng để lưu **địa chỉ** của biến khác.

```c
int a = 10;
int *p = &a;     // p lưu địa chỉ của a

printf("a  = %d\n", a);
printf("&a = %p\n", (void*)&a);
printf("p  = %p\n", (void*)p);     // giống &a
printf("*p = %d\n", *p);           // 10 – giá trị tại địa chỉ p trỏ tới
```

| Ký hiệu | Ý nghĩa |
|---|---|
| `int *p` | Khai báo p là con trỏ tới `int` |
| `&a` | Địa chỉ của `a` |
| `*p` | Giá trị tại ô nhớ mà `p` trỏ tới (**giải tham chiếu**) |

<details>
<summary>Giải thích kỹ: dấu <code>*</code> có hai nghĩa</summary>

Dấu `*` dễ gây nhầm vì xuất hiện ở hai ngữ cảnh:

| Vị trí | Ý nghĩa | Ví dụ |
|---|---|---|
| Trong **khai báo** | "biến này là con trỏ" | `int *p;` |
| Trong **câu lệnh** | "lấy giá trị tại địa chỉ" | `*p = 20;` / `printf("%d", *p);` |

Và `&` ngược lại: lấy **địa chỉ** của một biến.

Con trỏ cũng có kiểu (`int *`, `float *`, `char *`) để máy biết khi đọc ô nhớ đó phải đọc bao nhiêu byte và hiểu như loại dữ liệu nào. Con trỏ `int *` không được trỏ vào `float`.

Con trỏ luôn chiếm kích thước cố định (8 byte trên máy 64-bit) dù trỏ tới kiểu gì.

</details>

<details>
<summary>Hình dung bộ nhớ</summary>

```
 Biến     Địa chỉ     Giá trị
  a       0x1000        10
  p       0x1004      0x1000   ──► trỏ tới a
```

Ghi `*p = 20;` tức là ghi 20 vào ô nhớ `0x1000` → `a` cũng đổi thành 20.

</details>

### 1.2 Khởi tạo con trỏ

```c
int *p = NULL;   // chưa trỏ đi đâu – nên gán NULL
```

> Không bao giờ dùng `*p` khi `p` chưa trỏ vào vùng nhớ hợp lệ hoặc là `NULL` → chương trình crash.

### 1.3 Con trỏ làm tham số hàm

Trả lời câu hỏi buổi 5: muốn hàm sửa biến bên ngoài, truyền **địa chỉ**.

```c
void hoanDoi(int *x, int *y) {
    int tam = *x;
    *x = *y;
    *y = tam;
}

int main() {
    int a = 3, b = 7;
    hoanDoi(&a, &b);
    printf("a = %d, b = %d\n", a, b);   // a = 7, b = 3
    return 0;
}
```

<details>
<summary>Giải thích kỹ: theo dõi <code>hoanDoi(&a, &b)</code></summary>

Ban đầu `a = 3` (ở địa chỉ 0x100), `b = 7` (ở địa chỉ 0x104).

1. Gọi `hoanDoi(&a, &b)`: truyền **địa chỉ** → trong hàm `x = 0x100`, `y = 0x104`.
2. `int tam = *x;` → `tam = 3` (giá trị tại địa chỉ 0x100, chính là `a`).
3. `*x = *y;` → ghi 7 vào địa chỉ 0x100 → `a = 7`.
4. `*y = tam;` → ghi 3 vào địa chỉ 0x104 → `b = 3`.

Vì hàm có địa chỉ thật của `a`, `b` nên sửa được **biến gốc**. Nếu hàm chỉ nhận `int x, int y` (truyền giá trị) thì chỉ đổi chỗ hai bản sao rồi mất, `a`, `b` giữ nguyên.

Quy tắc: *muốn hàm thay đổi biến nào thì truyền địa chỉ (`&`) của biến đó.*

</details>

Hàm cũng có thể "trả về" nhiều kết quả qua tham số con trỏ:

```c
void minMax(int a[], int n, int *min, int *max) {
    *min = *max = a[0];
    for (int i = 1; i < n; i++) {
        if (a[i] < *min) *min = a[i];
        if (a[i] > *max) *max = a[i];
    }
}
// gọi: minMax(a, n, &mn, &mx);
```

### 1.4 Con trỏ và mảng

Tên mảng là địa chỉ phần tử đầu: `a` tương đương `&a[0]`.

```c
int a[5] = {10, 20, 30, 40, 50};
int *p = a;

printf("%d\n", *p);         // 10
printf("%d\n", *(p + 2));   // 30  (giống a[2])
p++;                        // p trỏ sang phần tử kế tiếp
printf("%d\n", *p);         // 20
```

| Cách viết | Tương đương |
|---|---|
| `a[i]` | `*(a + i)` |
| `&a[i]` | `a + i` |

`p + 1` không cộng 1 byte mà cộng `sizeof(int)` byte – con trỏ tự bước đúng kích thước kiểu.

Duyệt mảng bằng con trỏ:

```c
for (int *q = a; q < a + 5; q++)
    printf("%d ", *q);
```

<details>
<summary>Giải thích kỹ: phép tính trên con trỏ</summary>

Giả sử `a` ở địa chỉ 1000, mỗi `int` 4 byte:

```
địa chỉ: 1000 1004 1008 1012 1016
giá trị :  10    20   30   40   50
          a[0]  a[1] a[2] a[3] a[4]
```

- `p = a` → `p = 1000`; `p + 2` = `1000 + 2*4 = 1008` → trỏ `a[2]`; `*(p + 2) = 30`.
- `p++` → `p = 1004` (nhảy 4 byte, đúng 1 phần tử).
- Hiệu hai con trỏ `q2 - q1` cho **số phần tử** giữa chúng, không phải số byte.
- Điều kiện `q < a + 5` dừng khi `q` vượt phần tử cuối (`a + 5` là địa chỉ ngay sau mảng, chỉ để so sánh, không được đọc).

Vì `a[i]` được trình biên dịch dịch thành `*(a + i)` nên bạn dùng cách nào cũng được; viết `a[i]` dễ đọc hơn, viết con trỏ thì linh hoạt hơn.

</details>

### 1.5 Con trỏ tới struct

```c
SinhVien sv = {"An", 8.5f};
SinhVien *p = &sv;

printf("%s\n", (*p).ten);
printf("%s\n", p->ten);      // cách viết gọn, dùng nhiều hơn
p->diem = 9.0f;
```

---

## 2. Nhập Xuất Tệp

Tệp giúp lưu dữ liệu **lâu dài** (sau khi tắt chương trình vẫn còn).

### 2.1 Các bước

```
1. Mở tệp      FILE *f = fopen("ten.txt", "chế_độ");
2. Kiểm tra    if (f == NULL) { ... }
3. Đọc / ghi   fscanf / fprintf
4. Đóng tệp    fclose(f);
```

| Chế độ | Ý nghĩa |
|---|---|
| `"r"` | Đọc (tệp phải tồn tại) |
| `"w"` | Ghi (tạo mới, **xoá nội dung cũ**) |
| `"a"` | Ghi nối vào cuối tệp |

### 2.2 Ghi tệp

```c
#include <stdio.h>

int main() {
    FILE *f = fopen("data.txt", "w");
    if (f == NULL) {
        printf("Khong mo duoc tep!\n");
        return 1;
    }

    int n = 3;
    fprintf(f, "%d\n", n);
    for (int i = 1; i <= n; i++)
        fprintf(f, "%d ", i * 10);

    fclose(f);
    return 0;
}
```

Nội dung `data.txt`:
```
3
10 20 30
```

<details>
<summary>Giải thích kỹ: <code>FILE *</code>, <code>fopen</code>, <code>fprintf</code></summary>

- `FILE` là kiểu dữ liệu có sẵn (trong `stdio.h`) lưu thông tin về một tệp đang mở. `FILE *f` là con trỏ tới nó – coi như "tay cầm" để thao tác với tệp.
- `fopen(tên, chế_độ)` nhờ hệ điều hành mở tệp. Thất bại (không có tệp, không có quyền, sai đường dẫn) thì trả về `NULL` → **luôn kiểm tra**, nếu không `fprintf` vào `NULL` sẽ crash.
- `fprintf(f, ...)` giống hệt `printf` nhưng ghi vào tệp `f` thay vì màn hình. Mọi format `%d`, `%f`, `\n` dùng y như cũ.
- `fclose(f)` đảm bảo dữ liệu được ghi hẳn xuống đĩa và giải phóng tay cầm. Quên đóng có thể làm mất dữ liệu cuối.
- Tệp được tạo trong **thư mục làm việc hiện tại** của chương trình (thường là thư mục chứa file `.exe`/nơi chạy lệnh), muốn chỗ khác thì ghi đường dẫn đầy đủ: `"C:/data/data.txt"`.

</details>

### 2.3 Đọc tệp

```c
#include <stdio.h>

int main() {
    FILE *f = fopen("data.txt", "r");
    if (f == NULL) {
        printf("Khong tim thay tep!\n");
        return 1;
    }

    int n, a[100];
    fscanf(f, "%d", &n);
    for (int i = 0; i < n; i++)
        fscanf(f, "%d", &a[i]);
    fclose(f);

    for (int i = 0; i < n; i++)
        printf("%d ", a[i]);
    return 0;
}
```

<details>
<summary>Giải thích kỹ: <code>fscanf</code> hoạt động ra sao</summary>

`fscanf(f, "%d", &x)` giống `scanf` nhưng đọc từ tệp. Nó:
1. Bỏ qua khoảng trắng và xuống dòng ở đầu;
2. Đọc ký tự để ghép thành một số nguyên;
3. Dừng tại ký tự đầu tiên không thuộc số;
4. Trả về **số giá trị đọc thành công** (ở đây là 1, hoặc 0/`EOF` khi hết).

Con trỏ vị trí đọc của tệp tự động tiến lên sau mỗi lần đọc nên các lần `fscanf` kế tiếp đọc tiếp dữ liệu phía sau – không cần tự quản lý vị trí.

Vì vậy trong ví dụ trên: lần đầu đọc `3` vào `n`, các lần sau lần lượt đọc `10`, `20`, `30`.

</details>

<details>
<summary>Đọc đến hết tệp khi chưa biết số phần tử</summary>

`fscanf` trả về số giá trị đọc được; trả về `EOF` (hoặc `0`) khi hết dữ liệu.

```c
int x, dem = 0;
while (fscanf(f, "%d", &x) == 1) {
    printf("%d ", x);
    dem++;
}
```

</details>

### 2.4 Tệp với struct

```c
// Ghi danh sách sinh viên, mỗi dòng: ten diem
for (int i = 0; i < n; i++)
    fprintf(f, "%s %.1f\n", ds[i].ten, ds[i].diem);

// Đọc lại
while (fscanf(f, "%s %f", ds[n].ten, &ds[n].diem) == 2)
    n++;
```

> Tên không có khoảng trắng khi dùng `%s` (ví dụ `An`, `Binh_Nguyen`).

---

## 3. Tóm Tắt Nhanh

```
Địa chỉ        ->  &a
Con trỏ        ->  int *p = &a;     *p là giá trị tại p
Hàm đổi biến   ->  void f(int *x)   gọi f(&a)
Mảng           ->  a[i]  ==  *(a+i)
Struct         ->  p->ten
Tệp            ->  fopen / fprintf / fscanf / fclose  (kiểm tra NULL!)
```

---

## Bài Tập Buổi 7

### Bài 1 – Dễ
Khai báo `int a = 5` và con trỏ `p` trỏ tới `a`. Dùng `p` để tăng `a` lên gấp đôi, in `a`, `*p` và địa chỉ.

### Bài 2 – Hoán đổi
Viết hàm `hoanDoi(int *x, int *y)`. Nhập 2 số, hoán đổi và in kết quả. Sau đó viết hàm `sapXep2(int *a, int *b)` để đảm bảo `a <= b`.

### Bài 3 – Con trỏ và mảng
Nhập mảng `n` phần tử. **Chỉ dùng con trỏ** (không dùng `a[i]`) để: in mảng, tính tổng, tìm max.

### Bài 4 – Min/Max qua tham số
Viết hàm `void minMax(int a[], int n, int *min, int *max)`. Gọi trong `main` và in kết quả.

### Bài 5 – Ghi tệp
Nhập `n` số nguyên, ghi vào tệp `input.txt` theo định dạng: dòng 1 là `n`, dòng 2 là các số.

### Bài 6 – Đọc tệp
Đọc `input.txt` ở bài 5, tính tổng, số lớn nhất rồi ghi kết quả vào `output.txt`.

### Bài 7 – Khá (struct + tệp)
Nhập danh sách `n` sinh viên (tên không dấu cách, điểm), ghi vào `sv.txt`. Đọc lại, in danh sách, in sinh viên có điểm cao nhất.

---

## Lỗi Thường Gặp

| Lỗi | Nguyên nhân | Cách sửa |
|---|---|---|
| Crash (segmentation fault) | Dùng `*p` khi `p` chưa khởi tạo / NULL | Gán `p = &biến` trước, kiểm tra NULL |
| Hoán đổi không có tác dụng | Truyền giá trị thay vì địa chỉ | Dùng `int *` và gọi bằng `&` |
| Quên `*` khi gán | Viết `p = 5` thay vì `*p = 5` | Phân biệt `p` (địa chỉ) và `*p` (giá trị) |
| `fopen` trả NULL | Sai tên / sai thư mục tệp | Kiểm tra đường dẫn, luôn kiểm tra NULL |
| Tệp bị mất nội dung | Mở `"w"` thay vì `"a"` | Dùng `"a"` nếu muốn ghi nối |
| Dữ liệu ghi chưa lưu | Quên `fclose` | Luôn đóng tệp |

---

Buổi tiếp theo: Buổi 8 – Con trỏ và Chuỗi
