# Buổi 13 – Giới Thiệu Qua Đệ Quy + Ôn Tập Buổi 10, 11, 12
## Môn: Kỹ Thuật Lập Trình C

---

## Mục tiêu buổi học

- Hiểu đệ quy là gì, gồm **điều kiện dừng** và **bước đệ quy**
- Viết được các hàm đệ quy đơn giản (giai thừa, Fibonacci, tổng, ước chung lớn nhất)
- Ôn tập và củng cố cấp phát động: mảng 1 chiều, struct, mảng 2 chiều

---

# Phần A – Giới Thiệu Qua Đệ Quy

## 1. Đệ quy là gì?

**Đệ quy** là khi một hàm **tự gọi chính nó** để giải bài toán nhỏ hơn.

Ví dụ: `n! = n × (n-1)!`, tức bài toán `n!` quy về bài toán `(n-1)!`.

Mọi hàm đệ quy cần 2 phần:

| Phần | Vai trò |
|---|---|
| **Điều kiện dừng** | Trường hợp nhỏ nhất, trả kết quả trực tiếp |
| **Bước đệ quy** | Gọi lại chính hàm với bài toán nhỏ hơn |

> Thiếu điều kiện dừng → hàm gọi nhau vô tận → tràn stack → crash.

<details>
<summary>Giải thích kỹ: cách tư duy khi viết đệ quy</summary>

Khi gặp bài đệ quy, tự trả lời 3 câu:
1. **Bài nhỏ nhất là gì** và kết quả của nó? → điều kiện dừng. (`0! = 1`, tổng của 0 số = 0)
2. **Bài lớn liên hệ thế nào với bài nhỏ hơn một bước?** → bước đệ quy. (`n! = n * (n-1)!`)
3. **Mỗi lần gọi, bài toán có thực sự nhỏ đi và hướng tới điều kiện dừng không?** (`n` giảm 1 mỗi lần → sẽ về 0)

Tin tưởng vào "niềm tin đệ quy": giả sử `giaiThua(n-1)` đã **chạy đúng**, bạn chỉ cần lo phần còn lại (nhân thêm `n`). Đừng cố hình dung toàn bộ chuỗi gọi trong đầu.

Bộ nhớ: mỗi lời gọi tạo một "khung" riêng trên stack chứa `n` riêng của nó. Gọi quá sâu (ví dụ `n = 1 000 000`) sẽ đầy stack → lỗi *stack overflow*.

</details>

---

## 2. Ví dụ 1 – Giai thừa

```c
#include <stdio.h>

long long giaiThua(int n) {
    if (n == 0 || n == 1)       // điều kiện dừng
        return 1;
    return n * giaiThua(n - 1); // bước đệ quy
}

int main() {
    printf("5! = %lld\n", giaiThua(5));
    return 0;
}
```

<details>
<summary>Theo dõi quá trình chạy giaiThua(4)</summary>

```
giaiThua(4) = 4 * giaiThua(3)
                   giaiThua(3) = 3 * giaiThua(2)
                                      giaiThua(2) = 2 * giaiThua(1)
                                                         giaiThua(1) = 1   ← dừng
                                      giaiThua(2) = 2 * 1 = 2
                   giaiThua(3) = 3 * 2 = 6
giaiThua(4) = 4 * 6 = 24
```

Các lời gọi xếp chồng lên nhau trên **stack**, đến khi chạm điều kiện dừng thì lần lượt trả kết quả về.

</details>

## 3. Ví dụ 2 – Tổng 1 đến n

```c
int tong(int n) {
    if (n == 0) return 0;
    return n + tong(n - 1);
}
```

## 4. Ví dụ 3 – Fibonacci

`F(0) = 0, F(1) = 1, F(n) = F(n-1) + F(n-2)`

```c
int fib(int n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}
```

> Cách này chạy chậm với `n` lớn vì tính lặp lại nhiều lần. Chỉ dùng để làm quen.

<details>
<summary>Giải thích kỹ: vì sao Fibonacci đệ quy chậm?</summary>

`fib(5)` gọi `fib(4)` và `fib(3)`; `fib(4)` lại gọi `fib(3)` và `fib(2)`... → `fib(3)` bị tính **nhiều lần** như nhau:

```
                fib(5)
              /        \
          fib(4)       fib(3)
          /    \        /   \
      fib(3) fib(2)  fib(2) fib(1)
```

Số lời gọi tăng gần gấp đôi mỗi khi `n` tăng 1 (độ phức tạp xấp xỉ 2ⁿ). Với `n = 40` đã mất vài giây. Cách nhanh hơn là dùng vòng lặp với hai biến giữ hai số trước.

</details>

## 5. Ví dụ 4 – Ước chung lớn nhất (Euclid)

```c
int ucln(int a, int b) {
    if (b == 0) return a;
    return ucln(b, a % b);
}
```

<details>
<summary>Giải thích kỹ: Euclid chạy thế nào?</summary>

Dựa vào tính chất: `ucln(a, b) = ucln(b, a % b)`.

`ucln(48, 18)` → `ucln(18, 12)` → `ucln(12, 6)` → `ucln(6, 0)` → trả về **6**.

- Điều kiện dừng `b == 0`: mọi số chia hết cho 0 → UCLN là `a`.
- Mỗi lần gọi, `b` mới (`a % b`) luôn nhỏ hơn `b` cũ → chắc chắn về 0.

</details>

## 6. Ví dụ 5 – Đệ quy trên mảng

