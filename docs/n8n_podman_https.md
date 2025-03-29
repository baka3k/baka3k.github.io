# 🚀 Cài đặt n8n với HTTPS bằng Podman trên macOS

## 🎯 1. Cài đặt và chuẩn bị môi trường

### Cài đặt Podman

```bash
brew install podman
podman machine init
podman machine start
```

Kiểm tra lại Podman hoạt động:
```bash
podman version
```

### Tạo thư mục dự án

```bash
mkdir ~/n8n_podman && cd ~/n8n_podman
```

## 🌐 2. Tạo mạng riêng cho Podman

Tạo một network riêng để các container "nhìn thấy nhau":

```bash
podman network create n8n_network
```

Kiểm tra lại:
```bash
podman network ls
```

## 🔧 3. Cài đặt và chạy n8n

Tạo và chạy container n8n:

```bash
podman run -d --name n8n --network n8n_network \
  -p 5678:5678 \
  docker.io/n8nio/n8n:latest
```

Kiểm tra logs xem n8n đã chạy chưa:
```bash
podman logs n8n
```

## 🔒 4. Cài đặt và cấu hình Caddy làm reverse proxy

### Tạo file Caddyfile

Tạo file cấu hình `Caddyfile` trong thư mục dự án:

```bash
touch Caddyfile
```

Nội dung file:

```plaintext
:8443 {
    reverse_proxy n8n:5678
    tls /etc/caddy/certs/localhost.pem /etc/caddy/certs/localhost-key.pem
    log {
        output stdout
        level DEBUG
    }
}
```

### Tạo thư mục chứa chứng chỉ SSL

```bash
mkdir certs
```

**Tạo chứng chỉ SSL tự ký:**

```bash
openssl req -x509 -nodes -newkey rsa:2048 -keyout certs/localhost-key.pem \
    -out certs/localhost.pem -days 365 \
    -subj "/CN=localhost"
```

### Chạy container Caddy

```bash
podman run -d --name caddy --network n8n_network \
  -p 8443:8443 \
  -v $(pwd)/Caddyfile:/etc/caddy/Caddyfile \
  -v $(pwd)/certs:/etc/caddy/certs \
  docker.io/library/caddy:latest
```

## 🔥 5. Kiểm tra lại hệ thống

### Kiểm tra container đang chạy

```bash
podman ps
```

Kết quả sẽ hiển thị 2 container: `n8n` và `caddy`.

### Kiểm tra logs

```bash
podman logs caddy
podman logs n8n
```

### Truy cập n8n qua HTTPS

Mở trình duyệt và truy cập:

```plaintext
https://localhost:8443
```

Nếu trình duyệt cảnh báo chứng chỉ, cứ chọn **Advanced > Proceed**.

---

🚀 **Xong rồi!** Bạn đã cài đặt thành công n8n với HTTPS thông qua Caddy và Podman. 🎉✨

