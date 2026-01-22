# Máy chủ AI nhẹ (CPU-only)

Dành cho máy cấu hình yếu (RAM thấp, không GPU hoặc GPU cũ). Dùng mô hình GGUF (Q4_K_M/Q5_K_M) và tối ưu CPU.

## 1) Cài dependencies
```
py -m pip install -r requirements.txt
```

## 2) Chuẩn bị model
- Tải file GGUF (Q4_K_M/Q5_K_M)
- Đặt vào thư mục `models/`

## 3) Chạy server
```
set MODEL_DIR=models
set DEFAULT_MODEL=llama3.1-8b-instruct-q4_k_m.gguf
py -m uvicorn app.main:app --host 0.0.0.0 --port 8001
```

## 4) One-click (máy yếu)
- `scripts/start_cpu_low.bat`
- `scripts/start_cpu_low.ps1`

## 5) Test nhanh
```
curl http://localhost:8001/health
curl http://localhost:8001/models
```

## 6) Sinh nội dung (stream)
```
curl -N -X POST http://localhost:8001/generate ^
  -H "Content-Type: application/json" ^
  -d "{\"prompt\":\"Xin chao\",\"stream\":true}"
```

## 7) Cấu hình tối ưu CPU
Chỉnh trong `config.json` hoặc biến môi trường:
- `N_CTX` (khuyên 512–2048)
- `N_THREADS` (bằng số core CPU)
- `N_BATCH` (64–128)
- `MAX_TOKENS` (128–512)
- `USE_MMAP=1`, `USE_MLOCK=0`

## 8) Chạy 24/7
**Linux**: dùng `deploy/systemd/ai-server.service`

```
sudo systemctl daemon-reload
sudo systemctl enable --now ai-server
```

**Windows**: import `deploy/windows/ai-server-task.xml`
