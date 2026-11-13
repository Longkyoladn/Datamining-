# Phân cụm khách hàng sàn thương mại điện tử

Bài thực nghiệm giữa kỳ môn Phân tích và Khai phá dữ liệu, Trường Đại học Công nghệ — ĐHQGHN. Mã sinh viên: 24022391 (N = 220 bản ghi, 12 thuộc tính, tối thiểu 11 ngoại lai).

Bài làm sinh dữ liệu khách hàng giả lập, đưa vào giá trị thiếu, nhiễu và ngoại lai, sau đó thực hiện EDA, tiền xử lý và chuẩn bị đặc trưng. Cuối cùng, bài so sánh K-Means trên dữ liệu chưa xử lý (baseline) và dữ liệu đã xử lý (processed).

## Cấu trúc

```
customer_segmentation/
├── data/
│   ├── customers_original.csv       dữ liệu sạch
│   ├── customers_corrupted.csv      sau khi thêm missing, noise, outliers
│   ├── customers_preprocessed.csv   sau tiền xử lý (thang đo nghiệp vụ)
│   ├── customers_prepared.csv       ma trận đặc trưng 220 × 20 cho K-Means
│   └── prepared_customer_ids.csv    ánh xạ dòng -> customer_id
├── metadata/simulation_metadata.csv nhãn giả lập và cờ can thiệp (không dùng để huấn luyện)
├── notebooks/customer_segmentation.ipynb
├── figures/                         biểu đồ, tiền tố theo câu (a_, b_, ...)
├── results/
│   ├── clustering_metrics.csv       chỉ số của hai nhánh và tham chiếu sạch
│   ├── cluster_profiles.csv         hồ sơ cụm, tên cụm
│   ├── k_selection.csv              khảo sát k = 2..8
│   ├── preprocessing_changes.csv    các ô bị điều chỉnh ở câu c
│   ├── cluster_labels.csv           nhãn cụm theo customer_id
│   └── pipeline_checks.csv          kết quả kiểm chứng tự động
├── reports/
│   ├── report.tex                   báo cáo (XeLaTeX)
│   ├── ai_declaration.tex           khai báo AI và công cụ (XeLaTeX)
│   └── generated/                   macro và bảng LaTeX do notebook sinh ra
├── requirements.txt
└── README.md
```

## Chạy lại

```bash
pip install -r requirements.txt
cd notebooks
jupyter notebook customer_segmentation.ipynb   # Kernel > Restart & Run All
```

Notebook phải chạy tuần tự từ cell đầu. Bộ sinh ngẫu nhiên được khởi tạo bằng `np.random.default_rng(42)`, và K-Means dùng `random_state=42`, `n_init=20`. Phần cuối notebook chạy 12 kiểm tra tự động theo yêu cầu đề bài, kết quả lưu ở `results/pipeline_checks.csv`. Trong các kiểm tra này có bước sinh lại dữ liệu với cùng seed và so khớp với các tệp đã lưu.

## Báo cáo

Báo cáo không chứa số liệu gõ tay. Mọi con số được lấy từ `reports/generated/`, do notebook ghi ra sau khi chạy. Để biên dịch, chạy notebook trước, rồi từ thư mục `reports/`:

```bash
xelatex report.tex
xelatex report.tex          # lần hai để cập nhật tham chiếu
xelatex ai_declaration.tex
xelatex ai_declaration.tex
```

Có thể biên dịch trên Overleaf: tải lên thư mục `reports/` (gồm `generated/`) và `figures/` với cùng cấu trúc tương đối, chọn trình biên dịch XeLaTeX.
