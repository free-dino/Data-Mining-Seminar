**Vấn đề phát hiện ngoại lệ trong phát trực tuyến** thường xuất hiện trong bối cảnh dữ liệu đa chiều hoặc dòng dữ liệu chuỗi thời gian. Việc phát hiện ngoại lệ trong dòng dữ liệu đa chiều nói chung khá khác biệt so với phát hiện ngoại lệ trong chuỗi thời gian. Trong trường hợp sau, mỗi chuỗi thời gian được coi là một đơn vị, trong khi các mối quan hệ tương quan theo thời gian thường yếu hơn trong dữ liệu đa chiều. Phần này sẽ chỉ đề cập đến phát hiện ngoại lệ trong dòng dữ liệu đa chiều.

 Có hai loại ngoại lệ có thể xuất hiện trong dòng dữ liệu đa chiều:

1. **Ngoại lệ dựa trên phát hiện ở các bản ghi cá nhân.**  
    một điểm dữ liệu đơn lẻ nổi bật, ví dụ, một tin tức quan trọng đầu tiên trên một chủ đề. Điều này thường là tín hiệu ban đầu của một sự kiện mới.Ngoại lệ này cũng thường được gọi là **sự mới lạ**.
    
2. **Ngoại lệ dựa trên thay đổi trong xu hướng tổng hợp của dữ liệu đa chiều.**  
	 Những thay đổi trong xu hướng chung, chẳng hạn một loạt tin tức về một chủ đề do một sự kiện lớn. Điều này phản ánh sự thay đổi đáng kể trong dữ liệu.
    Ví dụ, một sự kiện bất thường như một cuộc tấn công khủng bố có thể dẫn đến một loạt các tin tức về một chủ đề cụ thể. Điều này đại diện cho một ngoại lệ tổng hợp dựa trên một khung thời gian cụ thể. Loại thay đổi điểm này thường bắt đầu với một ngoại lệ cá nhân thuộc loại đầu tiên. Tuy nhiên, một ngoại lệ cá nhân không phải lúc nào cũng phát triển thành một điểm thay đổi tổng hợp. Khái niệm này liên quan chặt chẽ đến **trôi dạt khái niệm** (concept drift). Khi sự thay đổi của dữ liệu và xu hướng diễn ra từ từ, đó là **trôi dạt khái niệm**. Nhưng nếu sự thay đổi xảy ra đột ngột (ví dụ, một sự kiện bất ngờ), thì đó là **ngoại lệ**
    
Cả hai loại ngoại lệ (hoặc điểm thay đổi) sẽ được thảo luận trong phần này.

# 1. Individual Data Points as Outliers
(Các điểm dữ liệu cá nhân là ngoại lệ)

Một điểm dữ liệu cá nhân có thể là ngoại lệ nếu nó nằm ngoài phạm vi bình thường của các điểm trong một khoảng thời gian nhất định (cửa sổ thời gian W).

- **Dựa vào đâu để phát hiện ngoại lệ?**
    -> **Khoảng cách k-láng giềng gần nhất:** Điểm ngoại lệ được xác định dựa trên khoảng cách từ điểm đó đến k điểm gần nhất khác trong cửa sổ dữ liệu. Nếu khoảng cách này lớn hơn một ngưỡng xác định, điểm đó có thể được coi là ngoại lệ.
    
- **Thách thức trong dòng dữ liệu phát trực tuyến:**
    Dữ liệu luôn thay đổi: Có điểm dữ liệu mới được thêm vào và điểm cũ bị xóa đi.
    -> Do đó, điểm ngoại lệ phải được cập nhật liên tục để phản ánh trạng thái dữ liệu hiện tại.
    
- **Thuật toán LOF (Local Outlier Factor):**
    - **LOF là gì?** LOF đánh giá mức độ mà một điểm dữ liệu bị cô lập so với các điểm lân cận của nó.
    - Khi dữ liệu mới xuất hiện, thuật toán thực hiện hai việc:
        - **Tính toán điểm cho dữ liệu mới:** Bao gồm khoảng cách tiếp cận và điểm LOF.
        - **Cập nhật điểm cho các dữ liệu cũ bị ảnh hưởng:** Các điểm xung quanh dữ liệu mới hoặc bị xóa sẽ được cập nhật, nhưng những điểm không bị ảnh hưởng thì không cần thay đổi.

Do các phương pháp dựa trên khoảng cách có chi phí tính toán cao, nên nhiều phương pháp đã đề cập trước đây vẫn khá tốn kém trong bối cảnh dữ liệu phát trực tuyến. Vì vậy, độ phức tạp của quá trình phát hiện ngoại lệ có thể được cải thiện đáng kể bằng cách sử dụng phương pháp trực tuyến dựa trên phân cụm. Phương pháp **microclustering** có thể tự động phát hiện ngoại lệ cùng với việc phát hiện các cụm.

