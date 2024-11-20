# Phần 4: Kỹ thuật đếm và Ước lượng mô-men (moments) 
**NOTE 4**

### Bài toán

Trong stream data analysis), một bài toán phổ biến là **ước lượng mô-men của phân phối tần suất của các phần tử trong luồng mà không cần lưu trữ toàn bộ dữ liệu**. Mô-men là một giá trị thống kê cho thấy phân phối tần suất và độ biến động của các phần tử, từ đó cho chúng ta cái nhìn về sự đa dạng và phân bố của chúng trong luồng.(Bài toán được nói rõ qua phần 4.2 và 4.3) và ta có bài toán FLAJOLET-MARTIN cho **bài toán đếm các phần tử duy nhất trong luồng dữ liệu lớn**
### ==4.1 Đếm các phần tử duy nhất==:

Trong bối cảnh khai thác dữ liệu dòng, một trong những bài toán quan trọng là **đếm các phần tử duy nhất** trong một luồng dữ liệu, đặc biệt khi số lượng phần tử trong tập dữ liệu có thể rất lớn và không thể lưu trữ toàn bộ trong bộ nhớ.

#### **Vấn Đề Đếm Phần Tử Duy Nhất và Các Thách Thức**

- **Vấn Đề**: Cho một luồng dữ liệu,  cần xác định số lượng phần tử duy nhất trong luồng đó. Khi dữ liệu là quá lớn, không thể lưu trữ hết tất cả các phần tử duy nhất vào bộ nhớ, và không thể duy trì tất cả các giá trị trong bộ nhớ.
    
- **Thách thức**:
    
    1. **Kích thước bộ nhớ hạn chế**: Không thể lưu trữ tất cả các phần tử của luồng trong bộ nhớ.
    2. **Sự thay đổi động của luồng dữ liệu**: Các phần tử mới có thể tiếp tục xuất hiện trong luồng, yêu cầu các thuật toán phải có khả năng xử lý dữ liệu mới mà không cần phải tính lại từ đầu.
    3. **Ước lượng gần đúng**: Vì bộ nhớ bị giới hạn, nên phải sử dụng các phương pháp ước lượng để tính toán số phần tử duy nhất mà không cần phải giữ tất cả thông tin trong bộ nhớ.

#### **Thuật Toán Flajolet-Martin: Nguyên Lý và Ứng Dụng**

**Thuật toán Flajolet-Martin là một trong những thuật toán nổi bật để giải quyết bài toán đếm các phần tử duy nhất trong luồng dữ liệu lớn mà không yêu cầu quá nhiều bộ nhớ.**

- **Nguyên lý cơ bản**: Thuật toán này sử dụng các hàm băm (hash functions) để mã hóa các phần tử trong luồng và sau đó dựa trên các giá trị băm để ước lượng số lượng phần tử duy nhất.
- **Ưu điểm**:
    
    1. **Tiết kiệm bộ nhớ**: Chỉ cần một lượng bộ nhớ nhỏ để lưu trữ các giá trị băm, thay vì lưu trữ toàn bộ phần tử trong luồng.
    2. **Hiệu quả với luồng lớn**: Thuật toán có thể xử lý được các luồng dữ liệu lớn, thậm chí vô hạn
- **Ứng dụng**: Thuật toán Flajolet-Martin có thể được sử dụng trong nhiều ứng dụng như:
    
    1. **Đếm các phần tử duy nhất** trong các luồng dữ liệu lớn.
    2. **Ứng dụng trong các hệ thống phân tán**
    3. **Giảm tải bộ nhớ trong các hệ thống xử lý dữ liệu thời gian thực**. (Về bài toán Flajolet-Martin, mình đã chứng minh ở note 3)
####  **Yêu Cầu về Không Gian và Kết Hợp Các Ước Lượng**

- **Yêu cầu về không gian**: Để sử dụng thuật toán Flajolet-Martin, ta chỉ cần một không gian bộ nhớ rất nhỏ, có thể chỉ là vài giá trị băm. Điều này giúp giảm chi phí bộ nhớ đáng kể so với các phương pháp đếm thông thường
    
- **Kết hợp các ước lượng**: Để cải thiện độ chính xác, ta có thể kết hợp nhiều ước lượng từ các hàm băm khác nhau. Việc này giúp giảm sai số và cung cấp một ước lượng chính xác hơn về số lượng phần tử duy nhất trong luồng. 



## ==4.2 Ước lượng các mô-men== 

### **Hiểu Về Các Mô-Ment Thống Kê trong Luồng Dữ Liệu**

- **Mô-Ment Bậc 0**: là số lượng các phần tử duy nhất trong luồng dữ liệu. Mô-moment này có thể được ước lượng thông qua các phương pháp đếm các phần tử duy nhất như thuật toán Flajolet-Martin.
    
- **Mô-Ment Bậc 1**: Là tổng số lần xuất hiện của các phần tử trong luồng, tức là độ dài của luồng. 
    
