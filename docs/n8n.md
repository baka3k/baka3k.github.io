# 🚀 Cấu hình HTTPS cho n8n trên Docker (Windows)

Hướng dẫn này giúp bạn cài đặt **n8n** trên Docker và sử dụng **Caddy** để cấp chứng chỉ HTTPS tự động.

---

## 1️⃣ Cài đặt Caddy trên Windows  
Bạn có thể chạy **Caddy** trực tiếp trong Docker mà không cần cài đặt trên máy.

### 🏠 Tạo thư mục chứa cấu hình Caddy  
Mở **PowerShell** và chạy lệnh sau để tạo thư mục:  

```powershell
mkdir C:\n8n-data\caddy
cd C:\n8n-data\caddy
```

### 🗂️ Tạo file cấu hình Caddy (`Caddyfile`)  
Tạo một file mới tên **`Caddyfile`** trong thư mục `C:\n8n-data\caddy` với nội dung sau:

```caddyfile
https://n8n.localhost {
    reverse_proxy n8n:5678
    tls internal
}
```

> 📌 **Giải thích:**  
> - `https://n8n.localhost`: Caddy sẽ tạo domain `n8n.localhost` chạy HTTPS.  
> - `reverse_proxy n8n:5678`: Điều hướng request đến container `n8n`.  
> - `tls internal`: Sử dụng chứng chỉ **tự cấp (self-signed)** của Caddy.  

---

## 2️⃣ Chạy n8n và Caddy bằng Docker Compose  

### 📂 Tạo file `docker-compose.yml`  
Trong thư mục `C:\n8n-data`, tạo file `docker-compose.yml` với nội dung sau:

```yaml
version: "3"
services:
  n8n:
    image: n8nio/n8n
    restart: always
    environment:
      - GENERIC_TIMEZONE=Asia/Ho_Chi_Minh
    volumes:
      - C:\n8n-data:/home/node/.n8n
    networks:
      - n8n_network

  caddy:
    image: caddy
    restart: always
    volumes:
      - C:\n8n-data\caddy:/etc/caddy
    ports:
      - "443:443"
    networks:
      - n8n_network

networks:
  n8n_network:
    driver: bridge
```

---

## 3️⃣ Chạy Docker Compose  
Mở **PowerShell** và chạy lệnh sau:

```powershell
cd C:\n8n-data
docker compose up -d
```

> ✅ **Lệnh này sẽ:**  
> - Chạy **n8n** trên Docker.  
> - Chạy **Caddy** để cấp chứng chỉ HTTPS.  

---

## 4️⃣ Truy cập n8n qua HTTPS  
Mở trình duyệt và nhập:

```
https://n8n.localhost
```

> 📌 **Lưu ý:**  
> - Nếu thấy cảnh báo chứng chỉ không hợp lệ, hãy **chấp nhận** vì đây là chứng chỉ tự cấp.  
> - Nếu muốn sử dụng chứng chỉ hợp lệ từ **Let's Encrypt**, xem phần tiếp theo.  

---

## 🌟 Sử dụng chứng chỉ Let's Encrypt (chứng chỉ thật)  
Nếu bạn có **domain thật**, chỉnh sửa file **`Caddyfile`** như sau:

```caddyfile
yourdomain.com {
    reverse_proxy n8n:5678
    tls youremail@example.com
}
```

Sau đó, chạy lại lệnh:

```powershell
docker compose up -d
```

> 🔹 **Caddy sẽ tự động lấy chứng chỉ từ Let's Encrypt mà không cần thao tác thủ công.**  
> - Đảm bảo domain của bạn đã trỏ về **IP của server**.  
> - Nếu gặp lỗi, kiểm tra firewall và mở cổng **80, 443**.  

---

## 🌟 Kiểm tra logs của Caddy  
Nếu bạn cần kiểm tra logs của Caddy, chạy lệnh sau:

```powershell
docker logs -f caddy
```

Nếu mọi thứ hoạt động bình thường, bạn sẽ thấy thông báo cấp chứng chỉ thành công. 🎉  

---

