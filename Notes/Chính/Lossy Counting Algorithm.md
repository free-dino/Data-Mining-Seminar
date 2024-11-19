
## 1. Lossy Counting Algorithm là gì?

Lossy Counting là một thuật toán khai thác tần suất thường xuyên trong luồng dữ liệu khi không thể lưu trữ toàn bộ luồng dữ liệu trong bộ nhớ.

**Ý tưởng chính**:

- Dữ liệu được chia thành các phân đoạn (_segments_) có kích thước cố định $w = \frac{1}{\epsilon}$, trong đó $\epsilon$ là mức sai số mà người dùng chấp nhận.
- Thuật toán sẽ giảm số lần đếm của các mục ít xuất hiện và loại bỏ chúng khi cần thiết, đồng thời vẫn đảm bảo rằng không có mục thực sự thường xuyên nào bị bỏ sót.

---

## 2. Các bước hoạt động của thuật toán

### a. Phân chia dữ liệu thành các phân đoạn

- Luồng dữ liệu được chia thành các phân đoạn $S_1, S_2, \dots$, mỗi phân đoạn có kích thước $w = \frac{1}{\epsilon}$.
- Ví dụ: Nếu $\epsilon = 0.01$, mỗi phân đoạn sẽ chứa $w = 100$ mục.

### b. Duy trì tần suất trong một mảng

- Một mảng được duy trì để lưu tần suất của các mục xuất hiện trong phân đoạn. Khi một mục mới đến:
  - Nếu đã có trong mảng, giá trị đếm của mục đó sẽ tăng lên.
  - Nếu chưa có, mục mới sẽ được thêm vào mảng với tần suất ban đầu là 1.

### c. Loại bỏ mục ít phổ biến

- Khi một phân đoạn kết thúc, thuật toán thực hiện:
  - Giảm tần suất của tất cả các mục trong mảng đi 1.
  - Loại bỏ các mục có tần suất bằng 0 khỏi mảng.

**Ví dụ minh họa**:  
Giả sử bạn đang xử lý một luồng giao dịch:

- $S_1$: [A, B, A, C, A, B, B, C, C, A].
- Mỗi phân đoạn $S_i$ chứa $w = 5$ giao dịch.
- Sau phân đoạn $S_1$: Mảng chứa $\{A: 4, B: 3, C: 3\}$.
- Kết thúc $S_1$, giảm tất cả tần suất đi 1: $\{A: 3, B: 2, C: 2\}$.

---

## 3. Điều chỉnh sai số

- Sau khi xử lý $n$ mục dữ liệu, tổng số phân đoạn $r$ là:
	$r = O\left(\frac{n}{w}\right) = O(n \cdot \epsilon)$

- Để đảm bảo không đánh giá thấp bất kỳ tần suất nào, một lượng $\epsilon \cdot n$ được cộng thêm vào các tần suất cuối cùng.

---

## 4. Ưu điểm và hạn chế

### Ưu điểm:

1. **Hiệu quả về bộ nhớ**: Yêu cầu bộ nhớ $O\left(\frac{1}{\epsilon}\right)$.
   - Ví dụ: Nếu $\epsilon = 0.01$, chỉ cần bộ nhớ đủ để lưu 100 mục.
2. **Không có âm tính giả (false negatives)**: Bất kỳ mục nào thực sự thường xuyên đều được báo cáo.

### Hạn chế:

1. **Dương tính giả (false positives)**: Một số mục ít phổ biến có thể bị nhầm lẫn là thường xuyên, tùy thuộc vào giá trị $\epsilon$.
2. **Không thích ứng với Concept Drift**: Không thể điều chỉnh khi phân phối dữ liệu thay đổi theo thời gian.

### **5. Tổng quát hóa cho tập mục thường xuyên (Frequent Itemsets)**

#### **a. Sử dụng batching**

Để áp dụng thuật toán cho tập mục thường xuyên, nhiều phân đoạn được xử lý cùng lúc, gọi là **batching**.

**Lý do**:  
Nếu xử lý từng phân đoạn riêng lẻ, số lượng các tập mục được phát hiện sẽ rất lớn, nhiều tập không liên quan. Khi batch $\eta$ phân đoạn, thuật toán chỉ xử lý các tập mục xuất hiện ít nhất $\eta$ lần, giảm thiểu số lượng tập không quan trọng.

**Ví dụ**:  
Luồng dữ liệu giao dịch:  
$S_1$: $\{A,B\}, \{A,C\}, \{B,C\}$  
$S_2$: $\{A,B\}, \{B,C\}, \{A,B\}$

Batch 2 phân đoạn $S_1$ và $S_2$, phát hiện các tập mục thường xuyên với $min\_support = 2$:

**Tập mục thường xuyên**:  
$\{A,B\}, \{B,C\}$

#### **b. Xử lý và loại bỏ tập mục**

Sau mỗi batch:
- Giảm tần suất của tất cả các tập mục đi $\eta$.
- Loại bỏ các tập mục có tần suất bằng 0 hoặc âm.

---

### **6. Ví dụ thực tế**

**Kịch bản**: Một cửa hàng bán lẻ muốn phân tích các sản phẩm thường xuyên được mua cùng nhau.

- Chia luồng giao dịch thành các phân đoạn (mỗi phân đoạn chứa 100 giao dịch).
- Duy trì mảng tần suất cho các tập hợp sản phẩm.
- Loại bỏ các sản phẩm hoặc tập hợp ít phổ biến tại cuối mỗi phân đoạn.

**Kết quả**: Cửa hàng có thể phát hiện nhanh các sản phẩm phổ biến trong một khung thời gian ngắn, chẳng hạn như tuần hoặc tháng gần đây.

---

### **7. So sánh với Reservoir Sampling**

| **Thuật toán**         | **Ưu điểm**                            | **Hạn chế**                                               |
| ---------------------- | -------------------------------------- | --------------------------------------------------------- |
| **Lossy Counting**     | Tiết kiệm bộ nhớ, không có âm tính giả | Không thích ứng với concept drift                         |
| **Reservoir Sampling** | Thích ứng với concept drift, linh hoạt | Có thể bỏ sót các mục thường xuyên nếu kích thước mẫu nhỏ |