- **Mô-Ment Bậc 2**: Là tổng các bình phương của tần suất xuất hiện các phần tử trong luồng . Mô-moment này đo lường mức độ phân bố không đều của các phần tử trong luồng. Nếu sự phân phối của các phần tử là không đồng đều, mô-moment bậc 2 sẽ có giá trị cao.
    

Các mô-moment bậc cao hơn cung cấp các thông tin chi tiết hơn về sự phân phối của các phần tử trong luồng, giúp đánh giá mức độ tập trung hay phân tán của các giá trị
### **Thuật Toán Alon-Matias-Szegedy (AMS) cho Mô-Moment Bậc 2**

Thuật toán **AMS** là một thuật toán nổi bật được sử dụng để ước lượng **mô-moment bậc 2** trong các luồng dữ liệu.

#### **Nguyên lý của Thuật toán AMS**:

Giả sử rằng chúng ta có một luồng dữ liệu với độ dài nhất định là $n$. Ta sẽ xem xét cách xử lý các luồng dữ liệu có độ dài thay đổi. Giả sử không đủ không gian để đếm tất cả các $m_i$​ cho tất cả các phần tử trong luồng, chúng ta vẫn có thể ước lượng mô-men bậc hai của luồng dữ liệu với một lượng không gian hạn chế; càng sử dụng nhiều không gian, độ chính xác của ước lượng càng cao.

Chúng ta sẽ tính một số biến. Với mỗi biến $X$, ta lưu trữ:

1. Một phần tử cụ thể từ tập hợp các phần tử, được gọi là $X.element$.
2. Một số nguyên $X.value$, là giá trị của biến.

####  Cách tính giá trị của một biến $X$:

 - Chọn một vị trí trong luồng từ 1 đến $n$, chọn ngẫu nhiên và đồng đều.
 - Đặt $X.elementX$ là phần tử tại vị trí đó và khởi tạo $X.value=1$.
 - Khi đọc luồng, tăng $X.value$ thêm 1 mỗi lần gặp lại $X.elementX$.

### **Mô-Ment Bậc Cao và Xử Lý Luồng Dữ Liệu Vô Hạn**

- **Mô-Ment Bậc Cao**: Các mô-moment bậc cao hơn,  như mô-moment bậc 3 và bậc 4, có thể cung cấp thông tin chi tiết hơn về sự phân tán của dữ liệu.
- **Thách thức với Luồng Dữ Liệu Vô Hạn**: Luồng dữ liệu vô hạn (hoặc gần như vô hạn) có thể xuất hiện trong các tình huống như dữ liệu phát sinh từ các cảm biến, các luồng mạng hoặc trong các hệ thống thời gian thực.Đây là thách thức lớn vì chúng ta không đủ luu trữ toàn bộ dữ liệu
- Các thuật toán như AMS và các phương pháp băm có thể giúp xử lý các luồng dữ liệu vô hạn mà không yêu cầu bộ nhớ lớn, nhờ vào khả năng ước lượng mô-moment mà không cần lưu trữ tất cả dữ liệu đã thấy.
- **Ước Lượng Các Mô-Moment Bậc Cao**: Để ước lượng các mô-moment bậc cao ( bậc 3 và 4), các phương pháp kết hợp nhiều hàm băm hoặc sử dụng các cấu trúc dữ liệu nâng cao như cây nhị phân hoặc các kỹ thuật học máy có thể được áp dụng.
- #### Công thức tổng quát cho mô-men bậc $k$

    Đối với mô-men bậc $k$ bất kỳ (với$k≥2$), chúng ta có thể biến giá trị $v=X.value$ thành công thức ước lượng:$$
n(v^k - (v - 1)^k)
$$
    **$n$: độ dài của luồng dữ liệu
    $v$**: Đây là số lần xuất hiện của một phần tử cụ thể (kí hiệu là $X.element$) trong luồng dữ liệu
    **k**:  là bậc của mô-men mà  muốn ước lượng
    **value**: là thuộc tính của biến $X$, biểu thị **số lần xuất hiện của một phần tử cụ thể trong luồng dữ liệu** mà $X$ đang theo dõi


## ==4.3 Đếm số lượng 1 trong 1 cửa sổ==


#### Giới thiệu về thuật toán Datar-Gionis-Indyk-Motwani (DGIM)

Thuật toán **DGIM** được thiết kế để xử lý các luồng dữ liệu mà không cần lưu trữ toàn bộ dữ liệu trong bộ nhớ, do đó rất hiệu quả với luồng dữ liệu lớn và yêu cầu tính toán nhanh. Mục tiêu chính của DGIM là:

- Giảm thiểu dung lượng bộ nhớ cần thiết để lưu trữ luồng dữ liệu.
- Cung cấp phương pháp đếm số lượng **1** trong một cửa sổ kích thước cố định gần đây, sao cho đáp ứng được các truy vấn nhanh chóng và chính xác.

#### Trả lời truy vấn, giảm thiểu lỗi và mở rộng đếm số 1

