# CluStream Algorithm

## 1. CluStream Algorithm là gì?

**CluStream Algorithm** là một phương pháp phân cụm dữ liệu luồng nhằm xử lý **concept drift** (thay đổi trong phân phối dữ liệu theo thời gian). Nó cung cấp khả năng phân cụm dữ liệu luồng trên nhiều khung thời gian khác nhau, điều mà **STREAM Algorithm** không thực hiện được.

### Ý tưởng chính:

- **Phân cụm hai giai đoạn:**
    - **Microclustering (trực tuyến):** Tóm tắt dữ liệu luồng theo thời gian thực dưới dạng các microcluster, lưu trữ thông tin chi tiết nhưng cô đọng.
    - **Macroclustering (ngoại tuyến):** Tóm tắt và tái phân cụm các microcluster để phân tích trên các khung thời gian cụ thể do người dùng yêu cầu.
- **Lợi ích:** CluStream cung cấp khả năng linh hoạt để tái phân cụm dữ liệu trên các khung thời gian bất kỳ, giảm tải bộ nhớ mà vẫn duy trì chất lượng phân tích.

---

## 2. Các thành phần trong CluStream Algorithm

### 2.1 Microcluster

**Microcluster** là cách tóm tắt thông tin chi tiết của dữ liệu luồng. Mỗi microcluster lưu trữ các thống kê tổng hợp về dữ liệu trong một khoảng thời gian.

#### Định nghĩa:

Một microcluster được biểu diễn bởi một bộ 5 thông tin chính, gọi là (2·d + 3)-tuple:

- **CF2x:** Tổng bình phương các giá trị dữ liệu trên mỗi chiều.
- **CF1x:** Tổng giá trị dữ liệu trên mỗi chiều.
- **CF2t:** Tổng bình phương các dấu thời gian.
- **CF1t:** Tổng các dấu thời gian.
- **n:** Số lượng điểm dữ liệu.

#### Tính chất:

Microcluster có tính cộng gộp:

- Có thể cập nhật trực tuyến bằng cách cộng thêm dữ liệu mới.
- Có thể tính toán lại cho một khoảng thời gian cụ thể bằng phép trừ giữa các microcluster ở hai thời điểm.

### 2.2 Pyramidal Time Frame

Microcluster được lưu trữ tại các mốc thời gian chụp nhanh (snapshots) theo cấu trúc hình chóp (pyramidal), giúp cân bằng giữa yêu cầu lưu trữ và khả năng truy xuất dữ liệu.

- **Các snapshot gần đây:** Lưu trữ chi tiết hơn (ngắn hạn).
- **Các snapshot cũ hơn:** Lưu trữ ít chi tiết hơn (dài hạn).
#### Các nguyên tắc chính của **Pyramidal Time Frame**:

1. **Lưu trữ các snapshot tại các mức độ phân giải khác nhau**:
    
    - Các **snapshot** được lưu trữ tại các thời điểm khác nhau tùy thuộc vào **order** của snapshot đó. Càng gần thời điểm hiện tại, các snapshot càng được lưu trữ với tần suất cao hơn, điều này tạo ra một **hình chóp** (pyramidal structure) trong cách lưu trữ.
    - **Order** của snapshot quy định mức độ phân giải thời gian mà snapshot được lưu trữ, với các snapshot ở các cấp thấp (order cao) lưu trữ ở mức độ phân giải thô hơn, trong khi các snapshot ở các cấp cao (order thấp) lưu trữ ở mức độ phân giải chi tiết hơn.
2. **Lưu trữ các snapshot dựa trên các quy tắc phân giải**:
    
    - Các **snapshot của order i** được lưu tại các thời điểm sao cho thời gian hệ thống phải chia hết cho αi\alpha^iαi, với α\alphaα là một số nguyên và α≥1\alpha \geq 1α≥1.
    - **Chỉ lưu trữ các snapshot gần nhất**: Ở mỗi order, chỉ giữ lại αl+1\alpha^l + 1αl+1 snapshot gần nhất. Điều này giúp giảm thiểu việc lưu trữ dư thừa và tiết kiệm bộ nhớ.
3. **Sử dụng tính toán gần đúng cho các khung thời gian**:
    
    - Bằng cách sử dụng các snapshot đã lưu trong **pyramidal time frame**, hệ thống có thể **xây dựng các phân cụm theo khung thời gian bất kỳ** mà không cần phải truy xuất lại toàn bộ dữ liệu.
    - Các snapshot lưu trữ trong khoảng thời gian tương ứng sẽ được sử dụng để tạo các phân cụm cho các khoảng thời gian gần đúng (horizon-specific clustering).

