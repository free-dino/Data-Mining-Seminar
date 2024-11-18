## I. Lọc:
### 1. Tại sao lại cần lọc luồng dữ liệu:
- Chúng ta muốn lấy những bộ trong luồng mà đạt một tiêu chuẩn nào đó.
### 2. Pros and Cons:
Pros:
- Cải thiện chất lượng của dữ liệu và cho phép tập trung vào dữ liệu đáng chú ý.
- Khái quát hoá dữ liệu tốt hơn và giúp trích xuất các tín hiệu liên quan.
- Hỗ trợ việc thích ứng với sự thay đổi của luồng dữ liệu.
Cons:
- Có thể lọc đi dữ liệu quan trọng hoặc giới thiệu sự thiên vị trong dữ liệu.
- Việc lựa chọn tiêu chuẩn lọc vô cùng phức tạp và yêu cầu tinh chỉnh chi tiết, đặc biệt là khi luồng dữ liệu thay đổi liên tục.
- Việc lọc quá mức có thể làm mất cơ hội tìm ra các khuôn mẫu hiếm.
## II. Lấy mẫu luồng dữ liệu
### 1. Tại sao lại cần lấy mẫu luồng dữ liệu:
- Lấy mẫu là một trong số những phương pháp linh động nhất để tóm tắt luồng dữ liệu.
- Sau khi đã lấy mẫu từ dữ liệu, hầu như mọi thuật toán offline có thể được áp dụng lên mẫu.
### 2. Ví dụ:
Một công cụ tìm kiếm nhận vào một luồng truy vấn, và nó muốn tìm hiểu hành vi của người dùng:
- Luồng tìm kiếm bao gồm các bộ (user, query, time) 
- Câu hỏi: "Tỉ lệ lặp lại các truy vấn của người dùng trong tháng trước"
- Chỉ lưu trữ 1/10 của luồng
Truy vấn lấy mẫu ở trên, như nhiều truy vấn khác về thống kê người dùng, không thể được giải đáp bằng việc lấy mẫu từ truy vấn của từng người dùng. Thay vào đó, chúng ta phải chọn 1/10 của tổng số người dùng và sử dụng toàn bộ tìm kiếm của họ, bỏ qua tìm kiếm của những người dùng khác.
Để làm việc này, chúng ta có thể làm như sau:
- Mỗi lần một truy vấn đi tới luồng, chúng ta kiểm tra xem người dùng có nằm trong mẫu.
- Nếu có, chúng ta thêm truy vấn vào mẫu và ngược lại. Tuy nhiên, nếu chúng ta không có lưu trữ về việc đã nhìn thấy người dùng bao giờ chưa thì chúng ta tạo một số nguyên ngẫu nhiên trong đoạn [0, 9]. Nếu số đó là 0, chúng ta đánh dấu người dùng với giá trị "in", còn nếu số đó khác 0, chúng ta đánh dấu với giá trị "out".
Phương pháp này hiệu quả miễn là chúng ta có thể lưu trữ danh sách người dùng và giá trị in/out của họ trong bộ nhớ chính vì không có thời gian để kiểm tra thông tin này trong dữ liệu cho mỗi truy vấn. Chúng ta có thể tránh việc lưu trữ này bằng cách sử dụng một hàm băm: Chúng ta băm tên mỗi người dùng vào 1 trong 10 bucket, 0 đến 9. Bằng cách này, chúng ta sử dụng hàm băm như một hệ thống tạo số ngẫu nhiên với đặc tính quan trọng là luôn trả về cùng số với cùng người dùng. Nhờ vậy, chúng ta có thể quyết định giá trị in/out mà không cần lưu trữ.

### 3. Pros and cons:
Pros:
- Giảm bớt bộ nhớ sử dụng, yêu cầu lưu trữ, chi phí tính toán
- Cho phép xử lí thời gian thực và cập nhật mô hình lấy mẫu liên tục
- Đánh giá gần đúng một luồng dữ liệu dựa trên một tập nhỏ từ luồng dữ liệu đó, tránh việc overfit
Cons:
- Mất mát thông tin và thiếu hiểu biết về quá khứ của luồng vào bất cứ thời điểm nào.
- Độ chính xác giảm và có thể không biểu hiện rõ các sự kiện hiếm.
- Bảo đảm mẫu thể hiện được trạng thái của luồng khá khó khăn với việc luồng dữ liệu thay đổi liên tục theo thời gian.