- **Phương pháp microclustering:**
    - Microclustering là một kỹ thuật phân cụm cho phép phát hiện đồng thời các cụm và các điểm ngoại lệ trong dòng dữ liệu.
    - Thay vì kiểm tra từng điểm dữ liệu, microclustering tổ chức dữ liệu thành các cụm nhỏ gọn, giúp giảm đáng kể chi phí tính toán.
    Ví dụ, thuật toán **CluStream** điều chỉnh một cách rõ ràng việc tạo ra các cụm mới trong dòng dữ liệu khi một điểm dữ liệu đến không nằm trong bán kính thống kê được xác định của các cụm hiện có trong dữ liệu. Các điểm dữ liệu như vậy có thể được coi là ngoại lệ.
    
- **Tại sao phân cụm phù hợp với dòng dữ liệu?**
    - Trong dòng dữ liệu, luôn có một số lượng lớn điểm dữ liệu mới, đủ để duy trì các cụm chi tiết (granular clusters).
    - Các cụm này không chỉ giúp hiểu xu hướng dữ liệu mà còn giúp phát hiện những điểm bất thường.

tóm lại, có thể phát hiện ngoại lệ bằng **hai phương pháp chính** đã đề cập:

1. **Dùng k-nearest neighbor (k-NN) và LOF:**
    - Dựa trên khoảng cách từ một điểm dữ liệu đến k điểm gần nhất trong một cửa sổ dữ liệu (w).
    - Nếu khoảng cách này vượt quá một ngưỡng, điểm đó được coi là ngoại lệ.
    - Phương pháp này hiệu quả trong các kịch bản dữ liệu nhỏ, nhưng khi dữ liệu lớn hoặc liên tục (streaming data), tính toán k-NN trở nên tốn kém về thời gian và tài nguyên.
    
2. **Dùng phân cụm (Clustering):**
    - Dữ liệu được nhóm thành các cụm nhỏ (microclusters) dựa trên sự tương đồng.
    - Một điểm dữ liệu mới được coi là **ngoại lệ** nếu nó nằm ngoài bán kính thống kê của bất kỳ cụm nào.
    - Phương pháp này thích hợp hơn với dòng dữ liệu lớn, vì chỉ cần duy trì thông tin tổng hợp của cụm thay vì kiểm tra từng cặp điểm.

# 2. Aggregate Change Points as Outliers
(Điểm thay đổi tổng hợp là ngoại lệ)
## 2.1. Tóm tắt khái niệm về mật độ vận tốc (velocity density)
Những thay đổi đột ngột trong các xu hướng cục bộ và toàn phần của dữ liệu cơ bản thường chỉ ra các sự kiện bất thường trong dữ liệu. Một cách để đo lường sự **drift** (trôi khái niệm) là sử dụng khái niệm **velocity density** (mật độ vận tốc). Ý tưởng trong việc ước tính mật độ vận tốc là xây dựng hồ sơ mật độ dựa trên vận tốc của dữ liệu. Điều này tương tự với khái niệm ước tính mật độ hạt nhân (kernel density estimation) trong các tập dữ liệu tĩnh.
	Phương pháp ước tính mật độ hạt nhân $\bar{f}(\bar{X})$ cho $n$ điểm dữ liệu và hàm hạt nhân $K_h ^′(⋅)$ được định nghĩa như sau:
$$
	f(\bar{X}) = \frac{1}{n} \sum_{i=1}^{n} K'_h(\bar{X} - \bar{X}_i)

$$

hàm hạt nhân được sử dụng là hàm Gaussian với độ rộng $h$.
$$
K'_h(\bar{X} - \bar{X}_i) \propto e^{-\|\bar{X} - \bar{X}_i\|^2 / (2h^2)}


$$

**Ý nghĩa:**
	- Điểm dữ liệu càng gần $X$, giá trị của hàm hạt nhân càng cao.
    - Băng thông $h$ quyết định độ "mịn" của ước tính mật độ:
        - $h$ lớn: Mật độ được làm mịn nhiều hơn, ít nhạy với các biến động nhỏ.
        - $h$ nhỏ: Mật độ nhạy hơn với biến động, nhưng có thể bị nhiễu.
        - **Silverman’s Rule:** Một phương pháp phổ biến để chọn $h$ tự động dựa trên dữ liệu.
        
Các phép tính mật độ vận tốc được thực hiện trên một cửa sổ thời gian có kích thước $h_t​$. Trực quan cho thấy giá trị $h_t$​ xác định khoảng thời gian mà sự tiến hóa được đo lường.
	Ví dụ: nếu $h_t$ được chọn lớn thì kỹ thuật ước lượng mật độ vận tốc sẽ đưa ra các xu hướng dài hạn, trong khi nếu $h_t$ được chọn nhỏ thì các xu hướng đó tương đối ngắn hạn. 

