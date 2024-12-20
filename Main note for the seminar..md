# Mục tiêu:
Cung cấp kiến thức khái quát về:
- Luồng dữ liệu
- Các cấu trúc dữ liệu hiệu quả
- Các thuật toán khai phá mẫu (pattern)
- Các thuật toán phân cụm
- Các thuật toán phát hiện điểm ngoại lai
- Phân lớp trong luồng dữ liệu

# Phần 1: Giới thiệu về luồng dữ liệu
## 1.1 Tổng quan:
- Các hoạt động hàng ngày và các công nghệ mới phát triển dẫn đến nhu cầu thu thập thông tin. 
- Lượng dữ liệu này sẽ tích lũy liên tục qua một thời gian ngắn.
- Nếu ko thu thập hoặc xử lý ngay sẽ mất đi.
- Vì lượng dữ liệu tích lũy rất nhanh nên sẽ ko thể nào thu thập theo kiểu truyền thống
- ### 1.1.1 Ví dụ thực tế về luồng dữ liệu:
	- Giao dịch: 
	- Web click
	- Mạng xã hội
	- Mạng truyền thông

	- Một số nguồn luồng dữ liệu: .....
- ### 1.1.2 Định nghĩa:
	 - Luồng dữ liệu là các dòng dữ liệu ko giới hạn, liên tục được sinh ra bởi nhiều nguồn. **Không có điểm đầu và điểm cuối**.
- ### 1.1.3 Một vài tính chất của luồng dữ liệu
	- Liên tục và ko giới hạn.
	- Thời gian thực
	- Khối lượng lớn và vận tốc lớn
	- Bất biến
- ### 1.1.4 Giới thiệu về hệ thống quản lý luồng dữ liệu (DSMS):
	![[Pasted image 20241118013304.png]]
## 1.2 Các thách thức chính trong xử lý luồng dữ liệu:
- Luồng dữ liệu thường đc xử lý dưới một số ràng buộc nhất định:
	- Ràng buộc 1 lần duy nhất: *Dữ liệu đến rất nhanh và liên tục -> chỉ có thể xử lý 1 lần duy nhất.*
	- Trôi dạt khái niệm: *Dữ liệu có thể thay đổi theo thời gian*
	- Ràng buộc về tài nguyên: *Người dùng ko có quyền đối với tốc độ và tần suất đến của dữ liệu*
	- Ràng buộc phạm vi rộng lớn: *khi các đặc trưng của dữ liệu là rời rạc, chúng có thể là những số rất lớn hoặc rất đặc trưng.*
- Yêu cầu phân tích thời gian thực
- Giới hạn bộ nhớ và tính toán
# Phần 2: Các cấu trúc dữ liệu tóm gọn cho luồng dữ liệu
Có hai loại cấu trúc dữ liệu tóm gọn:
- Cấu trúc chung: đc dùng cho mọi trường hợp một cách trực tiếp.
- Các cấu trúc cụ thể: Dùng cho các trường hợp cụ thể như là đếm tần suất, đếm số lượng
## 2.1 Lấy mẫu dự trữ (Reservoir Sampling)
**Phương pháp**:
Với một một mẫu dự trữ size $k$, $k$ điểm dữ liệu đầu tiên của luồng sẽ đc dùng để khởi tạo quá trình dự trữ, 2 điều sau đây sẽ đc áp dụng:
- Chèn điểm dữ liệu thứ $n$ vào mẫu dự trữ với xác suất $k/n$ 
- Nếu được chèn vào thì ngẫu nhiên loại bỏ một trong $k$ điểm trong mẫu dự trữ, nhường chỗ cho điểm mới.

![[Pasted image 20241119201827.png]]

### 2.1.1 Xử lý chuyển dịch khái niệm:
**Bias function**: 
Với $f(r,n)$ là một hàm bias cho điểm dữ liệu thứ $r$ tại thời điểm đến của điểm dữ liệu thứ $n$, một mẫu biased $\mathcal S(n)$ tại thời điểm đến của điểm dữ liệu thứ $n$ trong luồng được định nghĩa là một mẫu sao cho xác suất tương đối $p(r,n)$ của điểm thứ $r$ thuộc mẫu $S(n)$ (có cỡ n) tỉ lệ thuận với $f(r, n)$
Hàm exponential bias: 
$$
f(r,n) = e^{\ld(n-r)}
$$
- $\ld$ là tỉ lệ bias và thường nằm trong khoảng $[0,1]$, $\ld=0$ nghĩa là unbiased và ngược lại
- Hàm exponential bias là một hàm ko bộ nhớ

**Chúng ta chỉ xét trường hợp $k < 1 / \ld$**:
Mẫu của exponential bias từ một luồng với độ dài vô hạn, độ dài của nó sẽ ko vượt quá $1/\ld$ 

Thuật toán:
Bắt đầu: một bộ nhớ dự trữ trống.
Chúng ta sẽ dùng chính sách (policy) này để lấp đầy bộ nhớ dự trữ:
- 1. Khi điểm thứ $n+1$ xuất hiện:
	- Chèn điểm đó vào bộ nhớ dự trữ với xác suất $\ld \cdot k$ 
	- Tung một đồng xu với xác xuất thành công: $F(n) \in \{0,1\}$ ~ tỉ lệ bộ nhớ đã bị lấp đầy.
		- Nếu thành công: Chọn ngẫu nhiên một điểm trong bộ nhớ và thay thế với điểm mới
		- Nếu thất bại: Thêm điểm mới vào bộ nhớ và ko cần xóa
* 2. 1. Bộ nhớ đc lấp đầy nhanh lúc ban đầu và chậm lại khi gần đạt dung lượng tối đa.
### 2.1.2 Các giới hạn có ích cho việc lấy mẫu

