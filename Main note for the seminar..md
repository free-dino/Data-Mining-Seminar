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

**Với trường hợp $k < 1 / \ld$**
"Vấn đề sẽ trở nên đáng quan tâm nếu ở trong tình trạng không gian bộ nhớ bị giới hạn, khi mà kích cỡ bộ nhớ dự trữ $k$ nhỏ hơn $1/\ld$"
Mẫu của exponential bias từ một luồng với độ dài vô hạn, độ dài của nó sẽ ko vượt quá $1/\ld$ 

Thuật toán:
Bắt đầu: một bộ nhớ dự trữ trống.
Chúng ta sẽ dùng policy này để lấp đầy bộ nhớ dự trữ:

## 2.2 Các cấu trúc tóm gọn cho các miền lớn
## 2.2.1 Tổng quan
## 2.2.2 Các cấu trúc dữ liệu
### 2.2.2.1 Bộ lọc Bloom
### 2.2.2.2 Count-min sketch
### 2.2.2.3 Ước lượng các mô-men và Thuật toán AMS
### 2.2.2.4 Đếm các phần tử duy nhất và thuật toán Flajolet-Martin
"\[flaʒɔlɛ\]" 
#### 2.2.2.5 Đếm số lượng 1 trong 1 cửa sổ và thuật toán DGIM
### 2.2.2.6 Của sổ suy giảm
## 2.3 So sánh
