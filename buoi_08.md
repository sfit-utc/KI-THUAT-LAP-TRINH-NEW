# Buổi 8 – Con Trỏ và Chuỗi
## Môn: Kỹ Thuật Lập Trình C

---

## Mục tiêu buổi học

Sau buổi này sinh viên có thể:
- Hiểu chuỗi là mảng `char` kết thúc bằng `'\0'`
- Dùng con trỏ `char *` để duyệt và xử lý chuỗi
- Nhập chuỗi có khoảng trắng bằng `fgets`
- Dùng thành thạo các hàm trong `<string.h>` và tự viết lại chúng
- Xử lý bài toán: đảo chuỗi, đếm từ, chuẩn hoá chuỗi, kiểm tra đối xứng

---

## 1. Ôn lại – Chuỗi là mảng char

```c
char s[20] = "Hello";
```

Trong bộ nhớ:

```
 index:  0    1    2    3    4    5
 giá trị 'H'  'e'  'l'  'l'  'o'  '\0'
```

Ký tự `'\0'` đánh dấu **kết thúc chuỗi**. Mảng phải đủ chỗ cho cả `'\0'` (chuỗi 5 ký tự cần ít nhất 6 ô).

<details>
<summary>Giải thích kỹ: vì sao chuỗi cần <code>'\0'</code>?</summary>

Mảng không tự biết mình "dùng mấy ô". C không lưu độ dài chuỗi; thay vào đó **quy ước**: chuỗi kết thúc tại ký tự có mã 0 (`'\0'`). Mọi hàm (`printf`, `strlen`, `strcpy`...) đều đọc đến khi gặp `'\0'` thì dừng.

- `char s[20] = "Hello";` → máy tự thêm `'\0'` sau chữ `o`, các ô còn lại đều là `'\0'`.
- `'0'` (ký tự số 0, mã 48) **khác** `'\0'` (mã 0). Hãy phân biệt.
- Nếu tự xây chuỗi bằng cách gán từng ký tự mà quên `'\0'` cuối, `printf` sẽ in tiếp cả rác phía sau cho đến khi tình cờ gặp byte 0.
- `strlen("Hello")` = 5 (không tính `'\0'`) nhưng `sizeof("Hello")` = 6.

</details>

### Duyệt chuỗi

```c
int len = 0;
while (s[len] != '\0')
    len++;
printf("Do dai: %d\n", len);
```

Chạy từng bước với `"Hi"`: `len=0`, `s[0]='H'` ≠ `'\0'` → `len=1`; `s[1]='i'` → `len=2`; `s[2]='\0'` → dừng, độ dài = 2.

---

## 2. Con Trỏ và Chuỗi

### 2.1 Duyệt chuỗi bằng con trỏ

```c
char s[] = "Hello";
char *p = s;

while (*p != '\0') {
    printf("%c ", *p);
    p++;
}
```

Viết gọn: `while (*p)` vì `'\0'` có giá trị 0 (sai).

<details>
<summary>Giải thích kỹ: duyệt bằng con trỏ</summary>

```
s:     'H'  'e'  'l'  'l'  'o'  '\0'
        ^
        p  (bắt đầu)
```

- `*p` là ký tự mà `p` đang trỏ; `p++` dời sang ký tự kế (mỗi `char` 1 byte).
- Vòng lặp lặp đến khi `*p` là `'\0'`.
- Sau vòng lặp `p` trỏ vào `'\0'`; `p - s` chính là độ dài chuỗi.
- Nếu cần giữ `s` nguyên thì luôn dùng con trỏ phụ `p`, vì địa chỉ đầu chuỗi mất là không tìm lại được.

</details>

### 2.2 Mảng char và con trỏ chuỗi hằng

```c
char a[] = "Hello";    // mảng – SỬA được:  a[0] = 'J';
char *b  = "Hello";    // trỏ tới chuỗi hằng – KHÔNG được sửa
```

> `b[0] = 'J';` gây crash vì chuỗi hằng nằm ở vùng nhớ chỉ đọc. Muốn sửa thì dùng mảng.

