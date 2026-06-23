# 🦷 Dental U-Net Segmentation

Ứng dụng web phân đoạn sâu răng từ ảnh X-ray bằng U-Net. Dự án gồm backend Flask phục vụ API và frontend tĩnh để hiển thị kết quả.

## 🎯 Tính năng chính

- ✨ Chỉ dùng U-Net cho segmentation
- 🌐 Frontend HTML/JS đơn giản, trực quan
- ⚡ Inference nhanh với PyTorch
- 🧠 Tự động xử lý ảnh đầu vào và tạo mặt nạ segmentation
- 🔧 Cấu hình dễ dàng qua `backend/config.py`

## 📋 Yêu cầu

- Python 3.8+
- PyTorch 2.0+
- CUDA tùy chọn nếu muốn chạy GPU
- Windows / Linux / Mac

## 📦 Cài đặt đầy đủ

### 1. Chuẩn bị môi trường

Mở terminal và chuyển vào thư mục dự án:

```bash
cd d:\Luận\Dental_UNet_Segmentation\backend
```

### 2. Tạo môi trường ảo

```bash
python -m venv venv
```

### 3. Kích hoạt môi trường ảo

**Windows:**
```powershell
venv\Scripts\activate
```

**Linux/Mac:**
```bash
source venv/bin/activate
```

### 4. Cài đặt thư viện

```bash
pip install -r requirements.txt
```

### 5. Đặt model vào thư mục

Copy file `best_unet_dental_model.pth` vào thư mục `backend/models/`:

```powershell
copy path\to\best_unet_dental_model.pth backend\models\
```

## 🚀 Chạy ứng dụng

### Cách 1: Chạy trực tiếp bằng Python

```bash
python app.py
```

Sau khi chạy, server sẽ hoạt động tại:

- `http://127.0.0.1:5001`
- `http://localhost:5001`

### Cách 2: Dùng `run.bat` (Windows)

File `run.bat` đã được chuẩn bị sẵn để kích hoạt môi trường và chạy ứng dụng. Bạn chỉ cần chạy:

```powershell
run.bat
```

### Cách 3: Dùng `run.ps1` (PowerShell)

```powershell
.\run.ps1
```

## 🌐 Sử dụng frontend

1. Mở trình duyệt và truy cập `http://localhost:5001`
2. Chọn ảnh X-ray răng
3. Nhấn nút **Phân tích**
4. Xem mặt nạ segmentation và ảnh chồng lên kết quả

> Frontend hiện tại gửi ảnh đến endpoint mặc định: `POST /api/unet-segment`.
>
> Nếu frontend không tải được giao diện, bạn vẫn có thể dùng API trực tiếp.

## 📡 API chi tiết

### 1. Health check

```http
GET /health
```

Response mẫu:

```json
{
  "status": "ok",
  "model": {
    "unet_loaded": true,
    "device": "cpu"
  }
}
```

### 2. Segmentation

```http
POST /api/segment
POST /api/unet-segment
Content-Type: multipart/form-data

Form data:
- image: file ảnh X-ray
```

Response mẫu:

```json
{
  "success": true,
  "mask_available": true,
  "mask_pixels": 1435,
  "segmentation": "data:image/jpeg;base64,...",
  "image": "data:image/jpeg;base64,..."
}
```

## 🧩 Giải thích cấu trúc dự án

```
Dental_UNet_Segmentation/
├── ARCHITECTURE.md
├── QUICKSTART.md
├── README.md
├── requirements-dev.txt
├── run.bat
├── run.ps1
├── SETUP.md
├── test_model_load.py
├── backend/
│   ├── app.py
│   ├── config.py
│   ├── requirements.txt
│   ├── models/
│   │   └── best_unet_dental_model.pth
│   └── utils/
│       ├── __init__.py
│       ├── predictor.py
│       └── unet_model.py
└── frontend/
    ├── index.html
    ├── css/
    │   └── style.css
    └── js/
        └── app.js
```

## 🔧 Cấu hình quan trọng

Mở `backend/config.py` và điều chỉnh nếu cần:

```python
UNET_THRESHOLD = float(os.environ.get('UNET_THRESHOLD', '0.05'))
UNET_DEVICE = os.environ.get('UNET_DEVICE', 'cpu')
API_PORT = int(os.environ.get('API_PORT', 5001))
MAX_IMAGE_SIZE = 16 * 1024 * 1024
```

### Các cấu hình có thể thay đổi

- `UNET_DEVICE`: `'cpu'` hoặc `'cuda'`
- `API_PORT`: cổng server
- `UNET_THRESHOLD`: ngưỡng phân loại mặt nạ
- `MAX_IMAGE_SIZE`: giới hạn kích thước ảnh upload

## ✅ Kiểm tra nhanh

1. Model đã nằm trong `backend/models/best_unet_dental_model.pth`
2. Chạy `python app.py` không báo lỗi
3. Mở `http://localhost:5001`
4. Gửi ảnh lên, nhận kết quả segmentation

## 🛠️ Khắc phục sự cố

### 1. Không tìm thấy model
- Kiểm tra file `backend/models/best_unet_dental_model.pth`
- Nếu thiếu, copy file model vào đúng thư mục

### 2. Lỗi port bị chiếm
- Thay `API_PORT` trong `backend/config.py`
- Hoặc tắt tiến trình đang dùng port 5001

### 3. Không có GPU
- Chuyển `UNET_DEVICE` thành `'cpu'`
- Ứng dụng vẫn chạy bình thường nhưng chậm hơn

### 4. Ảnh không hợp lệ
- Chỉ dùng định dạng `jpg`, `jpeg`, `png`, `bmp`
- Ảnh phải là ảnh X-ray răng rõ nét

## 📌 Lưu ý

- Server hiện tại dùng Flask development server. Nếu dùng production, nên deploy bằng WSGI server như Gunicorn hoặc uWSGI.
- File `run.bat` và `run.ps1` chỉ phục vụ chạy local.

## 📝 License

MIT License

## 👨‍💻 Tác giả

- Dự án mẫu cho phân đoạn ảnh y tế với U-Net.


- Dental AI Project
- Phát triển cho nghiên cứu nha khoa

## 📧 Support

Để được hỗ trợ, vui lòng tạo issue hoặc liên hệ.

---

**Version**: 1.0.0  
**Last Updated**: 2026-06-15
