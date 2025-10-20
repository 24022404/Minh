# 📈 Stock Price Big Data Analysis - Project Minh

Hệ thống phân tích dữ liệu chứng khoán quy mô lớn sử dụng Hadoop HDFS và Apache Spark.

---

## 🎯 Mục tiêu

- Xây dựng hệ thống Big Data phân tán
- Lưu trữ và xử lý dữ liệu chứng khoán
- Phân tích dữ liệu bằng PySpark
- Hiển thị kết quả qua Web Dashboard

---

## 🏗️ Kiến trúc

**Hadoop HDFS Cluster:**
- 1 NameNode (quản lý metadata)
- 4 DataNode (lưu trữ phân tán)
- ResourceManager + NodeManager
- HistoryServer

**Apache Spark Cluster:**
- 1 Spark Master
- 4 Spark Workers

**Services:**
- Jupyter Notebook (PySpark)
- Nginx Web Dashboard

---

## 🚀 Quick Start

### 1. Khởi động hệ thống
```bash
docker-compose up -d
```

### 2. Upload dữ liệu lên HDFS
```bash
docker cp ./datack namenode:/datack
docker exec namenode hdfs dfs -put /datack/*.csv /datack/
```

### 3. Chạy phân tích
```bash
# Lấy token Jupyter
docker logs pyspark-notebook | grep token

# Truy cập: http://localhost:8888
# Upload Stock_Analysis.ipynb và Run All
```

### 4. Xem Dashboard
```bash
# Copy results
docker cp pyspark-notebook:/home/jovyan/work/results.json ./web/

# Truy cập: http://localhost:8000
```

---

## 📍 URLs

| Service | URL | Mô tả |
|---------|-----|-------|
| **Dashboard** | http://localhost:8000 | Kết quả phân tích |
| **Jupyter** | http://localhost:8888 | PySpark notebook |
| **NameNode** | http://localhost:9870 | HDFS Web UI |
| **Spark Master** | http://localhost:8080 | Spark cluster |

---

## 📊 Phân tích

1. **Giá trung bình theo năm** - Xu hướng giá
2. **Top 10 cổ phiếu volatile** - Độ biến động
3. **So sánh công ty lớn** - AAPL, GOOG, AMZN, MSFT

---

## 📂 Cấu trúc

```
Minh/
├── docker-compose.yml
├── hadoop.env
├── base/
├── namenode/
├── datanode/
├── resourcemanager/
├── nodemanager/
├── historyserver/
├── datack/              # Dữ liệu CSV
├── notebook/
│   └── Stock_Analysis.ipynb
├── web/
│   └── index.html
└── README.md
```

---

## 🛠️ Công nghệ

- **Hadoop** 3.2.1
- **Spark** 3.0.1
- **Docker** & Docker Compose
- **PySpark** + Python
- **Chart.js** (visualization)

---

## 📝 Tài liệu

Xem chi tiết trong:
- `QUICKSTART.md` - Hướng dẫn nhanh
- `report/Report.md` - Báo cáo chi tiết

---

**Tác giả:** Minh  
**Ngày:** 2025