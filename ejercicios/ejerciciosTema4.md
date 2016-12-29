#Ejercicios Gestión de infraestructuras virtuales

##Ejercicio 1
###Instala LXC en tu versión de Linux favorita. Normalmente la versión en desarrollo, disponible tanto en GitHub como en el sitio web está bastante más avanzada; para evitar problemas sobre todo con las herramientas que vamos a ver más adelante, conviene que te instales la última versión y si es posible una igual o mayor a la 2.0.

Para instalar lxc he usado `sudo apt-get install lxc1`

Para comprobar la versión instalada hay que utilizar `lxc --version`

Mi versión es la 2.0.8


##Ejercicio 2
###Instalar una distro tal como Alpine y conectarse a ella usando el nombre de usuario y clave que indicará en su creación

Debemos usar `sudo lxc-create -t alpine -n una-caja` para la creación de un contenedor con alpine-linux. Con -t le indicamos el template y con -n el nombre

Una vez creada debemos arrancarlo

`sudo lxc-start -n una-caja`

Ahora que nuestro contenedor está activado, podemos conectarnos a él

`sudo lxc-attach -n una-caja`

![ejercicio2](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/ejercicio2_zpshqebhtfw.png)

##Ejercicio 3
###Provisionar un contenedor LXC usando Ansible o alguna otra herramienta de configuración que ya se haya usado

Lo primero que debemos hacer es instalar el plugin de lxc para Vagrant en caso de que no dispongamos de él

`sudo vagrant plugin install vagrant-lxc`

Tras instalarlo, tenemos que usar una caja base de Atlas

`vagrant init fgrehm/wheezy64-lxc`

Antes de levantar el contenedor, debemos editar el Vagrantfile generado para indicarle que utilice un playbook de ansible. El playbook usado puede ser consultado [aquí](https://raw.githubusercontent.com/acasadoquijada/MyStudentBot/master/provision/ansible/playbook.yml)

Finalmente, procedemos a lanzar la máquina.

`sudo vagrant up --provider=lxc`

Y conectarnos a ella

`sudo vagrant ssh`

![ejercicio3](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/ejercicio3_zpsgc8uzmlv.png)

##Ejercicio 4
###Instalar una imagen alternativa de Ubuntu y alguna adicional, por ejemplo de CentOS.

Para instalar una imagen de Ubuntu, podemos usar

`sudo docker pull ubuntu`

![ejercicio4-1](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/ejercicio4-1_zpsauzqi2f3.png)

Una opción para comprobar que la instalación ha sido exitosa es ejecutar

`sudo docker run ubuntu ls`

![ejercicio4-2](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/ejercicio4-2_zpsnl33w6li.png)

Para instalar CentOS basta con repetir los comandos anteriores sustituyendo `ubuntu` por `centos`

![ejercicio4-3](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/ejercicio4-3_zps1ekxvpo7.png)

![ejercicio4-4](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/ejercicio4-4_zpsstppidcl.png)


##Ejercicio 5
###Crear a partir del contenedor anterior una imagen persistente con commit.

En este caso voy a utilizar un contenedor con alpine.

Si no disponemos de uno podemos instalarlo con

`sudo docker pull alpine`

Lo primero que debemos hacer es conectarnos a él e instalar todo lo necesario.

`apk update`
`apk upgrade`
`apk add git perl`

![ejercicio5-1](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/ejercicio5-1_zpszp6ozpoe.png)

Una vez instalado lo necesario hay que obtener el ID del contenedor. Para ello usamos

`sudo docker ps -a`

![ejercicio5-2](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/ejercicio5-2_zpswec7rn9x.png)

En mi caso la ID del contenedor de alpine es `37af57cffd76`

Ahora realizamos el commit

`sudo docker commit 37af57cffd76 alpine2`

Y podemos comprobar que se ha creado correctamente una nueva imagen conectándonos a ella.

`sudo docker run -it alpine2`

![ejercicio5-3](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/ejercicio5-3_zpsuqzyocbb.png)

##Ejercicio 6
###Reproducir los contenedores creados anteriormente usando un Dockerfile

Tenemos que empezar creando un Dockerfile indicando lo que va a ser instalado en el contenedor

~~~
FROM alpine:latest
MAINTAINER acasadoquijada <acasadoquijada@GMail.com>


RUN apk update && apk upgrade
RUN apk add perl
RUN apk add git
~~~

Con el Dockerfile creado ahora hay que ejecutar

`sudo docker build -t acasadoquijada/alpine .`

Nota: No olvidar el punto al final, si no el comando no funcionará

![ejercicio6](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/ejercicio6_zpsje2lklug.png)

Ya creado el contenedor podemos conectarnos a él y comprobar que se ha instalado lo que se le indicó en el Dockerfile

![ejercicio6-2](http://i1045.photobucket.com/albums/b460/Alejandro_Casado/ejercicio6-2_zpsz3223l6e.png)



















































