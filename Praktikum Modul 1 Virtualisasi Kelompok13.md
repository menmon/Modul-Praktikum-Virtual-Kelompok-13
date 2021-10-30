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

1. ##### Rename ubuntu_php5.6 menjadi ubuntu_landing, serta rubah IP mengikuti skema yang baru



Our step is create new folder and move the folder at the same time to change the name, but actually it make a new folder then remove it to new one. This is the command




> ```
> sudo mv var/lib/lxc/ubuntu_php5.6 /var/lib/lxc/ubuntu_landing
> ```
>
> 

   



1. ##### Install lxc debian 9 dengan nama debian_php5.6



after that using command

```
 sudo lxc-start -n ubuntu_landing
```

 and 

```
sudo lxc-attach -n ubuntu_landing
```

 

then set IP ubuntu landing from 10.0.3.102 to 10.0.3.103 with command 

```
nano /etc/network/interfaces 
```



and save.



 Restart network with command

```
 systemctl restart networking.service
```

check IP using command  

```
sudo lxc-ls -f
```

and see the IP has changed.


2.  Install lxc debian 9 with name debian_php5.6 with this code

   ```
   sudo lxc-create -n debian_php5.6 -t download -- --dist debian --release sretch --arch amd64 --force-cache --no-validate --server images.linux containers.org
   ```



then install curl with command 

```
apt install nano net-tools curl
```



 When done setting  dhcp static on debian_php5.6 using ip 10.0.3.102 with go to directory using command

```
nano /etc/network/interfaces
```



Then restart with command

 

```
systemctl restart networking.service
```



3. ##### setup nginx pada debian_php5.6 untuk domain http://lxc_php5.dev , buat halaman index.html yang menerangkan informasi nama lxc





 	Go to container using command

```
sudo lxc-attach -n debian_php5.6
```



​	Install nginx with command

```
sudo apt install nginx nginx-extras
```



​	go to the sites-available directory using with

```
cd /etc/nginx/sites-available
```



​	make file and fill with nano text editor with command

```
sudo nano lxc_php5.6.dev
```



rename the server with lxc_php5.dev and save

```
   cd ../sites-enabled
```

while on directory /etc/nginx/sites-available



create file link from sites-available directory inside sites-enabled udaing command

```
ln -s /etc/nginx/sites-available/lxc_php5.6.dev
```



check it with command

```
nginx -t
```



reload it nginx with [command]()

```
nginx -s reload
```



Go to the hosts directory using command 

```
nano /etc/hosts`
```

Add the localhost ip 127.0.0.1 of lxc_php5.6 with the file name lxc_php5.dev.



Now go to html directory using command

```
cd /var/www/html
```



create new directory with name lsx_php5.6 using command

```
mkdir lxc_php5.6
```



Copy the default index.nginx-debian.html to the lxc_php5.6 file and name it index.html using command 

```
cp index.nginx-debian.html lxc_php5.6/index.html
```



go to lxc_php5.6 with

```
cd lxc_php5.6
```



Edit index.html using command

```
sudo nano index.html
```



Check file index.html  if connect using comand

```
curl -i http://lxc_php5.dev
```



But  we got an error here



4. ##### setup nginx pada ubuntu_landing untuk domain http://lxc_landing.dev , buat halaman index.html yang menerangkan informasi nama lxc

   

first step 

```
sudo apt install nginx nginx-extras 
```



Go to

```
cd /etc/nginx/sites-available
```



make a file with

```
touch lxc_php5.6.dev
```



​    Edit lxc_php5.6.dev file using 

```
nano lxc_php5.6.dev
```

 , name it the server name with lxc_landing.dev and save it



Go to the 

```
 `cd ../sites-enabled`.
```



Create lxc_php5.6 file link from sites-available directory in sites-enabled directory using command 

```
ln -s /etc/nginx/sites-available/lxc_php5.6.dev
```

.

check the script with command

```
nginx -t
```



Reload the nginx  using command 

```
`nginx -s reload`
```



go to the hosts directory using command 

```
`nano /etc/hosts`
```

.Add the localhost ip 127.0.0.1 of lxc_php5.6 with the file name lxc_landing.dev.



go to the html directory 

`cd /var/www/html`



Create a new directory and name it lxc_php5.6 

```
`mkdir lxc_php5.6`.
```



Copy the default index.nginx-debian.html to the lxc_php5.6 file and name it index.html using command 

```
`cp index.nginx-debian.html lxc_php5.6/index.html`.
```



go to the lxc_php5.6 directory 

```
`cd lxc_php5.6`.
```



Edit index.html 

```
 `nano index.html` 
```

 save it.



Check index.html

```
curl -i http://lxc_landing.dev
```



#### ANALYSYS

1. ##### Why prefer using debian 9 then ubuntu 16.04?
    
    First debian 9 basically for beginners and stable for use, more than that debian 9 available for free. More than that ubuntu 16.04 and debian 9 is not different that much.
    
    
    
2. ##### Why use LXC virtualization on the website schema that will be developed?

    Cause LXC was used for virtualization based OS allow creation and running multiple in  isolated Linux virtual environments (VE) with single host or user.

    

3. ##### what is a proxy server? why can we think of vm.local as a proxy server?

    proxy is a system that allows us to be able to access the internet network using a different IP from the one received by the device. This system uses a proxy server to work. While the proxy server itself is a device or computer that is used to provide proxy services. VM.local can be a server cause with configuration on host admin we can use it anyway in some of operating system, that can be our bridge to the browser.




Sekian dari kami, mohon maaf kami kurang melakukan secara optimal karena terdapat beberapa kendala. 

Terima kasih
