**Cấu trúc tóm tắt**(Synopsis Structures) là các công cụ mạnh mẽ trong khai thác luồng dữ liệu, bao gồm khai thác mẫu thường xuyên. Các cấu trúc này rất hiệu quả vì:

1. **Linh hoạt**: Có thể áp dụng cho nhiều thuật toán khai thác khác nhau.
2. **Hỗ trợ suy giảm thời gian**: Tích hợp giảm trọng số các dữ liệu cũ (temporal decay), giúp tập trung vào dữ liệu mới hơn.

Dưới đây là hai phương pháp phổ biến sử dụng cấu trúc tóm tắt: **Reservoir Sampling** và 
**Sketches**

### **1. Reservoir Sampling**

#### **Nguyên lý hoạt động**:

1. **Duy trì một mẫu con (reservoir)** từ luồng dữ liệu.
    - Mẫu này đại diện cho tập dữ liệu đầy đủ nhưng được lưu trong bộ nhớ giới hạn.
2. **Áp dụng thuật toán khai thác mẫu thường xuyên** trên mẫu con.
    - Kết quả là các mẫu thường xuyên được rút ra từ dữ liệu mẫu.

#### **Ưu điểm**:

- **Tính linh hoạt**:
    - Bất kỳ thuật toán khai thác mẫu thường xuyên nào (như Apriori, FP-Growth) đều có thể áp dụng lên reservoir.
    - Hỗ trợ khai thác mẫu theo các tiêu chí đặc biệt, như mẫu bị ràng buộc hoặc mẫu thú vị.
- **Giảm sai sót**:
    - Với các ngưỡng hỗ trợ thấp hơn, có thể giảm số lượng **false negatives** (bỏ sót mẫu thường xuyên).
    - Sử dụng **Chernoff Bound** để xác định xác suất một mẫu là **false positive** (mẫu không thực sự thường xuyên).

#### **Ví dụ**:

- **Tình huống**: Phân tích giỏ hàng của khách hàng trong một chuỗi siêu thị trực tuyến.
    - Dữ liệu: Luồng giao dịch liên tục của khách hàng.
    - Mục tiêu: Tìm tập hợp sản phẩm thường xuyên được mua cùng nhau.
- **Giải pháp**:
    - Sử dụng reservoir sampling để duy trì một mẫu giao dịch nhỏ.
    - Áp dụng FP-Growth lên mẫu để tìm các tập sản phẩm phổ biến.

#### **Đối phó với sự thay đổi theo thời gian (Concept Drift)**:

- Có thể dùng **decay-biased sampling** để ưu tiên các giao dịch mới.
- Trọng số của các mục trong mẫu sẽ giảm dần theo thời gian, tạo ra một khái niệm hỗ trợ theo thời gian (temporal support).

### **2. Sketches**

#### **Nguyên lý hoạt động**:

1. **Sketches** là cấu trúc dữ liệu nén dùng để ước lượng tần suất mục.
2. Chúng hoạt động tốt trong việc xác định các mục thường xuyên (heavy hitters), nhưng gặp khó khăn khi xác định tập hợp mục (frequent itemsets).

#### **Ưu điểm**:

- **Ước lượng chính xác tần suất tương đối** của các mục phổ biến.
- **Tiêu tốn ít bộ nhớ**: Sketches chỉ cần bộ nhớ nhỏ để lưu trữ.
- **Ứng dụng linh hoạt**: Có thể sử dụng các phương pháp như **Count-Min Sketch** hoặc **AMS Sketch**.

#### **Hạn chế**:

- Khó áp dụng trực tiếp cho việc tìm tập hợp mục (frequent itemsets).
- Sai số của ước lượng phụ thuộc vào tổng tần suất của tất cả các mục trong luồng.

#### **Ví dụ**:

- **Tình huống**: Giám sát mạng để phát hiện địa chỉ IP nhận nhiều gói tin nhất.
    - Dữ liệu: Luồng gói tin chứa địa chỉ IP đích.
    - Mục tiêu: Xác định các địa chỉ IP được truy cập nhiều nhất (heavy hitters).
- **Giải pháp**:
    - Sử dụng Count-Min Sketch để lưu trữ và ước lượng tần suất các địa chỉ IP.
    - Các địa chỉ IP như "192.168.1.1""192.168.1.1""192.168.1.1" được xác định chính xác là phổ biến dựa trên ước lượng của sketch.


### So sánh hai phương pháp: Reservoir Sampling và Sketches  

| **Phương pháp**        | **Ưu điểm**                                                                                                   | **Hạn chế**                                                                                           | **Ứng dụng chính**                               |
| ---------------------- | ------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| **Reservoir Sampling** | Linh hoạt, hỗ trợ nhiều thuật toán; giảm *false negatives* và *false positives*; xử lý concept drift dễ dàng. | Bộ nhớ hạn chế nên chỉ phân tích được mẫu nhỏ, cần cẩn thận chọn ngưỡng hỗ trợ.                       | Khai thác mẫu thường xuyên, tập hợp mục.         |
| **Sketches**           | Tiêu tốn ít bộ nhớ; ước lượng chính xác tần suất của *heavy hitters*; triển khai dễ dàng.                     | Khó áp dụng cho tập hợp mục; độ chính xác phụ thuộc vào tổng tần suất của tất cả các mục trong luồng. | Xác định các mục thường xuyên (*heavy hitters*). |
