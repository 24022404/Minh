# 🚀 QUICK START - 5 PHÚT CHẠY XONG

## Bước 1: Start (2 phút)
```bash
docker-compose up -d
# Đợi 2-3 phút để services khởi động
```

## Bước 2: Upload data (1 phút)
```bash
docker cp ./datack namenode:/datack
docker exec namenode hdfs dfs -put /datack/*.csv /datack/
```

## Bước 3: Run analysis (1 phút)
```bash
# Lấy Jupyter token
docker logs pyspark-notebook | grep token

# Mở browser: http://localhost:8888
# Paste token, upload Stock_Analysis.ipynb, chạy Run All
```

## Bước 4: View dashboard (30 giây)
```bash
# Copy results
docker cp pyspark-notebook:/home/jovyan/work/results.json ./web/

# Mở browser: http://localhost:8000
```

## ✅ DONE! 

### Check các URL:
- Dashboard: http://localhost:8000
- Jupyter: http://localhost:8888
- HDFS: http://localhost:9870
- Spark: http://localhost:8080

---

## 🐛 Troubleshooting

**Lỗi port đã dùng?**
```bash
docker-compose down
sudo lsof -i :8888  # tìm process
kill -9 <PID>
```

**Container không start?**
```bash
docker-compose logs namenode
docker-compose restart
```

**HDFS không có data?**
```bash
docker exec namenode hdfs dfs -ls /datack
```