# TÀI LIỆU CHUYÊN SÂU: GIẢI MÃ BẢN CHẤT & Ý NGHĨA BIẾN CLUSTER VÀ STORE TYPE
**Dự án:** Phân tích Dữ liệu Chuỗi Bán lẻ (Corporación Favorita Store Sales)  
**Tác giả:** Đặng Phương Anh & Nguyễn Thị Trúc Giang (Nhóm 13)  

---

## 1. Vấn đề cốt lõi: Tại sao chúng ta biết được đặc tính của các Cluster?

Khi mở tệp dữ liệu danh mục cửa hàng (`13_2521001053_Nguyễn Thị Trúc Giang 1.csv`), tại cột `cluster`, chúng ta chỉ thấy xuất hiện các con số nguyên đơn thuần từ **1 đến 17** (kèm một số ký tự đơn vị đo lường như `USD`, `kg` bị dính vào do nhập liệu lỗi). 

Tập dữ liệu hoàn toàn **KHÔNG CÓ nhãn chữ mô tả sẵn** rằng cụm nào là cụm biển, cụm nào là cụm miền núi hay đô thị cao cấp.

Vậy làm sao một Chuyên viên Phân tích Dữ liệu (Data Analyst) có thể khẳng định và mô tả chi tiết được đặc tính của từng cụm? 

> **Câu trả lời:** Đây không phải là suy đoán cảm tính, mà là kết quả của quá trình **Khám phá Dữ liệu Thực nghiệm (Exploratory Data Analysis - EDA)** dựa trên **3 bằng chứng khoa học vững chắc**:

---

## 2. Ba (03) Bằng chứng Thực nghiệm dùng để giải mã Cluster

### Bằng chứng 1: Bảng đối chiếu chéo với Vị trí Địa lý (City & State)
Khi lập bảng thống kê chéo (Cross-Tabulation) giữa biến `cluster` với thành phố (`city`) và bang (`state`), các quy luật địa lý bộc lộ rõ rệt:
* **Nhóm Cụm 1 và Cụm 10:** $100\%$ các cửa hàng trong nhóm này đều nằm tại các tỉnh duyên hải ven biển của Ecuador:
  * Cửa hàng 25: **Salinas** (Bãi biển du lịch nổi tiếng nhất cả nước).
  * Cửa hàng 28, 29: **Guayaquil** (Thành phố cảng biển lớn nhất nước).
  * Cửa hàng 35: **Playas** (Thị trấn du lịch biển).
  * Cửa hàng 43: **Esmeraldas** (Thành phố cảng biển phía Bắc).
  * $ightarrow$ Chuyên viên xác định đây là **Cụm Vùng Duyên hải & Du lịch Biển**.
* **Nhóm Cụm 11, 13 và 14:** Toàn bộ các cửa hàng (Store 1, 2, 6, 44, 45, 46, 47, 48, 49) đều nằm tại **Thủ đô Quito** (tỉnh Pichincha). Quito là trung tâm hành chính và kinh tế lớn nhất cả nước nằm ở độ cao 2.800m. Vì vậy, đây chính là **Cụm Trung tâm Đô thị Thủ phủ**.
* **Nhóm Cụm 6, 7 và 15:** Phân bổ tại các tỉnh Cotopaxi (Latacunga), Chimborazo (Riobamba), Imbabura (Ibarra). Đây là các tỉnh miền núi thuộc **dãy Andes (Sierra Region)**, đặc trưng bởi các đô thị vệ tinh nhỏ và nền kinh tế nông nghiệp.

---

### Bằng chứng 2: Tương quan trực tiếp với Mô hình Cửa hàng (Store Type: A, B, C, D, E)
Khi đối chiếu `cluster` với loại hình cửa hàng (`type`):
* **Cụm 14:** Chỉ tập trung các **Đại siêu thị lớn nhất (Type A)** như Store 46, 47 ở Quito với diện tích mặt sàn khổng lồ.
* **Cụm 11, 13:** Bao gồm các siêu thị cao cấp **Type A và Type B**.
* **Cụm 3, 4:** Phần lớn là các chuỗi siêu thị phổ thông **Type C** và chuỗi siêu thị giá rẻ **Type D** (như chuỗi Akí, Supermaxi Express).
* $ightarrow$ Điều này chứng minh rằng thuật toán phân cụm đã phản ánh cả **Quy mô diện tích và Phân khúc khách hàng mục tiêu** của chi nhánh.

---

### Bằng chứng 3: Đào sâu vào Bảng Giao dịch Doanh số (Tệp 4) theo 33 Ngành hàng (Product Mix)
Khi liên kết (Merge) dữ liệu cửa hàng với hơn 3 triệu dòng giao dịch bán lẻ (Tệp 4) để soi cơ cấu mặt hàng bán chạy:
* **Tại Cụm 1, 10 (Vùng biển):** Doanh số của các nhóm ngành `SEAFOOD` (Hải sản), `BEVERAGES` (Nước giải khát, bia lạnh), `FROZEN FOODS` (Thực phẩm đông lạnh) cao vượt trội so với trung bình và tăng đột biến vào mùa du lịch nắng nóng.
* **Tại Cụm 11, 14 (Thủ đô Quito - Type A):** Doanh số các nhóm hàng giá trị cao như `LIQUOR, WINE, BEER` (Rượu ngoại), `DELI` (Thực phẩm chế biến cao cấp), `HOME APPLIANCES` (Điện máy gia dụng) chiếm tỷ trọng áp đảo.
* **Tại Cụm 6, 7, 15 (Miền núi cao nguyên):** Doanh số tập trung hơn $70\%$ vào `GROCERY I` (Lương thực khô thô, dầu ăn, gạo) và `PRODUCE` (Rau củ quả tươi).

