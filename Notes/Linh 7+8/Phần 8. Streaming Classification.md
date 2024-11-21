 Vấn đề phân loại dữ liệu luồng (streaming classification) đặc biệt khó khăn bởi sự tác động của **concept** (trôi dạt khái niệm) . Một cách tiếp cận đơn giản là sử dụng **mẫu bộ nhớ đệm (reservoir sample)** để tạo ra một biểu diễn gọn nhẹ của dữ liệu huấn luyện. Biểu diễn này có thể được sử dụng để xây dựng mô hình ngoại tuyến (offline). Nếu cần, có thể sử dụng **mẫu bộ nhớ đệm dựa trên sự suy giảm (decay-based reservoir sample)** để xử lý concept drift.

Cách tiếp cận này có lợi thế là có thể sử dụng bất kỳ thuật toán phân loại thông thường nào, vì các thách thức liên quan đến luồng dữ liệu đã được xử lý trong giai đoạn lấy mẫu. Ngoài ra, cũng có nhiều phương pháp chuyên biệt được đề xuất dành riêng cho phân loại dữ liệu luồng.
## 1. Very Fast Decision Trees - VFDT

1. **Cây quyết định rất nhanh (VFDT) là gì?**
    - VFDT là một thuật toán xây dựng **cây quyết định** để phân loại dữ liệu từ luồng (streaming data).
    - Do dữ liệu luồng đến liên tục, không thể lưu trữ toàn bộ dữ liệu, VFDT chỉ dựa vào một **mẫu nhỏ** nhưng đảm bảo kết quả gần giống với việc phân tích toàn bộ dữ liệu.
    
2. **Định lý Hoeffding là gì và tại sao quan trọng?**
    - **Định lý Hoeffding** cung cấp một ràng buộc toán học để ước tính rằng một mẫu nhỏ từ một tập dữ liệu lớn đủ để đưa ra các quyết định chính xác với **xác suất cao**.
    - Với VFDT, định lý này được sử dụng để quyết định **khi nào đủ dữ liệu để chia cây**.
    - 
3. **Cách VFDT hoạt động:**
    - Khi dữ liệu đến theo luồng:
        1. **Các nút cao hơn của cây** (nơi phân chia ảnh hưởng lớn nhất) được xây dựng trước, khi có đủ dữ liệu để đảm bảo tính chính xác.
        2. **Các nút thấp hơn** (ít quan trọng hơn) được xây dựng sau, khi dữ liệu cho các nhánh con đã đủ.
	    3. **Cây phát triển dần dần (incremental):** Không cần tất cả dữ liệu từ trước, chỉ cần xử lý từng bước với dữ liệu mới đến.
    
4. **Giả định quan trọng:**
    - Luồng dữ liệu không thay đổi (không có **concept drift**). Điều này có nghĩa là dữ liệu hiện tại có thể được coi như đại diện tốt cho toàn bộ luồng dữ liệu trong tương lai.

5. **Cách xác định khi nào đủ dữ liệu để thực hiện split**
	- **Chọn thuộc tính split**:
	    - Thuộc tính được chọn dựa trên các tiêu chí như **Gini Index** (chỉ số đo độ không đồng nhất).
	    - Tại một nút:
        - $G_i$​: Gini Index của thuộc tính tốt nhất i trên toàn bộ dữ liệu.
        - $G'_i​$: Gini Index của thuộc tính tốt nhất i trên mẫu dữ liệu.
        - $G_j$​: Gini Index của thuộc tính tốt thứ hai j trên toàn bộ dữ liệu.
        - $G'_j​:$ Gini Index của thuộc tính tốt thứ hai j trên mẫu.
	- **Vấn đề tiềm ẩn**:
	    - Do sai lệch mẫu, có thể xảy ra trường hợp $G'_i < G'_j$ trên mẫu, nhưng trên toàn bộ dữ liệu, thực tế $G_j < G_i$ dẫn đến chọn sai thuộc tính split.
	- **Vai trò của Hoeffding Bound**:
	    - Nếu số mẫu n đủ lớn, xác suất xảy ra sai sót trên $(G_j < G_i)$ có thể được kiểm soát với xác suất cao $1 - \delta$
	    - n phụ thuộc vào:
	        - $\epsilon = G'_j - G'_i$: Độ chênh lệch giữa hai giá trị Gini Index trên mẫu.
	        - $\delta$: Xác suất người dùng chấp nhận cho lỗi.

---

 6. Công thức tính giá trị n
$$
    n = \frac{R^2\ln\left(\frac{1}{\delta}\right)}{2 \epsilon^2}

$$
    - δ: Xác suất lỗi mà người dùng chấp nhận.
    - Giá trị của R biểu thị phạm vi của tiêu chí phân chia. Đối với chỉ số Gini, giá trị của R là 1 và đối với tiêu chí entropy, giá trị là log(k), trong đó k là số lớp. Các mối quan hệ gần nhau trong tiêu chí phân chia tương ứng với các giá trị nhỏ của ϵ