##### Ví dụ minh họa:

Giả sử luồng dữ liệu bắt đầu từ thời điểm t=1t = 1t=1, với α=2\alpha = 2α=2 và l=2l = 2l=2, điều này có nghĩa là chúng ta lưu trữ **5 snapshots** cho mỗi order (vì 22+1=52^2 + 1 = 522+1=5). Khi đồng hồ hệ thống đến **thời điểm 55**, các snapshot sẽ được lưu tại các thời điểm cụ thể như trong bảng dưới đây:

|**Order**|**Các thời điểm snapshot**|
|---|---|
|**0**|55, 54, 53, 52, 51|
|**1**|54, 52, 50, 48, 46|
|**2**|52, 48, 44, 40, 36|
|**3**|48, 40, 32, 24, 16|
|**4**|48, 32, 16|
|**5**|32|

- **Order 0** lưu trữ snapshot tại các thời điểm chia hết cho α0=1\alpha^0 = 1α0=1, tức là tất cả các thời điểm gần nhất.
- **Order 1** lưu trữ tại các thời điểm chia hết cho α1=2\alpha^1 = 2α1=2, tức là các thời điểm như 54, 52, v.v.
- **Order 2** lưu trữ tại các thời điểm chia hết cho α2=4\alpha^2 = 4α2=4, tức là các thời điểm như 52, 48, v.v.
- Và tiếp tục như vậy cho các order cao hơn.
#### Lợi ích:

- Giảm dung lượng lưu trữ cần thiết.
- Hỗ trợ truy xuất thông tin phân cụm ở các khung thời gian khác nhau.

**Ví dụ:**  
Dữ liệu luồng được chụp nhanh mỗi phút trong 1 giờ đầu tiên, mỗi giờ trong ngày đầu tiên, và mỗi ngày trong tháng đầu tiên.

---

## 3. Cách hoạt động của CluStream Algorithm

### Giai đoạn 1: Microclustering (trực tuyến)

Khi dữ liệu luồng đến, các điểm dữ liệu được gán vào các microcluster gần nhất. Các thông tin trong microcluster (CF2x, CF1x, CF2t, CF1t, n) được cập nhật bằng phép cộng.

### Giai đoạn 2: Macroclustering (ngoại tuyến)

Khi cần phân cụm ở khung thời gian cụ thể, các microcluster được sử dụng để tái phân cụm. Việc tái phân cụm được thực hiện bằng cách áp dụng các thuật toán như k-means lên các microcluster.

#### Ví dụ:

Người dùng yêu cầu phân cụm dữ liệu trong 1 giờ gần nhất. Sử dụng các microcluster từ các snapshot trong 1 giờ để tái phân cụm.

## 4. Kết Luận

### Ưu điểm:

- **Thích nghi với concept drift:** Bằng cách sử dụng microcluster và snapshot, CluStream có thể xử lý sự thay đổi phân phối dữ liệu.
- **Phân cụm đa khung thời gian:** Hỗ trợ phân tích dữ liệu ở các cấp độ thời gian khác nhau.
- **Tính toán hiệu quả:** Tính cộng gộp của microcluster giúp giảm chi phí tính toán.

### Hạn chế:

- **Phụ thuộc vào các tham số:** Số lượng microcluster và cấu trúc pyramidal cần được tối ưu hóa phù hợp với dữ liệu.
- **Tốn tài nguyên hơn STREAM Algorithm:** Việc duy trì microcluster và snapshot đòi hỏi nhiều bộ nhớ và tính toán hơn.

---

## 5. So sánh với STREAM Algorithm

|**Tiêu chí**|**CluStream Algorithm**|**STREAM Algorithm**|
|---|---|---|
|**Concept Drift**|Xử lý tốt, tái phân cụm theo thời gian|Không xử lý được|
|**Phân cụm thời gian**|Hỗ trợ khung thời gian khác nhau|Không hỗ trợ|
|**Hiệu quả tính toán**|Tốn tài nguyên hơn|Tính toán đơn giản hơn|
|**Ứng dụng**|Phân tích nâng cao, theo dõi xu hướng dài hạn|Tổng hợp nhanh, bộ nhớ hạn chế|

