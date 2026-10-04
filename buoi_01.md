# Buổi 1 – Nhập Xuất, Biến, Kiểu Dữ Liệu, Struct
## Môn: Kỹ Thuật Lập Trình C

---

## Mục tiêu buổi học

Sau buổi này sinh viên có thể:
- Hiểu chương trình C trông như thế nào
- Khai báo biến, dùng đúng kiểu dữ liệu
- Nhập dữ liệu từ bàn phím, in kết quả ra màn hình
- Định nghĩa và dùng struct đơn giản

---

## 1. Giới thiệu môn học

| Mục | Nội dung |
|---|---|
| Môn học | Kỹ Thuật Lập Trình |
| Ngôn ngữ | C (dùng compiler GCC / Dev-C++ / VS Code) |
| Đánh giá | Luyện tập buổi 6, 9, 13, 14 — Thi thử tổng kết buổi 15 |
| Lời khuyên | Code tay nhiều, đọc lỗi bình tĩnh, hỏi ngay khi không hiểu |

### Chương trình C đầu tiên – Hello World

```c
#include <stdio.h>

int main() {
    printf("Hello World!\n");
    return 0;
}
```

Kết quả:
```
Hello World!
```

<details>
<summary>Giải thích từng dòng</summary>

```c
#include <stdio.h>
```
Dòng này nạp thư viện `stdio.h` vào chương trình.
`stdio` viết tắt của **Standard Input Output** – thư viện cung cấp các hàm nhập xuất như `printf`, `scanf`.
Không có dòng này thì không dùng được `printf`.

```c
int main() {
```
Mọi chương trình C đều phải có hàm `main`. Đây là nơi chương trình bắt đầu chạy.
`int` phía trước nghĩa là hàm này sẽ trả về một số nguyên khi kết thúc.

```c
    printf("Hello World!\n");
```
Hàm `printf` in chuỗi ký tự ra màn hình.
`\n` là ký tự xuống dòng – nếu không có thì con trỏ vẫn đứng ngay sau chữ "World!".

```c
    return 0;
```
Trả về giá trị 0 cho hệ điều hành, báo hiệu chương trình chạy thành công.
Quy ước: trả về 0 là thành công, khác 0 là có lỗi.

```c
}
```
Đóng hàm `main`.

</details>

---

## 2. Biến và Kiểu Dữ Liệu

**Biến** là ô nhớ dùng để lưu giá trị. Mỗi biến phải có tên và kiểu dữ liệu.

### 2.1 Các kiểu cơ bản

| Kiểu | Ý nghĩa | Kích thước | Ví dụ giá trị |
|---|---|---|---|
| `int` | Số nguyên | 4 byte | -5, 0, 100 |
| `float` | Số thực (7 chữ số) | 4 byte | 3.14f |
| `double` | Số thực (15 chữ số) | 8 byte | 3.14159265 |
| `char` | Một ký tự | 1 byte | 'A', '5' |

<details>
<summary>Giải thích chi tiết từng kiểu</summary>

**int – số nguyên**
Lưu các số không có phần thập phân.
Phạm vi: khoảng -2 tỷ đến +2 tỷ (chính xác: -2,147,483,648 đến 2,147,483,647).
```c
int diem = 9;
int nhietdo = -5;
int so_luong = 0;
```

**float – số thực đơn**
Lưu số có phần thập phân, chính xác khoảng 6-7 chữ số.
Khi gán giá trị cần thêm chữ `f` ở cuối.
```c
float chieu_cao = 1.70f;
float pi = 3.14f;
```

**double – số thực đôi**
Giống float nhưng chính xác hơn (khoảng 15 chữ số). Dùng khi cần tính toán chính xác cao.
```c
double pi = 3.14159265358979;
```

**char – ký tự**
Lưu một ký tự duy nhất, đặt trong dấu nháy đơn `' '`.
```c
char loai = 'A';
char kytu = '5';   // đây là ký tự '5', không phải số 5
```

</details>

### 2.2 Khai báo biến

```c
int tuoi;               // khai báo chưa có giá trị
int diem = 9;           // khai báo và gán ngay
float chieu_cao = 1.70f;
char kytu = 'A';
```

<details>
<summary>Quy tắc đặt tên biến</summary>

**Hợp lệ:**
```c
int tuoi;
int chieuCao;       // camelCase
int chieu_cao;      // snake_case
int _bien;          // bắt đầu bằng dấu gạch dưới cũng được
```

**Không hợp lệ:**
```c
int 1tuoi;          // lỗi: bắt đầu bằng số
int tuổi;           // lỗi: có dấu tiếng Việt
int int;            // lỗi: trùng từ khoá
int chieu cao;      // lỗi: có khoảng trắng
```

**Lưu ý quan trọng:** C phân biệt chữ hoa/thường. `tuoi` và `Tuoi` là 2 biến khác nhau.

