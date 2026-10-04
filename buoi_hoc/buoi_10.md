# Buổi 10 – Cấp Phát Động – Mảng 1 Chiều
## Môn: Kỹ Thuật Lập Trình C

---

## Mục tiêu buổi học

Sau buổi này sinh viên có thể:
- Hiểu vì sao cần cấp phát động
- Dùng `malloc`, `calloc`, `realloc`, `free`
- Tạo mảng 1 chiều có kích thước nhập lúc chạy chương trình
- Tránh rò rỉ bộ nhớ và các lỗi thường gặp với con trỏ

---

## 1. Tại sao cần cấp phát động?

Mảng thường: kích thước phải biết **trước khi chạy**.

```c
int a[100];       // luôn tốn 100 ô, nhập quá 100 thì tràn
```

Vấn đề: nhập `n = 5` thì phí; nhập `n = 1000` thì thiếu.

**Cấp phát động**: xin đúng số ô cần dùng khi chương trình đang chạy, dùng xong thì trả lại.

---

## 2. Các hàm cấp phát (thư viện `<stdlib.h>`)

```c
#include <stdlib.h>
```

| Hàm | Công dụng |
|---|---|
| `malloc(số_byte)` | Xin vùng nhớ, **không** khởi tạo (giá trị rác) |
| `calloc(số_phần_tử, kích_thước)` | Xin vùng nhớ, khởi tạo toàn bộ về **0** |
| `realloc(p, số_byte_mới)` | Đổi kích thước vùng nhớ đã xin |
| `free(p)` | Trả vùng nhớ lại cho hệ thống |

Các hàm cấp phát trả về **con trỏ** tới vùng nhớ, hoặc `NULL` nếu thất bại.

<details>
<summary>Giải thích kỹ: stack, heap và vì sao phải tự <code>free</code></summary>

Bộ nhớ chương trình chia phần quan trọng thành hai vùng:

- **Stack**: chứa biến cục bộ và tham số. Tự cấp phát khi vào hàm, tự thu hồi khi ra khỏi hàm. Kích thước phải biết lúc biên dịch (`int a[100]`) và dung lượng nhỏ (khoảng vài MB).
- **Heap**: vùng lớn dành cho `malloc`. Xin bao nhiêu tùy ý lúc chạy, **sống cho đến khi `free`**, không phụ thuộc hàm nào đang chạy.

Biến con trỏ `a` vẫn nằm trên stack (chỉ 8 byte), nhưng nó trỏ tới khối lớn trên heap:

```
Stack:  a  ──────►  Heap: [ a[0] ][ a[1] ] ... [ a[n-1] ]
```

Nếu hàm kết thúc mà chưa `free` và con trỏ `a` mất → khối heap vẫn còn nhưng không ai biết địa chỉ để dùng hay trả → **rò rỉ bộ nhớ**. Chương trình chạy lâu sẽ ngốn hết RAM.

</details>

### 2.1 malloc

```c
int n;
printf("Nhap n: ");
scanf("%d", &n);

int *a = (int*)malloc(n * sizeof(int));
if (a == NULL) {
    printf("Khong du bo nho!\n");
    return 1;
}
```

<details>
<summary>Giải thích</summary>

- `sizeof(int)` = 4 byte → cần `n * 4` byte cho `n` số nguyên.
- `malloc` trả về `void*` (con trỏ không kiểu) nên ép kiểu `(int*)`.
- Luôn **kiểm tra NULL**: nếu hết bộ nhớ, `malloc` trả `NULL`, dùng tiếp sẽ crash.

</details>

### 2.2 Dùng như mảng bình thường

Con trỏ `a` dùng được với `a[i]`:

```c
for (int i = 0; i < n; i++) {
    printf("a[%d] = ", i);
    scanf("%d", &a[i]);
}

int tong = 0;
for (int i = 0; i < n; i++)
    tong += a[i];
printf("Tong = %d\n", tong);
```

<details>
<summary>Giải thích kỹ: vì sao dùng được <code>a[i]</code> với con trỏ?</summary>

`a[i]` chỉ là cách viết của `*(a + i)`. `a` là địa chỉ ô đầu, `a + i` nhảy `i` phần tử (mỗi bước `sizeof(int)` byte), nên dùng mảng tĩnh hay vùng `malloc` đều giống nhau. Hai khác biệt duy nhất: `sizeof(a)` với con trỏ chỉ cho 8 (kích thước con trỏ) chứ không phải `n * 4`; và bạn phải tự nhớ `n`.

