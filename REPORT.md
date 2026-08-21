# Báo Cáo Lab Day 21 – CI/CD cho AI Systems

**Họ tên:** Phạm Văn Thắng  
**MSSV:** 2A202601359  
**Repo:** https://github.com/thangph18/K3A-Track2-Day21-2A202601359-PhamVanThang

---

## 1. Bộ Siêu Tham Số Đã Chọn

Sau khi thực hiện 3 thí nghiệm trên MLflow (Bước 1), bộ siêu tham số tốt nhất được chọn:

| Tham số | Giá trị |
|---|---|
| `n_estimators` | 100 |
| `max_depth` | 15 |
| `criterion` | entropy |
| `min_samples_split` | 2 |

**Lý do chọn:** Bộ tham số này đạt accuracy cao nhất (0.6840) trên tập đánh giá `eval.csv` khi huấn luyện trên `train_phase1.csv` (2998 mẫu). Việc dùng `criterion: entropy` (information gain) cho kết quả tốt hơn so với `gini` mặc định trên bộ dữ liệu wine quality này.

---

## 2. So Sánh Kết Quả Bước 2 và Bước 3

| Chỉ số | Bước 2 (2998 mẫu) | Bước 3 (5996 mẫu) | Thay đổi |
|---|:---:|:---:|---|
| **Accuracy** | 0.6840 | 0.7460 | +6.20% |
| **F1-Score** | 0.6826 | 0.7448 | +6.22% |

Nhận xét: Bổ sung thêm 2998 mẫu dữ liệu mới giúp mô hình tăng đáng kể cả accuracy và F1-score, chứng minh hiệu quả của quy trình huấn luyện liên tục (Continuous Training).

---

## 3. Khó Khăn Gặp Phải và Cách Giải Quyết

| Vấn đề | Nguyên nhân | Cách giải quyết |
|---|---|---|
| `dvc pull` lỗi JSON parse trên GitHub Actions | Nội dung `sa-key.json` bị ngắt dòng khi copy từ terminal vào GitHub Secret | Dùng `cat sa-key.json \| clip.exe` để copy chính xác; thêm `json.loads()` trong workflow để tự chuẩn hóa |
| Upload model lỗi `Bucket names must start and end with a number or letter` | Secret `CLOUD_BUCKET` chứa tiền tố `gs://` | Thêm logic tự động cắt bỏ `gs://` và ký tự thừa trong workflow |
| Deploy health check thất bại | Service cần 6-8 giây để tải model từ GCS, nhưng `sleep 5` quá ngắn | Thay `sleep 5` bằng vòng lặp retry (thử lại mỗi 2 giây, tối đa 30 giây) |

---

## 4. Danh Sách Ảnh Chụp Minh Chứng

1. `screenshots/1_mlflow_ui.png` – MLflow UI hiển thị 3 thí nghiệm.
2. `screenshots/2_github_actions.png` – GitHub Actions 4 jobs màu xanh.
3. `screenshots/3_vm_curl_test.png` – Kết quả `curl /health` và `curl /predict`.
4. `screenshots/4_cloud_storage.png` – Cloud Storage hiển thị `dvc/` và `models/latest/model.pkl`.