1. **Trả lời truy vấn**: DGIM sử dụng một hệ thống đếm **khoảng thời gian (buckets)** với kích thước lũy thừa của hai, giúp dễ dàng trả lời các truy vấn về số lượng **1** trong cửa sổ gần đây. Khi một truy vấn được yêu cầu, thuật toán sẽ xem xét các khoảng thời gian được lưu trữ và tính tổng để đưa ra kết quả xấp xỉ.
    
2. **Giảm thiểu lỗi**: Vì DGIM không lưu trữ dữ liệu chính xác cho mọi vị trí trong cửa sổ mà chỉ lưu trữ theo dạng nhóm (buckets), một số sai số có thể xuất hiện.
    
3. **Mở rộng đếm số 1**: DGIM không chỉ giới hạn ở việc đếm **1** trong các luồng dữ liệu nhị phân mà có thể mở rộng ra để đếm các giá trị khác hoặc tính toán các thuộc tính khác của luồng dữ liệu


## ==4.4 Cửa sổ suy giảm (decaying windows)==

**Định nghĩa và Tầm quan trọng của Cửa sổ Suy giảm**

Cửa sổ suy giảm là một kỹ thuật trong xử lý luồng dữ liệu, trong đó **các giá trị của dữ liệu cũ dần dần bị "phai mờ" hoặc giảm tầm quan trọng theo thời gian**. Thay vì lưu trữ tất cả dữ liệu với trọng số bằng nhau, phương pháp này áp dụng một hàm suy giảm để làm giảm giá trị hoặc trọng số của các dữ liệu cũ.

- **Lợi ích của cửa sổ suy giảm**:
    - Tiết kiệm bộ nhớ vì không cần phải lưu trữ toàn bộ dữ liệu.
    - Tập trung vào các dữ liệu mới nhất, cho phép hệ thống phản ứng nhanh hơn với thay đổi khá gần đây.
    - Phù hợp với các ứng dụng yêu cầu xử lý thời gian thực.

**Tìm các Phần tử Phổ biến Nhất với Cửa sổ Suy giảm**

Trong cửa sổ suy giảm, một cách tiếp cận phổ biến để xác định các phần tử phổ biến nhất là sử dụng thuật toán để duy trì danh sách các phần tử có tần suất xuất hiện cao nhất theo thời gian. **Các phần tử này sẽ có trọng số suy giảm theo thời gian để đảm bảo rằng các phần tử xuất hiện gần đây sẽ được ưu tiên hơn trong quá trình tính toán**.

Phương pháp này thường bao gồm các bước sau:

1. **Sử dụng hàm suy giảm**: Áp dụng một hàm suy giảm như hàm mũ hoặc tuyến tính để giảm trọng số của dữ liệu cũ.
2. **Cập nhật phần tử phổ biến**: Khi có dữ liệu mới, thuật toán sẽ cập nhật danh sách các phần tử phổ biến nhất dựa trên trọng số hiện tại của chúng.
3. **Tối ưu hóa bộ nhớ**: Chỉ lưu trữ các phần tử có trọng số cao, đồng thời loại bỏ hoặc giảm trọng số của các phần tử ít phổ biến hơn, giúp giảm thiểu yêu cầu bộ nhớ.

Cửa sổ suy giảm là một công cụ mạnh mẽ trong việc xử lý luồng dữ liệu lớn, giúp hệ thống phản ứng nhanh với các thay đổi mà vẫn tối ưu về mặt tài nguyên.

#### Ý nghĩa và Ứng dụng

Ước lượng mô-men giúp nắm bắt thông tin về sự phân phối và độ không đồng đều của các phần tử trong luồng dữ liệu mà không cần lưu trữ hoặc xử lý toàn bộ dữ liệu. Điều này rất hữu ích trong các ứng dụng như:

- **Phân tích mạng**: Phát hiện lưu lượng bất thường.
- **Phân tích dữ liệu người dùng**: Nhận diện hành vi hoặc sở thích bất thường.
- **Phát hiện gian lận**: Phân tích các mẫu giao dịch không đồng đều.

Các kỹ thuật đếm và ước lượng mô-men là nền tảng quan trọng trong xử lý luồng dữ liệu lớn, giúp giảm chi phí bộ nhớ và tính toán mà vẫn cung cấp các ước lượng thống kê chính xác.
### Hướng Tiếp Cận Giải Quyết

1. **Sử dụng Xấp Xỉ Thay vì Đếm Chính Xác**:
    
    - Đối với các luồng dữ liệu lớn, các thuật toán xấp xỉ như **Flajolet-Martin** cho đếm phần tử duy nhất và **Alon-Matias-Szegedy (AMS)** cho mô-men bậc 2 được dùng để giảm thiểu bộ nhớ cần thiết.
