Mục tiêu:
Cung cấp các kiến thức về: 
- Luồng dữ liệu
- Các cấu trúc dữ liệu hiệu quả
- Các thuật toán để khai phá mẫu (pattern)
- Các thuật toán phân cụm
- Thuật toán phát hiện điểm ngoại lai (outlier)
- Phân lớp trong luồng dữ liệu

# Phần 1: Giới thiệu về luồng dữ liệu (Quyển 2-3)
- ## 1.1. Tổng quan
  - Định nghĩa và tính chất của luồng dữ liệu
  - Ví dụ thực tế về luồng dữ liệu
  - Giới thiệu về hệ thống quản lý luồng dữ liệu (DSMS)
- ## 1.2 Các thách thức chính trong xử lý luồng dữ liệu
  - Yêu cầu phân tích thời gian thực
  - Giới hạn của bộ nhớ và bộ tính toán
  - Trôi dạt khái niệm (concept drift) và sự thay đổi phân bố dữ liệu
# Phần 2: Mô hình luồng dữ liệu và truy vấn (Quyển 3)
- ## 2.1 Mô hình luồng dữ liệu
  - Hiểu về các mô hình luồng dữ liệu cơ bản
  - Các nguồn luồng dữ liệu và truy vấn trên luồng dữ liệu
  - Những vấn đề chính trong xử lý luồng như độ trễ, lưu trữ và cập nhật mô hình
- ## 2.2 Lọc và Lấy mẫu luồng dữ liệu
  - Tầm quan trọng của việc lọc và lấy mẫu
  - Kỹ thuật lấy mẫu đại diện từ luồng dữ liệu
  - Ứng dụng của lọc với bộ lọc Bloom
# Phần 3: Cấu trúc dữ liệu tóm gọn cho luồng dữ liệu (Quyển 2-3)
- ## 3.1 Lấy mẫu dự trữ (Reservoir Sampling) (2-3)
  - Giới thiệu về lấy mẫu dự trữ
  - Xử lý trôi dạt khái niệm và các kĩ thuật lấy mẫu adaptive (thích ứng)
  - Các giới hạn lý thuyết cho lấy mẫu
- ## 3.2 Cấu trúc tóm gọn cho các miền (domain) lớn (2-3)
  - Tổng quan về cấu trúc tóm gọn cho các miền lớn
  - **Cấu trúc dữ liệu**:
    - Bộ lọc Bloom (2-3)
    - Count-min Sketch (2)
    - AMS Sketch (2-3)
    - Thuật toán Flajolet-Martin (đếm các phần tử duy nhất) (2-3)
  - So sánh các cấu trúc dữ liệu: các trường hợp sử dụng, điểm mạnh và hạn chế
# Phần 4: Kỹ thuật đếm và Ước lượng mô-men (moments) (3)
- ## 4.1 Đếm các phần tử duy nhất:
  - Vấn đề Đếm Phần tử Duy nhất và các thách thức
  - Thuật toán Flajolet-Martin: Nguyên lý và ứng dụng
  - Yêu cầu về không gian và kết hợp các ước lượng
- ## 4.2 Ước lượng các mô-men
  - Hiểu về các mô-men thống kê trong luồng dữ liệu
  - Thuật toán Alon-Matias-Szegedy (AMS) cho mô-men bậc 2
  - Mô-men bậc cao và xử lý luồng dữ liệu vô hạn
- ## 4.3 Đếm số lượng 1 trong 1 cửa sổ
  - Giới thiệu về thuật toán Datar-Gionis-Indyk-Motwani (DGIM)
  - Trả lời truy vấn, giảm thiểu lỗi và mở rộng đếm số 1
- ## 4.4 Cửa sổ suy giảm (decaying windows)
  - Định nghĩa và tầm quan trọng của cửa sổ suy giảm
  - Tìm các phần tử phổ biến nhất với cửa sổ suy giảm
# Phần 5: Khai phá mẫu thường xuyên trong luồng dữ liệu (2)
- ## 5.1 Tận dụng cấu trúc tóm gọn
  - Sử dụng lấy mẫu dự trữ và Sketchs cho khai phá mẫu
  - Thuật toán đếm mất mát (Lossy Counting) để phát hiện mẫu thường xuyên
  - Ứng dụng thức tế và các cân nhắc về hiệu suất.
# Phần 6: Phân cụm luồng dữ liệu: (2)
- ## 6.1 Tổng quan về Phân cụm Luồng dữ liệu
  - Thách thức của phân cụm các luồng dữ liệu động, có khối lượng lớn
- ## 6.2 Các thuật toán phân cụm chính
  - Thuật toán STREAM: Giới thiệu và ứng dụng
  - Thuật toán CluStream:
    - Định nghĩa vi cụm (Microcluster)
    - Thuật toán vi cụm và khung thời gian hình tháp
  - Phân cụm trong các miền lớn
# Phần 7: Phát hiện ngoại lai trong Luồng dữ liệu (2)
- ## 7.1 Kỹ thuật phát hiện ngoại lai
  - Phát hiện các điểm dữ liệu cá nhân là ngoại lệ
  - Xác định các điểm thay đổi tổng hợp (aggregate change points) như các dấu hiệu của ngoại lệ
  - Chiến lược giảm thiểu dương tính giả (false positive) trong luồng dữ liệu lớn.
# Phần 8: Phân lớp luồng dữ liệu(2):
- ## Giới thiệu về Phân lớp luồng dữ liệu:
  - Các thách thức chính trong phân loại luồng dữ liệu
  - Thuật toán và phương pháp:
    - Họ Cây Quyết Định rất nhanh (VFDT)
    - Các phương pháp vi cụm có giám sát
    - Các phương pháp hợp (ensemble) để nâng cao độ chính xác
  - Phân lớp trong các miền lớn và xử lý trôi dạt khái niệm.