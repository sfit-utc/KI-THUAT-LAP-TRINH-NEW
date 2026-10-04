# Buổi 11 – Cấp Phát Động Mảng 1 Chiều và Struct
## Môn: Kỹ Thuật Lập Trình C

---

## Mục tiêu buổi học

Sau buổi này sinh viên có thể:
- Cấp phát động một biến struct và mảng struct
- Truy cập trường qua con trỏ bằng `->`
- Viết hàm xử lý mảng struct động (nhập, xuất, tìm kiếm, sắp xếp)
- Thêm / xoá phần tử của mảng struct động bằng `realloc`
- Cấp phát động trường chuỗi bên trong struct
- Ghi/đọc mảng struct động từ tệp

---

## 1. Ôn lại – Struct và con trỏ struct

```c
typedef struct {
    char ten[50];
    int tuoi;
    float diem;
} SinhVien;

SinhVien sv = {"An", 20, 8.5f};
SinhVien *p = &sv;

printf("%s %d %.1f\n", p->ten, p->tuoi, p->diem);
```

`p->ten` ≡ `(*p).ten`.

<details>
<summary>Giải thích kỹ: vì sao có hai cách viết `.` và `->`?</summary>

- `sv` là **một struct thật** → truy cập trường bằng dấu chấm: `sv.diem`.
- `p` là **con trỏ trỏ tới struct** → muốn vào trường phải "đi theo địa chỉ" trước, tức giải tham chiếu `*p`, rồi mới dùng `.`: `(*p).diem`.
- Dấu `.` có độ ưu tiên **cao hơn** `*`, nên `*p.diem` bị hiểu thành `*(p.diem)` → sai. Bắt buộc phải có ngoặc `(*p).diem`.
- Để khỏi viết ngoặc, C có toán tử rút gọn `p->diem`. Hai cách hoàn toàn tương đương.

Quy tắc nhớ nhanh:

| Đang cầm | Dùng | Ví dụ |
|---|---|---|
| Struct | `.` | `sv.diem` |
| Con trỏ tới struct | `->` | `p->diem` |
| Phần tử của mảng struct `ds[i]` | `.` (vì `ds[i]` đã là struct) | `ds[i].diem` |

</details>

<details>
<summary>Giải thích kỹ: kích thước của struct</summary>

`sizeof(SinhVien)` là tổng kích thước các trường (có thể cộng thêm vài byte đệm do căn lề). Ví dụ ở trên khoảng `50 + 4 + 4 = 58` byte, thường được làm tròn lên 60.

Đừng tự cộng tay – **luôn dùng `sizeof(SinhVien)`** khi cấp phát để chương trình đúng trên mọi máy.

</details>

---

## 2. Cấp phát động một struct

```c
SinhVien *sv = (SinhVien*)malloc(sizeof(SinhVien));
if (sv == NULL) return 1;

printf("Ten : "); scanf(" %[^\n]", sv->ten);
printf("Tuoi: "); scanf("%d", &sv->tuoi);
printf("Diem: "); scanf("%f", &sv->diem);

printf("%s - %d tuoi - %.1f diem\n", sv->ten, sv->tuoi, sv->diem);
free(sv);
```

<details>
<summary>Giải thích từng dòng</summary>

1. `malloc(sizeof(SinhVien))` xin đúng một "hộp" đủ chứa **1 struct** trên heap. `(SinhVien*)` ép kiểu để con trỏ biết nó trỏ tới struct nào.
2. `if (sv == NULL)` – nếu hệ thống hết bộ nhớ thì `malloc` trả `NULL`; dùng tiếp sẽ crash nên phải kiểm tra.
3. `scanf(" %[^\n]", sv->ten)`:
   - dấu cách đầu bỏ qua ký tự xuống dòng còn sót lại;
   - `%[^\n]` đọc **mọi ký tự cho đến khi gặp xuống dòng** (cho phép tên có dấu cách);
   - `sv->ten` là mảng `char` nên **không cần `&`**.
4. `&sv->tuoi`: dấu `->` có độ ưu tiên cao hơn `&`, nên đây là `&(sv->tuoi)` = "địa chỉ của trường tuoi". Cần `&` vì `tuoi` là `int`.
5. `free(sv)` trả lại hộp nhớ. Sau lệnh này không được đụng vào `sv` nữa.

