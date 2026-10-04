# Buổi 12 – Cấp Phát Động – Mảng 2 Chiều
## Môn: Kỹ Thuật Lập Trình C

---

## Mục tiêu buổi học

Sau buổi này sinh viên có thể:
- Cấp phát ma trận `m x n` theo 2 cách: mảng con trỏ và mảng 1 chiều "phẳng"
- Truy cập, nhập, xuất, xử lý ma trận động
- Giải phóng đúng thứ tự
- Viết hàm tạo / giải phóng ma trận

---

## 1. Ý tưởng

Mảng 2 chiều tĩnh `int a[100][100]` luôn tốn 10 000 ô. Với cấp phát động, ma trận `m x n` chỉ dùng đúng `m * n` ô.

Có 2 cách phổ biến:

| Cách | Cấu trúc | Số lần `malloc` |
|---|---|---|
| **Cách 1** – con trỏ cấp 2 | Mảng `m` con trỏ, mỗi con trỏ trỏ tới 1 hàng | `m + 1` |
| **Cách 2** – mảng phẳng | 1 mảng `m * n` ô, truy cập `a[i * n + j]` | 1 |

---

## 2. Cách 1 – Con trỏ cấp 2 (`int **`)

```
   a ──► [ ptr0 ][ ptr1 ][ ptr2 ]       (mảng các con trỏ hàng)
            │       │       │
            ▼       ▼       ▼
          [ ... ] [ ... ] [ ... ]       (mỗi hàng là một mảng int)
```

### Cấp phát

```c
int m, n;
printf("Nhap m, n: ");
scanf("%d %d", &m, &n);

int **a = (int**)malloc(m * sizeof(int*));      // bước 1: mảng m con trỏ
if (a == NULL) return 1;

for (int i = 0; i < m; i++) {
    a[i] = (int*)malloc(n * sizeof(int));       // bước 2: mỗi hàng n số
    if (a[i] == NULL) return 1;
}
```

### Dùng như mảng 2 chiều thông thường

```c
for (int i = 0; i < m; i++)
    for (int j = 0; j < n; j++) {
        printf("a[%d][%d] = ", i, j);
        scanf("%d", &a[i][j]);
    }
```

<details>
<summary>Giải thích kỹ: <code>int **</code> là gì và vì sao <code>a[i][j]</code> chạy được?</summary>

- `int *` = "con trỏ tới `int`". `int **` = "con trỏ tới **con trỏ** tới `int`".
- `a` trỏ tới mảng các con trỏ; mỗi `a[i]` là một `int *` trỏ tới hàng `i`.
- `a[i][j]` đọc theo hai bước: `a[i]` cho **địa chỉ hàng i**, rồi `[j]` lấy phần tử thứ `j` của hàng đó. Tức `*(*(a + i) + j)`.
- Bước 1 dùng `sizeof(int*)` vì mỗi phần tử là một **con trỏ**; bước 2 dùng `sizeof(int)` vì mỗi phần tử là một **số nguyên**. Nhầm hai cái này là lỗi hay gặp.
- Các hàng có thể nằm ở những nơi rời nhau trên heap – khác với mảng 2 chiều thường (liền khối). Vì thế mỗi hàng phải `free` riêng.

Sơ đồ cho `m = 3`, `n = 4`:

```
a ─► [a[0]] ─► [ . . . . ]
    [a[1]] ─► [ . . . . ]
    [a[2]] ─► [ . . . . ]
```

</details>

### Giải phóng – ngược thứ tự cấp phát

```c
for (int i = 0; i < m; i++)
    free(a[i]);      // giải phóng từng hàng trước
free(a);             // rồi mới giải phóng mảng con trỏ
a = NULL;
```

> Nếu `free(a)` trước thì mất địa chỉ các hàng → không `free` được hàng → rò rỉ bộ nhớ.

---

## 3. Cách 2 – Mảng phẳng

