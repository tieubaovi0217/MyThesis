
# Hướng dẫn triển khai hệ thống hỗ trợ hội nghị trực tuyến trong môi trường ảo

Hướng dẫn triển khai **hệ thống hỗ trợ hội nghị trực tuyến trong môi trường ảo** bằng docker-compose.

## Prerequisites (Các cài đặt tiên quyết)
 - **Docker**: Cần cài đặt Docker trên máy tương ứng với các hệ điều hành 
    (Linux, macOS, Windows) theo hướng dẫn trong đường dẫn sau: https://docs.docker.com/engine/install.
 - **Docker-compose**: Cần cài đặt docker-compose theo hướng dẫn trong đường dẫn sau https://docs.docker.com/compose/install/.
    
**Lưu ý:** Đối với macOS và Windows, nếu đã cài đặt Docker Desktop thì đã được cài
    đặt sẵn Docker-Compose. Do đó, không cần cài đặt lại Docker-Compose.

## Cài đặt Local Certificate Authority
Vì những thành phần của hệ thống phải sử dụng giao thức `HTTPS` để giao tiếp với nhau, và bản hướng dẫn này
giúp triển khai hệ thống trên máy tính local nên ta cần cài đặt `local CA` cho các browser
để trust các website của hệ thống.

 - Cài đặt **mkcert**, trong bản này sẽ hướng dẫn sử dụng `mkcert` trên Windows. Đối với các
   điều hành khác Linux, macOS tham khảo hướng dẫn trong đường dẫn https://github.com/FiloSottile/mkcert).
 - Trong folder `SOFT`, có 2 executable file của `mkcert` (`mkcert-v1.4.4-windows-amd64.exe` và `mkcert-v1.4.4-windows-arm64.exe`),
   Tùy vào kiến trúc máy là ADM64 hay ARM64 mà sử dụng file tương ứng.
 - Sau đó, gõ command dưới đây để install local CA vào máy. Ví dụ đối với máy có kiến trúc amd64, ta sử 
   dụng command sau:
 ```bash
./mkcert-v1.4.4-windows-amd64.exe -install
 ```
 - Lúc này, local CA đã được install vào máy, lúc này ta cần tạo SSL certificate cho máy localhost
   dùng command sau:
```bash
./mkcert-v1.4.4-windows-amd64.exe localhost 127.0.0.1 ::1
```
- Lúc này, trong folder `SOFT` sẽ tạo ra 2 file là `localhost+2.pem` (certificate) và `localhost+2-key.pem` (key).
- Sử dụng 2 file certificate trên để tạo chứng chỉ SSL cho các thành phần của hệ thống. Ghi đè (overwrite)
   2 file này vào các folder sau trong folder `SETUP`: `web-server`, `webgl-server/certs/` và `frontend`.

   

## Triển khai hệ thống local

Các biến môi trường đã được setup sẵn trong file `.env`. Sau đó, ta bật terminal trong folder `SETUP` và
gõ command dưới đây:
```bash
docker-compose --env-file .env up --build -V -d

```

Sau đó truy cập vào https://localhost:3000 để bắt đầu sử dụng hệ thống.