- Gọi $X$ là một biến ngẫu nhiên với một phân phối xác suất: $f_X(x)$, kì vọng $\E[X]$, và phương sai $\text{Var}[X]$, chúng ta có các bất đẳng thức sau.
	- Bất đẳng thức Markov: Nếu $X$ là một biến ngẫu nhiên chỉ nhận các giá trị không âm, thì với bất kì hằng số $\a$ thỏa mãn $\E[X]<\a$, thì điều sau đây luôn đúng: $$P(X>\a) \le \frac { \E [X]}{\a} \iff \E[X] \ge \a P(X>\a)$$
	- Bất đẳng thức Chebychev: Nếu $X$ là một biến ngẫu nhiên bất kì, thì, với mọi hằng số $\a$, thì điều sau đây luôn đúng: $$P(|X-\E[X]> \a |) \le \frac {\text{Var}[X]}{\a^2}$$
	- Cận Chernoff cho đuôi dưới: Nếu $X$ là một biến ngẫu nhiên có thể biểu diễn như là tổng của $n$ biến ngẫu nhiên độc lập nhị phân (Bernoulli), mỗi biến có giá trị bằng $1$ với xác suất $p_i$: $$X = \sum_{i=1}^n X_i$$, thì với bất kì $\delta \in (0,1)$, chúng ta có: $$P(X < (1-\delta)\E[X]) < e^{-\E[X]\delta^2/2}$$
	- Cận Chernoff cho đuôi trên: Nếu $X$ là một biến ngẫu nhiên có thể biểu diễn như là tổng của $n$ biến ngẫu nhiên độc lập nhị phân (Bernoulli), mỗi biến có giá trị bằng $1$ với xác suất $p_i$: $$X = \sum_{i=1}^n X_i$$, Thì, với mọi $\delta \in (0, 2e-1)$, chúng ta có: $$P(X>(1-\delta)\E[X])<e^{-\E[X]\delta^2/4}$$
	- Bất đẳng thức Hoeffding: Nếu $X$ là một biến ngẫu nhiên có thể biểu diễn như là tổng của $n$ biến ngẫu nhiên độc lập, mỗi biến bị giới hạn trong khoảng $[l_i, u_i]$: $$X = \sum_{i=1}^n X_i$$, Thì, với mọi $\theta > 0$, ta có: $$\begin{align} P(X-\E[X] > \theta) &\le e^{-\frac {2\theta^2}{\sum_{i=1}^n (u_i - l_i)^2}} \\  P(\E[X]-X> \theta) & \le e^{-\frac {2\theta^2}{\sum_{i=1}^n (u_i - l_i)^2}} \end{align}$$

|           | Trường hợp                                   | Sức mạnh |
| --------- | -------------------------------------------- | -------- |
| Chebychev | Bất kì biến ngẫu nhiên nào                   | Yếu      |
| Markov    | Biến ngẫu nhiên không âm                     | Yếu      |
| Hoeffding | Tổng các biến độc lập ngẫu nhiên bị giới hạn | Mạnh     |
| Chernoff  | Tổng các biến độc lập Bernoulli ngẫu nhiên   | Mạnh     |

## 2.2 Các cấu trúc tóm gọn cho các miền lớn
## 2.2.1 Tổng quan
Hạn chế:
- Cặp định danh: Dữ liệu luồng thường bao gồm các cặp giá trị định danh, dẫn đến miền giá trị có số tổ hợp khổng lồ
- Giới hạn không gian: Với các miền lớn, việc lưu trữ ngay cả những thống kê đơn giản như đếm số lượng hoặc kiểm tra thành viên tập hợp là không khả thi do yêu cầu lưu trữ khổng lồ.
- Hạn chế của cá kĩ thuật đơn giản: Các kỹ thuật như mảng hoặc lấy mẫu dự trữ không hiệu quả trong các trường hợp này:
	- Mảng: Yêu cầu kích thước quá lớn
	- Lấy mẫu dự trữ ko phù hợp với việc đếm các phần tử khác biệt hoặc kiểm tra các thành viên của tập hợp
- Các cấu trúc chuyên biệt: Không có cấu trúc tóm gọn nào phù hợp cho tất cả các truy vấn.
## 2.2.2 Các cấu trúc dữ liệu
### 2.2.2.1 Bộ lọc Bloom
Một bộ lọc Bloom $\mathcal B$ bao gồm:
- 1. Một mảng gồm $m$ bits, khởi tạo tất cả đều bằng $0$.
- 2. Một tập hợp các hàm băm $h_1, h_2, \cdots, h_w$. Mỗi hàm băm ánh xạ giá trị "khóa" vào $m$ ô chứa, tương ứng với $m$ bits của mảng trên.
- 3. Một tập $\mathcal S$ với $n$ khóa.
Quy trình lọc: 
- Khởi tạo mảng toàn bộ thành $0$
- Với mỗi $x \in \mathcal S$: $h_i(x) \leftarrow 1$ 
![[Pasted image 20241120130201.png]]
Kiểm tra xem một khóa ở trong bộ lọc hay chưa:
- Dùng hàm băm để tìm vị trí đánh dấu
- Nếu tất cả vị trí = 1, thì có lẽ (chưa chắc) phần tử đã xuất hiện hay chưa
- Nếu có ít nhất 1 vị trí bằng 0, chắc chắn phần tử chưa từng xuất hiện
Bổ đề: Nếu một phần tử $x$ chưa từng xuất hiện trong tập $\mathcal S$, thì xác suất $F$ để phần tử $x$ đó bị coi là dương tính giả được cho bởi công thức: $$F = \Bigg[1 - \Bigg(1 - \frac 1 m\Bigg)^{w-n} \Bigg]^w$$
**Lợi ích**: Nhanh và tiết kiệm bộ nhớ
**Hạn chế**: Dương tính giả và ko thể xóa phần tử

Ứng dụng:
- Lọc phần tử trùng lặp
- Kiểm tra spam email
- Kiểm tra URL trong danh sách cấm