</details>

---

## 3. Nhập và Xuất Dữ Liệu

### 3.1 In ra màn hình – printf

```c
printf("Xin chao!\n");
printf("Tuoi: %d\n", tuoi);
printf("Diem: %.2f\n", diem_tb);
printf("Ky tu: %c\n", kytu);
```

| Format | Dùng cho |
|---|---|
| `%d` | int |
| `%f` | float / double |
| `%.2f` | float/double, 2 chữ số thập phân |
| `%c` | char |
| `%s` | chuỗi (string) |

<details>
<summary>Giải thích cách printf hoạt động</summary>

`printf` nhận vào một chuỗi định dạng, bên trong chuỗi đó có các **placeholder** bắt đầu bằng `%`.
Mỗi `%` sẽ được thay thế bằng giá trị của biến tương ứng theo thứ tự.

```c
int a = 10;
float b = 3.14f;
printf("a = %d, b = %.2f\n", a, b);
// In ra: a = 10, b = 3.14
```

Các ký tự đặc biệt trong chuỗi:

| Ký tự | Ý nghĩa |
|---|---|
| `\n` | Xuống dòng |
| `\t` | Tab (thụt vào) |
| `\\` | In dấu `\` |
| `\"` | In dấu `"` |

Ví dụ:
```c
printf("Dong 1\nDong 2\n");
// In ra:
// Dong 1
// Dong 2
```

</details>

### 3.2 Nhập từ bàn phím – scanf

```c
int tuoi;
printf("Nhap tuoi: ");
scanf("%d", &tuoi);
```

<details>
<summary>Tại sao phải có dấu & trước tên biến?</summary>

`&tuoi` có nghĩa là "địa chỉ của biến `tuoi`" trong bộ nhớ.

`scanf` cần biết **chỗ nào trong bộ nhớ** để lưu giá trị vừa nhập vào.
Nếu chỉ viết `scanf("%d", tuoi)` thì bạn đang truyền **giá trị** của `tuoi` (chưa khởi tạo, giá trị rác), không phải **địa chỉ** – chương trình sẽ crash.

```c
// Sai – chương trình crash
scanf("%d", tuoi);

// Đúng
scanf("%d", &tuoi);
```

Ngoại lệ: mảng và chuỗi không cần `&` vì tên mảng bản thân đã là địa chỉ.
```c
char ten[50];
scanf("%s", ten);   // không cần &ten
```

</details>

### Ví dụ hoàn chỉnh

```c
#include <stdio.h>

int main() {
    int tuoi;
    float chieu_cao;

    printf("Nhap tuoi: ");
    scanf("%d", &tuoi);

    printf("Nhap chieu cao (m): ");
    scanf("%f", &chieu_cao);

    printf("Ban %d tuoi, cao %.2f m\n", tuoi, chieu_cao);
    return 0;
}
```

Kết quả mẫu:
```
Nhap tuoi: 20
Nhap chieu cao (m): 1.70
Ban 20 tuoi, cao 1.70 m
```

---

## 4. Struct – Kiểu Dữ Liệu Tự Định Nghĩa

Khi cần nhóm nhiều thông tin liên quan thành một đơn vị, dùng `struct`.

Ví dụ thực tế: thông tin sinh viên gồm tên, tuổi, điểm – thay vì tạo 3 biến rời rạc, nhóm chúng lại trong một struct.

### 4.1 Cách định nghĩa

```c
struct SinhVien {
    char ten[50];
    int tuoi;
    float diem;
};
```

<details>
<summary>Giải thích cú pháp struct</summary>

```c
struct SinhVien {    // "SinhVien" là tên kiểu, đặt tuỳ ý
    char ten[50];    // mỗi dòng bên trong gọi là "trường" (field)
    int tuoi;
    float diem;
};                   // chú ý dấu chấm phẩy ở cuối – hay quên!
```

Sau khi định nghĩa, `struct SinhVien` trở thành một kiểu dữ liệu mới, dùng giống như `int` hay `float`.

```c
struct SinhVien sv1;   // khai báo biến kiểu struct SinhVien
struct SinhVien sv2;   // có thể khai báo nhiều biến
```

Truy cập các trường bằng dấu chấm `.`:
```c
sv1.tuoi = 20;
sv1.diem = 8.5f;
```

</details>

### 4.2 Cách dùng

```c
#include <stdio.h>

struct SinhVien {
    char ten[50];
    int tuoi;
    float diem;
};

