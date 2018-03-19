<div align="center"><img src="https://cdn.worldvectorlogo.com/logos/bludit.svg" width="350" height="400"></div>

<br/>
<text align="center" style="font-size:200px">Bludit</text>
<br/>

[Sekilas Tentang](#sekilas-tentang) | [Instalasi](#instalasi) | [Konfigurasi](#konfigurasi) | [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---:|:---:|:---:|:---:|:---:|:---:|:---:

# Sekilas Tentang
[`^ kembali ke atas ^`](#)

***Bludit*** merupakan sebuah CMS (*Content Management System*) yang digunakan untuk membuat blog pribadi dalam waktu singkat, bebas biaya dan open source. Bludit menggunakan flat-files untuk menyimpan kiriman dan halaman, tanpa perlu menginstall atau konfigurasi basisdata. Bludit mendukung kode Markdown dan HTML untuk konten kiriman dan halaman.


# Instalasi
[`^ kembali ke atas ^`](#)

**Kebutuhan Sistem :**
-   Ubuntu-16.04
-   Non-root user dengan pengaturan sudo privileges.
-   IP address statik.

**Proses Instalasi :**
- *Update* sistem dengan versi yang terbaru dan *install  packages* yang diperlukan
`$ sudo apt-get update -y`
`$ sudo apt-get upgrade -y`
`$ sudo apt-get install nano wget unzip git curl`

- *Install* Nginx, PHP dan modul PHP yang diperlukan
`$ sudo apt-get install nginx`
`$ sudo systemctl start nginx`  `sudo systemctl enable nginx`
`$ sudo apt-get -y install php7.0-fpm php7.0-cli php7.0 json php7.0-curl php7.0-gd php7.0-mysql php7.0-mbstring php7.0-xml`
`$ sudo nano /etc/php/7.0/cli/php.ini`

	```
	memory_limit = 512M
	date.timezone = UTC
	cgi.fix_pathinfo=0
	upload_max_filesize = 100M
	post_max_size = 100M
	```

	`$ sudo nano /etc/php/7.0/fpm/pool.d/user.conf`
	```
	[www-data]
	user = www-data
	group = www-data
	listen = /var/run/php/php7.0-www-data-fpm.sock
	listen.owner = www-data
	listen.group = www-data
	listen.mode = 0666
	pm = ondemand
	pm.max_children = 5
	pm.process_idle_timeout = 10s
	pm.max_requests = 200
	chdir = /
	```
	`$ sudo service php7.0-fpm restart`

- Download Bludit
`$ wget https://s3.amazonaws.com/bludit-s3/bludit-builds/bludit_latest.zip`
`$ sudo unzip bludit_latest.zip -d /var/www/html/`
`$ sudo chown -R www-data:www-data /var/www/html/bludit`
`$ sudo chmod -R 755 /var/www/html/bludit`

# Konfigurasi
[`^ kembali ke atas ^`](#)

- Konfigurasi Nginx
`$ sudo nano /etc/nginx/sites-available/bludit.conf`
	```
	server {
	    listen 80;
	    server_name 127.0.0.1;
	    root /var/www/html/bludit;
	    index index.php;location / {
	      try_files $uri $uri/ /index.php$is_args$args;
	    }access_log  /var/log/nginx/bludit.access.log;
	    error_log   /var/log/nginx/bludit.error.log;# Deny direct access to .txt files
	    location ~* /bl-content/.*\.txt$ { 
	        return 404; 
	    }location ~ \.php$ {
	        fastcgi_split_path_info ^(.+\.php)(/.+)$;
	        fastcgi_pass unix:/var/run/php/php7.0-www-data-fpm.sock;
	        fastcgi_index index.php;
	        include fastcgi_params;
	        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	        fastcgi_intercept_errors off;
	        fastcgi_buffer_size 16k;
	        fastcgi_buffers 4 16k;
	    }location ~ /\.ht {
	        deny all;
	    }
	}
	```
	`$ sudo ln -s /etc/nginx/sites-available/bludit.conf /etc/nginx/sites-enabled/`
	`$ sudo nginx -t`
	`$ sudo systemctl restart nginx`

- Konfigurasi Firewall
`$ sudo ufw enable`
`$ sudo ufw allow http`
`$ sudo ufw status`

# Maintenance
[`^ kembali ke atas ^`](#)

# Cara Pemakaian
[`^ kembali ke atas ^`](#)
1. Akses Bludit melalui localhost anda dengan masuk ke 127.0.0.1
2. Pilih bahasa.
<img src="https://drive.google.com/file/d/1Z2Hovgk1bLABvVUkVNrwMvn2g3kmUzpU/view?usp=sharing"></img>
3. Buat akun Bludit anda.
<img src="https://drive.google.com/file/d/140hGQnTQE4lc1Du7RLkqz8BsMQr_tnlQ/view?usp=sharing"></img>
4. Login dengan akun anda.
5. Untuk membuat konten baru, masuk ke halaman dashboard dengan cara klik `admin panel` dibagian `Set up your new site` 
6. Tampilan Dashboard admin
<img src="https://drive.google.com/file/d/1w42Z67Izh4S6hDAmLVQ1ILOuGb0wXCXj/view?usp=sharing"></img>
7. Untuk membuat post baru pilih menu `New content` pada bagian `PUBLISH`
8. PIlih `Save` dan konten telah ter-publish

# Pembahasan
[`^ kembali ke atas ^`](#)

# Referensi
https://hostpresto.com/community/tutorials/install-and-configure-bludit-cms-on-ubuntu-16-04/

https://www.bludit.com/