### Mô men
**Mô men tần suất**:
Với $f_i$ là tần suất xuất hiện của phần tử $i$ trong luồng, ta có:
$$
F_k = \sum_{i=1}^{n} f_i^k 
$$
$F_0$: Số các phần tử đặc trưng khác 0 của luồng
$F_1$: Tổng số lần xuất hiện của các phần tử trong luồng
$F_2$: Tổng bình phương tần suất, hữu ích cho việc ước tính độ xiên của dữ liệu
...
$F_\infty$ : $\max_i f_i$ 
### 2.2.2.2 Count-min sketch
"Count-Min Sketch là một cấu trúc dữ liệu xác suất dùng để ước lượng tần suất xuất hiện của các phần tử trong luồng dữ liệu một cách hiệu quả với bộ nhớ hạn chế."
- Ước lượng $F_1$ và $F_\infty$ 
![[Pasted image 20241120133430.png]]

Count-min Sketch bao gồm: 
- $w$ mảng số nguyên cỡ $m$ -> Không gian nhớ $O(mw)$
- Mảng thứ $i$ tương ứng với một hàm băm thứ $i$ $h_i(\cdot)$ 
- Các hàm băm $h_1(⋅),…,h_w(⋅)$ hoàn toàn độc lập với nhau, nhưng chỉ độc lập từng cặp đối với các đối số khác nhau. Nói cách khác, với bất kỳ hai giá trị $x_1$ và $x_2​$, $h_i(x_1)$ và $h_i(x_2)$ là độc lập.

Quy trình cập nhật Count-min Sketch:
- Tất cả các ô trong sketch được khởi tạo bằng $0$
- Tại hàm băm $i$, chúng ta sẽ cập nhật $(i, h_i(x)) \leftarrow (i, h_i(x)) + 1$ 

Ước lượng tần suất xuất hiện với Count-min Sketch:
- Đưa input $x$ vào các hàm băm, nhận vị trí trong từng hàm
- Lấy giá trị nhỏ nhất
Ưu điểm: Tiết kiệm bộ nhớ, dễ sử dụng
Nhược điểm: Ước lượng sai lệch, không chính xác
Ứng dụng: Ước lượng tần suất xuất hiện của phần tử
### 2.2.2.3 AMS Sketch
**Alon-Matias-Szegedy (AMS) sketch**
Thuật toán: 
- 1. Dùng hàm băm ngẫu nhiên: Hàm băm $h(i)$ ánh xạ phần tử thứ $i$ vào $\{-1, 1\}$  một cách ngẫu nhiên đồng nhất
- 2. Cập nhật luồng: Duy trì một bộ đếm $Z$, khởi tạo bằng $0$
	- Khi $a_i$ xuất hiện trong luồng, cập nhật $Z$: $$Z = \sum_i h(a_i)$$
- 3. Ước lượng $F_2$: $$F_2 \approx Z^2$$
### 2.2.2.4 Đếm các phần tử duy nhất và thuật toán Flajolet-Martin
Ước lượng: $F_0$
Thuật toán:
- 1. Chọn một hàm băm $h(x)$ ánh xạ phần tử vào khoảng số nguyên lớn: $[0, 2^L -1]$
- 2. Theo dõi bit 1 có trọng số thấp nhất:
	- Với mỗi giá trị của hàm băm, tìm vị trí bên phải nhất của bit 1.
	- Theo dõi điểm $R_\max$ là vị trí max của các vị trí vừa tìm được.
	- Tuy nhiên có thể ước lượng $R_\max$: $$\E[R_\max]=\log_2 (\phi n)$$, với $\phi = 0.77351$.
- 3. Ước lượng số lượng các phần tử duy nhất: $$D = \frac{2^{R_\max}}{\phi}$$
Ưu điểm: Không gian lưu trữ nhỏ, hiệu quả cho dữ liệu lớn
Nhược điểm: Ước lượng sai lệch, không thể truy vấn các phần tử
### 2.2.2.5 Đếm số lượng 1 trong 1 cửa sổ và thuật toán DGIM
"Thuật toán DGIM là một thuật toán rất hiệu quả trong việc ước tính số lượng 1 trong một cửa sổ của luồng nhị phân, để đếm chính xác, chúng ta cần rất nhiều tài nguyên"

Thuật toán DGIM: show cái ảnh
"
Một cửa sổ cỡ N
- 1. Gom nhóm trong luồng
	- Mỗi nhóm sẽ thể hiện một chuỗi 1 liên tục, mỗi nhóm có:
		- Một kích cỡ: số lượng số 1 nó biểu diễn
		- Một mốc thời gian: Thời điểm số 1 gần nhất xuất hiện
	- Mỗi nhóm không chồng lên nhau và mỗi nhóm liên tục.
- 2. Mỗi nhóm tuân theo các luật sau:
	- Nhóm tận cùng bên phải luôn luôn là 1 
	- Kích cỡ của mỗi nhóm sẽ là $2^k$
	- Mỗi kích cỡ 
	- Trên 1 cửa sổ, luôn luôn có tối đa 2 nhóm cỡ $2^k$ bất kì
- 3. Hợp các nhóm:
	- Khi có một bit 1 đến, tạo 1 nhóm cỡ 1
	- Nếu phạm phải luật, hai nhóm cũ nhất sẽ hợp lại thành 1 nhóm lớn hơn
	- Chuyển mốc thời gian về nhóm gần nhất.
- 4. Ước lượng số lượng bit 1: 
	- Để ước lượng số lượng bit 1 trong N bit cuối:
		- Cộng kích thước của tất cả các nhóm có dấu thời gian trong khoảng cửa sổ
		- Với các nhóm cũ nhất mà đè lên cửa sổ, chúng ta dùng phần đè lên cửa sổ 
"

![[Pasted image 20241120214312.png]]

# Phần 3. Khai phá mẫu thường xuyên trong luồng dữ liệu

