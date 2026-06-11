# SỬ DỤNG DJANGO ĐỂ TẠO WEB QUẢN LÝ TIỆM CẦM ĐỒ 
## DJANGO_WEB_CAM_DO
#### Họ tên : Nguyễn Thế Dương
#### MSSV : K225480106007
### 1. TỔ CHỨC CSDL CHO HỆ THỐNG QUẢN LÝ TIỆM CẦM ĐỒ:

### 2. SỬ DỤNG DOCKER TRÊN UBUNTU
1. Thiết kế Cơ sở dữ liệu (CSDL)
- KhachHang: ID, Họ tên, CMND/CCCD, Số điện thoại, Địa chỉ.
- LoaiTaiSan: ID, Tên loại (Xe máy, Điện thoại, Laptop...).
- HopDong: ID, Khách hàng (FK), Loại tài sản (FK), Tên tài sản, Giá trị cầm, Lãi suất, Ngày cầm, Ngày hết hạn, Trạng thái (Đang cầm, Đã chuộc, Quá hạn).
Chào Dương, với deadline vào ngày kia (09/05), khối lượng công việc này khá lớn nhưng nếu đi đúng trình tự thì hoàn toàn kịp. Dưới đây là lộ trình chi tiết để bạn hoàn thiện hệ thống quản lý tiệm cầm đồ bằng Django và Docker trên Ubuntu.

1. Thiết kế Cơ sở dữ liệu (CSDL)
Theo yêu cầu, bạn cần vẽ ra giấy và chụp ảnh. Về logic, một hệ thống cầm đồ cơ bản cần các bảng sau:

KhachHang: ID, Họ tên, CMND/CCCD, Số điện thoại, Địa chỉ.

LoaiTaiSan: ID, Tên loại (Xe máy, Điện thoại, Laptop...).

HopDong: ID, Khách hàng (FK), Loại tài sản (FK), Tên tài sản, Giá trị cầm, Lãi suất, Ngày cầm, Ngày hết hạn, Trạng thái (Đang cầm, Đã chuộc, Quá hạn).

2. Cấu trúc thư mục dự án
<img width="615" height="211" alt="image" src="https://github.com/user-attachments/assets/485cc4a2-3c1a-4102-a2cf-ee42df68ed42" />

3. Tạo Dockerfile & requirements.txt cho Django
<img width="1103" height="283" alt="Untitled" src="https://github.com/user-attachments/assets/4592538a-048d-4a2a-bbc6-eca78d6e0542" />
- Cấu hình Docker & Django
  Sử dụng **sudo nano** để sửa các file sau:
  1. django_app/requirements.txt
  <img width="1257" height="126" alt="image" src="https://github.com/user-attachments/assets/b58392b2-b613-47e7-b019-70295cdce8d9" />

  2. django_app/Dockerfile
  <img width="1119" height="564" alt="image" src="https://github.com/user-attachments/assets/bb9d57f7-d5c2-4084-a32b-6cd398407419" />

  3. docker-compose.yml ( File này sẽ liên kết MariaDB, PhpMyAdmin và Django)
<img width="1142" height="882" alt="image" src="https://github.com/user-attachments/assets/5a393370-5130-4b2b-bc43-1550747306e0" />
4. Bây giờ chạy lại lệnh khởi tạo dự án:
*docker compose run --rm web django-admin startproject myproject*<br>     
<img width="1900" height="975" alt="image" src="https://github.com/user-attachments/assets/3a46d219-c7d3-4832-a76b-32c8e2ddc47b" />
5. Cấu hình CSDL trong settings.py
<img width="1247" height="855" alt="image" src="https://github.com/user-attachments/assets/d84d477a-76ee-4c30-9a00-bcdba79caf99" />
6.Định nghĩa các bảng trong models.py
tạo một App mới trước: **docker compose exec web python manage.py startapp app_camdo**
 <img width="1581" height="549" alt="image" src="https://github.com/user-attachments/assets/47db93cb-de7d-43f3-91f4-40667b3036f8" />
7. Chạy lệnh đồng bộ (Migrate)
- Đây là lúc Django sẽ tự động vào MariaDB để tạo bảng:   
     - docker compose exec web python manage.py makemigrations  
    - docker compose exec web python manage.py migrate   
8. Tạo View xử lý logic "Con nợ"   
- Dùng **sudo nano django_app/app_camdo/views.py**    
<img width="1196" height="438" alt="image" src="https://github.com/user-attachments/assets/603dbe37-6ab8-41ef-a5e6-ad6b5c4dc167" />  
 
9. Tạo giao diện (Template)
  - Tạo thư mục templates: mkdir -p django_app/app_camdo/templates
  - Tạo file giao diện: sudo nano django_app/app_camdo/templates/home.html
<img width="1752" height="912" alt="image" src="https://github.com/user-attachments/assets/766d59c5-3afd-41fb-8917-d7a6d11ed9ef" />
10. Cấu hình URL   
    - Sửa file django_app/myproject/urls.py:  
<img width="1299" height="686" alt="image" src="https://github.com/user-attachments/assets/644080b9-6495-407a-a0e5-99de88cfae1c" />  

11. Kiểm tra trang Admin
- Giao diện trang chủ 
<img width="1919" height="590" alt="image" src="https://github.com/user-attachments/assets/55b4eaf2-c100-43a2-b4d4-a1fb98c5ae63" />
- Add hợp đồng
  <img width="862" height="645" alt="image" src="https://github.com/user-attachments/assets/9a0b0d0a-e3da-4e86-83fc-21b63d4639ce" />
- Add khách Hàng
  <img width="912" height="497" alt="image" src="https://github.com/user-attachments/assets/d112ed91-3b79-4dc2-bed0-f362164b71a9" />
- Add loại tài sản
<img width="951" height="554" alt="image" src="https://github.com/user-attachments/assets/d3f5ca40-8ad3-4a22-a036-6dbb9782fdf0" />

12. Bước cuối: Cloudflare Tunnel
#### Tải cloudflared
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb
#### Chạy tunnel public port 8000 của Django
cloudflared tunnel --url http://localhost:8000

- Bước 1: Login và xác thực tên miền
  Trên Terminal Ubuntu, bạn chạy lệnh: **cloudflared tunnel login**
- Bước 2: Tạo Tunnel và lấy ID
  Bạn tạo một tunnel đặt tên là camdo_tunnel
- Bước 3: Cấu hình DNS (Kích hoạt sub-domain)
Để sub-domain camdo.wapvip.io.vn trỏ về máy, chạy lệnh : **cloudflared tunnel route dns camdo_tunnel camdo.wapvip.io.vn**
- Bước 4: Tạo file cấu hình config.yml
  Dùng lệnh: nano ~/.cloudflared/config.yml
  <img width="1353" height="419" alt="image" src="https://github.com/user-attachments/assets/fefe505d-2d4a-4c1f-a0d7-c209448c31d5" />

<img width="1904" height="875" alt="image" src="https://github.com/user-attachments/assets/ca8513c0-30fe-4296-bb86-2e39ba366e62" />

<img width="792" height="886" alt="image" src="https://github.com/user-attachments/assets/39c261c8-0592-4ffe-a1b5-d3997f4c69a6" />

<img width="1919" height="489" alt="image" src="https://github.com/user-attachments/assets/3be4b51b-f6f3-469a-aabc-dfcb26317f20" />













