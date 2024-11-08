# Data-Mining-Seminar

Đây là repo cho bài tập seminar với chủ đề là: "Mining Streams Data". Những gì chúng ta cần làm là đọc và viết.

# 1. Giới thiệu về cách làm việc với Obsidian.
## 1.1. Tải Obsidian về máy.
Truy cập trang web: [Download - Obsidian](https://obsidian.md/download) để tải Obsidian về máy. Đừng mở vội.
## 1.2. Clone repo này về máy bằng git bash hoặc terminal

Git bash: 

![Git bash](image/Pasted%20image%2020241108143813.png)

Có thể chuột phải trên màn hình, ấn vào Open git bash here. Gõ:
```
git clone https://github.com/free-dino/Data-Mining-Seminar.git
```

Sau đó, mở Obsidian lên, bạn sẽ màn hình này hiện lên:

![Obsidian](image/Pasted%20image%2020241108145346.png)

Ấn vào Open folder as vault, chọn folder vừa clone về và bắt đầu, màn hình làm việc sẽ hiện lên.

Bây giờ thay vì đọc đống README này, bạn nên đọc trong Obsidian để tương tác và hiểu cách dùng hơn.

## 1.3. Viết biểu thức toán học bằng KaTeX.

Về cơ bản KaTeX là bản nhỏ gọn hơn của LaTeX.

Có 2 loại biểu thức toán học, viết cùng dòng và viết xuống dòng. (Ấn vào các phương trình toán học để hiểu rõ hơn):

- **Viết cùng dòng**: bắt đầu bằng cách viết \$$ và bắt đầu viết biểu thức toán học ở giữa, biểu thức sẽ đc viết ngang hàng với chữ viết. Ví dụ : $y = ax+b$ 
- **Viết xuống dòng**: Bắt đầu với \$\$\$$ và cũng bắt đầu viết ở giữa, biểu thức sẽ đc viết xuống dòng và căn giữa, ví dụ $$y = ax + b$$
### 1.3.1 Với các biểu thức nhiều dòng:
- Sử dụng \begin{align} \end{align} và các biểu thức ở trong cặp begin và end. 
- Xuống dòng bằng cách dùng dấu \\\\ 
- Đánh dấu điểm gióng hàng bằng dấu &

Ví dụ:
$$
\begin{align}
A &= 1 + 2 + 3 + 4 + 5 \\
&= (1+2) + (3+4) + 5 \\
& = 3 + 7 +5
\end{align}
$$