## 3.1 Tận dụng Các cấu trúc tóm gọn
### 3.1.1 Lấy mẫu dự trữ:
- Duy trì một mẫu động từ luồng dữ liệu
- **Độ linh hoạt**: Cho phép sử dụng hầu hết mọi thuật toán khai thác mẫu phổ biến trên mẫu được lưu trong bộ nhớ.
- **Xử lý trôi dạt khái niệm:** Lấy mẫu hồ chứa có trọng số suy giảm theo thời gian có thể được sử dụng để ưu tiên dữ liệu mới và thích nghi với trôi dạt khái niệm.
- **Hạn chế:** Có thể không phù hợp cho mọi ứng dụng, chẳng hạn như các ứng dụng yêu cầu đếm chính xác hoặc có số lượng mục riêng biệt rất lớn.
### 3.1.2 Sketches
- **Count-Min Sketch:** Có thể được sử dụng để ước tính tần suất của các mục và nhận diện các mục xuất hiện nhiều trong luồng dữ liệu.
- **AMS Sketch:** Cũng hiệu quả trong việc nhận diện heavy hitters và cung cấp các giới hạn khác so với count-min sketch.
- **Hạn chế:** Phác thảo thường hiệu quả hơn trong việc ước tính tần suất của các mục phổ biến. Tuy nhiên, chúng không dễ dàng áp dụng cho việc khai thác tập hợp mục phổ biến.

## 3.2 Thuật toán chuyên biệt: Lossy Counting
### 3.2.1 Thuật toán:
#### 3.2.1.1: Phân chia dữ liệu thành các phân đoạn:
- Luồng dữ liệu được chia thành các phân đoạn $S_1, S_2, \dots$, mỗi phân đoạn có kích thước $w = \frac{1}{\epsilon}$.
#### 3.2.1.2 Duy trì tần suất trong một mảng:
- Một mảng được duy trì để lưu tần suất của các mục xuất hiện trong phân đoạn. Khi một mục mới đến:
  - Nếu đã có trong mảng, giá trị đếm của mục đó sẽ tăng lên.
  - Nếu chưa có, mục mới sẽ được thêm vào mảng với tần suất ban đầu là 1.
#### 3.2.1.3 Loại bỏ mục ít phổ biến
- Khi một phân đoạn kết thúc, thuật toán thực hiện:
  - Giảm tần suất của tất cả các mục trong mảng đi 1.
  - Loại bỏ các mục có tần suất bằng 0 khỏi mảng.
### 3.2.2 Điều chỉnh sai số:
- Sau khi xử lý $n$ mục dữ liệu, tổng số phân đoạn $r$ là:
	$r = O\left(\frac{n}{w}\right) = O(n \cdot \epsilon)$

- Để đảm bảo không đánh giá thấp bất kỳ tần suất nào, một lượng $\epsilon \cdot n$ được cộng thêm vào các tần suất cuối cùng.

### 3.2.3 Ưu điểm và hạn chế
#### 3.2.3.1 Ưu điểm:
1. **Hiệu quả về bộ nhớ**: Yêu cầu bộ nhớ $O\left(\frac{1}{\epsilon}\right)$.
2. **Không có âm tính giả (false negatives)**: Bất kỳ mục nào thực sự thường xuyên đều được báo cáo.
#### 3.2.3.2 Hạn chế:
1. **Dương tính giả (false positives)**: Một số mục ít phổ biến có thể bị nhầm lẫn là thường xuyên, tùy thuộc vào giá trị $\epsilon$.
2. **Không có âm tính giả (false negatives)**: Bất kỳ mục nào thực sự thường xuyên đều được báo cáo.
#### 3.2.3.3 Ví dụ thực tế
**Kịch bản**: Một cửa hàng bán lẻ muốn phân tích các sản phẩm thường xuyên được mua cùng nhau.

- Chia luồng giao dịch thành các phân đoạn (mỗi phân đoạn chứa 100 giao dịch).
- Duy trì mảng tần suất cho các tập hợp sản phẩm.
- Loại bỏ các sản phẩm hoặc tập hợp ít phổ biến tại cuối mỗi phân đoạn.

**Kết quả**: Cửa hàng có thể phát hiện nhanh các sản phẩm phổ biến trong một khung thời gian ngắn, chẳng hạn như tuần hoặc tháng gần đây.

#### 3.2.3.4 So sánh với lấy mẫu dự trữ

| **Thuật toán**         | **Ưu điểm**                            | **Hạn chế**                                               |
| ---------------------- | -------------------------------------- | --------------------------------------------------------- |
| **Lossy Counting**     | Tiết kiệm bộ nhớ, không có âm tính giả | Không thích ứng với concept drift                         |
| **Reservoir Sampling** | Thích ứng với concept drift, linh hoạt | Có thể bỏ sót các mục thường xuyên nếu kích thước mẫu nhỏ |