int main() {
    struct SinhVien sv;

    printf("Nhap ten: ");
    scanf("%s", sv.ten);        // chuỗi không cần &

    printf("Nhap tuoi: ");
    scanf("%d", &sv.tuoi);

    printf("Nhap diem: ");
    scanf("%f", &sv.diem);

    printf("\n--- Thong tin sinh vien ---\n");
    printf("Ten : %s\n", sv.ten);
    printf("Tuoi: %d\n", sv.tuoi);
    printf("Diem: %.1f\n", sv.diem);

    return 0;
}
```

Kết quả mẫu:
```
Nhap ten: AnhTuan
Nhap tuoi: 20
Nhap diem: 8.5

--- Thong tin sinh vien ---
Ten : AnhTuan
Tuoi: 20
Diem: 8.5
```

### 4.3 Dùng typedef cho gọn

```c
typedef struct {
    char ten[50];
    int tuoi;
    float diem;
} SinhVien;

// Từ đây dùng SinhVien thay vì struct SinhVien
SinhVien sv;
```

<details>
<summary>typedef hoạt động như thế nào?</summary>

`typedef` cho phép đặt **bí danh** cho một kiểu dữ liệu.

Không dùng typedef:
```c
struct SinhVien sv;        // phải viết đủ "struct SinhVien"
```

Dùng typedef:
```c
typedef struct {
    char ten[50];
    int tuoi;
    float diem;
} SinhVien;

SinhVien sv;               // gọn hơn, không cần từ khoá "struct"
```

Trong thực tế hầu hết code C đều dùng typedef vì ngắn gọn hơn.

</details>

---

## 5. Tóm Tắt Nhanh

```
Khai báo biến   ->  int x;  /  float y = 1.5f;
In ra           ->  printf("%d", x);
Nhập vào        ->  scanf("%d", &x);
Struct          ->  nhóm nhiều biến lại, truy cập bằng dấu chấm  sv.ten
typedef         ->  đặt tên ngắn gọn hơn cho struct
```

---

## Bài Tập Buổi 1

### Bài 1 – Dễ
Viết chương trình:
- Nhập vào 2 số nguyên `a`, `b`
- In ra tổng, hiệu, tích, thương của chúng

Gợi ý output:
```
Nhap a: 10
Nhap b: 3
Tong: 13
Hieu: 7
Tich: 30
Thuong: 3.33
```

<details>
<summary>Gợi ý làm bài 1</summary>

- Khai báo 2 biến `int a, b`
- Dùng `scanf` nhập 2 lần
- Phép thương: `(float)a / b` – cần ép kiểu sang float, nếu không `10/3` sẽ ra `3` thay vì `3.33`

```c
printf("Thuong: %.2f\n", (float)a / b);
```

</details>

---

### Bài 2 – Trung bình
Viết chương trình nhập thông tin 1 học sinh gồm: Họ tên, Tuổi, Điểm Toán, Điểm Văn, Điểm Anh.
- Tính và in ra điểm trung bình.
- Nếu điểm TB >= 8.0 in "Gioi", >= 6.5 in "Kha", còn lại in "Trung binh".

Gợi ý: dùng struct để lưu thông tin học sinh.

<details>
<summary>Gợi ý làm bài 2</summary>

Định nghĩa struct:
```c
typedef struct {
    char ten[50];
    int tuoi;
    float toan, van, anh;
} HocSinh;
```

Tính điểm trung bình:
```c
float tb = (hs.toan + hs.van + hs.anh) / 3.0f;
```

Xếp loại (phần này dùng if/else – sẽ học kỹ ở buổi 2):
```c
if (tb >= 8.0f)
    printf("Gioi\n");
else if (tb >= 6.5f)
    printf("Kha\n");
else
    printf("Trung binh\n");
```

</details>

---

### Bài 3 – Khá
Viết chương trình:
- Định nghĩa struct `HinhChuNhat` gồm `chieuDai` và `chieuRong` (kiểu `float`)
- Nhập vào thông số hình chữ nhật
- Tính và in ra chu vi và diện tích

<details>
<summary>Gợi ý làm bài 3</summary>

```c
typedef struct {
    float chieuDai;
    float chieuRong;
} HinhChuNhat;
```

Công thức:
```
Chu vi    = 2 * (chieuDai + chieuRong)
Dien tich = chieuDai * chieuRong
```

</details>

---

## Lỗi Thường Gặp

| Lỗi | Nguyên nhân | Cách sửa |
|---|---|---|
| Không có output | Quên `\n` hoặc quên `return 0` | Thêm vào |
| Giá trị in ra sai | Dùng sai format (`%d` cho float) | Kiểm tra lại `%f` hay `%d` |
| Chương trình crash | Quên `&` trong `scanf` | Thêm `&` trước tên biến |
| Tên biến lỗi | Có dấu hoặc bắt đầu bằng số | Đổi lại tên |
| Struct không hoạt động | Quên dấu `;` sau dấu `}` của struct | Thêm `;` vào cuối |

---

Buổi tiếp theo: Buổi 2 – Toán tử và Câu lệnh rẽ nhánh
