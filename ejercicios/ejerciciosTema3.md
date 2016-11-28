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

##Ejercicio 2
###Instalar una máquina virtual ArchLinux o FreeBSD para KVM, otro hipervisor libre, usando Vagrant y conectar con ella.


##Ejercicio 3
###Crear un script para provisionar `nginx` o cualquier otro servidor web que pueda ser útil para alguna otra práctica

En este caso he creado un `playbook` de ansible que se encarga de provisionar nginx

~~~
- hosts: all
  sudo: true
  tasks:
    - name: Actualizamos
      apt: update_cache=yes
    - name: Instalar nginx
      apt: name=nginx state=present
~~~

##Ejercicio 4
### Configurar tu máquina virtual usando `vagrant` con el provisionador ansible

Para provisionar mi máquina voy a usar el siguiente `playbook` de ansible:

~~~
- hosts: all
  sudo: true
  tasks:
    - name: Actualizamos
      apt: update_cache=yes
    - name: Instalar pip
      apt: name=python-setuptools state=present
      apt: name=python-dev state=present 
      apt: name=python-pip state=present
    - name: Instalamos supervisor
      apt: name=supervisor state=present
    - name: Instalamos pyTelegramBotAPI
      pip: name=pyTelegramBotAPI
      #http://docs.ansible.com/ansible/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module
      #Contraseña es hola
    - name: Creamos usuario con contraseña y acceso sudo
user: name=user shell=/bin/bash group=admin password=$6$8K/WYk4Ajov$j84F1tY4SSY0T46HEVw14lYgkaQKeXyIS/X0mtvBMVkxD5SRt
~~~


Tras esto, debemos modificar el fichero `Vagrantfile` de la siguiente forma:

~~~
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"

  config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.provision :ansible do |ansible|
    ansible.playbook = "playbook.yml"
  end

end
~~~

Teniendo los preparativos realizados, levantamos la máquina.

`vagrant up`

En caso de que no se provisione automáticamente.

`vagrant provision`

Para comprobar que todo ha funciona nos conectamos via ssh a la máquina.

![ejercicio4](http://i66.tinypic.com/14wblzp.png)
















    