# Phần 4. Phân cụm luồng dữ liệu
## 4.1 Các thuật toán phân cụm chính
### 4.1.1 Thuật toán STREAM
STREAM Algorithm là một phương pháp phân cụm dữ liệu luồng dựa trên kỹ thuật **k-medians**, nhằm giải quyết bài toán phân cụm trong dữ liệu lớn mà bộ nhớ có giới hạn.
#### 4.1 Cách hoạt động của thuật toán STREAM
##### Bước 1: Chia luồng dữ liệu thành các phân đoạn
- Luồng dữ liệu $S$ được chia thành các phân đoạn nhỏ ($S_1, ..., S_r$) với kích thước $m$ điểm dữ liệu mỗi phân đoạn.  
- Giá trị $m$ được xác định dựa trên bộ nhớ sẵn có.
##### Bước 2: Phân cụm từng phân đoạn (k-means clustering)
- Áp dụng thuật toán k-medians lên mỗi phân đoạn $S_i$ để tìm $k$ đại diện tối ưu ($Y_1, ..., Y_k$).  
- Tính toán mục tiêu: Tối thiểu hóa tổng bình phương khoảng cách (**SSQ**) từ các điểm dữ liệu đến trung tâm cụm.
$$\text{Objective}(S, Y) = \sum_{X_i \in S} \text{dist}(X_i, Y_{ji})^2$$
- $X_i$: điểm dữ liệu trong phân đoạn $S_i$.
- $Y_{ji}$: đại diện gần nhất của $X_i$.
##### Bước 3: Lưu trữ và gán trọng số cho các đại diện
- Sau khi xử lý xong phân đoạn đầu tiên $S_1$, lưu trữ $k$ đại diện cùng với trọng số (số lượng điểm gán vào mỗi đại diện).  
- Tiếp tục xử lý phân đoạn tiếp theo $S_2$ độc lập để tìm $k$ đại diện mới.
##### Bước 4: Phân cụm cấp độ cao hơn (Hierarchical Clustering)
Sau khi xử lý $r$ phân đoạn, tổng số đại diện $r \cdot k$ có thể vượt quá giới hạn bộ nhớ. Khi đó:
- Tiến hành phân cụm cấp cao hơn để giảm số lượng đại diện.
- Sử dụng thuật toán k-medians, nhưng có thêm trọng số khi phân cụm.
##### Bước 5: Kết quả cuối cùng
- Khi kết thúc luồng dữ liệu hoặc cần kết quả phân cụm, kết hợp tất cả các đại diện từ các cấp khác nhau và áp dụng k-medians một lần nữa.
##### Đánh giá thuật toán
**Yếu tố ảnh hưởng đến chất lượng**:
- **Thuật toán k-medians** được sử dụng: Chất lượng của phân cụm phụ thuộc vào mức độ tối ưu của thuật toán k-medians áp dụng trong từng phân đoạn.
- **Phân rã vấn đề thành các phân đoạn nhỏ**: Dữ liệu được xử lý theo từng phần, có thể ảnh hưởng đến chất lượng phân cụm cuối cùng.

**Kết quả**:  
STREAM Algorithm đảm bảo rằng chất lượng cuối cùng không tệ hơn $5 \cdot c$, trong đó $c$ là mức xấp xỉ của thuật toán k-medians được sử dụng.

### 4.2 Thuật toán CluStream
**CluStream Algorithm** là một phương pháp phân cụm dữ liệu luồng nhằm xử lý **concept drift** (thay đổi trong phân phối dữ liệu theo thời gian). Nó cung cấp khả năng phân cụm dữ liệu luồng trên nhiều khung thời gian khác nhau, điều mà **STREAM Algorithm** không thực hiện được.
### 4.2.1 Các thành phần chính trong thuật toán CluStream 
#### 4.2.1.1 Microcluster
**Microcluster** là cách tóm tắt thông tin chi tiết của dữ liệu luồng. Mỗi microcluster lưu trữ các thống kê tổng hợp về dữ liệu trong một khoảng thời gian.
##### Định nghĩa:
Một microcluster được biểu diễn bởi một bộ 5 thông tin chính, gọi là (2·d + 3)-tuple:

- **CF2x:** Tổng bình phương các giá trị dữ liệu trên mỗi chiều.
- **CF1x:** Tổng giá trị dữ liệu trên mỗi chiều.
- **CF2t:** Tổng bình phương các dấu thời gian.
- **CF1t:** Tổng các dấu thời gian.
- **n:** Số lượng điểm dữ liệu.
##### Tính chất:
Microcluster có tính cộng gộp:
- Có thể cập nhật trực tuyến bằng cách cộng thêm dữ liệu mới.
- Có thể tính toán lại cho một khoảng thời gian cụ thể bằng phép trừ giữa các microcluster ở hai thời điểm.

### 4.2.2 Cách hoạt động của Thuật toán CluStream
#### Phase 1: Microclustering (trực tuyến)
- Khi dữ liệu luồng đến, các điểm dữ liệu được gán vào các microcluster gần nhất. Các thông tin trong microcluster (CF2x, CF1x, CF2t, CF1t, n) được cập nhật bằng phép cộng.
##### Phase 2: Macroclustering (ngoại tuyến)
- Khi cần phân cụm ở khung thời gian cụ thể, các microcluster được sử dụng để tái phân cụm. Việc tái phân cụm được thực hiện bằng cách áp dụng các thuật toán như k-means lên các microcluster.

### 4.2.3 Ưu điểm và hạn chế
#### Ưu điểm
- Thích nghi với trôi dạt khái niệm
- Phân cụm đa khung thời gian
- Tính toán hiệu quả
#### Hạn chế
- Phụ thuộc vào tham số
- Tốn tài nguyên hơn thuật toán STREAM

## 4.3 So sánh
| **Tiêu chí**           | **CluStream Algorithm**                       | **STREAM Algorithm**           |
| ---------------------- | --------------------------------------------- | ------------------------------ |
| **Trôi dạt khái niệm** | Xử lý tốt, tái phân cụm theo thời gian        | Không xử lý được               |
| **Phân cụm thời gian** | Hỗ trợ khung thời gian khác nhau              | Không hỗ trợ                   |
| **Hiệu quả tính toán** | Tốn tài nguyên hơn                            | Tính toán đơn giản hơn         |
| **Ứng dụng**           | Phân tích nâng cao, theo dõi xu hướng dài hạn | Tổng hợp nhanh, bộ nhớ hạn chế |
# Phần 5. Xác định ngoại lệ trong luồng
Có hai ngoại lệ có thể xuất hiện trong luồng dữ liệu đa chiều:
**Ngoại lệ cá nhân**: là những điểm nổi bật riêng lẻ, thường là tín hiệu ban đầu của sự kiện mới (còn gọi là sự mới lạ).
**Ngoại lệ tổng hợp**: phản ánh thay đổi trong xu hướng tổng thể, có thể xuất hiện từ ngoại lệ cá nhân nhưng không phải lúc nào cũng vậy.

## 5.1 Các điểm dữ liệu riêng như là ngoại lệ

