- Xin chào mọi người. Mình là Trần Quốc Sáng, đại diện nhóm 8 thuyết trình về chủ đề khai phá luồng dữ liệu.

- Mục tiêu của ngày hôm nay là giúp các bạn hiểu được những kiến thức bao quát về:
	-  Thế nào là luồng dữ liệu
	- Các cấu trúc dữ liệu hiệu quả cho luồng dữ liệu
	- Các thuật toán khai phá mẫu (pattern)
	- Các thuật toán phân cụm
	- Các thuật toán phát hiện điểm ngoại lai
	- Phân lớp trong luồng dữ liệu

- Bước đến phần 1:

- Ví dụ thực tế: 
	- Giao dịch: Dữ liệu giao dịch thường đc tạo ra bởi hoạt động mua bán của khách hàng. 
	- Web click: Dữ liệu tạo ra mỗi khi khách hàng tương tác với trang web
	- Vệ tinh: Vệ tinh gửi hàng loạt các hình ảnh chụp trái đất về trung tâm

- Định nghĩa:

- Tính chất: 

- Không có giới hạn về số lượng dữ liệu hoặc thời gian kết thúc, do đó hệ thống phải sẵn sàng xử lý trong thời gian dài mà không bị gián đoạn.
- Dữ liệu trong luồng được sinh ra và xử lý gần như **ngay lập tức** sau khi xuất hiện. Điều này phù hợp với các ứng dụng cần ra quyết định nhanh, như phát hiện gian lận, hệ thống cảnh báo, hoặc phân tích hành vi người dùng.
- **Khối lượng lớn**: Dữ liệu thường đến từ nhiều nguồn khác nhau, dẫn đến khối lượng dữ liệu khổng lồ.**Vận tốc lớn**: Dữ liệu được gửi tới với tốc độ cao, đòi hỏi hệ thống phải có khả năng tiếp nhận và xử lý nhanh để tránh bị quá tải.

-  Giới thiệu về ...
-  Chúng ta có thể coi một bộ xử lý luồng là một hệ thống quản lý dữ liệu
- Bất kì lượng luồng dữ liệu nào cũng có thể chảy vào hệ thống. Mỗi luồng sẽ khác nhau về vận tốc, độ lớn. Khác nhau về kiểu dữ liệu. Thời gian dữ liệu đến cũng thường ko đồng đều nhau
- Các luồng được lưu trữ trong kho lớn, nma chúng ta sẽ giả định là ko thể truy vấn dữ liệu từ kho này. Nơi này thường chỉ lưu nhật kí, bản ghi của các luồng, được dùng để xem xét lại nếu có sự cố gì xảy ra.
- Có một kho hoạt động, nơi này sẽ tóm tắt hoặc lưu từng phần của các luồng, và chúng ta cũng có thể truy vấn dữ liệu từ đó. Tuy nhiên bộ nhớ của nó có giới hạn, chúng ta ko thể lưu trữ toàn bộ thông tin có được từ luồng.

- Lấy mẫu dự trữ là một trong những phương pháp linh hoạt nhất để tóm tắt luồng dữ liệu. Có thể áp dụng cho hầu như toàn bộ mọi bài toán về xử luồng.


Chuyển dịch khái niệm:

Thường thì dữ liệu nào sẽ có ích với chúng ta. Dữ liệu cũ hay mới? Mới thường có ích hơn. Bài toán đặt ra là: Làm sao để có thể ưu tiên việc đưa dữ liệu mới vào và bỏ bớt dữ liệu cũ trong hồ dự trữ.

Để tạo được mẫu bias S(n), chúng ta cần tính giá trị bias bằng hàm f(r,n) cho các giá trị trong 1 luồng, sao cho f(r,n) sẽ tỉ lệ thuận với xác suất p(r,n)