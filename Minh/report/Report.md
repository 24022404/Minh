# BÁO CÁO ĐỒ ÁN BIG DATA
**Đề tài: Hệ thống phân tích giá cổ phiếu Big Data**  
**Project: Minh**

---

## 1. TỔNG QUAN DỰ ÁN

### 1.1 Mục tiêu
- Xây dựng hệ thống Big Data phân tán với Hadoop và Spark
- Lưu trữ dữ liệu chứng khoán trên HDFS
- Phân tích dữ liệu bằng PySpark
- Hiển thị kết quả qua Web Dashboard

### 1.2 Phạm vi
- Dữ liệu: Giá cổ phiếu thị trường Mỹ (1999-2020)
- Quy mô: Hàng nghìn công ty, hàng triệu giao dịch
- Công nghệ: Hadoop 3.2.1, Spark 3.0.1, Docker

---

## 2. KIẾN TRÚC HỆ THỐNG

### 2.1 Hadoop HDFS Cluster
**NameNode:**
- Vai trò: Quản lý metadata, namespace của HDFS
- Port: 9870 (Web UI)

**DataNode (4 nodes):**
- Vai trò: Lưu trữ dữ liệu phân tán với replication
- Đảm bảo fault-tolerance và high availability

**YARN Services:**
- ResourceManager: Quản lý tài nguyên cluster
- NodeManager: Thực thi tasks trên từng node
- HistoryServer: Lưu lịch sử các jobs

### 2.2 Apache Spark Cluster
**Spark Master:**
- Điều phối xử lý phân tán
- Port: 8080 (Web UI)

**Spark Workers (4 workers):**
- Thực thi các tasks song song
- Tối ưu hóa performance

### 2.3 Application Layer
- **Jupyter Notebook**: Interface cho PySpark
- **Web Dashboard**: Hiển thị kết quả phân tích
- **Nginx**: Web server cho dashboard

---

## 3. TRIỂN KHAI

### 3.1 Containerization với Docker
Sử dụng Docker Compose để orchestrate 10+ services:
- Dễ dàng deploy và scale
- Isolate môi trường
- Reproducible setup

### 3.2 Lưu trữ dữ liệu
**Nguồn dữ liệu:**
- API: AlphaVantage
- Format: CSV files
- Thời gian: 1999-2020

**HDFS Storage:**
```bash
hdfs dfs -put /datack/*.csv /datack/
```
- Data được phân tán trên 4 DataNodes
- Replication factor: 3 (default)

---

## 4. PHÂN TÍCH DỮ LIỆU

### 4.1 Phương pháp
Sử dụng PySpark DataFrame API để:
- Đọc dữ liệu từ HDFS
- Transform và aggregate data
- Tính toán các metrics

### 4.2 Các phân tích thực hiện

**Phân tích 1: Giá trung bình theo năm**
```python
avg_year = df.groupBy("Year").agg(avg("Close").alias("avg"))
```
- Mục đích: Xem xu hướng giá qua các năm
- Output: Line chart

**Phân tích 2: Top 10 cổ phiếu biến động**
```python
volatile = df.groupBy("Company").agg(stddev("Close").alias("vol"))
```
- Mục đích: Xác định cổ phiếu rủi ro cao
- Metric: Standard deviation
- Output: Horizontal bar chart

**Phân tích 3: So sánh công ty lớn**
```python
major = df.filter(col("Company").isin(["AAPL","GOOG","AMZN","MSFT"]))
```
- Mục đích: So sánh giá các tech giants
- Companies: Apple, Google, Amazon, Microsoft
- Output: Bar chart

---

## 5. KẾT QUẢ

### 5.1 Web Dashboard
**Features:**
- 3 biểu đồ interactive với Chart.js
- 1 bảng dữ liệu chi tiết
- Responsive design
- Real-time data loading

**URL:** http://localhost:8000

### 5.2 Performance
- Processing time: ~2-5 phút (tùy data size)
- Distributed processing: 4 Spark workers
- Scalable: Có thể thêm workers khi cần

### 5.3 Screenshots
[Chèn ảnh Dashboard]
[Chèn ảnh HDFS Web UI]
[Chèn ảnh Spark Master UI]

---

## 6. HƯỚNG DẪN SỬ DỤNG

### 6.1 Requirements
- Docker Desktop
- RAM: 8GB minimum (16GB recommended)
- Disk: 20GB

### 6.2 Setup
```bash
# 1. Clone project
git clone <repo-url>

# 2. Start services
docker-compose up -d

# 3. Upload data
docker cp ./datack namenode:/datack
docker exec namenode hdfs dfs -put /datack/*.csv /datack/

# 4. Run analysis
# Access Jupyter: localhost:8888
# Run Stock_Analysis.ipynb

# 5. View dashboard
# Copy results.json to web/
# Access: localhost:8000
```

### 6.3 Troubleshooting
- Xem QUICKSTART.md
- Check logs: `docker-compose logs <service>`

---

## 7. KẾT LUẬN

### 7.1 Đạt được
✅ Xây dựng thành công hệ thống Big Data phân tán  
✅ Lưu trữ và xử lý dữ liệu lớn với HDFS và Spark  
✅ Phân tích dữ liệu hiệu quả với PySpark  
✅ Hiển thị kết quả trực quan qua Web Dashboard  
✅ Đáp ứng đầy đủ yêu cầu đề bài  

### 7.2 Ưu điểm
- Architecture mở rộng được (scalable)
- Fault-tolerant với HDFS replication
- Easy deployment với Docker
- Interactive visualization

### 7.3 Hạn chế
- Chưa có real-time streaming
- Phân tích còn cơ bản
- Chưa có machine learning models

### 7.4 Hướng phát triển
**Ngắn hạn:**
- Thêm phân tích nâng cao (correlation, trends)
- Cải thiện UI/UX của dashboard
- Add authentication cho web dashboard

**Dài hạn:**
- Implement Kafka cho real-time data
- Thêm ML models (price prediction)
- Scale to production với Kubernetes

---

## 8. TÀI LIỆU THAM KHẢO

1. Apache Hadoop Documentation
2. Apache Spark Documentation
3. Docker Documentation
4. AlphaVantage API

---

**Sinh viên thực hiện:** Minh  
**Ngày hoàn thành:** [Điền ngày]  
**Lớp:** [Điền lớp]  
**MSSV:** [Điền MSSV]