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