- **Ý nghĩa**:
    - Số lượng mẫu n tăng khi:
        - Độ chênh lệch ϵ nhỏ (chất lượng các thuộc tính gần như ngang nhau -> cần phân biệt rõ hơn giữa các thuộc tính).
        - Xác suất lỗi δ nhỏ (yêu cầu độ chính xác cao hơn).

	Cách tiếp cận cây Hoeffding xác định xem chênh lệch hiện tại trong chỉ số Gini giữa thuộc tính phân tách tốt nhất và tốt nhất thứ hai có ít nhất là $\frac{√ R2·ln(1/δ)} {2n}$ để bắt đầu phân tách hay không. Điều này mang lại sự đảm bảo về chất lượng phân chia tại một nút cụ thể.

**Hạn chế của Hoeffding Tree**:
    - Khi các thuộc tính có chất lượng gần như ngang nhau, việc phân chia nút bị trì hoãn, dẫn đến thời gian xử lý kéo dài.
    - Một khi quyết định chia đã được thực hiện, nó không thể thay đổi.
- **Cải tiến của VFDT (Very Fast Decision Tree)**:
    - Phá vỡ các trường hợp hòa gần (near ties) một cách tích cực hơn.
    - Tắt các nút lá ít triển vọng.
    - Cải thiện độ chính xác bằng cách:
        1. Loại bỏ các thuộc tính chia kém hiệu quả.
        2. Gộp tính toán trung gian cho nhiều điểm dữ liệu.
- **Hạn chế của VFDT**:
    - Không được thiết kế để xử lý **concept drift** (sự thay đổi trong phân phối dữ liệu).

---

 7. **CVFDT (Concept-Adaptive VFDT)**
Để giải quyết **concept drift**, CVFDT (Concept-Adaptive VFDT) được phát triển với hai cải tiến chính:

**Cải tiến 1: Cửa sổ trượt (Sliding Window)**
- Sử dụng một **cửa sổ trượt** để giới hạn tác động của dữ liệu lịch sử.
- Khi cửa sổ di chuyển:
    - Thống kê của các điểm dữ liệu mới được thêm vào.
    - Thống kê của các điểm dữ liệu cũ bị loại bỏ.
- **Ý nghĩa**:
    - Các nút không còn thỏa mãn **Hoeffding Bound** sẽ bị thay thế.

**Cải tiến 2: Cây con thay thế (Alternate Subtrees)**
- Mỗi nút trong cây chính được gán một danh sách **cây con thay thế** (alternate subtrees).
- Các cây con được xây dựng đồng thời với cây chính.
- Khi thuộc tính chia tốt nhất thay đổi do **concept drift**, cây con thay thế sẽ được sử dụng để thay thế cây chính.

**Kết quả thực nghiệm**
- CVFDT đạt độ chính xác cao hơn so với VFDT trong các luồng dữ liệu có sự thay đổi khái niệm (**concept-drifting data streams**).
## 2. Supervised Microcluster Approach
Microcluster có giám sát là một phương pháp phân loại dựa trên mẫu (instance-based classification). Trong mô hình này, giả định rằng một luồng dữ liệu huấn luyện (training stream) và một luồng dữ liệu kiểm tra (test stream) được nhận đồng thời theo thời gian. Vì hiện tượng **concept drift**, việc điều chỉnh mô hình một cách linh hoạt là rất quan trọng.

- Trong phương pháp phân loại **k-nearest neighbor**, nhãn lớp thống trị (dominant class label) trong số k láng giềng gần nhất sẽ được báo cáo làm kết quả phân loại.
- Tuy nhiên, trong kịch bản xử lý luồng dữ liệu, việc tính toán k láng giềng gần nhất cho mỗi trường hợp kiểm tra là rất khó khăn vì kích thước luồng dữ liệu tăng dần không ngừng.

Để giải quyết vấn đề này:

1. **Microclustering** được sử dụng để tạo một bản tóm tắt dữ liệu có kích thước cố định, không tăng lên khi luồng dữ liệu tiếp diễn.
2. Một biến thể microcluster có giám sát được áp dụng, trong đó các điểm dữ liệu thuộc các lớp khác nhau **không được phép trộn lẫn trong cùng một cụm** (cluster).
3. Microcluster được cập nhật sao cho các điểm dữ liệu chỉ được gán vào các cụm thuộc cùng một nhãn lớp. Do đó, **nhãn lớp được gán cho các microcluster thay vì từng điểm dữ liệu riêng lẻ**.
4. Nhãn lớp thống trị của k-microcluster gần nhất sẽ được báo cáo là nhãn lớp kết quả.

