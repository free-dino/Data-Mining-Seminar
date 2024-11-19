### **Khai thác mẫu thường xuyên trong luồng dữ liệu (Frequent Pattern Mining in Data Streams)**

Việc khai thác mẫu thường xuyên trong luồng dữ liệu rất khó khăn vì dữ liệu đến liên tục theo thời gian thực và không thể lưu toàn bộ vào bộ nhớ. Điều này yêu cầu các thuật toán phải xử lý dữ liệu chỉ qua một lượt hoặc sử dụng bộ nhớ giới hạn.

### **Bối cảnh và các trường hợp**

#### **1. Trường hợp miền dữ liệu lớn (Massive-Domain Scenario)**

- **Định nghĩa**: Miền dữ liệu chứa rất nhiều phần tử khác nhau, đến mức khó xác định các mục (item) thường xuyên (hay còn gọi là _heavy hitters_).
    
- **Thách thức**:
    
    - Số lượng mục khả dĩ quá lớn, không thể lưu đếm từng mục trong bộ nhớ.
    - Các phương pháp truyền thống (như mảng lưu đếm số lần xuất hiện) không khả thi.
- **Ví dụ**:
    
    - Trong hệ thống giám sát lưu lượng mạng, mỗi gói tin có địa chỉ IP đích được xem là một mục.
    - Miền địa chỉ IP (IPv4 có 2³² địa chỉ) quá lớn để lưu trữ đếm cho từng địa chỉ.
    - **Mục tiêu**: Tìm các địa chỉ IP nhận được nhiều gói tin nhất (_heavy hitters_).
- **Giải pháp**:
    
    - Sử dụng cấu trúc dữ liệu xấp xỉ như **Count-Min Sketch**:
        - Đây là một cấu trúc dữ liệu xác suất dùng hàm băm để ước lượng tần suất mục.
        - Nó giúp tóm tắt luồng dữ liệu với sai số có thể kiểm soát.

#### **2. Trường hợp miền dữ liệu vừa phải (Conventional Scenario)**

- **Định nghĩa**: Số lượng mục lớn nhưng có thể quản lý, tức là vừa với bộ nhớ chính.
    
- **Thách thức**:
    
    - Đếm mục đơn lẻ là dễ dàng, nhưng khai thác mẫu thường xuyên (như các tập hợp mục thường xuyên) lại khó vì các thuật toán truyền thống (như **Apriori**, **FP-Growth**) yêu cầu nhiều lượt quét qua dữ liệu.
    - Trong luồng dữ liệu, phải tuân thủ _ràng buộc một lượt_ (one-pass constraint), nghĩa là thuật toán cần xử lý dữ liệu ngay khi nó đến.
- **Ví dụ**:
    
    - Trên một nền tảng bán lẻ trực tuyến, khách hàng liên tục mua các sản phẩm, và ta muốn tìm các tập mục thường xuyên (ví dụ: "khách hàng mua bánh mì thường mua bơ").
    - Dữ liệu là luồng các giao dịch của khách hàng, và cần khai thác mẫu thường xuyên trong thời gian thực.
- **Giải pháp**:
    
    - Hai cách tiếp cận chính:
        1. **Cách tiếp cận dựa trên bản tóm tắt (Synopsis-Based Approaches)**:
            - Sử dụng các cấu trúc tóm tắt như **Sliding Windows** (cửa sổ trượt), **Damped Windows** (cửa sổ giảm trọng số), hoặc **Histograms**.
            - Ví dụ: Cửa sổ trượt chỉ lưu các giao dịch gần nhất và cập nhật mẫu dựa trên các giao dịch này.
            - Kết hợp bản tóm tắt với các thuật toán truyền thống (như FP-Growth) để khai thác mẫu.
        2. **Các thuật toán khai thác chuyên biệt cho luồng (Streaming Mining Algorithms)**:
            - Thiết kế thuật toán tối ưu cho dữ liệu luồng.
            - Ví dụ: **FP-Stream**:
                - Dựa trên cấu trúc FP-Tree nhưng được cập nhật liên tục.
                - Các giao dịch mới được thêm vào cây, và mẫu thường xuyên được trích xuất định kỳ.

#### 3.So sánh hai trường hợp: Miền dữ liệu lớn vs. Miền dữ liệu vừa phải  

| **Khía cạnh**      | **Trường hợp miền dữ liệu lớn**        | **Trường hợp miền dữ liệu vừa phải**      |
| ------------------ | -------------------------------------- | ----------------------------------------- |
| **Trọng tâm**      | Tìm mục thường xuyên (*heavy hitters*) | Tìm tập hợp mục thường xuyên (*patterns*) |
| **Thách thức**     | Bộ nhớ lớn để lưu đếm                  | Nhiều lượt quét qua dữ liệu               |
| **Ứng dụng ví dụ** | Giám sát gói tin mạng                  | Khai thác tập hợp sản phẩm bán lẻ         |
| **Thuật toán**     | *Count-Min Sketch*...                  | *FP-Stream*....                           |