### 5.1.1 Phương pháp khoảng cách: K-NN và LOF
- **Nguyên lý**: Các phương pháp này xác định ngoại lệ dựa trên khoảng cách giữa một điểm dữ liệu và $k$ lang giềng gần nhất của nó trong một cửa sổ trượt

![[Pasted image 20241121035933.png]]


![[Pasted image 20241121040025.png]]
- Thách thức: 
	- Luồng dữ liệu liên tục yêu cầu cập nhật thường xuyên để đảm bảo kết quả phát hiện phù hợp với trạng thái hiện tại của dữ liệu
	- Việc tính toán khoảng cách k-NN trong một dòng dữ liệu động có thể tiêu tốn nhiều tài nguyên tính toán
### 5.1.2 Phương pháp phân cụm: Microclustering
- Microcluster: Nhóm các điểm dữ liệu thành một số lượng bộ tóm tắt nhỏ gọn cố định gọi là microcluster, cho phép xử lý hiệu quả các dòng dữ liệu lớn.
- Xác định ngoại lệ: Các điểm dữ liệu nằm ngoài bán kính thống kê được xác định của các microcluster hiện tại thường được đánh dấu là ngoại lệ.

#### Lợi ích của phân cụm trong phát hiện ngoại lệ trong dòng dữ liệu
- **Hiệu quả:** Các phương pháp phân cụm chỉ yêu cầu duy trì thống kê tóm tắt của các cụm, tránh được gánh nặng tính toán so cặp giữa các điểm dữ liệu cá nhân.
- **Khả năng mở rộng:** Các kỹ thuật microcluster xử lý hiệu quả dòng dữ liệu liên tục, duy trì đại diện kích thước cố định của dữ liệu bất kể độ dài của dòng.
- **Thích nghi với trôi dạt khái niệm:** Bằng cách điều chỉnh ranh giới cụm và tạo các cụm mới, các phương pháp microcluster có thể thích nghi với các xu hướng dữ liệu tiến hóa và xác định các ngoại lệ báo hiệu thay đổi trong hành vi dòng dữ liệu.

-> Microcluster thường phù hợp hơn với các luồng dữ liệu lớn nhờ hiệu quả và khả năng thích nghi với trôi dạt khái niệm.

## 5.2 Các điểm thay đổi tổng hợp như là ngoại lệ.
### 5.2.1 Mật độ vận tốc (velocity density):
Đo lường sự thay đổi của dòng dữ liệu qua các khung thời gian khác nhau.
#### 5.2.1.1 Các tham số chính:
- **Cửa sổ thời gian $(h_t)$:** Xác định khung thời gian mà các thay đổi trong mật độ dữ liệu được đo lường. Giá trị $(h_t)$ lớn nắm bắt các xu hướng dài hạn, trong khi $(h_t)$ nhỏ tập trung vào các biến động ngắn hạn.
- **Tham số làm mịn không gian $(h_s)$:** Tương tự với độ rộng hạt nhân trong ước lượng mật độ hạt nhân truyền thống, tham số này kiểm soát mức độ làm mịn được áp dụng cho phân phối không gian của các điểm dữ liệu.
#### 5.2.1.2 Cách tính mật độ vận tốc:
$$V(h_s, h_t)(X, T) = \frac{F(h_s, h_t)(X, T) - R(h_s, h_t)(X, T - h_t)}{h_t}$$

Với:
- **Ước lượng mật độ lát thời gian xuôi $F(h_s, h_t)(X, t)$:** Biểu thị mật độ tại vị trí $X$ và thời điểm $t$, dựa trên các điểm dữ liệu đã xuất hiện trong cửa sổ thời gian quá khứ $(t - h_t, t)$.
- **Ước lượng mật độ lát thời gian ngược $R(h_s, h_t)(X, t)$:** Biểu thị mật độ tại vị trí $X$ và thời điểm $t$, dựa trên các điểm dữ liệu sẽ xuất hiện trong cửa sổ thời gian tương lai $(t, t + h_t)$.
### 5.2.2 Các Điểm Dữ Liệu Và Thời Điểm Có Sự Thay Đổi Đột Ngột
- **Các điểm dữ liệu ngoại lệ:** Những điểm nằm trong vùng có giá trị tuyệt đối của mật độ vận tốc bất thường lớn, biểu thị các thay đổi đột ngột trong phân phối dữ liệu cục bộ. Những điểm này có thể là các trường hợp đầu tiên của một xu hướng hoặc sự kiện mới đang gây ra sự thay đổi trong phân phối dữ liệu tổng thể.
- **Các thời điểm ngoại lệ:** Những thời điểm có mật độ vận tốc tuyệt đối tổng hợp cao, biểu thị sự thay đổi đáng kể trong phân phối dữ liệu trên toàn bộ không gian dữ liệu. Những thời điểm này có thể tương ứng với các sự kiện quan trọng hoặc sự thay đổi trong quá trình tạo dữ liệu cơ bản.
### 5.2.3 Các Lợi Ích Chính Của Mật Độ Vận Tốc Trong Phát Hiện Ngoại Lệ
- Nhạy bén với các thay đổi tổng hợp
- Linh hoạt trong phân tích khung thời gian
- Đo lường định lượng
- Phát hiện sớm

# Phần 6: Phân lớp trong luồng dữ liệu:
### 1. Very Fast Decision Trees - VFDT

1. **Cây quyết định rất nhanh (VFDT) là gì?**
    - VFDT là một thuật toán xây dựng **cây quyết định** để phân loại dữ liệu từ luồng (streaming data).
    - Do dữ liệu luồng đến liên tục, không thể lưu trữ toàn bộ dữ liệu, VFDT chỉ dựa vào một **mẫu nhỏ** nhưng đảm bảo kết quả gần giống với việc phân tích toàn bộ dữ liệu.
    
2. **Định lý Hoeffding là gì và tại sao quan trọng?**
    - **Định lý Hoeffding** cung cấp một ràng buộc toán học để ước tính rằng một mẫu nhỏ từ một tập dữ liệu lớn đủ để đưa ra các quyết định chính xác với **xác suất cao**.
    - Với VFDT, định lý này được sử dụng để quyết định **khi nào đủ dữ liệu để chia cây**.