Vì sao dùng cấp phát động cho **một** struct? Với một biến thì thường không cần; kỹ thuật này quan trọng khi struct được tạo ra trong một hàm rồi trả về cho nơi khác dùng (xem mục sau).

</details>

### Hàm tạo struct trên heap

```c
SinhVien* taoSV(char *ten, int tuoi, float diem) {
    SinhVien *sv = (SinhVien*)malloc(sizeof(SinhVien));
    if (sv == NULL) return NULL;
    strcpy(sv->ten, ten);
    sv->tuoi = tuoi;
    sv->diem = diem;
    return sv;
}
```

Biến cục bộ sẽ mất khi hàm kết thúc, còn vùng nhớ trên heap thì **vẫn tồn tại** → hàm trả về địa chỉ hợp lệ. Người gọi chịu trách nhiệm `free`.

---

## 3. Mảng struct động

```c
int n;
printf("So sinh vien: ");
scanf("%d", &n);

SinhVien *ds = (SinhVien*)malloc(n * sizeof(SinhVien));
if (ds == NULL) {
    printf("Khong du bo nho!\n");
    return 1;
}
```

<details>
<summary>Giải thích kỹ: bộ nhớ của mảng struct trông thế nào?</summary>

`malloc(n * sizeof(SinhVien))` xin **một khối liên tục** gồm `n` struct nằm kề nhau:

```
ds ──► [ ds[0] ][ ds[1] ][ ds[2] ] ... [ ds[n-1] ]
        ten,tuoi,diem  ...
```

- `ds[i]` là struct thứ `i` → trường của nó là `ds[i].ten`, `ds[i].diem` (dùng dấu chấm).
- `&ds[i]` là **địa chỉ** của struct thứ `i` → kiểu `SinhVien*`, dùng khi muốn hàm sửa trực tiếp struct đó.
- `ds + i` cũng là địa chỉ của struct thứ `i` (con trỏ tự nhảy đúng `sizeof(SinhVien)` byte mỗi bước).

</details>

### 3.1 Hàm nhập / xuất

```c
void nhapSV(SinhVien *sv) {
    printf("Ten : "); scanf(" %[^\n]", sv->ten);
    printf("Tuoi: "); scanf("%d", &sv->tuoi);
    printf("Diem: "); scanf("%f", &sv->diem);
}

void xuatSV(SinhVien sv) {
    printf("%-20s %3d %5.1f\n", sv.ten, sv.tuoi, sv.diem);
}

void nhapDS(SinhVien *ds, int n) {
    for (int i = 0; i < n; i++) {
        printf("--- Sinh vien %d ---\n", i + 1);
        nhapSV(&ds[i]);
    }
}

void xuatDS(SinhVien *ds, int n) {
    for (int i = 0; i < n; i++)
        xuatSV(ds[i]);
}
```

<details>
<summary>Giải thích kỹ: vì sao <code>nhapSV</code> nhận con trỏ còn <code>xuatSV</code> nhận giá trị?</summary>

- **Nhập** phải *sửa* struct gốc. Truyền theo giá trị chỉ sửa bản sao rồi bản sao mất → struct gốc không đổi. Vì vậy truyền địa chỉ (`SinhVien *sv`) và gọi bằng `nhapSV(&ds[i])`.
- **Xuất** chỉ đọc nên truyền giá trị cũng được. Tuy nhiên truyền giá trị nghĩa là **sao chép cả struct** (khoảng 60 byte) mỗi lần gọi. Với struct lớn nên truyền con trỏ cho nhẹ:
  `void xuatSV(const SinhVien *sv)` và gọi `xuatSV(&ds[i])`. Từ khoá `const` cam kết hàm không sửa dữ liệu.

</details>

<details>
<summary>Giải thích kỹ: <code>%-20s %3d %5.1f</code></summary>

- `%-20s`: chuỗi chiếm đúng 20 ký tự, dấu `-` nghĩa là canh **trái** (thiếu thì thêm khoảng trắng bên phải).
- `%3d`: số nguyên rộng 3 ký tự, canh phải.
- `%5.1f`: số thực rộng 5 ký tự trong đó 1 chữ số sau dấu phẩy.

Nhờ vậy các hàng của bảng in ra thẳng cột.

</details>

### 3.2 Tìm kiếm – hàm trả về con trỏ tới phần tử