<details>
<summary>Giải thích kỹ: vì sao <code>char a[]</code> sửa được còn <code>char *b</code> thì không?</summary>

- `char a[] = "Hello";` → tạo mảng 6 ô và **sao chép** chữ "Hello" vào. Mảng nằm trên stack, là bản của riêng bạn nên sửa thoải mái.
- `char *b = "Hello";` → chỉ tạo con trỏ `b` trỏ tới chuỗi "Hello" được cuốn sẵn trong chương trình (vùng chỉ đọc). Nhiều dòng có cùng chữ "Hello" có thể dùng chung một vùng nên không cho sửa.
- Con trỏ `b` có thể đổi hướng (`b = a;`) nhưng mảng `a` thì không gán lại được (`a = b;` lỗi).
- Cách an toàn: nếu chỉ cần đọc, khai báo `const char *b = "Hello";` để trình biên dịch cảnh báo khi lỡ sửa.

</details>

### 2.3 Hàm nhận chuỗi

```c
int doDai(char *s) {          // tương đương char s[]
    int n = 0;
    while (*s++)
        n++;
    return n;
}
```

<details>
<summary>Giải thích `while (*s++)`</summary>

`*s++` đọc ký tự tại `s` rồi mới di chuyển `s` sang ký tự tiếp theo. Vòng lặp dừng khi gặp `'\0'` (giá trị 0). Mỗi vòng `n` tăng 1 nên cuối cùng là độ dài chuỗi.

</details>

---

## 3. Nhập Xuất Chuỗi

| Cách | Đặc điểm |
|---|---|
| `scanf("%s", s)` | Đọc **một từ**, dừng ở khoảng trắng |
| `scanf(" %[^\n]", s)` | Đọc cả dòng (đã dùng ở các buổi trước) |
| `fgets(s, sizeof(s), stdin)` | Đọc cả dòng, **an toàn** không tràn bộ nhớ, giữ lại `'\n'` |
| `puts(s)` / `printf("%s\n", s)` | In chuỗi |

```c
char ten[50];
printf("Nhap ho ten: ");
fgets(ten, sizeof(ten), stdin);
ten[strcspn(ten, "\n")] = '\0';     // xoá ký tự '\n' ở cuối
printf("Xin chao %s\n", ten);
```

> Nếu trước `fgets` có `scanf("%d", ...)`, ký tự `'\n'` còn sót lại sẽ làm `fgets` đọc ra chuỗi rỗng. Thêm `getchar();` sau `scanf` để bỏ nó đi.

<details>
<summary>Giải thích kỹ: <code>fgets</code> và <code>strcspn</code></summary>

- `fgets(s, sizeof(s), stdin)`: đọc **tối đa** `sizeof(s) - 1` ký tự (còn chỗ cho `'\0'`) hoặc đến hết dòng. Nhờ có giới hạn nên không bao giờ tràn mảng – an toàn hơn `scanf("%s")`.
- Nó **giữ lại** ký tự xuống dòng `'\n'` ngay trước `'\0'`, nên khi in sẽ thấy xuống dòng thừa.
- `strcspn(s, "\n")` trả về vị trí ký tự `'\n'` đầu tiên trong `s` (hoặc độ dài chuỗi nếu không có). Gán `'\0'` vào vị trí đó là cắt mất `'\n'`.
- Vì sao `scanf("%d")` rồi `fgets` bị lỗi? `scanf` chỉ đọc số, phím Enter (`'\n'`) vẫn nằm trong bộ đệm. `fgets` gặp ngay `'\n'` → đọc ra chuỗi rỗng. `getchar();` đọc và bỏ `'\n'` đó.

</details>

---

## 4. Thư Viện string.h

```c
#include <string.h>
```

