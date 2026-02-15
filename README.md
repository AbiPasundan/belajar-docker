<!-- 

ssh

keygen dengan akhir .pub adalah key public 
keygen tanpa akhir .pub adalah key private

# nginx jika menggunakan alphine:stable maka dia ada di /bin/sh
# buat image untuk project frontend dan push ke ghcr

docker tag IMAGE_NAME:latest ghcr.io/OWNER/IMAGE_NAME:latest

docker push ghcr.io/OWNER/IMAGE_NAME:latest

 -->

# Docker

## Docker Image

Mirip installer aplikasi, di dalam docker image terdapat aplikasi dan dependency
Sebelum menjalankan aplikasi docker, kita perlu memastikan memiliki docker image di app tersebut

## Docker registry

Docker Registry adalah tempat untuk menyimpan docker image

Docker menggunakan Docker registry, kita bisa menyimpan image yang kita buat dan bisa digunakan di Docker Daemon dimanapun selama bisa terkoneksi dengan Docker registry

Docker Registry:

* Docker hub
* digital ocean container
* google cloud
* amazon web service
* mikocok ajure

### Melihat Docker Image

untuk melihat docker image yang terdapat di dalam docker daemon kita bisa menggunakan

* `docker image ls`

atau sortcut-nya `docker images`

jika tidak memasukan ':' by default akan menginstall versi latest

### commond command

* `docker image ls`
* `docker image pull nama image`
contoh: docker image pull redis:latest
* `docker image rm nama image`
docker image rm alpine:latest
sortcut untuk menghapus bisa menggunakan `rmi` contoh `docker rmi namaimage`

## Docker Container

Jika docker image seperti installer aplikasi, maka docker container mirip seperti hasil installernya

Satu docker image bisa digunakan untuk membuat beberapa docker container asalkan nama docker containernya berbeda

Jika kita sudah membuat docker container, maka docker image yang digunakan tidak bisa dihapus, karena docker container tidak mengcopy isi docker image tapi hanya menggunakan isinya saja

another definition

* Bentuk image yang sedang berjalan dan dapat diakses melalui network yang sama
* Container dapat menjalankan berbagai macam aplikasi sekaligus
* setidaknya harus ada 1 aplikasi yang berjalan agar container tetap dalam keadaan running

### status container

Saat membuat container secara default container tersebut tidak akan berjalan
Mirip ketika menginstall aplikasi jika tidak dijalankan maka aplikasi tersebut tidak akan berjalan begitu juga container
Oleh karena itu setelah membuat container kita perlu menjalankannya jika memang ingin menjalankan containernya

### common command

untuk melihat container

* `docker container ls -a` # untuk melihat semua container baik yang sedang berjalan maupun tidak berjalan
* `docker container ls` # untuk melihat container yang sedang berjalan

Membuat container

by default jika tidak menggunakan tag maka versi yang akan digunakan adalah versi latest. Contoh perintah

* `docker container create --name contohredis redis` # Tanpa version tag otomatis akan mengambil versi latest
* `docker container create --name contohredis redis:latest`
* `docker container create --name ubutntu:ssh ubuntu:noble`

perintah `docker container create ...` bisa diganti dengan `docker run`

menjalankan container
untuk menjalanakan image sehingga bisa menjadi container perintahnya
`container start namacontainer/containerid`
contoh

* `docker container start contohredis`

menghentikan container
container stop namacontainer/containerid

* `docker container stop contohredis`

Menghapus Container
`container rm namacontainer/containerid`

* docker container rm contohredis

## Container Log

kadang saat terjadi masalah dengan aplikasi yang terdapat di container seringkali kita ingin melihat detail dari log aplikasi
Hal ini dilakukan untuk melihat detail kejadian apa yang terjadi di aplikasi, sehingga akan memudahkan kita ketika mendapatkan masalah

### melihat container log

untuk melihat log aplikasi kita bisa menggunakan perintah `docker container logs containerid/containername`

* `docker container logs contohredis`

jika kita ingin melihatnya secara realtime kita bisa menggunakan

* `docker container logs -f contohredis` # menambahkan flag -f sebelum nama container

## Container Exec

saat menggunakan container, aplikasi yang terdapat di dalam container hanya bisa diakses dari dalam container
oleh karena itu kadang kita perlu masuk ke dalam containernya
untuk masuknya kita bisa menggunakan container exec, digunakan untuk mengeksekusi kode program yang terdapat di dalam container