```c
SinhVien* timMax(SinhVien *ds, int n) {
    SinhVien *max = &ds[0];
    for (int i = 1; i < n; i++)
        if (ds[i].diem > max->diem)
            max = &ds[i];
    return max;
}

// dùng:
SinhVien *gioiNhat = timMax(ds, n);
printf("Cao nhat: %s - %.1f\n", gioiNhat->ten, gioiNhat->diem);
```

<details>
<summary>Giải thích kỹ</summary>

- Biến `max` **không lưu điểm** mà lưu *địa chỉ của sinh viên đang dẫn đầu*. So sánh `ds[i].diem > max->diem` là so điểm của sinh viên thứ `i` với điểm của người đang dẫn đầu.
- Khi tìm thấy người tốt hơn, chỉ cần gán lại địa chỉ: `max = &ds[i]` – không phải sao chép cả struct.
- Hàm trả về con trỏ **trỏ vào mảng `ds`** (vẫn còn sống sau khi hàm kết thúc), nên hợp lệ. Nếu trả về địa chỉ của biến cục bộ trong hàm thì sai.
- Lưu ý: nếu sau đó `free(ds)` hoặc `realloc(ds, ...)`, con trỏ `gioiNhat` sẽ **treo** (không còn hợp lệ). Hãy dùng xong trước khi thay đổi mảng.

</details>

### 3.3 Sắp xếp

```c
void sapXepGiam(SinhVien *ds, int n) {
    for (int i = 0; i < n - 1; i++)
        for (int j = i + 1; j < n; j++)
            if (ds[i].diem < ds[j].diem) {
                SinhVien tam = ds[i];
                ds[i] = ds[j];
                ds[j] = tam;
            }
}
```

<details>
<summary>Giải thích kỹ</summary>

- Thuật toán: với mỗi vị trí `i`, so với mọi phần tử đứng sau (`j > i`); nếu phần tử sau có điểm cao hơn thì đổi chỗ. Sau vòng `j`, `ds[i]` là phần tử lớn nhất trong đoạn còn lại.
- Ta đổi chỗ **cả struct** chứ không đổi từng trường. Phép gán `ds[i] = ds[j]` giữ nguyên toàn bộ tên, tuổi, điểm của sinh viên – nếu chỉ đổi điểm thì tên và điểm sẽ bị lệch nhau.
- Struct gán được bằng `=` (C sao chép từng byte). Ngoại lệ cần chú ý: nếu struct chứa **con trỏ** (mục 5) thì phép gán chỉ sao chép địa chỉ chứ không sao chép vùng nhớ mà con trỏ trỏ tới.
- Đổi chiều sắp xếp: đổi dấu `<` thành `>`.
- Muốn sắp theo tên: dùng `strcmp(ds[i].ten, ds[j].ten) > 0`.

</details>

---

## 4. Thêm / Xoá Phần Tử

### 4.1 Thêm vào cuối danh sách

```c
void themSV(SinhVien **ds, int *n, SinhVien moi) {
    SinhVien *tam = (SinhVien*)realloc(*ds, (*n + 1) * sizeof(SinhVien));
    if (tam == NULL) return;
    *ds = tam;
    (*ds)[*n] = moi;
    (*n)++;
}
// gọi: themSV(&ds, &n, svMoi);
```

<details>
<summary>Giải thích kỹ: từng dòng và vì sao phải truyền <code>&ds</code>, <code>&n</code></summary>

Hàm này cần **thay đổi hai biến của `main`**: số phần tử `n` và con trỏ `ds`.

- `n` là `int` → muốn sửa phải truyền `int *n` (địa chỉ của `n`), trong hàm dùng `*n`.
- `realloc` có thể **chuyển cả khối sang địa chỉ khác** nếu không đủ chỗ mở rộng tại chỗ. Khi đó con trỏ `ds` bên ngoài phải được cập nhật → phải truyền địa chỉ của con trỏ. Địa chỉ của `SinhVien*` có kiểu `SinhVien**`. Trong hàm, `*ds` là con trỏ gốc.

Từng dòng:
1. `realloc(*ds, (*n + 1) * sizeof(SinhVien))` – xin khối mới to hơn 1 phần tử, nội dung cũ được giữ nguyên.
2. `if (tam == NULL) return;` – thất bại thì mảng cũ vẫn còn nguyên (đó là lý do dùng biến `tam`).
3. `*ds = tam;` – cập nhật con trỏ gốc.
4. `(*ds)[*n] = moi;` – đặt phần tử mới vào vị trí cuối (chỉ số `n` vì đang có `n` phần tử `0..n-1`). Ngoặc `(*ds)` bắt buộc vì `[]` ưu tiên hơn `*`.
5. `(*n)++;` – tăng số phần tử. Cũng cần ngoặc, vì `*n++` sẽ tăng con trỏ chứ không tăng giá trị.