3. **Cách VFDT hoạt động:**
    - Khi dữ liệu đến theo luồng:
        1. **Các nút cao hơn của cây** (nơi phân chia ảnh hưởng lớn nhất) được xây dựng trước, khi có đủ dữ liệu để đảm bảo tính chính xác.
        2. **Các nút thấp hơn** (ít quan trọng hơn) được xây dựng sau, khi dữ liệu cho các nhánh con đã đủ.
	    3. **Cây phát triển dần dần (incremental):** Không cần tất cả dữ liệu từ trước, chỉ cần xử lý từng bước với dữ liệu mới đến.
    
4. **Giả định quan trọng:**
    - Luồng dữ liệu không thay đổi (không có **concept drift**). Điều này có nghĩa là dữ liệu hiện tại có thể được coi như đại diện tốt cho toàn bộ luồng dữ liệu trong tương lai.

5. **Cách xác định khi nào đủ dữ liệu để thực hiện split**
	- **Chọn thuộc tính split**:
	    - Thuộc tính được chọn dựa trên các tiêu chí như **Gini Index** (chỉ số đo độ không đồng nhất).
	    - Tại một nút:
        - $G_i$​: Gini Index của thuộc tính tốt nhất i trên toàn bộ dữ liệu.
        - $G'_i​$: Gini Index của thuộc tính tốt nhất i trên mẫu dữ liệu.
        - $G_j$​: Gini Index của thuộc tính tốt thứ hai j trên toàn bộ dữ liệu.
        - $G'_j​:$ Gini Index của thuộc tính tốt thứ hai j trên mẫu.
	- **Vấn đề tiềm ẩn**:
	    - Do sai lệch mẫu, có thể xảy ra trường hợp $G'_i < G'_j$ trên mẫu, nhưng trên toàn bộ dữ liệu, thực tế $G_j < G_i$ dẫn đến chọn sai thuộc tính split.
	- **Vai trò của  Hoeffding**:
	    - Nếu số mẫu n đủ lớn, xác suất xảy ra sai sót trên $(G_j < G_i)$ có thể được kiểm soát với xác suất cao $1 - \delta$
	    - n phụ thuộc vào:
	        - $\epsilon = G'_j - G'_i$: Độ chênh lệch giữa hai giá trị Gini Index trên mẫu.
	        - $\delta$: Xác suất người dùng chấp nhận cho lỗi.

---

 6. Công thức tính giá trị n
$$
    n = \frac{R^2\ln\left(\frac{1}{\delta}\right)}{2 \epsilon^2}

$$
    - δ: Xác suất lỗi mà người dùng chấp nhận.
    - Giá trị của R biểu thị phạm vi của tiêu chí phân chia. Đối với chỉ số Gini, giá trị của R là 1 và đối với tiêu chí entropy, giá trị là log(k), trong đó k là số lớp. Các mối quan hệ gần nhau trong tiêu chí phân chia tương ứng với các giá trị nhỏ của ϵ
- **Ý nghĩa**:
    - Số lượng mẫu n tăng khi:
        - Độ chênh lệch ϵ nhỏ (chất lượng các thuộc tính gần như ngang nhau -> cần phân biệt rõ hơn giữa các thuộc tính).
        - Xác suất lỗi δ nhỏ (yêu cầu độ chính xác cao hơn).
**Hạn chế của Hoeffding Tree**:
    - Khi các thuộc tính có chất lượng gần như ngang nhau, việc phân chia nút bị trì hoãn, dẫn đến thời gian xử lý kéo dài.
    - Một khi quyết định chia đã được thực hiện, nó không thể thay đổi.
- **Cải tiến của VFDT (Very Fast Decision Tree)**:
    - Phá vỡ các trường hợp hòa gần (near ties) một cách tích cực hơn.
    - Tắt các nút lá ít triển vọng.
    - Cải thiện độ chính xác bằng cách:
        1. Loại bỏ các thuộc tính chia kém hiệu quả.
        2. Gộp tính toán trung gian cho nhiều điểm dữ liệu.
- **Hạn chế của VFDT**:
    - Không được thiết kế để xử lý **concept drift** (sự thay đổi trong phân phối dữ liệu).
 7. **CVFDT (Concept-Adaptive VFDT)**
Để giải quyết **concept drift**, CVFDT (Concept-Adaptive VFDT) được phát triển với hai cải tiến chính:

**Cải tiến 1: Cửa sổ trượt (Sliding Window)**
- Sử dụng một **cửa sổ trượt** để giới hạn tác động của dữ liệu lịch sử.
- Khi cửa sổ di chuyển:
    - Thống kê của các điểm dữ liệu mới được thêm vào.
    - Thống kê của các điểm dữ liệu cũ bị loại bỏ.
- **Ý nghĩa**:
    - Các nút không còn thỏa mãn **Hoeffding Bound** sẽ bị thay thế.

**Cải tiến 2: Cây con thay thế (Alternate Subtrees)**
- Mỗi nút trong cây chính được gán một danh sách **cây con thay thế** (alternate subtrees).
- Các cây con được xây dựng đồng thời với cây chính.
- Khi thuộc tính chia tốt nhất thay đổi do **concept drift**, cây con thay thế sẽ được sử dụng để thay thế cây chính.

**Kết quả thực nghiệm**
- CVFDT đạt độ chính xác cao hơn so với VFDT trong các luồng dữ liệu có sự thay đổi khái niệm (**concept-drifting data streams**).
## 2. Supervised Microcluster Approach
Vì hiện tượng **concept drift**, việc điều chỉnh mô hình một cách linh hoạt là rất quan trọng.