### masuk ke dalam container

Untuk masuk kedalam container, kita bisa mencoba mengeksekusi program bash script yang terdapat didalam container dengan bantuan container exec dengan perintah `docker container exec -i -t contohredis /bin/bash`

* `docker container exec` => perintah untuk menginisialisasi perintah docker exec
* `-i` => adalah argument interaktif, menjaga input tetap aktif atau simpelnya supaya nanti kita bisa mengguanakan terminal
* `-t` => adalah argument untuk alokasi psudo-TTY (terminal akses)
* `/bin/bash` => contoh kode program yang terdapat di dalam container atau simpelnya aplikasi terminal yang akan dijalanakn

contoh perintahnya

* `docker container exec -i -t contohredis /bin/bash`
* `docker container exec -it contohredis /bin/bash` # sortcut

## Container Port

Saat menjalankan image, image tersebut akan menjadi container dan container tersebut terisolali di dalam Docker
Artinya sistem host (misal laptop yang sedang digunakan) tidak bisa mengakses aplikasi yang ada di dalam container secara langung, jadi jika ingin mengakses container harus menjalankan container dulu dengan `container exec` supaya bisa masuk ke dalam container
Aplikasi akan berjalan di port tertentu, misal redis akan berjalan di port 6379 kita bisa melihat port apa saja yang digunakan dengan melihat semua daftar container

misal jika kita mengakses port dari redis kita tidak bisa melakukannya karena port tersebut telah terisolasi oleh docker

### port fowarding

Docker memiliki kemampuan untuk melakukan port fowarding, yaitu meneruskan sebuah port yang terdapat di sistem hostnya ke dalam docker container
cara ini cocok jika kita ingin mengekspos port yang terdapat di container ke luar melalui sistem hostnya

Untuk melakukan port fowarding kita harus menentukan portnya dari awal jika sudah terlanjut membuatnya tanpa menentukan portnya maka disarankan untuk buat ulang

Untuk melakukan port fowarding bisa menggunakan perintah berikut ketika membuat containernya:

* `docker container create --name namacontainer --publish posthost:portcontainer image:tag`
jika ingin melakukan port fowarding lebih dari satu bisa menambahkan dua kali parameter --publish

--publish bisa disingkat dengan -p

contoh perintah

* `docker container create --name nginx --publish 8080:80 nginx:latest`

## Container Environment Variable

environment Variable salah satu teknik agar config bisa diubah secara dinamis
Dengan menggunakan container enviorment varibale kita bisa mengubah config tanpa harus mengubah kode aplikasinya lagi
Docker Container memiliki parameter yang bisa kita gunakan untuk mengirim enviorment variable ke aplikasi yang terdapat di dalam container

untuk menambah enviorment variable kita bisa menggunakan `--env` atau `e` contoh

* `docker container create --name namacontainer --env KEY="value" --env KEY2="value" image:tag`
* `docker container create --name container --publish 27017:27017 --env MONGO_INITDB_ROOT_USERNAME=hello --env MONGO_INITDB_ROOT_PASSWORD=test mongo:latest`

penjelasan

`docker container create` => Perintah untuk membuat container baru, tapi belum dijalankan. Kalau ingin langsung jalan, biasanya pakai docker run.
`--name container` => Memberi nama container menjadi: `container` Jadi nanti bisa dijalankan dengan: docker start container
`--publish 27017:27017` => Mapping port. Port 27017 di komputer (host) Diteruskan ke port 27017 di dalam container Jadi nanti bisa akses MongoDB lewat:
`--env MONGO_INITDB_ROOT_USERNAM=hello` => Mengatur environment variable untuk container.
`--env MONGO_INITDB_ROOT_PASSWORD=test` => Mengatur password root MongoDB menjadi: `test`
`mongo:latest` => Gunakan image bernama mongo Dengan tag versi latest

## Container Stats

Saat menjalankan beberapa container di sistem host penggunaan resource seperti cpu dan memory hanya terlihat digunakan oleh docker saja
Kadang kita inign melihat detail dari penggunaan resource untuk tiap containernya
Docker bisa melihat penggunaan resource dari tiap container yang sedang berjalan dengan perintah `docker container stats`

## Container Resource Limit

