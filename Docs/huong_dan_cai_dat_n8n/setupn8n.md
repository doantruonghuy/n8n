**Hướng dẫn này sẽ bao gồm các bước sau:**

1. Cập nhật hệ thống.
2. Cài đặt Docker và Docker Compose.
3. Cấu hình n8n với Docker Compose.
4. Cấu hình Reverse Proxy (Nginx) với SSL (Let's Encrypt).
5. Cấu hình n8n tự động khởi động.

**Yêu cầu:**

1. Một VPS chạy hệ điều hành Ubuntu 20.04.
2. Quyền truy cập SSH (với người dùng có quyền sudo).
3. Một tên miền đã trỏ về địa chỉ IP (103.211.200.39) của VPS của bạn (ví dụ: n8n.smartfix.vn). Nếu chưa có, bạn có thể bỏ qua bước cài đặt Nginx và SSL, nhưng nên cân nhắc cài đặt sau này để bảo mật.

** Các bước thực hiện: **

**_Bước 1:_** Cập nhật hệ thống và cài đặt các gói cần thiết:

1.1. Kết nối SSH vào VPS của bạn và chạy các lệnh sau để cập nhật hệ thống và cài đặt một số gói cần thiết:

```Bash```
```
sudo apt update
sudo apt upgrade -y
sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates
```
***Bước 2:*** Cài đặt Docker và Docker Compose

2.1. Cài đặt Docker Engine:

```Bash```

```curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
```
```
sudo usermod -aG docker ${USER} # Thêm người dùng hiện tại vào nhóm docker để không cần sudo khi chạy lệnh docker
```

# Khởi động lại phiên SSH để áp dụng thay đổi hoặc chạy lệnh sau
# newgrp docker

2.3. Kiểm tra xem Docker đã cài đặt thành công chưa:
```
Bash```
```
docker run hello-world
# Nếu bạn thấy thông báo "Hello from Docker!", nghĩa là Docker đã cài đặt thành công.

2.2. Cài đặt Docker Compose
Kiểm tra phiên bản Docker Compose mới nhất tại https://github.com/docker/compose/releases. Thay thế v2.20.2 bằng phiên bản mới nhất nếu có.

Bash

sudo curl -L "https://github.com/docker/compose/releases/download/v2.20.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
Kiểm tra xem Docker Compose đã cài đặt thành công chưa:

Bash

docker-compose --version
Bước 3: Cấu hình n8n với Docker Compose
Tạo một thư mục để lưu trữ cấu hình n8n:

Bash

mkdir -p ~/n8n
cd ~/n8n
Tạo file docker-compose.yml với nội dung sau. Bạn có thể sử dụng nano hoặc vim:

Bash

nano docker-compose.yml
Dán nội dung sau vào file. Hãy thay đổi your.n8n.domain bằng tên miền thực tế của bạn.

YAML

version: '3.8'

services:
  n8n:
    image: n8n.io/n8n
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_HOST=${N8N_HOST}
      - N8N_PORT=5678
      - N8N_PROTOCOL=https
      - NODE_ENV=production
      - WEBHOOK_URL=https://${N8N_HOST}/
      - GENERIC_TIMEZONE=Asia/Ho_Chi_Minh # Thay đổi múi giờ nếu cần
      # - DB_TYPE=postgres
      # - DB_POSTGRES_HOST=your_postgres_host
      # - DB_POSTGRES_PORT=5432
      # - DB_POSTGRES_DATABASE=n8n
      # - DB_POSTGRES_USER=n8n
      # - DB_POSTGRES_PASSWORD=your_db_password
    volumes:
      - ~/.n8n:/home/node/.n8n
    networks:
      - n8n_network

networks:
  n8n_network:
    driver: bridge
Lưu ý quan trọng:

N8N_HOST: Đây là tên miền mà bạn sẽ sử dụng để truy cập n8n. Ví dụ: n8n.yourdomain.com.

WEBHOOK_URL: Phải khớp với N8N_HOST và sử dụng https://.

GENERIC_TIMEZONE: Thay đổi Asia/Ho_Chi_Minh thành múi giờ của bạn nếu cần (ví dụ: America/New_York, Europe/London).

Cơ sở dữ liệu: Mặc định, n8n sử dụng SQLite và lưu trữ dữ liệu trong thư mục $HOME/.n8n trên VPS của bạn (được mount vào /home/node/.n8n bên trong container). Nếu bạn muốn sử dụng PostgreSQL hoặc MySQL, bạn cần bỏ comment các dòng DB_TYPE và cấu hình thông tin kết nối cơ sở dữ liệu của bạn.

Tạo file .env để lưu trữ biến môi trường N8N_HOST:

Bash

nano .env
Dán nội dung sau vào file, thay your.n8n.domain bằng tên miền của bạn:

N8N_HOST=your.n8n.domain
Lưu và đóng file (Ctrl+X, Y, Enter).

Bây giờ, khởi chạy n8n bằng Docker Compose:

Bash

docker-compose up -d
Lệnh này sẽ tải image n8n (nếu chưa có) và khởi chạy container n8n ở chế độ nền.

Kiểm tra trạng thái của container:

Bash

docker-compose ps
Bạn sẽ thấy n8n_n8n_1 đang ở trạng thái Up.

Lúc này, n8n đã chạy trên cổng 5678 của VPS của bạn. Tuy nhiên, để truy cập qua tên miền và sử dụng HTTPS, chúng ta cần cấu hình Nginx.

Bước 4: Cấu hình Reverse Proxy (Nginx) với SSL (Let's Encrypt)
4.1. Cài đặt Nginx
Bash

sudo apt install -y nginx
sudo ufw allow 'Nginx Full' # Mở cổng 80 và 443 trên firewall
sudo systemctl enable nginx
sudo systemctl start nginx
Kiểm tra trạng thái Nginx:

Bash

sudo systemctl status nginx
Đảm bảo Nginx đang chạy.

4.2. Cấu hình Nginx cho n8n
Tạo file cấu hình Nginx cho tên miền của bạn:

Bash

sudo nano /etc/nginx/sites-available/n8n.conf
Dán nội dung sau vào file. Thay thế your.n8n.domain bằng tên miền thực tế của bạn.

Nginx

server {
    listen 80;
    listen [::]:80;
    server_name your.n8n.domain;

    # Redirect HTTP to HTTPS
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name your.n8n.domain;

    ssl_certificate /etc/letsencrypt/live/your.n8n.domain/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your.n8n.domain/privkey.pem;

    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    location / {
        proxy_pass http://localhost:5678; # n8n chạy trên cổng 5678
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_read_timeout 900s; # Tăng timeout để xử lý các workflow dài
    }
}
Lưu và đóng file.

Tạo symbolic link từ sites-available sang sites-enabled để kích hoạt cấu hình:

Bash

sudo ln -s /etc/nginx/sites-available/n8n.conf /etc/nginx/sites-enabled/
Kiểm tra cú pháp cấu hình Nginx:

Bash

sudo nginx -t
Nếu không có lỗi, khởi động lại Nginx:

Bash

sudo systemctl restart nginx
4.3. Cài đặt Certbot và Let's Encrypt SSL
Cài đặt Certbot (công cụ để lấy chứng chỉ SSL từ Let's Encrypt):

Bash

sudo snap install core
sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
Chạy Certbot để lấy chứng chỉ SSL và tự động cấu hình Nginx:

Bash

sudo certbot --nginx -d your.n8n.domain
Bạn sẽ được yêu cầu nhập địa chỉ email, đồng ý với các điều khoản dịch vụ và có thể chọn có muốn nhận email thông báo hay không. Certbot sẽ tự động cấu hình Nginx và lấy chứng chỉ SSL.

Sau khi hoàn tất, Certbot cũng sẽ tự động thiết lập một tác vụ cronjob để tự động gia hạn chứng chỉ SSL.

Bước 5: Cấu hình n8n tự động khởi động (đã được xử lý bởi restart: always trong Docker Compose)
Với restart: always trong file docker-compose.yml, container n8n sẽ tự động khởi động lại nếu nó dừng hoặc nếu VPS của bạn khởi động lại. Bạn không cần phải làm gì thêm cho bước này.

Hoàn tất và truy cập n8n
Bây giờ, bạn có thể truy cập n8n qua trình duyệt web bằng tên miền của bạn:

https://your.n8n.domain

Lần đầu tiên truy cập, bạn sẽ được hướng dẫn tạo tài khoản quản trị.

Một số lệnh Docker Compose hữu ích:
Khởi động n8n: cd ~/n8n && docker-compose up -d

Dừng n8n: cd ~/n8n && docker-compose down

Xem logs của n8n: cd ~/n8n && docker-compose logs -f

Khởi động lại n8n: cd ~/n8n && docker-compose restart n8n

Chúc mừng bạn đã cài đặt n8n thành công trên VPS Ubuntu 20.04! Nếu bạn gặp bất kỳ vấn đề nào trong quá trình cài đặt, đừng ngần ngại hỏi nhé.