</details>

### 4.2 Xoá tại vị trí k

```c
void xoaSV(SinhVien *ds, int *n, int k) {
    for (int i = k; i < *n - 1; i++)
        ds[i] = ds[i + 1];
    (*n)--;
}
```

<details>
<summary>Giải thích kỹ</summary>

Xoá phần tử `k` bằng cách **dồn các phần tử phía sau lên một ô**:

```
trước:  [A][B][C][D][E]   xoá k = 1 (B)
dồn:    [A][C][D][E][E]   (ds[1]=ds[2], ds[2]=ds[3], ds[3]=ds[4])
n--:    [A][C][D][E]      ô cuối bị "bỏ quên" vì n giảm 1
```

- Vòng lặp chạy đến `i < *n - 1` vì `ds[i + 1]` phải còn nằm trong mảng.
- Hàm chỉ cần `int *n` (đổi số phần tử), không cần `**` vì không `realloc`, địa chỉ mảng không đổi.
- Nếu muốn trả bớt bộ nhớ: sau khi giảm `n` có thể `realloc(ds, n * sizeof(SinhVien))`, nhưng khi đó lại phải truyền `SinhVien **ds` như hàm thêm. Với bài nhỏ thường bỏ qua bước này.
- Cần kiểm tra `0 <= k < *n` trước khi xoá để tránh truy cập ngoài mảng.

</details>

---

## 5. Cấp Phát Động Trường Chuỗi Trong Struct

Thay vì `char ten[50]` cố định (luôn tốn 50 byte, tên dài hơn thì tràn), dùng `char *ten` và xin **vừa đủ** độ dài.

```c
typedef struct {
    char *ten;
    float diem;
} HocSinh;

HocSinh hs;
char buf[200];
printf("Nhap ten: ");
scanf(" %[^\n]", buf);

hs.ten = (char*)malloc(strlen(buf) + 1);   // +1 cho '\0'
strcpy(hs.ten, buf);

// ... dùng ...

free(hs.ten);     // phải free trường trước
```

<details>
<summary>Giải thích kỹ</summary>

- `char *ten` chỉ là **một con trỏ** (8 byte), ban đầu chứa giá trị rác – chưa trỏ vào đâu. Phải `malloc` cho nó trước khi ghi.
- Ta không biết tên dài bao nhiêu trước khi nhập, nên đọc tạm vào mảng đệm `buf[200]`, rồi đo `strlen(buf)` và xin đúng `strlen + 1` byte (ô cuối cho `'\0'`). Quên `+ 1` là lỗi rất phổ biến, gây ghi tràn bộ nhớ.
- `strcpy(hs.ten, buf)` sao chép nội dung. **Không viết `hs.ten = buf`**: đó chỉ gán địa chỉ, khi `buf` đổi thì `hs.ten` đổi theo, và `free` sẽ sai.
- Thứ tự giải phóng: struct chứa con trỏ → **free các vùng mà con trỏ trỏ tới trước**, rồi mới free struct/mảng chứa chúng. Nếu free mảng trước, bạn mất địa chỉ các vùng tên → rò rỉ bộ nhớ.

```c
// mảng HocSinh động:
for (int i = 0; i < n; i++)
    free(ds[i].ten);
free(ds);
```

- Cẩn thận khi **gán struct** `a = b`: chỉ sao chép con trỏ `ten`, hai struct cùng trỏ một vùng; `free` cả hai sẽ bị lỗi *double free*. Muốn sao chép thật phải `malloc` vùng mới và `strcpy`.

</details>

---

## 6. Ghi/Đọc Mảng Struct Từ Tệp

```c
// Ghi
FILE *f = fopen("sv.txt", "w");
if (f == NULL) return 1;
fprintf(f, "%d\n", n);
for (int i = 0; i < n; i++)
    fprintf(f, "%s|%d|%.1f\n", ds[i].ten, ds[i].tuoi, ds[i].diem);
fclose(f);

// Đọc
f = fopen("sv.txt", "r");
if (f == NULL) return 1;
fscanf(f, "%d\n", &n);
ds = (SinhVien*)malloc(n * sizeof(SinhVien));
for (int i = 0; i < n; i++)
    fscanf(f, " %[^|]|%d|%f", ds[i].ten, &ds[i].tuoi, &ds[i].diem);
fclose(f);
```