- Trong phương pháp phân loại **k-nearest neighbor**, nhãn lớp thống trị (dominant class label) trong số k láng giềng gần nhất sẽ được báo cáo làm kết quả phân loại.
- Tuy nhiên, trong kịch bản xử lý luồng dữ liệu, việc tính toán k láng giềng gần nhất cho mỗi trường hợp kiểm tra là rất khó khăn vì kích thước luồng dữ liệu tăng dần không ngừng.

Để giải quyết vấn đề này:

1. **Microclustering** được sử dụng để tạo một bản tóm tắt dữ liệu có kích thước cố định, không tăng lên khi luồng dữ liệu tiếp diễn.
2. Một biến thể microcluster có giám sát được áp dụng, trong đó các điểm dữ liệu thuộc các lớp khác nhau **không được phép trộn lẫn trong cùng một cụm** (cluster).
3. Microcluster được cập nhật sao cho các điểm dữ liệu chỉ được gán vào các cụm thuộc cùng một nhãn lớp. Do đó, **nhãn lớp được gán cho các microcluster thay vì từng điểm dữ liệu riêng lẻ**.
4. Nhãn lớp thống trị của k-microcluster gần nhất sẽ được báo cáo là nhãn lớp kết quả.

**Tuy nhiên, phương pháp này không tính đến những thay đổi cần thiết đối với thuật toán do hiện tượng trôi dạt khái niệm**. Do sự trôi dạt khái niệm, các xu hướng trong luồng sẽ thay đổi. 
- Để tăng độ chính xác, **chỉ các microcluster trong các khoảng thời gian cụ thể (time horizons)** mới được sử dụng.
- . **Horizon gần nhất** thường có liên quan nhất, nhưng không phải lúc nào cũng đúng nếu luồng dữ liệu bất ngờ quay lại các xu hướng cũ hơn.
-  Để giải quyết vấn đề này:
    - Một phần của luồng dữ liệu huấn luyện được tách ra làm **luồng xác thực (validation stream)**.
    - Các phần gần đây của luồng xác thực được sử dụng để kiểm tra độ chính xác trên các khoảng thời gian khác nhau.
    - Horizon tối ưu (optimal horizon) được chọn dựa trên kết quả kiểm tra này.
- Sau đó, phương pháp k-láng giềng gần nhất được áp dụng trên luồng dữ liệu kiểm tra dựa trên horizon tối ưu đã được xác định.

## 3. Phương pháp Ensemble

Một phương pháp ensemble (tổ hợp) mạnh mẽ đã được đề xuất cho bài toán phân loại luồng dữ liệu (data streams). Phương pháp này cũng được thiết kế để xử lý **concept drift** (sự trôi dạt khái niệm) vì nó có thể tính đến sự tiến hóa hiệu quả trong dữ liệu cơ bản
- **Chia luồng dữ liệu thành các khối (chunks):**  
    Thay vì xử lý toàn bộ luồng dữ liệu cùng lúc, luồng được chia nhỏ thành từng đoạn dữ liệu liên tiếp.
    
- **Huấn luyện nhiều bộ phân loại trên từng khối:**  
    Điểm phân loại cuối cùng được tính như một hàm của các điểm số từ từng khối dữ liệu. Cụ thể, các mô hình phân loại trong tổ hợp, chẳng hạn như **C4.5, RIPPER, Naive Bayesian**, được áp dụng trên các khối dữ liệu liên tiếp trong luồng dữ liệu.
    
- **Tổ hợp các bộ phân loại (Ensemble):**  
    Kết quả cuối cùng được tính dựa trên điểm số từ tất cả các bộ phân loại đã huấn luyện.
    - **Trọng số động:** Các bộ phân loại được gán trọng số dựa trên độ chính xác dự kiến để phù hợp với dữ liệu hiện tại.
    
- **Ưu điểm của Ensemble:**
	- Phương pháp này linh hoạt, vì nó có thể điều chỉnh trọng số để phù hợp với dữ liệu thay đổi.
	- Độ chính xác cao hơn, vì các bộ phân loại kết hợp sẽ giảm thiểu sai số so với chỉ dùng một bộ phân loại duy nhất

## 4. Massive-Domain Streaming Classification
Nhiều ứng dụng xử lý luồng dữ liệu có các thuộc tính rời rạc đa chiều với độ lớn miền rất cao. Trong những trường hợp như vậy, việc sử dụng các bộ phân loại thông thường trở nên khó khăn do giới hạn bộ nhớ. Phương pháp **count-min sketch** có thể được sử dụng để giải quyết những thách thức này.
1. **Count-Min Sketch cho từng lớp:**  
    Mỗi lớp được gắn với một cấu trúc sketch (bản tóm lược), giúp theo dõi các tổ hợp phổ biến (**r-combinations**) trong dữ liệu huấn luyện.
    - Các tổ hợp này được giới hạn bởi giá trị **r ≤ k**, với **k** nhỏ
    -
2. **Thêm các tổ hợp vào sketch:**
    - Mỗi điểm dữ liệu trong luồng sẽ sinh ra các tổ hợp r-combination.
    - Các tổ hợp này được thêm vào sketch tương ứng của lớp đó dưới dạng **pseudo-items** (mục giả).'
    
3. **Phân biệt giữa các lớp dựa trên pseudo-items:**
    - Các lớp khác nhau sẽ có các **pseudo-items** phổ biến khác nhau.
    - Các **pseudo-items** có tính phân biệt cao sẽ được xác định nhằm tạo ra các **quy tắc ngầm** (**implicit rules**) liên kết các mục giả với từng lớp. Các quy tắc này không thực sự được vật chất hóa, mà được lưu trữ ngầm trong các sketch và chỉ được trích xuất khi cần phân loại một điểm kiểm tra.
    
4. **Phân loại dữ liệu kiểm tra (test):**
    - Các mục giả mang tính phân biệt cao sẽ được tìm ra bằng cách trích xuất thống kê từ các sketch cụ thể của từng lớp, phương pháp hoạt động như một **bộ phân loại dựa trên quy tắc** (rule-based classifier).