2. **Chọn Lọc Ngẫu Nhiên Một Số Phần Tử**:
    
    -  Dùng AMS chọn một số vị trí trong luồng dữ liệu và chỉ tập trung vào tần suất của các phần tử xuất hiện tại những vị trí đó. Từ đó, chúng ước lượng tổng thể dựa trên các biến ngẫu nhiên này.
    - Cách tiếp cận này làm giảm bộ nhớ sử dụng, đồng thời vẫn duy trì độ chính xác cao của ước lượng.
3. **Phân Chia Dữ Liệu Thành Các Cửa Sổ Trượt Hoặc Suy Giảm**:
    
    - Đối với các luồng dữ liệu vô hạn, kỹ thuật cửa sổ trượt hoặc cửa sổ suy giảm (decaying window) được sử dụng để chỉ quan tâm đến các phần tử gần đây nhất trong luồng, giúp cập nhật ước lượng khi luồng tiếp tục phát triển.


### Phương Pháp Đánh Giá

1. **Đánh Giá Độ Chính Xác Dựa Trên Sai Số**:
2. **Đo Lường Tài Nguyên Sử Dụng**:
3. **Kiểm Thử Trên Các Luồng Dữ Liệu Khác Nhau**:


### Các Vấn Đề Mở

1. **Nâng Cao Độ Chính Xác với Bộ Nhớ Hạn Chế**:
2. **Phát Triển Thuật Toán cho Mô-men Bậc Cao Hơn**:
    
    - Các mô-men bậc cao hơn (k > 2), cần có các thuật toán tối ưu cho các ứng dụng cụ thể.
3. **Ứng Dụng trong Các Luồng Dữ Liệu Không Gian và Thời Gian Cao**:
4. **Giảm Sai Số Tích Lũy trong Các Cửa Sổ Suy Giảm**:
    
    - Khi sử dụng cửa sổ suy giảm, sai số có thể tích lũy dần theo thời gian.Nên Nghiên cứu thêm về các phương pháp giảm thiểu sai số 
5. **Áp Dụng cho Dữ Liệu Không Đầy Đủ hoặc Nhiễu**:
    - Phát triển các thuật toán có thể xử lý và ước lượng chính xác mô-men trong môi trường có dữ liệu thiếu sót hoặc nhiễu loạn cũng là một thách thức lớn.





**NOTE 3**


# Phần 3: Cấu trúc dữ liệu tóm gọn cho luồng dữ liệu (Quyển 2-3)


### Cấu Trúc Dữ Liệu Tóm Gọn Cho Luồng Dữ Liệu - **==Bài Toán Vấn Đề==**

Khi xử lý luồng dữ liệu lớn, đặc biệt là trong các bài toán về đếm và ước lượng mô-men (moments), cần phải sử dụng các cấu trúc dữ liệu đặc biệt để lưu trữ thông tin một cách hiệu quả mà không cần phải giữ toàn bộ dữ liệu trong bộ nhớ. Sau đây là một số cấu trúc dữ liệu phổ biến và các phương pháp áp dụng cho các bài toán này:
  - **Cấu Trúc Dữ Liệu Flajolet-Martin (FM)**
  - **Cấu Trúc Dữ Liệu Sketch (HyperLogLog, Count-Min Sketch)**
  - **Cấu Trúc Dữ Liệu AMS (Alon-Matias-Szegedy)**
  - **Cấu Trúc Dữ Liệu Bloom Filter**
  - **Count-Min Sketch**
  - **Thuật toán Flajolet-Martin (đếm các phần tử duy nhất)**
  -Các bài toán này sẽ được nên bên dưới dây

##  Lấy mẫu dự trữ (Reservoir Sampling) (2-3)

  - ***Giới thiệu về lấy mẫu dự trữ***
     1. **Giới thiệu**: Lấy mẫu là một trong những phương pháp linh hoạt nhất để tóm tắt luồng dữ liệu. Ưu điểm chính của việc lấy mẫu so với các cấu trúc dữ liệu tóm tắt khác là nó có thể áp dụng cho bất kỳ ứng dụng nào. Sau khi đã lấy được mẫu từ dữ liệu, gần như bất kỳ thuật toán ngoại tuyến nào cũng có thể được áp dụng cho mẫu này. Đó là phương pháp ưu tiên trong các kịch bản luồng dữ liệu, mặc dù nó có một số hạn chế đối với các ứng dụng nhất định.Trong ngữ cảnh luồng dữ liệu, phương pháp duy trì một mẫu động từ dữ liệu được gọi là _reservoir sampling_. Mẫu thu được từ phương pháp này gọi là _reservoir sample_
     2. ==**Bài toán và nguyên lý của lấy mẫu dự trữ**==: Với 1 mẫu dự trữ có kích thức k , k điểm dữ liệu đầu tiên trong luồng được đưa vào mẫu, với điểm dữ liệu n trong luồn đến, 2 điều sau đây được áp dụng là :
        - Chèn điểm dữ liệu thứ n vào mẫu dự  trữ với xs k/n 
        - Nếu điểm dữ liệu đến được chèn vào, thì random loại bỏ một trong k điểm dữ liệu cũ trong mẫu dự trữ để nhường chỗ cho điểm dữ liệu mới.  
        
 - ***Xử lý trôi dạt khái niệm và Các kĩ thuật lấy mẫu adaptive (thích ứng)***
    - ==Bài toán==: khi xử lý luồng dữ liệu, dự liệu mới có thể quan trọng hơn dữ liệu cũ là hợp lý và dữ liệu cũ có thể không phản ánh chính xác các xu hướng, hoạt động hay đặc điểm hiện tại của dữ liệu sau khi các nguyễn dữ liệu mới được đến.Bài toán được đặt ra sẽ là cần đưa các dữ liệu gần đây vào mẫu với xác suất cao hơn vì dữ liệu gần đây sẽ thường có giá trị phân tích cao hơn cũ. Một trong các kỹ thuật lấy mẫu thích ứng(adaptive) Mình sẽ có 1 hàm có tên là [bias function]() (hàm thiên lệch) . Hàm này sẽ xác định xác suất của mỗi điểm có được chọn vào reservoir(bộ lưu trữ) dựa trên thời gian nó đến. Về hàm [bias function]() và cách chứng minh có ở trong ghi chú xanh ở trên


