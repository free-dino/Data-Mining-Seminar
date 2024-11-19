### **1. STREAM Algorithm là gì?**

STREAM Algorithm là một phương pháp phân cụm dữ liệu luồng dựa trên kỹ thuật **k-medians**, nhằm giải quyết bài toán phân cụm trong dữ liệu lớn mà bộ nhớ có giới hạn.

**Ý tưởng chính**:
- Chia luồng dữ liệu thành các phân đoạn nhỏ ($S_1, S_2, ..., S_r$) để xử lý cục bộ từng phần.
- Áp dụng thuật toán k-medians lên mỗi phân đoạn để chọn ra $k$ đại diện tối ưu nhất cho mỗi cụm.
- Sau khi xử lý một số phân đoạn, nếu số lượng đại diện vượt quá giới hạn bộ nhớ, thực hiện phân cụm tiếp theo ở cấp độ cao hơn (**hierarchical clustering**).

---

### **2. Cách hoạt động của STREAM Algorithm**

#### **Bước 1: Chia luồng dữ liệu thành các phân đoạn**
Dữ liệu luồng $S$ được chia thành các phân đoạn nhỏ ($S_1, ..., S_r$) với kích thước $m$ điểm dữ liệu mỗi phân đoạn.  
Giá trị $m$ được xác định dựa trên bộ nhớ sẵn có.

**Ví dụ**:  
Giả sử một luồng dữ liệu có 1 triệu điểm, và bộ nhớ chỉ chứa được tối đa 10,000 điểm.  
Chia luồng dữ liệu thành 100 phân đoạn ($m=10,000$).

#### **Bước 2: Phân cụm từng phân đoạn (k-medians clustering)**  
Áp dụng thuật toán k-medians lên mỗi phân đoạn $S_i$ để tìm $k$ đại diện tối ưu ($Y_1, ..., Y_k$).  
Tính toán mục tiêu: Tối thiểu hóa tổng bình phương khoảng cách (**SSQ**) từ các điểm dữ liệu đến trung tâm cụm.

$\text{Objective}(S, Y) = \sum_{X_i \in S} \text{dist}(X_i, Y_{ji})^2$

- $X_i$: điểm dữ liệu trong phân đoạn $S_i$.
- $Y_{ji}$: đại diện gần nhất của $X_i$.

**Ví dụ**:  
Một phân đoạn $S_1$ chứa các điểm $[2,3], [4,5], [10,11]$ với $k=2$.  
Các trung tâm cụm tối ưu là $Y_1 = [3,4], Y_2 = [10,11]$.  
Tổng SSQ:
\[
(2 - 3)^2 + (3 - 4)^2 + (10 - 10)^2 + (11 - 11)^2 = 2
\]

#### **Bước 3: Lưu trữ và gán trọng số cho các đại diện**
Sau khi xử lý xong phân đoạn đầu tiên $S_1$, lưu trữ $k$ đại diện cùng với trọng số (số lượng điểm gán vào mỗi đại diện).  
Tiếp tục xử lý phân đoạn tiếp theo $S_2$ độc lập để tìm $k$ đại diện mới.

**Ví dụ**:  
Phân đoạn $S_1$: $k=2, Y_1 = [3,4]$ (trọng số = 2), $Y_2 = [10,11]$ (trọng số = 1).  
Phân đoạn $S_2$: $k=2, Y_3 = [5,6], Y_4 = [12,13]$.

#### **Bước 4: Phân cụm cấp độ cao hơn (Hierarchical Clustering)**  
Sau khi xử lý $r$ phân đoạn, tổng số đại diện $r \cdot k$ có thể vượt quá giới hạn bộ nhớ. Khi đó:
- Tiến hành phân cụm cấp cao hơn để giảm số lượng đại diện.
- Sử dụng thuật toán k-medians, nhưng có thêm trọng số khi phân cụm.

**Ví dụ**:  
Sau $r=10$ phân đoạn, có tổng cộng 20 đại diện. Áp dụng phân cụm với $k=2$ để giảm còn 2 đại diện cấp cao hơn.

#### **Bước 5: Kết quả cuối cùng**
Khi kết thúc luồng dữ liệu hoặc cần kết quả phân cụm, kết hợp tất cả các đại diện từ các cấp khác nhau và áp dụng k-medians một lần nữa.

---

### **3. Đánh giá chất lượng thuật toán**

**Yếu tố ảnh hưởng đến chất lượng**:
- **Thuật toán k-medians** được sử dụng: Chất lượng của phân cụm phụ thuộc vào mức độ tối ưu của thuật toán k-medians áp dụng trong từng phân đoạn.
- **Phân rã vấn đề thành các phân đoạn nhỏ**: Dữ liệu được xử lý theo từng phần, có thể ảnh hưởng đến chất lượng phân cụm cuối cùng.

**Kết quả**:  
STREAM Algorithm đảm bảo rằng chất lượng cuối cùng không tệ hơn $5 \cdot c$, trong đó $c$ là mức xấp xỉ của thuật toán k-medians được sử dụng.
