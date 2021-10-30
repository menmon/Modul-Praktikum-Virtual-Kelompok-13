[TOC]

# 			Laporan Praktikum Modul 1 Virtualisasi Kelompok 13

# 					





​																										DISUSUN OLEH : 

​																							Andreas Juand P (1202192047 )

​																							M. Qoidul G. M.  (1202190025)







​																							JURUSAN TEKNOLOGI INFORMASI

​																					 INSTITUT TEKNOLOGI TELKOM SURABAYA

​																													2021

​																										



​										

## Keadaan

Gilang dan Adit adalah sysadmin junior pada suatu perusahaan. Perusahaan tersebut akan membuat sebuah website company profile. Dimana website tersebut berisi 3 hal, yaitu company profile, blog dan aplikasi. CTO menjelaskan strategi domain yang akan digunakan, yaitu:

- vm.local
  - berisi landing page
- vm.local/blog
  - berisi blog
- vm.local/app
  - berisi aplikasi perusahaan

Setelah mendengarkan strategi domain dari CTO, Mereka mencoba membuat virtualisasi sederhana dengan skema dibawah ini :

[![topologi](https://github.com/aldonesia/Sistem-Administrasi-Server-2021/raw/master/modul-1/assets/topologi.PNG)](https://github.com/aldonesia/Sistem-Administrasi-Server-2021/blob/master/modul-1/assets/topologi.PNG)

Dari skema diatas, mereka mementukan bahwa lxc_php5.dev akan digunakan sebagai server aplikasi dan lxc_php7.dev akan digunakan sebagai server blog dan vm.local digunakan sebagai proxy server serta untuk web landing page. Lantas, mereka bertemu dengan tim programmer. Tim Programmer memberi tahu kepada mereka bahwa ada perubahan baru baru ini terkait ubuntu 16.04.

[![news](https://github.com/aldonesia/Sistem-Administrasi-Server-2021/raw/master/modul-1/assets/news.PNG)](https://github.com/aldonesia/Sistem-Administrasi-Server-2021/blob/master/modul-1/assets/news.PNG)

Karena hal ini, mereka berdiskusi dengan tim programmer dan menghasilkan suatu keputusan dimana skema yang akan dibuat adalah:

[![soal_praktikum](https://github.com/aldonesia/Sistem-Administrasi-Server-2021/raw/master/modul-1/assets/soal_praktikum.png)](https://github.com/aldonesia/Sistem-Administrasi-Server-2021/blob/master/modul-1/assets/soal_praktikum.png)

## Soal Praktikum

Dari skema diatas maka mereka akan melakukan pekerjaan sebagai berikut :

1. Rename ubuntu_php5.6 menjadi ubuntu_landing, serta rubah IP mengikuti skema yang baru
2. Install lxc debian 9 dengan nama debian_php5.6
3. setup nginx pada debian_php5.6 untuk domain http://lxc_php5.dev , buat halaman index.html yang menerangkan informasi nama lxc
4. setup nginx pada ubuntu_landing untuk domain http://lxc_landing.dev , buat halaman index.html yang menerangkan informasi nama lxc
5. LXC ubuntu_landing harus auto start ketika vm dinyalakan, hal ini digunakan untuk menjaga agar website company profile tidak mengalami *downtime*
6. setup nginx pada vm.local untuk mengatur *proxy_pass* dimana :
   - mengakses [http://vm.local](http://vm.local/) akan diarahkan ke http://lxc_landing.dev
   - mengakses http://vm.local/blog akan diarahkan ke http://lxc_php7.dev
   - mengakses http://vm.local/app akan diartahkan ke http://lxc_php5.dev
7. untuk kebutuhan presentasi mereka, browser di laptop mereka harus dapat mengakses ketiga url tersebut.
8. Menyiapkan analisa untuk diserahkan ke CTO
   - mengapa untuk kebutuhan php5.6 tidak bisa menggunakan ubuntu 16.04, sehingga perlu diganti os ke debian 9?
   - kenapa harus menggunakan virtualisasi LXC pada skema website yang akan didevelop?
   - apa yang dimaksud dengan proxy server? kenapa vm.local bisa kita anggap sebagai proxy server?



## Jawaban 

1.   Our step is create new folder and move the folder at the same time to change the name, but actually it make a new folder then remove it to new one. This is the command




> sudo mv var/lib/lxc/ubuntu_php5.6 /var/lib/lxc/ubuntu_landing

   

![1.Rename to ubuntu_landing] (https://github.com/menmon/Modul-Praktikum-Virtual-Kelompok-13/blob/main/assets/1.Rename%20to%20ubuntu_landing.PNG)


2.  Install lxc debian 9 with name debian_php5.6 with this code

   sudo lxc-create -n debian_php5.6 -t download -- --dist debian --release sretch --arch amd64 --force-cache --no-validate --server images.linux containers.org

![2.Install lxc debian 9 dengan nama debian_php5.6](D:/Universitas%20Telkom/Semester%205/Sysadmin/Prak%201/2.Install%20lxc%20debian%209%20dengan%20nama%20debian_php5.6.PNG)

3. Set up Nginx 

go to the site wit command cd /etc/nginx/sites-available.

4

create file  with the name lxc_php5.6.dev wiith command nano lxc_php5.6 and save it

![3.Setup Nginx](D:/Universitas%20Telkom/Semester%205/Sysadmin/Prak%201/3.Setup%20Nginx.PNG)



Sekian dari kami, mohon maaf kami kurang melakukan secara optimal karena terdapat beberapa kendala. 

Terima kasih