- ***Các giới hạn lý thuyết cho lấy mẫu***
   - Phương pháp lấy mẫu dự trữ (reservoir sampling) cung cấp các mẫu dữ liệu, nhưng thường cần phải có các giới hạn chất lượngg cho kết quả thu được thông qua quá trình lấy mẫu này. Một ứng dụng phổ biến của việc lấy mẫu là ước tính các số liệu thống kê tổng hợp bằng cách sử dụng mẫu dự trữ. Độ chính xác của các số liệu này thường được định lượng bằng cách sử dụng các bất đẳng thức phần đuôi (tail inequalities).Các ==bài toán== bất đẳng thức sau đây:
     1. [[Bất đẳng thức Markov]]
     2. [[Bất đẳng thức Chebyshev]]
     3. [[Cận Chernoff cho phần đuôi phía dưới]]
     4. [[Cận Chernoff cho phần đuôi phía trên]]


Cấu trúc tóm gọn cho các miền (domain) lớn (2-3)

- ***Tổng quan về cấu trúc tóm gọn cho các miền lớn***
 1. Trong xử lý dữ liệu luồng, đặc biệt là dữ liệu có miền giá trị lớn , việc lưu trữ và quản lý dữ liệu đòi hỏi các cấu trúc tóm gọn (synopsis structures) đặc thù để giải quyết bài toán về dung lượng bộ nhớ và hiệu quả xử lý. Các cấu trúc tóm gọn này không chỉ giảm thiểu dung lượng lưu trữ mà còn tối ưu hóa cho các loại truy vấn khác nhau, như kiểm tra thành viên, đếm số phần tử khác biệt và xác định các phần tử xuất hiện thường xuyên nhất,..vvvv


