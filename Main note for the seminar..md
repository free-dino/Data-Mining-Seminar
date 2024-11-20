"Seminar ngày hôm nay của nhóm mình sẽ nói về luồng dữ liệu và xử lý luồng dữ liệu. Mục tiêu của Seminar này sẽ cũng cấp cho các bạn các kiến thức về: Luồng dữ liệu, Các cấu trúc dữ liệu đc dùng để lưu trữ dạng dữ liệu này, các thuật toán khai phá mẫu, các thuật toán phân cụm, phát hiện điểm ngoại lai, phân lớp trong luồng dữ liệu. Vì bài rất dài, nên nếu mọi nguời có câu hỏi thì xin hãy note lại và hỏi sau khi nhóm đã thuyết trình xong".

# Mục tiêu:
Cung cấp kiến thức về:
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
	- Giao dịch
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
	 "Chúng ta có thể hình dung một bộ xử lý dữ liệu luồng như là một hệ thống quản lý dữ liệu.
	 Bất kể luồng dữ liệu nào cũng có thể đc đưa vào hệ thống. Chúng có thể khác nhau về thời gian đến, tần suất đến và kể cả khác nhau về kiểu dữ liệu.
	 Có 2 nơi lưu trữ dữ liệu, nơi đầu tiên là kho lưu trữ lớn, chúng ta assume ko thể truy xuất dữ liệu từ kho chứa này.
	 Có 1 kho hoạt động, nó lưu các bản tóm tắt, hoặc là các phần nhỏ của luồng. Dữ liệu ở kho này đc dùng để trả lời các lệnh truy vấn. Tuy nhiên sức chứa của nó có hạn, ko thể lưu trữ toàn bộ thông tin."
## 1.2 Các thách thức chính trong xử lý luồng dữ liệu:
- Luồng dữ liệu thường đc xử lý dưới một số ràng buộc nhất định:
	- Ràng buộc 1 lần duy nhất: *Dữ liệu đến rất nhanh và liên tục -> chỉ có thể xử lý 1 lần duy nhất.*
	- Trôi dạt khái niệm: *Dữ liệu có thể thay đổi theo thời gian*
	- Ràng buộc về tài nguyên: *Người dùng ko có quyền đối với tốc độ và tần suất đến của dữ liệu*
	- Ràng buộc phạm vi rộng lớn: *khi các đặc trưng của dữ liệu là rời rạc, chúng có thể là những số rất lớn hoặc rất đặc trưng.*
- Yêu cầu phân tích thời gian thực
- Giới hạn bộ nhớ và tính toán

"Chúng ta khó có phần cứng có thể đáp ứng việc thu thập và phân tích luồng dữ liệu. Vì vậy chúng ta sẽ nhìn vào việc lấy mẫu dữ liệu, vì thường sẽ hiệu quả hơn khi ước tính kết quả hơn là tìm kết quả chính xác,"
# Phần 2: Các cấu trúc dữ liệu tóm gọn cho luồng dữ liệu
Có hai loại cấu trúc dữ liệu tóm gọn:
- Cấu trúc chung: đc dùng cho mọi trường hợp một cách trực tiếp.
- Các cấu trúc cụ thể: Dùng cho các trường hợp cụ thể như là đếm tần suất, đếm số lượng
## 2.1 Lấy mẫu dự trữ (Reservoir Sampling)
"Lấy mẫu là một trong những phương pháp linh hoạt nhất cho tóm tắt luồng, và đặc biệt là chúng có thể đc dùng cho nhiều trường hợp khác nhau"

"Trong lấy mẫu dự trữ, một mẫu gồm $k$ điểm sẽ đc duy trì một cách dynamically từ luồng dữ liệu."
"-> Phương pháp lấy mẫu sẽ hoạt động với "kiến thức ko đầy đủ" về lịch sử của luồng tại bất kì thời điêm nào. Nói cách khác, chúng ta phải đưa ra 2 quyết định: 1: Luật lấy mẫu, 2: Luật xóa khỏi mẫu"

