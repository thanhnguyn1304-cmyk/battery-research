# Báo cáo Tiến độ Implement: 

**Đề tài:** Sàng lọc thông số cấu trúc vật liệu Sb@C cho dung lượng chu kỳ cao trong pin Natri-ion bằng Machine Learning.
**Nguồn tham chiếu:** Giữa Bài báo gốc, *Machine Learning-assisted Structural Parameters Screening of Sb@C - Elsevier 2025*

**Phiên bản:** Bản Implementation hiện tại *(Vers_5_Sb@C composites...)*.

---

## Bước 1: Thu thập và Tiền xử lý dữ liệu (Data Preprocessing)

| Mục tiêu (TODO) | Trạng thái | Ghi chú / Report |
| :--- | :---: | :--- |
| Thu thập 90 mẫu dữ liệu với 6 đặc trưng gốc (IS, AC, RP, AS, DC, N) | ✅ | Dữ liệu đã được load đủ 90 dòng với các cột tương ứng. |
| Bổ sung Dung lượng ban đầu (IC) thông qua phương pháp CEP | ⚠️ | **Sai khác logic:** Bài báo dùng AC để nội suy IC. Trong code hiện đang dùng IC để điền khuyết (fillna) cho AC. |
| Xử lý giá trị khuyết thiếu (NaN) | ⚠️ | Có thay đổi: Sử dụng `KNNImputer` kết hợp `CEP` để làm sạch dữ liệu. Do mô tả sơ lược của bài báo không rõ ràng. |
| Chuẩn hóa dữ liệu (StandardScaler) | ✅ | Đã thực hiện Scaling trước khi đưa vào mô hình. |
| Chia tập dữ liệu Train/Test (8:2) | ✅ | Đã thực hiện `train_test_split` với `test_size=18/90`. |
| Feature Engineering | ⚠️ | Lược bỏ. Do bộ dữ liệu nhỏ nên dễ over-engineering, dẫn tới tạo quá nhiều nhiễu trong quá trình training model |

---

## Bước 2: Xây dựng và Huấn luyện mô hình (Model Training)

| Mục tiêu (TODO) | Trạng thái | Ghi chú / Report |
| :--- | :---: | :--- |
| Triển khai 4 thuật toán: RF, XGBoost, Adaboost, SVR | ✅ | Đã cài đặt đủ 4 thuật toán và sử dụng `Optuna` để tối ưu tham số. |
| Parameter Tuning | ✅ | Sử dụng `Optuna` để tối ưu tham số (tránh lạm dụng feature, hàm mục tiêu,...) |
| Kiểm tra chéo 10-fold cross-validation | ✅ | Đã thực hiện thông qua hàm `run_kfold_wrapper`. |
| Đánh giá bằng chỉ số $R^2$, RMSE, MAE | ✅ | Đã tính toán đầy đủ các chỉ số trên cả tập Train và Test. |
| Đạt hiệu suất tương đương bài báo ($R^2$ Test ~ 0.83) | ❌ | Hiệu suất hiện tại cao nhất là SVR ($R^2 \approx 0.68$) và AdaBoost ($R^2 \approx 0.64$), thấp hơn mục tiêu 0.83. |

---

## Bước 3: Phân tích và Giải thích mô hình (Model Explanation)

| Mục tiêu (TODO) | Trạng thái | Ghi chú / Report |
| :--- | :---: | :--- |
| Phân tích hệ số tương quan Pearson | ✅ | Đã thực hiện vẽ Heatmap và biểu đồ hồi quy tuyến tính. |
| Giải thích mô hình bằng SHAP (Global & Local) | ✅ | Đã hoàn thành vẽ biểu đồ. Model thiếu chính xác nên kết quả không thể sử dụng cho phân tích. |
| Giải thích mô hình bằng Permutation Importance | ⚠️ | Bài báo không sử dụng phương pháp này. Permutation Importance giúp xác định mức độ sử dụng đặc trưng của mô hình, nhưng không chỉ ra được hướng tác động (âm/dương) chi tiết như SHAP. |

---

## Bước 4: Sàng lọc và Tối ưu hóa (Screening & Prediction)

| Mục tiêu (TODO) | Trạng thái | Ghi chú / Report |
| :--- | :---: | :--- |
| Tạo tập dữ liệu cấu trúc nhân tạo (Exhaustive method) | ❌ | Cần viết thêm script tạo tổ hợp các biến (Grid search) để quét không gian vật liệu. |
| Nội suy IC cho tập nhân tạo bằng 4-means clustering | ❌ | Bước này cần thiết để đảm bảo tính vật lý cho dữ liệu giả lập. |
| Dự đoán dung lượng RC cho tập dữ liệu nhân tạo | ❌ |  |
| Trực quan hóa vùng tối ưu bằng Contour maps | ❌ |  |

---

## Bước 5: Đánh giá khả năng tổng quát hóa (Generalization)

| Mục tiêu (TODO) | Trạng thái | Ghi chú / Report |
| :--- | :---: | :--- |
| Thử nghiệm mô hình với các hệ vật liệu khác (Ge, Sn, Bi, Fe) | ✅ | Đã code. |
| Đặt hiệu xuất tương đương bài báo (Loss rate < 15%). | ❌ | Model chưa đạt độ chính xác để thử nghiệm. |

---

## Công việc còn lại
1.  **Chỉnh sửa logic CEP:** Đảm bảo việc tạo biến IC/AC tuân thủ đúng quy luật vật lý như mô tả trong nghiên cứu gốc.
2.  **Cải tiến hiệu suất mô hình:** ...
3.  **Các task liên quan tới mô phỏng mở rộng (Bước 4)**: ...
4.  **Thực hiện Exhaustive Screening:** Tạo loop để sinh dữ liệu giả lập và vẽ Contour Map để tìm sweet spot của cấu trúc vật liệu.

---