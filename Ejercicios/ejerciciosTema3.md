#Ejercicios Gestión de infraestructuras virtuales

##Ejercicio 1
###Instalar una máquina virtual Debian usando Vagrant y conectar con ella.

Como es natural, comenzamos instalando `Vagrant`

`sudo apt-get install vagrant`

Ahora descargamos la imagen de Debian

`vagrant box add debian https://github.com/holms/vagrant-jessie-box/releases/download/Jessie-v0.1/Debian-jessie-amd64-netboot.box`

![ejercicio5-1](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/tema%206/ejercicio5-1_zpsh5dcbisj.png)

Generamos el fichero vagranfile

`vagrant init debian`

![ejercicio5-2](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/tema%206/ejercicio5-2_zps7cxfhspc.png)

Lanzamos la máquina

`vagrant up`

![ejercicio5-3](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/tema%206/ejercicio5-3_zps6eypdx2a.png)

Finalmente comprobamos que podemos conectarnos via ssh a la máquina 

`vagrant ssh`

![ejercicio5-4](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/tema%206/ejercicio5-4_zpsyivzsruu.png)