Lưu ma trận thành 1 dãy `m * n` ô liên tiếp, **hàng này nối tiếp hàng kia**.

```c
int *a = (int*)malloc(m * n * sizeof(int));

// phần tử hàng i, cột j  nằm ở vị trí  i * n + j
a[i * n + j] = 5;
```

Giải phóng chỉ cần một lần: `free(a);`

| | Cách 1 | Cách 2 |
|---|---|---|
| Cú pháp truy cập | `a[i][j]` đẹp | `a[i*n+j]` |
| Bộ nhớ | rời rạc | liền khối, nhanh hơn |
| Giải phóng | nhiều bước | một bước |
| Hàng có độ dài khác nhau | có thể | không |

<details>
<summary>Giải thích kỹ: vì sao <code>a[i * n + j]</code>?</summary>

Ví dụ ma trận 2 hàng, 3 cột (`m = 2`, `n = 3`) lưu liền nhau:

```
vị trí: 0    1    2    3    4    5
        a00  a01  a02  a10  a11  a12
        └─ hàng 0 ─┘ └─ hàng 1 ─┘
```

Mỗi hàng có `n` phần tử nên trước hàng `i` có `i * n` phần tử. Phần tử cột `j` của hàng đó cách đầu hàng `j` ô → vị trí `i * n + j`. Ví dụ `a11` ở `1*3 + 1 = 4`. **N là số cột**, không phải số hàng.

Đây cũng chính là cách mảng 2 chiều thường được trình biên dịch lưu (vì sao phải ghi số cột khi truyền vào hàm ở buổi 5).

</details>

---

## 4. Viết Hàm Cho Ma Trận Động

```c
int** taoMaTran(int m, int n) {
    int **a = (int**)malloc(m * sizeof(int*));
    if (a == NULL) return NULL;
    for (int i = 0; i < m; i++) {
        a[i] = (int*)malloc(n * sizeof(int));
        if (a[i] == NULL) {                 // thất bại giữa chừng: dọn dẹp phần đã cấp
            for (int k = 0; k < i; k++) free(a[k]);
            free(a);
            return NULL;
        }
    }
    return a;
}

void giaiPhong(int **a, int m) {
    for (int i = 0; i < m; i++)
        free(a[i]);
    free(a);
}

void nhapMT(int **a, int m, int n) {
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++)
            scanf("%d", &a[i][j]);
}

void xuatMT(int **a, int m, int n) {
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++)
            printf("%4d", a[i][j]);
        printf("\n");
    }
}
```

<details>
<summary>Giải thích kỹ: vì sao <code>taoMaTran</code> phức tạp và phải dọn dẹp?</summary>

Cấp phát `m + 1` lần nên có thể hết bộ nhớ **giữa chừng** (ví dụ thất bại ở hàng 5/10). Lúc đó `i = 5`: đã cấp thành công 5 hàng (0..4) cùng mảng con trỏ. Nếu chỉ `return NULL` thì 6 khối đó bị rò rỉ. Vì vậy vòng `for (k < i)` free các hàng đã cấp, rồi free `a`.

Hàm trả về con trỏ → người gọi **phải kiểm tra NULL** và **phải gọi `giaiPhong`** khi dùng xong. `giaiPhong(a, m)` phải biết đúng số hàng `m` để free đủ (vì thế với ma trận chuyển vị `n x m` phải truyền `n`).

Gợi ý thiết kế: mỗi bài, viết bộ bốn hàm này một lần rồi dùng lại cho mọi bài ma trận.

</details>

### Chương trình hoàn chỉnh

```c
#include <stdio.h>
#include <stdlib.h>

/* ... các hàm ở trên ... */

int main() {
    int m, n;
    printf("Nhap m, n: ");
    scanf("%d %d", &m, &n);

    int **a = taoMaTran(m, n);
    if (a == NULL) {
        printf("Khong du bo nho!\n");
        return 1;
    }

    nhapMT(a, m, n);
    xuatMT(a, m, n);

    int tong = 0;
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++)
            tong += a[i][j];
    printf("Tong = %d\n", tong);

    giaiPhong(a, m);
    return 0;
}
```