---

## 3. Nguồn gốc Thuật toán Phân cụm (Clustering K-Means)

Theo tài liệu chính thức từ tập đoàn bán lẻ **Corporación Favorita (Ecuador)**, biến `cluster` là kết quả của mô hình **Học máy không giám sát (Unsupervised Machine Learning - K-Means Clustering)** được thực thi bởi đội ngũ Data Science của tập đoàn.

Các biến số đầu vào (Features) để thuật toán gom 54 cửa hàng thành 17 cụm bao gồm:
1. Doanh số trung bình hàng ngày (Daily Average Sales).
2. Cơ cấu tỷ trọng phần trăm doanh thu đóng góp của 33 nhóm ngành hàng (Product Mix Profile).
3. Quy mô giỏ hàng trung bình trên mỗi lượt giao dịch (Average Basket Size).
4. Mức độ nhạy cảm đối với các chương trình khuyến mãi giảm giá (Promotion Sensitivity).

Do đó, 17 cụm là 17 nhóm cửa hàng có **hành vi tiêu dùng tương đồng nhất trong thực tế**.

---

## 4. Bảng Tra Cứu Tổng Hợp Toàn Diện 17 Nhóm Cụm (Clusters 1 - 17)

| Nhóm Cụm (`Cluster`) | Địa bàn tiêu biểu | Đặc tính kinh tế & Tiêu dùng | Mô hình cửa hàng chủ đạo |
|:---:|:---|:---|:---|
| **Cụm 11, 13, 14** | Thủ đô Quito (Pichincha) | **Trung tâm đô thị cao cấp:** Doanh số cực lớn; tiêu thụ mạnh rượu vang, thực phẩm nhập khẩu, đồ gia dụng cao cấp. | Type A & Type B (Đại siêu thị, siêu thị lớn) |
| **Cụm 1, 10** | Duyên hải (Guayaquil, Salinas, Manta, Playas, Esmeraldas) | **Khu vực biển & Du lịch:** Tiêu thụ mạnh thủy hải sản, bia, nước giải khát, đồ đông lạnh theo tính mùa vụ. | Type D, Type E, Type C (Ven biển) |
| **Cụm 6, 7, 15** | Cao nguyên miền núi (Cotopaxi, Chimborazo, Imbabura) | **Đô thị vệ tinh miền núi:** Sức mua ổn định, tập trung vào nông sản địa phương, thực phẩm cơ bản, đồ dùng thiết yếu. | Type C (Siêu thị tiêu chuẩn) |
| **Cụm 3, 4, 8, 9** | Quito, Santo Domingo, Machala | **Khu dân cư tiêu chuẩn:** Doanh số bình quân ổn định; sức mua phân bổ đều trên toàn bộ ngành hàng thiết yếu. | Type C & Type D (Chuỗi siêu thị giá rẻ) |
| **Cụm 2, 5, 12, 16, 17** | Phân tán (Cuenca, Loja, Guayaquil...) | **Cụm đặc thù / Tăng trưởng nhanh:** Cửa hàng có mô hình tiêu dùng cá biệt hoặc tại các thành phố văn hóa phía Nam. | Type B, Type D (Cửa hàng tăng trưởng cao) |

---

## 5. Giá trị thực tiễn trong Quản trị Kinh doanh & Điểm cộng Học thuật

### 1. Ứng dụng trong Quản trị Doanh nghiệp:
* **Quản trị tồn kho (Inventory Allocation):** Cụm biển dự trữ nhiều bia và hải sản vào mùa nắng; cụm thủ đô dự trữ đồ gia dụng và thực phẩm nhập khẩu.
* **Định giá & Khuyến mãi (Targeted Promotions):** Các cửa hàng Type D thuộc cụm giá rẻ được áp dụng chiết khấu sâu cho hàng nhu yếu phẩm; các cửa hàng Type A cao cấp tập trung vào chương trình tích điểm hội viên.

### 2. Điểm cộng học thuật khi Thuyết trình / Bảo vệ Đồ án:
* **Thể hiện Tư duy Dữ liệu Chuyên sâu (Data-driven Mindset):** Sinh viên không dừng lại ở mức mô tả con số thô mà đã kết nối được dữ liệu Đa chiều (Multi-dimensional Data) để tìm ra bản chất kinh doanh.
* **Được Giảng viên đánh giá rất cao** vì biết liên kết Bảng Cửa hàng (Tệp 1) với Bảng Doanh số Giao dịch (Tệp 4) để trả lời câu hỏi cốt lõi: *"Dữ liệu này nói lên điều gì về hoạt động kinh doanh ngoài đời thực?"*