- . ***Cấu trúc dữ liệu và các bài toán***
   1. Bloom filter: là một cấu trúc dữ liệu được thiết kế cho các truy vấn xác định thành viên trong một tập hợp. Mục tiêu r là xác định liệu một phần tử đã từng xuất hiện trong luồng dữ liệu hay chưa, dựa trên phương pháp tóm tắt dữ liệu với độ chính xác xấp xỉ
    - Một [[Bloom filter]](chú thích ==bài toán==) bao gồm:
    
        - Một mảng `n` bit, ban đầu tất cả các bit đều được đặt là `0`.
        - Một tập hợp các hàm băm `h1, h2, ..., hk`. Mỗi hàm băm sẽ ánh xạ các giá trị "key" vào `n` buckets tương ứng với các bit của mảng bit.
        - Một tập `S` gồm `m` giá trị key.
        -  Mục đích là cho phép tất cả các phần tử trong luồng có khóa thuộc tập `S` đi qua, trong khi chối hầu hết các phần tử có khóa không thuộc `S`.
        - Để khởi tạo mảng bit, đặt tất cả các bit thành `0`. Đối với mỗi giá trị key trong `S`, băm nó bằng từng hàm băm `k`. Đặt mỗi bit có chỉ số là `hi(K)` thành `1` cho từng hàm băm `hi` và giá trị key `K` trong `S`.
        - Để kiểm tra một key `K` trong luồng, kiểm tra rằng tất cả các bit `h1(K), h2(K), ..., hk(K)` đều có giá trị là `1` trong mảng bit. Nếu tất cả đều là `1`, cho phép phần tử trong luồng đi qua. Nếu một hoặc nhiều bit là `0`, thì `K` chắc chắn không thuộc `S`, do đó từ chối phần tử trong luồng.


   2. Count-Min Sketch: là một cấu trúc dữ liệu được thiết kế để xử lý các truy vấn dựa trên count-based queries, điều mà Bloom Filter không làm được, vì Bloom Filter chỉ theo dõi các giá trị nhị phân. Count-Min khá là tương tự Bloom Filter, nhưng thay vì chỉ theo dõi các giá trị nhị phân (0 hoặc 1), nó theo dõi các giá trị số.


      - Các thành phần của Count-Min Sketch:

        -  **Mảng số nguyên**: Count-Min Sketch gồm nhiều mảng số nguyên (các hàng), mỗi mảng có độ dài m. Tổng kích thước của cấu trúc này là m×w  trong đó:
        - m là chiều dài của mỗi mảng.
        - w là số mảng (hoặc số hàm băm).
        - **Hàm băm**: Count-Min Sketch sử dụng w hàm băm độc lập. Mỗi hàm băm hi(x)ánh xạ một phần tử trong dòng dữ liệu đến một chỉ số trong mảng có độ dài m. Mỗi hàm băm hi​ ánh xạ phần tử dữ liệu x vào một chỉ số trong khoảng từ 0 đến m−1. Các hàm băm này được yêu cầu **độc lập từng cặp** (pairwise independent), nghĩa là chúng có thể hoạt động độc lập với nhau trong các hàm băm khác nhau.

    - Quy trình cập nhật Count-Min Sketch như sau: 

        - tất cả các ô trong Count-Min Sketch có kích thước $m×w$ đều được khởi tạo bằng 0
        - Đối với mỗi phần tử dòng dữ liệu đầu vào $x$, các hàm băm $h1(x),h2(x),…,hw​(x)$ được áp dụng. Đối với mảng thứ i, giá trị tại ô $hi(x)$ sẽ được tăng lên 1.
        - nếu Count-Min Sketch CM được hình dung như một mảng số 2 chiều có kích thước $w×m$ thì ô tại vị trí $(i,hi​(x))$ sẽ được tăng lên 1

        - Ta có định lý sau đây: (==Bài toán==)
        - Giả sử E(y) là ước lượng tần suất của phần tử $y$, sử dụng một Count-Min Sketch có kích thước w×m. Giả sử nfnfnf là tổng tần suất của tất cả các phần tử đã nhận cho đến nay, và G(y) là tần suất thực của phần tử $y$. Khi đó, với xác suất ít nhất là 1−e^−w giới hạn trên của ước lượng E(y) như sau:

           - $E(y)≤G(y)+nf⋅e^-m$

           - Trong đó, e là cơ số của logarithm tự nhiên  


   3. AMS Sketch:
     - Các cấu trúc synopsis khác nhau được thiết kế cho các loại truy vấn khác nhau.  Bloom filter và Count-Min sketch cung cấp các ước lượng tốt cho nhiều loại truy vấn, một số truy vấn, như các mômen bậc hai, có thể được giải quyết tốt hơn với AMS sketch . Trong AMS sketch, một giá trị nhị phân ngẫu nhiên được tạo ra từ {${{−1,1}}$ }cho mỗi phần tử của luồng bằng cách áp dụng một hàm băm lên phần tử đó. Những giá trị nhị phân này được giả định là độc lập 4 chiều .Nếu ít nhất bốn giá trị được tạo ra từ cùng một hàm băm được lấy mẫu, chúng sẽ độc lập thống kê với nhau. 
     - Xem xét một luồng trong đó phần tử thứ $i$ của luồng được liên kết với tần suất tổng hợp $f_i$​. Mômen bậc hai $F_2$​ của luồng dữ liệu, đối với một luồng có $n$ phần tử khác nhau, được định nghĩa như sau(12.25): $$
F_2 = \sum_{i=1}^{n} f_i^2
$$ Trong kịch bản miền rộng (massive-domain), nơi số lượng phần tử khác nhau là lớn, giá trị này khó ước lượng vì không thể duy trì các đếm chạy của tần suất $f_i$​ bằng một mảng. Tuy nhiên, nó có thể được ước lượng hiệu quả bằng cách sử dụng AMS sketch. Một ứng dụng thực tế, mômen bậc hai cung cấp một ước lượng về kích thước của phép kết hợp tự nó của một luồng dữ liệu liên quan đến thuộc tính miền rộng. Mômen bậc hai cũng có thể được coi là một biến thể của chỉ số Gini, chỉ số này đo lường mức độ lệch tần suất trên các phần tử khác nhau trong luồng dữ liệu. Khi sự lệch tần suất lớn, giá trị của $F_2$ cũng lớn và rất gần với giới hạn trên của nó $\left( \sum_{i=1}^{n} f_i \right)^2$.