| Hàm | Công dụng |
|---|---|
| `strlen(s)` | Độ dài chuỗi |
| `strcpy(dst, src)` | Sao chép |
| `strncpy(dst, src, n)` | Sao chép tối đa n ký tự |
| `strcat(dst, src)` | Nối `src` vào cuối `dst` |
| `strcmp(a, b)` | So sánh: `0` nếu bằng, `<0` nếu a < b, `>0` nếu a > b |
| `strchr(s, c)` | Tìm ký tự `c` đầu tiên, trả về con trỏ hoặc `NULL` |
| `strstr(s, sub)` | Tìm chuỗi con |

```c
char s[50] = "Ky thuat lap trinh";
if (strstr(s, "lap") != NULL)
    printf("Co chua 'lap'\n");
```

> Không dùng `==` để so sánh chuỗi (`s1 == s2` so sánh **địa chỉ**). Phải dùng `strcmp`.

<details>
<summary>Giải thích kỹ: <code>strcmp</code> trả về gì?</summary>

`strcmp` so sánh từng ký tự theo mã ASCII từ trái sang phải, dừng ở cặp khác nhau đầu tiên.

| Kết quả | Ý nghĩa | Ví dụ |
|---|---|---|
| `0` | Hai chuỗi giống hệt | `"abc"` và `"abc"` |
| `< 0` | Chuỗi 1 đứng trước theo thứ tự từ điển | `"abc"` và `"abd"` |
| `> 0` | Chuỗi 1 đứng sau | `"b"` và `"a"` |

Chữ hoa đứng trước chữ thường (`'A'`=65 < `'a'`=97), nên `strcmp("Z", "a") < 0`. Muốn không phân biệt hoa/thường thì đổi cả hai về cùng dạng trước.

`strcpy` và `strcat` **không kiểm tra** đích có đủ chỗ hay không: bạn phải tự đảm bảo mảng đích đủ dài (`strlen(src) + 1` ô cho `strcpy`).

</details>

### Tự viết lại hàm – strcpy

```c
void saoChep(char *dst, char *src) {
    while (*src) {
        *dst = *src;
        dst++;
        src++;
    }
    *dst = '\0';       // nhớ thêm ký tự kết thúc
}
```

---

## 5. Các Bài Toán Chuỗi Kinh Điển

### 5.1 Đảo ngược chuỗi

```c
void daoChuoi(char *s) {
    int n = strlen(s);
    for (int i = 0; i < n / 2; i++) {
        char tam = s[i];
        s[i] = s[n - 1 - i];
        s[n - 1 - i] = tam;
    }
}
```

### 5.2 Kiểm tra chuỗi đối xứng (palindrome)

```c
int doiXung(char *s) {
    int i = 0, j = strlen(s) - 1;
    while (i < j) {
        if (s[i] != s[j]) return 0;
        i++;
        j--;
    }
    return 1;
}
```

### 5.3 Đếm nguyên âm

```c
int demNguyenAm(char *s) {
    int dem = 0;
    for (; *s; s++) {
        char c = *s | 32;                 // đổi về chữ thường
        if (c=='a' || c=='e' || c=='i' || c=='o' || c=='u')
            dem++;
    }
    return dem;
}
```

### 5.4 Đếm số từ

Một từ bắt đầu khi gặp ký tự khác khoảng trắng mà ký tự đứng trước là khoảng trắng.

```c
int demTu(char *s) {
    int dem = 0, trongTu = 0;
    for (; *s; s++) {
        if (*s != ' ' && !trongTu) {
            dem++;
            trongTu = 1;
        } else if (*s == ' ') {
            trongTu = 0;
        }
    }
    return dem;
}
```

### 5.5 Đổi chữ hoa / thường

Bảng mã ASCII: `'a'` = 97, `'A'` = 65, chênh nhau 32.

```c
for (int i = 0; s[i]; i++)
    if (s[i] >= 'a' && s[i] <= 'z')
        s[i] -= 32;       // thành chữ hoa
```

Hoặc dùng `toupper`, `tolower` trong `<ctype.h>`.

<details>
<summary>Giải thích kỹ: các bài toán chuỗi</summary>