**Phương pháp**:
Với một một mẫu dự trữ size $k$, $k$ điểm dữ liệu đầu tiên của luồng sẽ đc dùng để khởi tạo quá trình dự trữ, 2 điều sau đây sẽ đc áp dụng:
- Chèn điểm dữ liệu thứ $n$ vào mẫu dự trữ với xác suất $k/n$ 
- Nếu được chèn vào thì ngẫu nhiên loại bỏ một trong $k$ điểm trong mẫu dự trữ, nhường chỗ cho điểm mới.

![[Pasted image 20241119201827.png]]
**Bổ đề**: 
Sau khi $n$ điểm của luồng dữ liệu đến bộ xử lý, xác suất mà bất kì điểm dữ liệu nào được cho vào mẫu dự trữ là như nhau và bằng $k/n$.

### 2.1.1 Xử lý chuyển dịch khái niệm:
"Trong xử lý luồng dữ liệu, thì dữ liệu mới thường quan trọng hơn dữ liệu cũ vì dữ liệu có sự thay đổi qua thời gian vì chúng có giá trị phân tích cao hơn. Vì vậy bài toán đặt ra là: Làm sao để xác suất đưa các dữ liệu gần đây vào mẫu cao hơn, chúng ta có thể đạt được điều này bằng một hàm bias"

**Bias function**: 
Với $f(r,n)$ là một hàm bias cho điểm dữ liệu thứ $r$ tại thời điểm đến của điểm dữ liệu thứ $n$, một mẫu biased $\mathcal S(n)$ tại thời điểm đến của điểm dữ liệu thứ $n$ trong luồng được định nghĩa là một mẫu sao cho xác suất tương đối $p(r,n)$ của điểm thứ $r$ thuộc mẫu $S(n)$ (có cỡ n) tỉ lệ thuận với $f(r, n)$

"Thông thường thì đây là một vấn đề mở để thực hiện lấy mẫu dự trữ với một hàm bias bất kì, tuy nhiên phương pháp thường đc dùng là hàm exponential bias"
Hàm exponential bias: 
$$
f(r,n) = e^{\ld(n-r)}
$$
- $\ld$ là tỉ lệ bias và thường nằm trong khoảng $[0,1]$, $\ld=0$ nghĩa là unbiased và ngược lại
- Hàm exponential bias là một hàm ko bộ nhớ
"Nghĩa là xác suất đưa 1 điểm bất kì vào trong mẫu ko phụ thuộc vào quá khứ hay thời gian đến".

**Chúng ta chỉ xét trường hợp $k < 1 / \ld$**:
"Vấn đề sẽ trở nên đáng quan tâm nếu ở trong tình trạng không gian bộ nhớ bị giới hạn, khi mà kích cỡ bộ nhớ dự trữ $k$ nhỏ hơn $1/\ld$"
Mẫu của exponential bias từ một luồng với độ dài vô hạn, độ dài của nó sẽ ko vượt quá $1/\ld$ 

Thuật toán:
Bắt đầu: một bộ nhớ dự trữ trống.
Chúng ta sẽ dùng chính sách (policy) này để lấp đầy bộ nhớ dự trữ:
- 1. Khi điểm thứ $n+1$ xuất hiện:
	- Chèn điểm đó vào bộ nhớ dự trữ với xác suất $\ld \cdot k$ 
	- Tung một đồng xu với xác xuất thành công: $F(n) \in [0,1]$ ~ tỉ lệ bộ nhớ đã bị lấp đầy.
		- Nếu thành công: Chọn ngẫu nhiên một điểm trong bộ nhớ và thay thế với điểm mới
		- Nếu thất bại: Thêm điểm mới vào bộ nhớ và ko cần xóa
* 2. 1. Bộ nhớ đc lấp đầy nhanh lúc ban đầu và chậm lại khi gần đạt dung lượng tối đa.
### 2.1.2 Các giới hạn có ích cho việc lấy mẫu
"Cần đánh giá xem các mẫu đã lấy có đủ chất lượng để thực hiện các phân tích tiếp theo hay ko".
"Một trong những ứng dụng của việc lấy mẫu là để ước lượng các giá trị thống kê tổng hợp như tổng, trung bình, trung vị, etc."
"Độ chính xác của những giá trị đó thường đc định lượng bằng các bất đẳng thức đuôi".

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
"Trong xử lý luồng, chúng ta gặp phải một số hạn chế khi xử lý các thuộc tính rời rạc với miền giá trị lớn"
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
"dương tính giả trong trường hợp này là: khi bộ lọc báo phần tử đã ở trong S trong khi nó ko hề có".
"Điểm đặc biệt là bộ lọc Bloom ko có âm tính giả, nghĩa là nếu nó chưa xuất hiện thì chắc chắn nó chưa xuất hiện, nếu "