- AMS sketch bao gồm $m$ thành phần sketch khác nhau, mỗi thành phần liên kết với một hàm băm độc lập. Mỗi hàm băm tạo ra thành phần sketch tương ứng của nó như sau. Một giá trị nhị phân ngẫu nhiên, với xác suất bằng nhau cho cả hai giá trị, được tạo ra cho phần tử luồng đến. Giá trị nhị phân này được ký hiệu là r∈{−1,1}, và được tạo ra bằng cách sử dụng hàm băm cho thành phần đó. Tần suất của mỗi phần tử luồng đến được nhân với $r$ và cộng vào thành phần tương ứng của sketch.
- Giả sử $r_i∈{−1,1}$ là giá trị ngẫu nhiên được tạo ra bởi một hàm băm cho phần tử thứ $i$ trong luồng. Thành phần tương ứng $Q$ của sketch, đối với một luồng có $n$ phần tử khác nhau với các tần suất tổng hợp $f1,…,fn$​,  là bằng (12.26)$$
Q = \sum_{i=1}^{n} f_i \cdot r_i
$$
  điều này xảy ra vì cách cập nhật $Q$ theo từng bước, mỗi khi một phần tử luồng được nhận.Giá trị của $Q$ là một biến ngẫu nhiên, phụ thuộc vào cách các giá trị nhị phân ngẫu nhiên $r1​,…,rn$​ được tạo ra bởi hàm băm. Giá trị của $Q$ có ích trong việc ước lượng mômen bậc hai.

4. Thuật toán Flajolet-Martin (đếm các phần tử duy nhất): 
    - Thuật toán Flajolet–Martin được thiết kế để đếm số lượng phần tử khác nhau trong luồng dữ liệu. Thuật toán này sử dụng một hàm băm $h(·)$ để ánh xạ một phần tử x trong luồng dữ liệu thành một số nguyên trong khoảng $[0, 2^L − 1]$. Giá trị L được chọn đủ lớn sao cho 2^L là một giới hạn trên cho số lượng phần tử khác nhau. Thông thường, giá trị L được chọn là 64 để thuận tiện trong việc triển khai, vì $2^64$ đủ lớn cho các ứng dụng thực tế. Do đó, biểu diễn nhị phân của số nguyên $h(x)$ sẽ có độ dài L.

    - Thuật toán tìm vị trí R của bit 1 ở phía bên phải nhất trong biểu diễn nhị phân của số $h(x)$. Vị trí này đại diện cho số lượng bit 0 liên tiếp từ phải sang trái trong biểu diễn nhị phân của h(x). Do đó, giá trị R đại diện cho số lượng bit 0 cuối cùng trong biểu diễn nhị phân. Giả sử Rmax là giá trị tối đa của R trong tất cả các phần tử của luồng. Giá trị Rmax có thể được duy trì một cách gia tăng trong quá trình xử lý luồng dữ liệu bằng cách áp dụng hàm băm cho mỗi phần tử trong luồng và cập nhật Rmax mỗi khi có phần tử mới.

      - Ý tưởng chính của thuật toán Flajolet–Martin là giá trị Rmax được duy trì động có quan hệ logarithm với số lượng phần tử khác nhau đã gặp trong luồng. Giải thích cơ bản là như sau: Với một hàm băm phân phối đồng đều, xác suất có R bit 0 liên tiếp trong biểu diễn nhị phân của một phần tử trong luồng là $2^{−R−1}$. Do đó, với n phần tử khác nhau trong luồng và một giá trị R cố định, số lần đạt được R bit 0 liên tiếp sẽ là $2^{−R−1} * n$. Do đó, khi R lớn hơn log(n), số lần đạt được giá trị này sẽ giảm nhanh chóng.

     - Giá trị kỳ vọng E[Rmax] của giá trị Rmax có thể được tính theo công thức sau: $$E[Rmax] = log2(φn), φ = 0.77351.
$$


         Độ lệch chuẩn là σ(Rmax) = 1.12. Vì vậy, giá trị $2^{(Rmax)}/φ$ cung cấp một ước tính cho số lượng phần tử khác nhau n.

         Để cải thiện ước tính của Rmax, có thể sử dụng các kỹ thuật sau:

         1. Sử dụng nhiều hàm băm và lấy giá trị trung bình của Rmax từ các hàm băm khác nhau.
        2. Để giảm thiểu sự biến động lớn, có thể sử dụng "trick trung vị" (mean-median trick). Trung vị của một tập hợp các giá trị trung bình được báo cáo. Điều này tương tự như mẹo sử dụng trong AMS sketch, và có thể sử dụng kết hợp giữa bất đẳng thức Chebychev và các giới hạn Chernoff để xác lập các đảm bảo chất lượng.
    

- So sánh các cấu trúc dữ liệu: các trường hợp sử dụng, điểm mạnh và hạn chế:

| Cấu Trúc dữ liệu | Trường hợp sử dụng              | Điểm mạnh                      | Hạn chế                                   |
| ---------------- | ------------------------------- | ------------------------------ | ----------------------------------------- |
| **Bộ lọc Bloom** | Kiểm tra tập hợp, phân tích web | Tiết kiệm bộ nhớ, tốc độ nhanh | False positives, không hỗ trợ xóa phần tử |

|                      |                                     |                              |                                     |
| -------------------- | ----------------------------------- | ---------------------------- | ----------------------------------- |
| **Count-min Sketch** | Đếm tần suất, phân tích dữ liệu lớn | Tiết kiệm bộ nhớ, dễ sử dụng | Ước lượng sai lệch, không chính xác |