- **Đảo chuỗi**: chỉ cần đổi chỗ `n/2` cặp (đầu ↔ cuối). Nếu lặp đến `n` sẽ đảo hai lần và chuỗi trở về như cũ. Cặp của `i` là `n - 1 - i` (vì chỉ số cuối là `n - 1`).
- **Đối xứng**: hai chỉ số `i` (trái), `j` (phải) tiến vào giữa; chỉ cần gặp một cặp khác nhau là kết luận không đối xứng; nếu gặp nhau mà chưa lệch thì đối xứng.
- **Đếm nguyên âm**: `c | 32` là mẹo đổi chữ hoa thành thường (chữ hoa và thường chỉ khác bit thứ 6, giá trị 32). Cách dễ hiểu hơn: `tolower(c)` trong `<ctype.h>`.
- **Đếm từ**: dùng cờ `trongTu`. Gặp ký tự khác khoảng trắng mà cờ đang tắt → bắt đầu từ mới (`dem++`, bật cờ). Gặp khoảng trắng → tắt cờ. Cách này đếm đúng kể cả khi có nhiều khoảng trắng liền nhau.
- **Đổi hoa/thường**: chỉ đổi ký tự nằm trong `'a'..'z'`, tránh động vào số, dấu câu.

</details>

---

## 6. Tóm Tắt Nhanh

```
Chuỗi          ->  char s[] ; kết thúc '\0'
Duyệt          ->  while (*p) { ...; p++; }
Nhập dòng      ->  fgets(s, sizeof(s), stdin)
So sánh        ->  strcmp(a, b) == 0     (không dùng ==)
Sao chép       ->  strcpy(dst, src)
Nhớ            ->  luôn chừa 1 ô cho '\0'
```

---

## Bài Tập Buổi 8

### Bài 1 – Dễ
Nhập chuỗi. In ra độ dài (tự đếm, không dùng `strlen`) và in từng ký tự trên một dòng bằng con trỏ.

### Bài 2 – Đảo chuỗi
Viết hàm đảo ngược chuỗi bằng 2 con trỏ (một đầu, một cuối). Nhập chuỗi, in kết quả.

### Bài 3 – Đối xứng
Kiểm tra chuỗi có phải palindrome không (bỏ qua chữ hoa/thường). Ví dụ `Level` → đối xứng.

### Bài 4 – Thống kê chuỗi
Nhập một câu. In ra: số ký tự, số từ, số nguyên âm, số chữ số.

### Bài 5 – Chuẩn hoá tên
Nhập họ tên bất kỳ (có thể thừa khoảng trắng, sai hoa/thường), ví dụ `"  nGuYEN   van  an "`. Xuất ra `"Nguyen Van An"`.


### Bài 6 – Khá
Nhập `n` tên (mỗi tên một dòng). Sắp xếp theo thứ tự bảng chữ cái bằng `strcmp`, rồi in ra. Mảng 2 chiều `char ds[100][50]`.

### Bài 7 – Tự viết hàm
Tự cài đặt `strlen`, `strcpy`, `strcat`, `strcmp` bằng con trỏ (không dùng thư viện), kiểm tra bằng `main`.

---

## Lỗi Thường Gặp

| Lỗi | Nguyên nhân | Cách sửa |
|---|---|---|
| In ra ký tự rác phía sau | Quên `'\0'` khi tự tạo chuỗi | Gán `*dst = '\0'` ở cuối |
| `if (s1 == s2)` luôn sai | So sánh địa chỉ | Dùng `strcmp(s1, s2) == 0` |
| Crash khi sửa chuỗi | Sửa chuỗi hằng `char *s = "..."` | Dùng mảng `char s[] = "..."` |
| `fgets` bỏ qua nhập | Còn `'\n'` sau `scanf` | Thêm `getchar();` |
| Tràn mảng | `strcpy`/`strcat` vào mảng quá nhỏ | Khai báo đủ lớn, chừa chỗ `'\0'` |
| Chuỗi có `'\n'` thừa | `fgets` giữ lại `'\n'` | `s[strcspn(s, "\n")] = '\0'` |

---

Buổi tiếp theo: Buổi 9 – Luyện tập buổi 7 + 8