```c
int tongMang(int a[], int n) {
    if (n == 0) return 0;
    return a[n - 1] + tongMang(a, n - 1);
}

int timMax(int a[], int n) {
    if (n == 1) return a[0];
    int m = timMax(a, n - 1);
    return (a[n - 1] > m) ? a[n - 1] : m;
}
```

## 7. Ví dụ 6 – In ngược số

```c
void inNguoc(int n) {
    if (n == 0) return;
    printf("%d", n % 10);
    inNguoc(n / 10);
}
// inNguoc(1234) in ra 4321
```

<details>
<summary>Giải thích kỹ: đệ quy trên mảng và in ngược</summary>

- `tongMang(a, n)`: tổng `n` phần tử = phần tử cuối `a[n-1]` + tổng `n-1` phần tử đầu. Khi `n == 0` mảng rỗng, tổng 0. Mỗi lần gọi, "độ dài" mảng thu nhỏ 1 → bài toán nhỏ đi.
- `timMax(a, n)`: max của `n` phần tử = lớn hơn giữa `a[n-1]` và max của `n-1` phần tử đầu. Điều kiện dừng `n == 1` (mảng 1 phần tử thì max là chính nó).
- `inNguoc(1234)`: in `1234 % 10 = 4`, rồi gọi `inNguoc(123)` in 3, ... → `4321`. Nếu đổi thứ tự hai dòng (gọi đệ quy trước rồi mới `printf`) thì sẽ in **xuôi** `1234` vì `printf` chạy khi các lời gọi quay về. Đây là thủ thuật thường gặp: **trước lời gọi = chạy từ trên xuống; sau lời gọi = chạy ngược lại**.

</details>

## 8. Đệ quy hay vòng lặp?

| | Đệ quy | Vòng lặp |
|---|---|---|
| Code | Ngắn, gần với công thức toán | Dài hơn chút |
| Tốc độ / bộ nhớ | Chậm hơn, tốn stack | Nhanh, tiết kiệm |
| Phù hợp | Cấu trúc tự nhiên đệ quy (cây, chia để trị) | Duyệt đơn giản |

---

## Bài tập đệ quy

1. Viết hàm đệ quy tính `x^n` (`x` thực, `n` nguyên ≥ 0).
2. Viết hàm đệ quy đếm số chữ số của `n`.
3. Viết hàm đệ quy tính tổng các chữ số của `n`.
4. Viết hàm đệ quy kiểm tra chuỗi đối xứng.
5. Viết hàm đệ quy đổi số thập phân sang nhị phân (in ra).
6. (Khó) Bài toán Tháp Hà Nội với `n` đĩa: in các bước chuyển.


---

# Phần B – Ôn Tập Buổi 10, 11, 12

## Checklist Cấp Phát Động

| Kiến thức | Cần nhớ |
|---|---|
| Mảng 1 chiều | `int *a = malloc(n * sizeof(int));` rồi `free(a);` |
| Kiểm tra | Luôn `if (a == NULL)` |
| Đổi kích thước | `tam = realloc(a, ...)` – dùng biến tạm |
| Mảng struct | `SinhVien *ds = malloc(n * sizeof(SinhVien));` – `ds[i].truong` |
| Con trỏ struct | `p->truong` |
| Ma trận | `a = malloc(m * sizeof(int*))` + từng hàng; `free` từng hàng rồi `free(a)` |
| Mỗi `malloc` | luôn có một `free` tương ứng |

## Bài Tập Ôn Tập

### Bài 1 – Mảng động
Nhập `n`, cấp phát mảng. Viết hàm xoá tất cả phần tử có giá trị `x` (dịch mảng, giảm `n`), sau đó `realloc` thu nhỏ mảng.

### Bài 2 – Mảng động + hàm trả về con trỏ
Viết hàm `int* locChan(int *a, int n, int *soChan)` trả về mảng **mới** chứa các số chẵn của `a`, và lưu số lượng vào `*soChan`.


### Bài 3 – Struct động
Struct `NhanVien` gồm tên, lương cơ bản, hệ số. Nhập `n` nhân viên, tính lương = cơ bản × hệ số, in bảng sắp xếp giảm dần theo lương và tổng quỹ lương.

### Bài 4 – Ma trận động
Nhập ma trận vuông `n x n` động. Kiểm tra có phải ma trận đối xứng không, tính tổng 2 đường chéo, tìm hàng có tổng lớn nhất.

### Bài 5 – Kết hợp đệ quy
Viết hàm đệ quy tính tổng các phần tử của mảng động (bài 1), và tính tổng các chữ số của từng phần tử.

### Bài 6 – Tổng hợp (khó)
Chương trình quản lý danh sách sinh viên có menu: thêm, xoá, tìm theo tên, sắp xếp theo điểm, lưu/đọc tệp. Dùng mảng động, không có rò rỉ bộ nhớ.

---

## Lỗi Thường Gặp

| Lỗi | Nguyên nhân | Cách sửa |
|---|---|---|
| Đệ quy chạy mãi rồi crash | Thiếu / sai điều kiện dừng | Xác định rõ trường hợp nhỏ nhất |
| Gọi đệ quy không thu nhỏ bài toán | `giaiThua(n)` gọi `giaiThua(n)` | Phải gọi với `n - 1` |
| Kết quả `n!` sai với `n` lớn | Tràn `int` | Dùng `long long` |
| Rò rỉ bộ nhớ ở ma trận | Chỉ `free(a)` | Free từng hàng trước |
| Crash sau `realloc` | Dùng con trỏ cũ | Gán lại `a = tam` |

---

Buổi tiếp theo: Buổi 14 – Luyện đề thi thử
