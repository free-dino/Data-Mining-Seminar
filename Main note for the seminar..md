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
- -> Luồng dữ liệu.
- ### 1.1.1 Ví dụ thực tế về luồng dữ liệu:
	- Dữ liệu từ cảm cảm biến.
	- Dữ liệu ảnh, video.
	- Internet và Web Traffic
- ### 1.1.2 Định nghĩa:
	 - Luồng dữ liệu là dữ liệu liên tục đc sinh ra bởi nhiều nguồn khác nhau
- ### 1.1.3 Một vài tính chất của luồng dữ liệu
	- Liên tục và ko giới hạn.
	- Khối lượng lớn
	- Nhạy cảm với thời gian
	- Động
	- Ko đầy đủ.
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
# Phần 2: Lấy mẫu dữ liệu:
"Như mình đã nói, thì đặc điểm của luồng dữ liệu là nó có một khối lượng rất lớn, chúng ta ko thể thu toàn bộ một luồng để trích xuất thông tin đc, chúng ta cần 1 một giải pháp tốt hơn, đó là lấy mẫu."

"Câu hỏi là làm sao để lấy được một mẫu có tính đại diện cho cả luồng?"
## 2.1: Ví dụ.
### Bài toán: 
Giả sử chúng ta có một search engine, ví dụ như là Google, nhận một luồng dữ liệu rất lớn là các truy vấn vấn có dạng : <User, Query, Time>
Trong đó: 
- User: Tên người dùng
- Query: Từ mà người đó search
- Time: Thời gian họ nhập truy vấn đó.
Bạn muốn biết: Tỷ lệ lặp lại của các tìm kiếm từ mọi người dùng trong tháng qua là bao nhiêu?

### Mô hình lấy mẫu:
Quyết định: Giữ lại 1/10 bộ dữ liệu một cách ngẫu nhiên.
Cách làm: 
- Gen 1 số bất kì từ 0-9
- Nếu số sinh ra = 0 -> giữ lại, còn lại thì bỏ.
-> Giữ lại 10%.

### Vấn đề nảy sinh: 
* Nếu chúng ta gọi :
	* $\mathbf s$: là số lượng tìm kiếm chỉ tìm kiếm 1 lần
	* $\mathbf d$: là số lượng tìm kiếm mà đc tìm kiếm 2 lần (lặp lại 1 lần).

	$\mathbf s / 10$ -> số truy vấn 1 lần đc lưu trữ lưu trữ 1 bản
	$\mathbf d /100$ -> số truy vấn 2 lần đc lưu trữ cả 2 bản
	$18 \mathbf d/ 100$ -> số truy vấn 2 lần chỉ đc lưu trữ 1 bản

"Vì bài nói rất dài, bọn mình sẽ để link github đến tài liệu và các chứng minh toán học có trong này, còn hiện tại các bạn hãy coi như những biểu thức chúng mình đưa lên đây là đáng tin cậy."

Đáp án đúng sẽ là :
$$
\frac {\mat d}{\mat s+ \mat d}
$$
Kết quả của phương pháp lấy mẫu là:
$$
\frac {\mat d / 100}{\mat s/10 + \mat d/100 + 18 \mat d/100} = \frac {\mat d}{10 \mat s + 19 \mat d}
$$

=> Ko có số dương nào cho $\mat s$ và $\mat d$ thỏa mãn: 
$$
\frac {\mat d}{\mat s + \mat d} = \frac {\mat d}{10\mat s + 19 \mat d}
$$