By default docker akan menggunakan semua CPU dan Memory yang diberikan ke Docker Desktop (Mac dan Wingdow) dan akan menggunakan semua CPU dan Memory yang tersedia di sistem host (Linux)
Jika terjadi kesalahan, misal container terlalu banyak memakan CPU dan memory maka bisa berdampak pada performa container lain atau bahkan ke sistem host
Jadi ada baiknya ketika membuat container kita memberikan resource limit terhadapt containernya

### Memberikan Resource Limit ke Memory

Saat membuat memory kita bisa menentukan jumlah memory yang bisa digunakan oleh container dengan menggubakan --memory diikuti dengan angka memory yang diperbolehkan untuk digunakan
Kita bisa menambahkan ukuran dalam bentuk b (bytes), k(kilo bytes) , m(mega bytes), atau g(giga bytes) misal 100m artinya 100mb

### Memberikan Resource Limit ke CPU

Selain mengatur memory, kita dapat mengatur berapa jumlah CPU yang bisa digunakan oleh container dengan parameter --cpus
Jika misal kita set dengan nilai 1.5 artinya container bisa menggunakan satu dan setengan CPU core

### Menambah Resource limit

Example Command:

`docker create --name smallnginx --memory 100m --cpus 0.5 --publish 8081:80  nginx:latest`

## Bind Mount

Bind Mount merupakan kemampuan melakukan Mountting (sharing) file atau folder yang terdapat di sistem host ke container yang terdapat di docker
Fitur ini sangat berguna ketika misal kita ingin mengirim konfigurasi dari luar container, atau misal menyimpan data yang dibuat di aplikasi di dalam container ke dalam folder di sistem host
Jika file atau folder tidak ada di sistem host, secara otomatis akan dibuatkan oleh docker tetapi disarankan folder sudah dibuat terlebih dahulu di host untuk menghindari masalah permission.
Untuk melakukan mounting, kita bisa menggunakan parameter `--mount` ketika membuat container
Isi dari parameter `--mount` memiliki aturan tersendiri

| Parameter     | keterangan                                                                        |
| ------------- |:---------------------------------------------------------------------------------:|
| type          | Tipe mount, bind atau volume                                                      |
| source        | Lokasi file atau folder di sistem host                                            |
| destination   | Lokasi file atau folder di container                                              |
| readonly      | Jika ada, maka file atau folder hanya bisa dibaca di container, tidak bisa ditulis|

Source itu lokasi file yang akan di mount ke dalam container
Destination lokasi file atau folder di dalam container
readonly hanya membaca

### Melakukan Mounting

Untuk melakukan mounting kita bisa menggunakan perintah berikut:

* `docker container create --name namacontainer --mount "type=bind,source=folder,destination=/folder,readonly" image:tag`

contoh perintah:

* `docker container create --name mongodata --mount "type=bind,source=~/Documents/docker,destination=/data/db" --publish 27018:27017 --env MONGO_INITDB_ROOT_USERNAME=helo --env MONGO_INITDB_ROOT_PASSWORD=test mongo:latest`

penjelasan perintah:

* `docker container create` => membuat container baru dari image dalam keadaab stop
* `--name mongodata` => memberikan nama pada container yang dibuat
* `--mount "type=bind,source=~/Documents/docker,destination=/data/db"` => bind mount yaitu menghubungkan folder host dengan folder di dalam container
  * `type=bind` => artinta menggunakan bind mount bukan volume
  * `source=~/Documents/docker` => folder host yang akan digunakan
  * `destination=/data/db` => folder didalam container. dengan bind mounth database disimpan didalam host data tidak hilang walaupun container dihapus
* `--publish 27018:27017` => port fowarding artinya port 27017 di container (default MongoDB) diteruskan ke port 27018 di host sehingga di host jika kita ingin akses mongo `localhost:27018` bukan `localhost:27017`
* `--env MONGO_INITDB_ROOT_USERNAME=helo` => Environment variable untuk konfigurasi MongoDB saat pertama kali dijalankan. Ini akan membuat user root dengan nama `helo`
* `--env MONGO_INITDB_ROOT_PASSWORD=test` => password untuk root mongodb `test`. Kedua env ini hanya digunakan saat database pertama kali dibuat. Kalau data sudah ada di /data/db, perubahan env ini tidak akan mengubah password lagi.
* `mongo:latest` => image yang digunakan

Untuk mengetahui folder destination data bisa melihat image atau container docs
