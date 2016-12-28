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

![ejercicio2]()

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

`vagrant ssh`

![ejercicio3]()