|   |   |   |   |
|---|---|---|---|
|**AMS Sketch**|Ước lượng phương sai, phân tích môments dữ liệu|Ước lượng chính xác hơn cho phương sai|Không thể trả về chính xác, độ phức tạp cao|

|   |   |   |   |
|---|---|---|---|
|**Flajolet-Martin**|Đếm phần tử duy nhất trong luồng dữ liệu|Không gian lưu trữ nhỏ, hiệu quả cho dữ liệu lớn|Ước lượng sai lệch, không thể truy vấn các phần tử|

## **==Ys nghĩa==**

  - - **Giảm thiểu bộ nhớ**: Các cấu trúc tóm gọn cho phép ước lượng các thông tin quan trọng từ dữ liệu mà không cần lưu trữ tất cả các phần tử, giúp tiết kiệm bộ nhớ và tài nguyên.
    
  - **Tăng hiệu suất xử lý**: Chúng giúp xử lý các dữ liệu lớn và liên tục trong thời gian thực (streaming data), hỗ trợ các hệ thống cần đưa ra quyết định nhanh mà không cần truy xuất dữ liệu gốc.
    
  - **Ước lượng gần đúng**: Các cấu trúc này cung cấp các ước lượng gần đúng với độ chính xác hợp lý, nhưng lại giảm thiểu chi phí về thời gian và bộ nhớ, rất hữu ích trong các bài toán như đếm số lượng phần tử duy nhất, tìm kiếm các yếu tố nổi

###  **==Ứng dụng ==**
- **Đếm các phần tử duy nhất (Distinct Element Counting)**:
- **Xử lý dữ liệu luồng (Stream Processing)**:
- **Tìm kiếm và nhận dạng mẫu trong dữ liệu lớn**:
    
####  **==Hướng tiếp cận giải quyết==**

1. **Phân tích và chọn cấu trúc tóm gọn phù hợp**:
    - **Mục tiêu**: Chọn một cấu trúc tóm gọn phù hợp với mục tiêu và đặc thù của bài toán .
    - **Cách làm**:
        - Đối với dữ liệu có tính chất phân phối đồng đều, các cấu trúc như **Count-Min Sketch** thì chọn.
        - Nếu mục tiêu là tìm số lượng phần tử duy nhất trong luồng, chọn **Flajolet-Martin Algorithm** .
        - **AMS Sketch** có thể được chọn khi yêu cầu ước lượng chính xác cho các bậc moment của dữ liệu trong các luồng lớn.
2. **Sử dụng các kỹ thuật cải tiến**:
    - **Kỹ thuật cải tiến như mean-median trick**: Dùng để giảm thiểu sai số trong các ước lượng của các cấu trúc tóm gọn, cải thiện độ chính xác và hiệu quả của các ước tính.
    - **Phương pháp kết hợp nhiều hash functions**: Để giảm sự sai lệch và cải thiện độ ổn định của các cấu trúc như Flajolet-Martin hoặc Count-Min Sketch, việc sử dụng nhiều hàm băm và lấy trung bình sẽ giúp tăng độ chính xác của các ước lượng.
3. **Ứng dụng phương pháp đánh giá hiệu quả**:
    - Sử dụng các **phương pháp đánh giá xác suất** như Chebyshev hoặc Chernoff bounds để xác định độ chính xác và tính ổn định của các cấu trúc tóm gọn. 

---

#####  **==Phương pháp đánh giá==**

1. **Đánh giá độ chính xác**:
    
 - Các cấu trúc tóm gọn như **Count-Min Sketch** và **AMS Sketch** có thể được đánh giá về độ chính xác của kết quả ước lượng so với dữ liệu gốc. 
 - Các **phương pháp phân tích sai số** như **sai số tuyệt đối** hoặc **sai số tương đối** có thể được sử dụng để đánh giá hiệu quả của các cấu trúc tóm gọn.
  2. **Đánh giá độ ổn định và hiệu quả**:
    
 - **Độ ổn định**: Các cấu trúc tóm gọn cần phải duy trì tính ổn định ngay cả khi xử lý các luồng dữ liệu có tính chất thay đổi nhanh.
 - **Hiệu suất không gian và thời gian**: Các cấu trúc tóm gọn cần phải có khả năng hoạt động trong thời gian thực mà không chiếm quá nhiều bộ nhớ. 
 - **Khả năng mở rộng**:
    - Đánh giá **khả năng mở rộng** của các cấu trúc tóm gọn khi làm việc với các bộ dữ liệu cực lớn hoặc trong môi trường phân tán (distributed environments).

---

### Các vấn đề mở

1. **Cải thiện độ chính xác trong các trường hợp bất thường**
2. **Khả năng xử lý trong môi trường phân tán**:
3. **Cải tiến thuật toán để giảm thiểu sai số trong môi trường không đồng đều**:
4. **Tăng tính linh hoạt cho các cấu trúc tóm gọn**:
5. **Đo lường và quản lý độ tin cậy của các ước lượng**:













