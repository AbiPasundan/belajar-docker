# Docker

## Docker Image

Mirip installer aplikasi, di dalam docker image terdapat aplikasi dan dependency
Sebelum menjalankan aplikasi docker, kita perlu memastikan memiliki docker image di app tersebut

### Melihat Docker Image 

untuk melihat docker image yang terdapat di dalam docker daemon kita bisa menggunakan 

- docker image ls / docker images

jika tidak memasukan ':' by default akan menginstall versi latest

### commond command

* docker image ls
* docker image pull redis:latest
* docker image rm alpine:latest

## Docker registry 

* Docker Registry adalah tempat untuk menyimpan docker image

* Docker menggunakan Docker registry, kita bisa menyimpan image yang kita buat dan bisa digunakan di Docker Daemon dimanapun selama bisa terkoneksi dengan Docker registry

Docker Registry: 
- Docker hub
- digital ocean container
- google cloud 
- amazon web service
- mikocok ajure

## Docker Container

* Jika docker image seperti installer aplikasi, maka docker container mirip seperti hasil installernya
* satu docker image bisa digunakan untuk membuat beberapa docker container asalkan nama docker containernya berbeda
* Jika kita sudah membuat docker container, maka docker image yang digunakan tidak bisa dihapus, karena docker container tidak mengcopy isi docker image tapi hanya menggunakan isinya saja 

another definition

- Bentuk image yang sedang berjalan dan dapat diakses melalui network yang sama
- Container dapat menjalankan berbagai macam aplikasi sekaligus 
- setidaknya harus ada 1 aplikasi yang berjalan agar container tetap dalam keadaan running

### status container

* saat membuat container secara default container tersebut tidak akan berjalan
* mirip ketika menginstall aplikasi jika tidak dijalankan maka aplikasi tersebut tidak akan berjalan begitu juga container
* oleh karena itu setelah membuat container kita perlu menjalankannya jika memang ingin menjalankan containernya

### common command

- untuk melihat container
* docker container ls -a # untuk melihat semua container baik yang sedang berjalan maupun tidak berjalan
* docker container ls # untuk melihat container yang sedang berjalan

- untuk membuat container
by default jika tidak menggunakan tag maka versi yang akan digunakan adalah versi latest

* docker container create --name contohredis redis:latest
* docker container create --name contohredis2 redis:latest

- menjalankan container
container start namacontainer/containerid
* docker container start contohredis
- menghentikan container
container start namacontainer/containerid
* docker container stop contohredis
container stop namacontainer/containerid
- menghapus container
container rm namacontainer/containerid
* docker container rm contohredis

## Container Log

kadang saat terjadi masalah dengan aplikasi yang terdapat di container seringkali kita ingin melihat detail dari log aplikasi
Hal ini dilakukan untuk melihat detail kejadian apa yang terjadi di aplikasi, sehingga akan memudahkan kita ketika mendapatkan masalah

### melihat container log

untuk melihat log aplikasi kita bisa menggunakan perintah docker container logs containerid/containername
* docker container logs contohredis

jika kita ingin melihatnya secara realtime kita bisa menggunakan
* docker container logs -f contohredis # menambahkan flag -f sebelum nama container

## Container Exec

saat menggunakan container, aplikasi yang terdapat di dalam container hanya bisa diakses dari dalam container
oleh karena itu kadang kita perlu masuk ke dalam containernya
untuk masuknya kita bisa menggunakan container exec, digunakan untuk mengeksekusi kode program yang terdapat di dalam container

### masuk ke dalam container

- untuk masuk kedalam container, kita bisa mencoba mengeksekusi program bash script yang terdapat didalam container dengan bantuan container exec
docker container exec -i -t contohredis /bin/bash

-i => adalah argument interaktif, menjaga input tetap aktif
-t => adalah argument untuk alokasi psudo-TTY (terminal akses)
/bin/bash => contoh kode program yang terdapat di dalam container

hukumnya wajib hhe wkwwkkw

* docker container exec -i -t contohredis /bin/bash
* docker container exec -it contohredis /bin/bash # sort cut

## Container Port

saat menjalankan image, image tersebut akan menjadi container dan container tersebut terisolali di dalam Docker 
artinya sistem hosy (misal laptop yang sedang digunakan) tidak bisa mengakses aplikasi yang ada di dalam container secara langung, jadi jika ingin mengakses container harus menjalankan container dulu dengan 'container exec' supaya bisa masuk ke dalam container
aplikasi akan berjalan di port tertentu, misal redis akan berjalan di port 6379 kita bisa melihat port apa saja yang digunakan dengan melihat semua daftar container

misal jika kita mengakses port dari redis kita tidak bisa melakukannya karena port tersebut telah terisolasi oleh docker

### port fowarding

Docker memiliki kemampuan untuk melakukan port fowarding, yaitu meneruskan sebuah port yang terdapat di sistem hostnya ke dalam docker container
cara ini cocok jika kita ingin mengekspos port yang terdapat di container ke luar melalui sistem hostnya

- melakukan port fowarding

commond comand
untuk membuat port fowarding kita harus menentukan portnya dari awal jika sudah terlanjut membuattanpa menentukan portnya maka disarankan untuk buat ulang

untuk melakukan port fowarding bisa menggunakan perintah berikut ketika membuat containernya:
* docker container create --name namacontainer --publish posthost:portcontainer image:tag
jika ingin melakukan port fowarding lebih dari satu bisa menambahkan dua kali parameter --publish

--publish bisa disingkat dengan -p

example command
* docker container create --name nginx --publish 8080:80 nginx:latest

## Container Environment Variable

Salah satu teknik agar config bisa diubah secara dinamis
dengan menggunakan container enviorment varibale kita bisa mengubah config tanpa harus mengubah kode aplikasinya lagi
Docker Container memiliki parameter yang bisa kita gunakan untuk mengirim enviorment variable ke aplikasi yang terdapat di dalam container

untuk menambah enviorment variable kita bisa menggunakan --env atau e contoh
* docker container create --name namacontainer --env KEY="value" --env KEY2="value" image:tag
* docker container create --name container --publish 27017:27017 --env MONGO_INITDB_ROOT_USERNAM=hello --env MONGO_INITDB_ROOT_PASSWORD=test mongo:latest

## Container Stats

Saat menjalankan beberapa container di sistem host penggunaan resource seperti cpu dan memory hanya terlihat digunakan oleh docker saja 
kadang kita inign melihat detail dari penggunaan resource untuk tiap containernya
Docker bisa melihat penggunaan resource dari tiap container yang sedang berjalan dengan perintah

* docker container stats

## Container Resource Limit

by default docker akan menggunakan semua CPU dan Memory yang diberikan ke Docker (Mac dan Wingdow)  dan akan menggunakan semua CPU dan Memory yang tersedia di sistem host (Linux)
Jika terjadi kesalahan, misal container terlalu banyak memakan CPU dan memory maka bisa berdampak pada performa container lain atau bahkan ke sistem host
jadi ada baiknya ketika membuat container kita memberikan resource limit terhadapt containernya

### Memory

* saat membuat memory kita bisa menentukan jumlah memory yang bisa digunakan oleh container dengan menggubakan --memory diikuti dengan angka memory yang diperbolehkan untuk digunakan
* Kita bisa menambahkan ukuran dalam bentuk b (bytes), k(kilo bytes) , m(mega bytes), atau g(giga bytes) misal 100m artinya 100mb

### CPU

Selain mengatur memory, kita dapat mengatur berapa jumlah CPU yang bisa digunakan oleh container dengan parameter --cpus
Jika misal kita set dengan nilai 1.5 artinya container bisa menggunakan satu dan setengan CPU core

### menambah resource limit

docker create --name smallnginx --memory 100m --cpus 0.5 --publish 8081:80  nginx:latest