Vì sao `scanf("%d", &a[i])` cần `&`? Khi đó `a[i]` là một số `int`, `&a[i]` là địa chỉ ô đó – giống mảng thường. Có thể viết tương đương `scanf("%d", a + i)`.

</details>

### 2.3 Giải phóng – free

```c
free(a);
a = NULL;     // tránh dùng nhầm con trỏ đã giải phóng
```

> Quy tắc vàng: **mỗi `malloc` phải có đúng một `free`**.

<details>
<summary>Giải thích kỹ: sau <code>free</code> chuyện gì xảy ra với <code>a</code>?</summary>

`free(a)` **chỉ trả khối nhớ**, không sửa biến `a`. `a` vẫn giữ địa chỉ cũ (gọi là *con trỏ treo*). Nếu lỡ đọc/ghi `a[0]` nữa → hành vi không xác định (có thể đọc được rác, có thể crash, đáng sợ nhất là thỉnh thoảng vẫn "chạy đúng").

Gán `a = NULL` sau `free` giúp: (1) dùng nhầm sẽ crash ngay, dễ phát hiện; (2) `free(NULL)` hợp lệ và không làm gì, nên lỡ `free` hai lần vẫn an toàn.

</details>

### 2.4 calloc

```c
int *a = (int*)calloc(n, sizeof(int));   // n ô, đều bằng 0
```

Tiện khi cần mảng đếm khởi tạo 0.

### 2.5 realloc – đổi kích thước

```c
int *tam = (int*)realloc(a, (n + 5) * sizeof(int));
if (tam == NULL) {
    printf("Khong du bo nho!\n");
    free(a);
    return 1;
}
a = tam;
n += 5;
```

> Luôn gán vào biến **tạm** trước. Nếu `realloc` thất bại và gán thẳng vào `a`, bạn mất con trỏ cũ (rò rỉ bộ nhớ).

<details>
<summary>Giải thích kỹ: <code>malloc</code> khác <code>calloc</code>, và <code>realloc</code> hoạt động thế nào</summary>

- `malloc(n * 4)`: chỉ xin 1 khối `n*4` byte; nội dung là **rác**. `calloc(n, 4)`: xin `n` ô mỗi ô 4 byte và **điền 0**, chạy chậm hơn chút. Dùng khi cần giá trị ban đầu 0 (mảng đếm, cờ).
- `realloc` có hai kịch bản:
  1. Sau khối cũ còn chỗ trống → mở rộng tại chỗ, địa chỉ không đổi.
  2. Không đủ chỗ → tìm khối mới đủ lớn, **sao chép** dữ liệu cũ sang, **tự free khối cũ**, trả địa chỉ mới.
  
  Vì kịch bản 2, không được giữ con trỏ cũ (nên `a = tam`), và không được `free(a)` lại sau khi `realloc` thành công.
- Phần mở rộng thêm của `realloc` có giá trị **rác**. Bạn phải tự gán cho các ô mới.
- Nếu thu nhỏ, dữ liệu phần bị cắt sẽ mất.

</details>

---

