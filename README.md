# Bai2_Portgress-Oodo
# Họ tên: Đậu Văn Khánh
# MSSV: K225480106099
# Lớp: K58KTP

# Giới thiệu tổng quan
## PostgreSQL + Odoo → Website
- PostgreSQL: hệ quản trị cơ sở dữ liệu (database) mạnh, dùng để lưu toàn bộ dữ liệu.
- Odoo: nền tảng ERP mã nguồn mở (quản lý bán hàng, website, kho, CRM…).
- Khi kết hợp:
  + Odoo xử lý logic + giao diện web.
  + PostgreSQL lưu dữ liệu phía sau.
👉 Hệ thống này tạo ra website + hệ thống quản lý doanh nghiệp (ERP) hoàn chỉnh

# Kiến trúc hệ thống
```User → Odoo (web/app) → PostgreSQL (database)```

# Các bước thực hiện
## Bước 1: Chuẩn bị môi trường
- Yêu cầu:
  + Ubuntu (máy thật hoặc máy ảo).
  + Đã cài Docker + Docker Compose.
- Kiểm tra:
```
docker --version
docker-compose --version
```
<img width="661" height="125" alt="image" src="https://github.com/user-attachments/assets/62b8dbe1-2fda-449b-a8ab-8eedd37d8bdd" />

## Bước 2: Tạo thư mục project
```
mkdir odoo_app
cd odoo_app
```
<img width="417" height="78" alt="image" src="https://github.com/user-attachments/assets/17bf028d-4bee-4b89-afb1-14049f935774" />

## Bước 3: Tạo file docker-compose.yml
```nano docker-compose.yml```

- Nội dung file docker-compose.yml:
```
version: '3.8'

services:
  db:
    image: postgres:15
    container_name: postgres
    restart: always
    environment:
      POSTGRES_DB: postgres
      POSTGRES_USER: odoo
      POSTGRES_PASSWORD: odoo
    volumes:
      - pg_data:/var/lib/postgresql/data

  odoo:
    image: odoo:17
    container_name: odoo
    restart: always
    depends_on:
      - db
    ports:
      - "8069:8069"
    environment:
      HOST: db
      USER: odoo
      PASSWORD: odoo

volumes:
  pg_data:
```
<img width="1478" height="760" alt="image" src="https://github.com/user-attachments/assets/a19815dd-0cc9-49d2-a746-9cc817908ba0" />

## Bước 4: Khởi chạy hệ thống
```
docker-compose up -d
```
<img width="1479" height="755" alt="image" src="https://github.com/user-attachments/assets/fdaa9f49-df18-4485-b139-1735847402b1" />

👉 Kiểm tra:
```docker ps```
<img width="1484" height="281" alt="image" src="https://github.com/user-attachments/assets/d73540b4-9433-434a-9aa6-44ba357c96e0" />

## Bước 5: Truy cập giao diện Odoo
- Mở trình duyệt: http://192.168.91.154:8069/
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/5a6e86c4-70e5-4eae-aced-0a391c577730" />

## Bước 6: Tạo database trong Odoo
- Điền thông tin:
  + Master Password: admin
  + Database Name: tùy ý
  + Email: tùy ý
  + Password: tùy ý

👉 Nhấn Create database
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/9d0378ba-f730-427c-b1d4-2ba0165300de" />

## Bước 7: Cài module
- Đăng nhập vào Odoo
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/ce7cedb9-2227-478a-a04c-5fee2477bae2" />

👉 Giao diện Odoo
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/e3b0134b-8048-4c76-abc8-6859a762d4cc" />

- Cài các module:
  + Website → tạo web
  + Sales → bán hàng
  + CRM → khách hàng
  + Inventory → kho

- Cài module Website: Trong Trang web -> nhấn Kích hoạt
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/40555864-9a60-4285-be8b-eddb7aaac8f1" />

- Cài thêm module cần thiết: Bán hàng, CRM, tồn kho

## Bước 8: Tạo website
- Truy cập: http://192.168.91.154:8069/
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/4b7cf9c8-6c36-4a3b-8398-87e3e78be43a" />

- Bật chế độ chỉnh sửa:
  + Góc trên bên phải, nhấn chỉnh sửa

- Kéo thả nội dung
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/fc716e3c-55fa-4562-b333-573885dda68c" />

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d8084ef8-b229-46b4-a844-2953617e0aaf" />

## Bước 9: Kiểm tra PostgreSQL
- Vào database:
```docker exec -it postgres psql -U odoo -d postgres```

- Xem danh sách database:
```\l```
<img width="1476" height="407" alt="image" src="https://github.com/user-attachments/assets/1a0a47b2-77ee-4251-ac22-ad35fd559a36" />

- Kết nối vào database
```\c bai2_portgress-odoo```

- Xem bảng dữ liệu
```\dt```
<img width="1478" height="755" alt="image" src="https://github.com/user-attachments/assets/c40a7207-2165-4983-9674-a18f9a9342b5" />

- Xem dữ liệu thật
👉 Xem khách hàng:
```SELECT name FROM res_partner LIMIT 5;```
<img width="787" height="223" alt="image" src="https://github.com/user-attachments/assets/b2eec6c3-0048-4eb4-9615-1931ef040520" />

👉 Xem user:
```SELECT login FROM res_users;```
<img width="651" height="269" alt="image" src="https://github.com/user-attachments/assets/5ca313d4-dd51-4796-b41c-a8d9f8ce8bbe" />
