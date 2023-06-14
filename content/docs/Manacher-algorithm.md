---
title: "Manacher's Algorithm"
date: 2023-06-12T17:27:53+07:00
author: "Ngo Van Trung"
tags: ["math", "algorithm"]
categories: ["knowledge"]
math: true
draft: false
cover:
  image: "images/example.jpg"
  alt: "Cover"
  caption: "Cover"
  relative: false # To use relative path for cover image, used in hugo Page-bundles---
---

Mình mới gặp và tìm hiểu thuật toán này mới đây thôi 🐳, khi đang làm 1 bài trên [leetcode](https://leetcode.com/problems/palindromic-substrings/description/) 😃

# Thế nào là 1 Palindrome ?

Mình hiểu `palindrome` là 1 chuỗi ký tự đối xứng. Ví dụ:

![example](../../images/example.jpg)

Chuỗi "cabadabac" là 1 `palindrome`. Vì chuỗi này đối xứng qua ký tự 'd' ở vị trí 4 (tính từ 0, theo cách index trong javascript). Hay chuỗi "acbaabca" cũng là 1 `palindrome`, chuỗi này đối xứng qua vị trí giữa 2 ký tự 'a'.

Để dễ hình dung hơn, các bạn có thể tưởng tưởng 1 `palindrome` được tạo ra bằng cách phản chiếu 1 chuỗi ký tự qua "gương".

Vậy `sub-palindromes` (còn gọi là `palindrome substrings`) là các chuỗi con của một chuỗi ban đầu và là `palindrome`.

Theo mình tìm hiểu thì `sub-palindromes` là một khái niệm quan trọng trong xử lý chuỗi và có ứng dụng trong nhiều bài toán, bao gồm tìm kiếm từ, mã hóa, nén dữ liệu và xử lý ngôn ngữ tự nhiên. Trong bài viết này mình sẽ nói đến 1 bài toán khá hay. Go go 🐳

# Bài toán: Tìm tất cả các sub-palindromes trong thời gian $O(n)$

### Bài toán:

> Cho chuỗi  $s$  với độ dài  $n$ . Tìm tất cả các cặp  $(i, j)$  sao cho chuỗi con  $s[i\dots j]$  là một chuỗi đối xứng. Chuỗi  $t$  là một chuỗi đối xứng khi  $t = t_{rev}$  ( $t_{rev}$  là một chuỗi đảo ngược của  $t$ ).

### Phân tích:

- Chuỗi đối xứng ở đây chính là `palindrome` mình đã đề cập ở trên.

- Trong trường hợp xấu nhất, chuỗi ban đầu có thể có tới  $O(n^2)$  chuỗi con đối xứng. Lúc đầu mình nhìn qua thì nghĩ là không có thuật toán tuyến tính nào cho bài toán này. Tức là sẽ không có bài viết này 👀

- Phân tích kỹ hơn 🔎: Các palindrome sẽ có độ dài lẻ và chẵn khác nhau, và được tính riêng là  $d_{odd}[i]$  và  $d_{even}[i]$ . Đối với các palindrome có độ dài chẵn, chúng ta giả định rằng chúng có tâm ở vị trí  $i$  nếu hai ký tự trung tâm của chúng là  $s[i]$  và  $s[i-1]$ .

  Ví dụ, chuỗi  $s = abababc$  có 3 palindromes có độ dài lẻ với tâm ở vị trí  $s[3] = b$ , tức là $d_{odd}[3] = 3$ :

  ![manacher odd](../../images/odd.jpg)

  Và chuỗi  $s = cbaabd$  có 2 palindromes có độ dài bằng nhau với tâm ở vị trí  $s[3] = a$ , tức là $d_{even}[3] = 2$ :

  ![manacher even](../../images/even.jpg)

Thực tế, vấn đề này đã có nhiều giải pháp: với String Hashing, nó có thể được giải quyết trong  $O(n\cdot \log n)$ , và với Suffix Trees và Fast LCA , vấn đề này có thể được giải quyết trong $O(n)$ . Các bạn có thể tự tìm hiểu.

Nhưng thuật toán mình nói trong đến trong bài này đơn giản hơn và có ít hằng số ẩn hơn về độ phức tạp của bộ nhớ và thời gian. Thuật toán này được phát hiện bởi **Glenn K. Manacher** vào năm 1975.

Về **ý tưởng** cốt lõi là tính toán số lượng palindrome hiện tại dựa trên thông tin palindrome được tính toán trước đó. Nghe giống DP nhỉ 👀

# Code thôi 💻

### Suy nghĩ chút 🤔

Để xử lý vấn đề palindrome có độ dài lẻ và chẵn khác nhau, thay vì tính toán cho cả 2 trường hợp. Chúng ta sẽ biến đổi chuỗi ban đầu 1 chút. Cụ thể, mình sẽ thêm ký tự `#` vào đầu, cuối và giữa các ký tự của chuỗi ban đầu. Trông nó sẽ như vậy:

![example](../../images/modify.jpg)

Sau khi modify chuỗi, chúng ta sẽ luôn thu được 1 chuỗi mới có độ dài lẻ. Và sẽ không có trường hợp palindrome có độ dài chẵn tạo bởi các ký tự trong chuỗi ban đầu.

Bởi vì giữa chúng luôn có ký tự **'#'**. Do đó chúng ta đã bỏ qua được vấn đề phải tính toán số lượng các palindromes có độ dài chẵn.

Vậy giờ mình chỉ cần tìm số lượng các palindrome lẻ và lưu lại. 👀 Nghe dễ nhỉ

### Thuật toán 📝

1. Khởi tạo một biến đếm tên `res` để theo dõi số lượng xâu đối xứng.
2. Tạo một chuỗi được sửa đổi `newS` bằng cách chèn ký tự `#` giữa mỗi ký tự của chuỗi đầu vào s.
3. Lặp lại từng chỉ số $i$ của chuỗi được sửa đổi `newS`.
4. Đối với mỗi chỉ số $i$, khởi tạo các biến `l` và `r` bằng 1, và `count` bằng 0.
5. Thực hiện vòng lặp while khi các ký tự tại các chỉ số $i - l$ và $i + r$ hợp lệ và bằng nhau. Vòng lặp này mở rộng xâu đối xứng được tập trung tại chỉ số i và đếm số lượng xâu đối xứng.
6. Bên trong vòng lặp, tăng `count` lên 1 và cập nhật `l` và `r` bằng cách tăng chúng lên 1.
7. Sau khi vòng lặp while kết thúc, thêm `Math.ceil(count / 2)` vào `res`. Điều này là vì mỗi chuỗi đối xứng hợp lệ được tính hai lần (một lần cho độ dài lẻ và một lần cho độ dài chẵn), vì vậy chúng ta chia `count` cho 2 và lấy giá trị làm tròn của nó.
8. Lặp lại các bước 4-7 cho tất cả các chỉ số $i$ của chuỗi được sửa đổi `newS`.
9. Cuối cùng, trả về giá trị của `res`, đại diện cho tổng số lượng xâu đối xứng trong chuỗi ban đầu s.

### Code ⌨️

Mình dùng **Javascript** để code bài này:

```javascript
var SubPalindromes = function (s) {
  let res = 0;
  // Tạo chuỗi newS được sửa đổi từ chuỗi s
  let newS = "#";
  for (let i = 0; i < s.length; i++) {
    newS += s[i] + "#";
  }
  // Lặp qua các vị trí trong chuỗi newS
  let n = newS.length,
    l,
    r,
    count;
  for (let i = 0; i < n; i++) {
    count = 0;
    l = 1;
    r = 1;
    // Kiểm tra hợp lệ và kiểm tra tính đối xứng của chuỗi con
    while (i - l >= 0 && i + r < n && newS[i - l] == newS[i + r]) {
      count++;
      l++;
      r++;
    }
    res += Math.ceil(count / 2);
  }
  return res;
};
```

$$
\overbrace{
s_{l+1}\ \ldots\
\underbrace{
s_{j-d_{\mathrm{odd}}[j]+1}\ \ldots\ s_j\ \ldots\ s_{j+d_{\mathrm{odd}}[j]-1}\
}\text{palindrome}\
\ldots\
\underbrace{
s{i-d_{\mathrm{odd}}[j]+1}\ \ldots\ s_i\ \ldots\ s_{i+d_{\mathrm{odd}}[j]-1}\
}\text{palindrome}\
\ldots\ s{r-1}\
}^\text{palindrome}\
\ldots
$$