**Lợi ích**: Nhanh và tiết kiệm bộ nhớ
**Hạn chế**: Dương tính giả và ko thể xóa phần tử

Ứng dụng:
- Lọc phần tử trùng lặp
- Kiểm tra spam email
- Kiểm tra URL trong danh sách cấm
### 2.2.2.2 Count-min sketch
"Count-Min Sketch là một cấu trúc dữ liệu xác suất dùng để ước lượng tần suất xuất hiện của các phần tử trong luồng dữ liệu một cách hiệu quả với bộ nhớ hạn chế."

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
- Lấy giá trị nhỏ nhất.

Ưu điểm: Tiết kiệm bộ nhớ, dễ sử dụng
Nhược điểm: Ước lượng sai lệch, không chính xác

Ứng dụng: Ước lượng tần suất xuất hiện của phần tử
### 2.2.2.3 Ước lượng các mô-men và Thuật toán AMS
**Tổng quan về mô men**:
Giả sử tập vũ trụ thể được sắp thứ tự để ta có thể nói về phần tử thứ $i$ với bất kỳ $i$ nào. Gọi $m_i$ là số lần xuất hiện của phần tử thứ $i$ với bất kỳ $i$ nào, khi đó, mô men bậc $k$ được tính bằng:
$$M = \sum_{i}(m_i)^k$$
Mô men bậc 0: Độ dài của luồng
Mô men bậc 1: Tổng của các phần tử trong luồng, $1/M$ là trung bình phần tử trong luồng.
Mô men bậc 2: kết hợp mô men bậc 1 và bậc 2 -> Phương sai
Mô men bậc 3: Giúp tính độ xiên
Mô men bạc 4: Giúp tính độ nhọn
....

"Các mô men bậc cao hơn sẽ cung cấp các thông tin chi tiết hơn về phân phối phần tử trong luồng, giúp đánh giá mức độ phân tán hay tập trung hay phân tán của giá trị"

**Mô men tần suất bậc 2**:
Với $f_i$ là tần suất xuất hiện của phần tử $i$ trong luồng, ta có:
$$
F_2 = \sum_{i=1}^{n} f_i^2 
$$
-> Đo độ xiên của dữ liệu

**Alon-Matias-Szegedy (AMS) sketch**
Thuật toán: 
- 1. Dùng hàm băm ngẫu nhiên: Hàm băm độc lập 4 chiều $h(i)$ ánh xạ phần tử thứ $i$ vào $\{-1, 1\}$  một cách ngẫu nhiên đồng nhất
- 2. Cập nhật luồng: Duy trì một bộ đếm $Z$, khởi tạo bằng $0$
	- Khi $i$ xuất hiện trong luồng với độ tăng $c$, cập nhật $Z$: $$Z \leftarrow Z + c \cdot h(i)$$
- 3. Ước lượng $F_2$: $$F_2 \approx Z^2$$

Ưu điểm: 
Ước lượng chính xác hơn cho phương thai
Nhược điểm: 
Không thể trả về chính xác, độ phức tạp cao
### 2.2.2.4 Đếm các phần tử duy nhất và thuật toán Flajolet-Martin
"\[flaʒɔlɛ\]" 

Thuật toán:
- 1. Chọn một hàm băm $h(x)$ ánh xạ phần tử vào khoảng số nguyên lớn: $[0, 2^L -1]$
- 2. Theo dõi bit 1 có trọng số thấp nhất


#### 2.2.2.5 Đếm số lượng 1 trong 1 cửa sổ và thuật toán DGIM
### 2.2.2.6 Của sổ suy giảm
## 2.3 So sánh