**Tuy nhiên, phương pháp này không tính đến những thay đổi cần thiết đối với thuật toán do hiện tượng trôi dạt khái niệm**. Do sự trôi dạt khái niệm, các xu hướng trong luồng sẽ thay đổi. 
- Để tăng độ chính xác, **chỉ các microcluster trong các khoảng thời gian cụ thể (time horizons)** mới được sử dụng.
- . **Horizon gần nhất** thường có liên quan nhất, nhưng không phải lúc nào cũng đúng nếu luồng dữ liệu bất ngờ quay lại các xu hướng cũ hơn.
-  Để giải quyết vấn đề này:
    - Một phần của luồng dữ liệu huấn luyện được tách ra làm **luồng xác thực (validation stream)**.
    - Các phần gần đây của luồng xác thực được sử dụng để kiểm tra độ chính xác trên các khoảng thời gian khác nhau.
    - Horizon tối ưu (optimal horizon) được chọn dựa trên kết quả kiểm tra này.
- Sau đó, phương pháp k-láng giềng gần nhất được áp dụng trên luồng dữ liệu kiểm tra dựa trên horizon tối ưu đã được xác định.

## 3. Phương pháp Ensemble

Một phương pháp ensemble (tổ hợp) mạnh mẽ đã được đề xuất cho bài toán phân loại luồng dữ liệu (data streams). Phương pháp này cũng được thiết kế để xử lý **concept drift** (sự trôi dạt khái niệm) vì nó có thể tính đến sự tiến hóa hiệu quả trong dữ liệu cơ bản
- **Chia luồng dữ liệu thành các khối (chunks):**  
    Thay vì xử lý toàn bộ luồng dữ liệu cùng lúc, luồng được chia nhỏ thành từng đoạn dữ liệu liên tiếp.
    
- **Huấn luyện nhiều bộ phân loại trên từng khối:**  
    Điểm phân loại cuối cùng được tính như một hàm của các điểm số từ từng khối dữ liệu. Cụ thể, các mô hình phân loại trong tổ hợp, chẳng hạn như **C4.5, RIPPER, Naive Bayesian**, được áp dụng trên các khối dữ liệu liên tiếp trong luồng dữ liệu.
    
- **Tổ hợp các bộ phân loại (Ensemble):**  
    Kết quả cuối cùng được tính dựa trên điểm số từ tất cả các bộ phân loại đã huấn luyện.
    - **Trọng số động:** Các bộ phân loại được gán trọng số dựa trên độ chính xác dự kiến để phù hợp với dữ liệu hiện tại.
    
- **Ưu điểm của Ensemble:**
	- Phương pháp này linh hoạt, vì nó có thể điều chỉnh trọng số để phù hợp với dữ liệu thay đổi.
	- Độ chính xác cao hơn, vì các bộ phân loại kết hợp sẽ giảm thiểu sai số so với chỉ dùng một bộ phân loại duy nhất

## 4. Massive-Domain Streaming Classification
Nhiều ứng dụng xử lý luồng dữ liệu có các thuộc tính rời rạc đa chiều với độ lớn miền rất cao. Trong những trường hợp như vậy, việc sử dụng các bộ phân loại thông thường trở nên khó khăn do giới hạn bộ nhớ. Phương pháp **count-min sketch** có thể được sử dụng để giải quyết những thách thức này.
1. **Count-Min Sketch cho từng lớp:**  
    Mỗi lớp được gắn với một cấu trúc sketch (bản tóm lược), giúp theo dõi các tổ hợp phổ biến (**r-combinations**) trong dữ liệu huấn luyện.
    - Các tổ hợp này được giới hạn bởi giá trị **r ≤ k**, với **k** nhỏ
    -
2. **Thêm các tổ hợp vào sketch:**
    - Mỗi điểm dữ liệu trong luồng sẽ sinh ra các tổ hợp r-combination.
    - Các tổ hợp này được thêm vào sketch tương ứng của lớp đó dưới dạng **pseudo-items** (mục giả).'
    
3. **Phân biệt giữa các lớp dựa trên pseudo-items:**
    - Các lớp khác nhau sẽ có các **pseudo-items** phổ biến khác nhau.
    - Các **pseudo-items** có tính phân biệt cao sẽ được xác định nhằm tạo ra các **quy tắc ngầm** (**implicit rules**) liên kết các mục giả với từng lớp. Các quy tắc này không thực sự được vật chất hóa, mà được lưu trữ ngầm trong các sketch và chỉ được trích xuất khi cần phân loại một điểm kiểm tra.
    
4. **Phân loại dữ liệu kiểm tra (test):**
    - Các mục giả mang tính phân biệt cao sẽ được tìm ra bằng cách trích xuất thống kê từ các sketch cụ thể của từng lớp, phương pháp hoạt động như một **bộ phân loại dựa trên quy tắc** (rule-based classifier).