<details>
<summary>Giải thích kỹ</summary>

Nội dung tệp `sv.txt` sẽ như sau:

```
2
Nguyen Van An|20|8.5
Tran Thi Binh|21|9.0
```

- Dòng đầu ghi **số lượng `n`** để lúc đọc biết cần `malloc` bao nhiêu phần tử – đây là mẹo quan trọng khi lưu mảng động.
- Dùng dấu `|` ngăn cách các trường vì tên có thể chứa dấu cách (dấu cách không phân tách được).
- `%[^|]` đọc mọi ký tự cho đến khi gặp `|`; ký tự `|` tiếp theo trong định dạng đọc và bỏ qua dấu phân cách đó.
- Dấu cách đầu `" %[^|]"` để bỏ qua ký tự xuống dòng ở cuối dòng trước.
- Sau mỗi `fopen` luôn kiểm tra `NULL`; sau khi dùng xong luôn `fclose`.
- Mở `"w"` sẽ **xoá nội dung cũ**; muốn ghi thêm vào cuối dùng `"a"`.

</details>

---

## 7. Tóm Tắt Nhanh

```
Một struct     ->  SinhVien *p = malloc(sizeof(SinhVien));  p->diem
Mảng struct    ->  SinhVien *ds = malloc(n * sizeof(SinhVien)); ds[i].diem
Sửa từ hàm     ->  truyền con trỏ (SinhVien *sv)
Thêm phần tử   ->  realloc, truyền SinhVien **ds và int *n
Xoá phần tử    ->  dồn các phần tử sau lên, n--
Trường chuỗi   ->  malloc(strlen(s)+1) ; free trước khi free struct
```

---

## Bài Tập Buổi 11

### Bài 1 – Dễ
Định nghĩa struct `SanPham` (ten, gia, soLuong). Cấp phát động một sản phẩm, nhập, in thành tiền, `free`.

### Bài 2 – Mảng struct động
Nhập `n` sinh viên (tên, điểm toán, điểm lý). Dùng mảng động. In ra danh sách kèm điểm trung bình từng người và người có điểm TB cao nhất.

### Bài 3 – Sắp xếp
Với danh sách bài 2, viết hàm sắp xếp giảm dần theo điểm TB và in bảng thẳng hàng.

### Bài 4 – Tìm kiếm
Nhập tên cần tìm (dùng `strcmp`), in thông tin nếu có; nếu không in "Khong tim thay".

### Bài 5 – Thêm / Xoá
Viết menu: 1) Thêm sinh viên 2) Xoá theo vị trí 3) In danh sách 0) Thoát. Dùng `realloc` khi thêm.

### Bài 6 – Khó
Mở rộng bài 5: thêm chức năng lưu danh sách ra tệp và nạp lại từ tệp khi chạy chương trình. Giải phóng toàn bộ bộ nhớ khi thoát.

---

## Lỗi Thường Gặp

| Lỗi | Nguyên nhân | Cách sửa |
|---|---|---|
| Dùng `.` thay `->` (hoặc ngược lại) | Nhầm struct và con trỏ struct | `->` cho con trỏ, `.` cho struct |
| `malloc(n)` thiếu byte | Quên `sizeof` | `n * sizeof(SinhVien)` |
| Mất dữ liệu sau `realloc` | Gán trực tiếp, không kiểm tra NULL | Dùng biến tạm |
| `themSV` không có tác dụng | Truyền `ds`, `n` theo giá trị | Truyền `&ds`, `&n` |
| Rò rỉ bộ nhớ | Quên `free` trường chuỗi | Free trường rồi mới free struct |
| Tên bị cắt | Dùng `%s` với tên có khoảng trắng | Dùng `" %[^\n]"` |
| Ghi tràn khi cấp phát chuỗi | Quên `+ 1` cho `'\0'` | `malloc(strlen(s) + 1)` |
| Con trỏ treo sau `realloc` | Giữ con trỏ cũ tới phần tử | Lấy lại địa chỉ sau khi `realloc` |

---

Buổi tiếp theo: Buổi 12 – Cấp phát động – Mảng 2 chiều