## 2.2. Mật độ lát cắt thời gian xuôi và ngược (Forward & Reverse Time-Slice Density)

Giả sử t là thời điểm hiện tại và S là tập hợp các điểm dữ liệu đã đến trong cửa sổ thời gian $(t−h_t, t)$. Tốc độ tăng mật độ tại vị trí không gian $\mathbf{X}$ và thời gian t được ước lượng thông qua hai đại lượng:

1. **Ước lượng mật độ theo lát thời gian xuôi** (forward time-slice density estimate).
	Đo lường hàm mật độ cho tất cả các vị trí không gian tại thời điểm t dựa trên tập dữ liệu đã đến trong cửa sổ thời gian quá khứ $(t−h_t, t)$.
	Giả sử dữ liệu thứ i trong tập S được ký hiệu là $(X_i,t_i)$, với i từ 1 đến |S| (số lượng dữ liệu trong S). Khi đó, **Ước lượng mật độ theo lát thời gian xuôi** $F_{(h_s, h_t)}(X,t)$ tại vị trí không gian X và thời gian t được định nghĩa như sau:

$$
\
F_{(h_s, h_t)}(\mathbf{X}, t) = C_f \cdot \sum_{i=1}^{|S|} K_{(h_s, h_t)}(\mathbf{X} - \mathbf{X}_i, t - t_i)
\

$$
Trong đó:

- $K_{(h_s, h_t)}(⋅)$: Hàm làm mượt hạt nhân không-thời gian.
- $h_s$​: Bước làm mượt không gian.
- $h_t​:$ Bước làm mượt thời gian.
- $C_f$​: Hằng số chuẩn hóa để đảm bảo tổng mật độ trên không gian bằng 1, được định nghĩa bởi:
$$
\
\int_{\text{Tất cả } \mathbf{X}} F_{(h_s, h_t)}(\mathbf{X}, t) \, \delta \mathbf{X} = 1
\

$$

 2. **Ước lượng mật độ theo lát thời gian ngược** (reverse time-slice density estimate).
	đo lường hàm mật độ tại t dựa trên tập dữ liệu sẽ đến trong cửa sổ thời gian tương lai 
	$(t, t+h_t)$. Tuy nhiên, giá trị này không thể tính toán được cho đến khi các điểm dữ liệu thực sự xuất hiện.
	Giả sử tập hợp các điểm dữ liệu trong khoảng thời gian tương lai $(t, t+h_t)$ được ký hiệu là U. Tương tự như trước, giá trị $C_r$ được chọn làm hằng số chuẩn hóa.
	Khi đó, **Ước lượng mật độ lát thời gian ngược** $R_{(h_s, h_t)}(X, t)$ được định nghĩa như sau:
$$
	\
R_{(h_s, h_t)}(\mathbf{X}, t) = C_r \cdot \sum_{i=1}^{|U|} K_{(h_s, h_t)}(\mathbf{X} - \mathbf{X}_i, t_i - t)
\

$$
		Trong đó:
		- Tập U bao gồm các điểm dữ liệu trong tương lai.
		- $C_r​$: Hằng số chuẩn hóa để đảm bảo tổng mật độ trên toàn bộ không gian bằng 1 
		
	Mật độ vận tốc $V_{(h_s, h_t)}(X, T$) tại vị trí không gian X và thời gian T được định nghĩa như sau:
$$
	\
V_{(h_s, h_t)}(\mathbf{X}, T) = \frac{F_{(h_s, h_t)}(\mathbf{X}, T) - R_{(h_s, h_t)}(\mathbf{X}, T - h_t)}{h_t}
\

$$
	Lưu ý rằng ước tính mật độ lát cắt thời gian ngược được xác định bằng đối số tạm thời là     $(T − h_t)$, và do đó các điểm trong tương lai đối với $(T − h_t)$ đã biết tại thời điểm T. Giá trị dương của mật độ vận tốc tương ứng với sự gia tăng mật độ dữ liệu tại một điểm nhất định. Giá trị âm của mật độ vận tốc tương ứng với việc giảm mật độ dữ liệu tại một điểm nhất định. Nói chung, người ta đã chứng minh rằng khi hàm nhân không gian thời gian được xác định như dưới đây thì mật độ vận tốc tỷ lệ thuận với tốc độ thay đổi của mật độ dữ liệu tại một điểm nhất định
$$
	K_{(h_s, h_t)}(\mathbf{X}, t) = \left(1 - \frac{t}{h_t}\right) \cdot K'_{h_s}(\mathbf{X})
$$

- **Outliers (điểm bất thường)** trong ngữ cảnh này là các điểm dữ liệu X tại các thời điểm t mà có giá trị **vận tốc mật độ** (velocity density) bất thường lớn (hoặc nhỏ) so với bình thường.
- Sử dụng giá trị tuyệt đối của **vận tốc mật độ địa phương** để phát hiện sự thay đổi đột ngột.