## 3. Chương Trình Hoàn Chỉnh

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n;
    printf("Nhap so phan tu: ");
    scanf("%d", &n);

    int *a = (int*)malloc(n * sizeof(int));
    if (a == NULL) {
        printf("Khong du bo nho!\n");
        return 1;
    }

    for (int i = 0; i < n; i++) {
        printf("a[%d] = ", i);
        scanf("%d", &a[i]);
    }

    int max = a[0];
    for (int i = 1; i < n; i++)
        if (a[i] > max) max = a[i];
    printf("Max = %d\n", max);

    free(a);
    return 0;
}
```

<details>
<summary>Giải thích kỹ: chương trình hoàn chỉnh</summary>

Theo thứ tự thời gian:
1. Nhập `n` trước → biết cần bao nhiêu ô (điều mảng thoả kích thước cố định không làm được).
2. `malloc` xin đúng `n` ô; kiểm tra NULL, thất bại thì `return 1` để báo lỗi.
3. Dùng bình thường như mảng: nhập, tìm max (`max` khởi tạo `a[0]` rồi duyệt từ 1).
4. `free(a)` trước `return 0` – đây là lúc cuối cùng có thể trả bộ nhớ.

Lưu ý: nếu `return` sớm ở giữa chừng (ví dụ sau khi thấy lỗi) thì cũng phải `free` trước khi thoát – đây là lý do nhiều người viết một điểm thoát duy nhất ở cuối hàm.

</details>

---

## 4. Truyền Mảng Động Vào Hàm

```c
void nhapMang(int *a, int n) {
    for (int i = 0; i < n; i++)
        scanf("%d", &a[i]);
}
```

### Hàm tạo mảng và trả về con trỏ

```c
int* taoMang(int n) {
    int *a = (int*)malloc(n * sizeof(int));
    return a;               // trả về địa chỉ vùng nhớ trên heap
}
```

> Không bao giờ trả về địa chỉ biến cục bộ (như mảng `int a[10]` khai báo trong hàm) – hàm kết thúc là vùng nhớ đó mất.

<details>
<summary>Stack và Heap</summary>

| | Stack | Heap |
|---|---|---|
| Chứa | Biến cục bộ, tham số | Vùng nhớ xin bằng `malloc` |
| Quản lý | Tự động | Lập trình viên (`free`) |
| Sống đến | Hết hàm | Đến khi `free` |

</details>

### Hàm cấp phát qua tham số con trỏ

Nếu muốn hàm cấp phát và gán lại cho con trỏ bên ngoài, truyền địa chỉ của con trỏ (con trỏ cấp 2):

```c
void taoMang2(int **a, int n) {
    *a = (int*)malloc(n * sizeof(int));
}

int *arr = NULL;
taoMang2(&arr, 5);
```

---

## 5. Tóm Tắt Nhanh

```
Xin nhớ      ->  int *a = (int*)malloc(n * sizeof(int));
Kiểm tra     ->  if (a == NULL) ...
Dùng         ->  a[i]
Đổi kích thước -> tam = realloc(a, moi * sizeof(int));
Trả lại      ->  free(a); a = NULL;
```

---

## Bài Tập Buổi 10

### Bài 1 – Dễ
Nhập `n`, cấp phát mảng `n` số nguyên, nhập, in và tính tổng, trung bình. Nhớ `free`.

### Bài 2 – Số nguyên tố
Nhập `n`, cấp phát mảng, nhập `n` số. In ra các số nguyên tố trong mảng và số lượng.

### Bài 3 – Hàm
Viết các hàm làm việc với mảng động: `int* taoMang(int n)`, `void nhapMang(int *a, int n)`, `void xuatMang(int *a, int n)`, `void sapXep(int *a, int n)`.

### Bài 4 – Mảng đếm
Nhập `n` số có giá trị từ `0` đến `9`. Dùng `calloc` tạo mảng đếm 10 phần tử, in số lần xuất hiện của từng chữ số.

### Bài 5 – Thêm phần tử
Nhập mảng `n` phần tử. Nhập `x` và vị trí `k`. Dùng `realloc` để thêm `x` vào vị trí `k`, dịch các phần tử phía sau. In mảng mới.


### Bài 6 – Nhập đến khi âm (khó)
Không biết trước số lượng: nhập số nguyên đến khi gặp số âm. Mỗi lần nhập thêm, dùng `realloc` tăng mảng 1 ô. Cuối cùng in tất cả các số đã nhập theo thứ tự giảm dần.

---

## Lỗi Thường Gặp

| Lỗi | Nguyên nhân | Cách sửa |
|---|---|---|
| Crash khi dùng mảng | `malloc` trả về NULL không kiểm tra | `if (a == NULL)` |
| Rò rỉ bộ nhớ | Quên `free` | Mỗi `malloc` đi cùng một `free` |
| Double free | `free` hai lần cùng một con trỏ | Gán `a = NULL` sau khi `free` |
| Use after free | Dùng con trỏ sau khi `free` | Không dùng, gán NULL |
| Cấp phát thiếu | Viết `malloc(n)` thay vì `n * sizeof(int)` | Nhân với `sizeof(kiểu)` |
| Truy cập `a[n]` | Chỉ số vượt phạm vi | Chỉ số hợp lệ `0..n-1` |

---

Buổi tiếp theo: Buổi 11 – Cấp phát động mảng 1 chiều và struct