---

## 5. Ma Trận Chuyển Vị – Hàm Trả Về Ma Trận Mới

```c
int** chuyenVi(int **a, int m, int n) {
    int **b = taoMaTran(n, m);        // kích thước n x m
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++)
            b[j][i] = a[i][j];
    return b;
}
// dùng xong: giaiPhong(b, n);   // nhớ: b có n hàng
```

<details>
<summary>Giải thích kỹ: ma trận chuyển vị</summary>

Ma trận `a` cỡ `m x n` thì chuyển vị `b` cỡ `n x m`: hàng của `a` thành cột của `b`, nên `b[j][i] = a[i][j]`.

```
a (2x3):  1 2 3        b (3x2):  1 4
          4 5 6                  2 5
                                 3 6
```

Chú ý khi tạo `b`: `taoMaTran(n, m)` (đảo thứ tự), và duyệt bằng `i < m`, `j < n` theo ma trận **gốc**. Lỗi hay gặp: duyệt theo `b` nhưng dùng giới hạn của `a`, dẫn đến ghi ngoài mảng.

</details>

---

## 6. Tóm Tắt Nhanh

```
Cấp phát      ->  a = malloc(m * sizeof(int*));
                  for i: a[i] = malloc(n * sizeof(int));
Truy cập      ->  a[i][j]
Giải phóng    ->  for i: free(a[i]);  free(a);
Mảng phẳng    ->  malloc(m*n*sizeof(int)) ; a[i*n+j]
```

---

## Bài Tập Buổi 12

### Bài 1 – Dễ
Nhập `m`, `n`. Cấp phát ma trận động, nhập, in ra, tính tổng. Giải phóng bộ nhớ.

### Bài 2 – Tổng hàng, tổng cột
Với ma trận động, in tổng từng hàng, tổng từng cột và phần tử lớn nhất (kèm vị trí).

### Bài 3 – Hàm
Viết các hàm `taoMaTran`, `nhapMT`, `xuatMT`, `giaiPhong`. Viết thêm hàm `tongDuongCheo` cho ma trận vuông.

### Bài 4 – Cộng 2 ma trận
Nhập 2 ma trận cùng kích thước `m x n`. Tạo ma trận tổng bằng cấp phát động và in kết quả.

### Bài 5 – Nhân 2 ma trận
Nhập `A (m x n)` và `B (n x p)`. Tính `C = A × B` (`m x p`) với `C[i][j] = Σ A[i][k] * B[k][j]`.


### Bài 6 – Tam giác Pascal (khó)
Nhập `n`. Dùng mảng 2 chiều **mà hàng `i` có `i + 1` phần tử** (cấp phát hàng có độ dài khác nhau). In tam giác Pascal `n` dòng.


---

## Lỗi Thường Gặp

| Lỗi | Nguyên nhân | Cách sửa |
|---|---|---|
| Crash khi nhập | Quên cấp phát từng hàng | Vòng lặp `a[i] = malloc(...)` |
| Rò rỉ bộ nhớ | Chỉ `free(a)` | `free` từng hàng rồi mới `free(a)` |
| Sai kích thước khi chuyển vị | Nhầm `m` và `n` | Ma trận mới `n x m`, giải phóng `n` hàng |
| `malloc(m * sizeof(int))` cho `int**` | Dùng nhầm `sizeof(int)` | Dùng `sizeof(int*)` |
| Truy cập sai ở mảng phẳng | Dùng `i * m + j` | Công thức đúng `i * n + j` (n là số cột) |

---

Buổi tiếp theo: Buổi 13 – Giới thiệu qua Đệ quy + Ôn tập buổi 10, 11, 12
