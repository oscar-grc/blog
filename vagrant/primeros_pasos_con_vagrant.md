![Primeros pasos en Vagrant](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-primeros-pasos-con-vagrant.png)

> Por: **Oscar García** - 04/06/2021

### Primeros pasos con Vagrant

###### En esta serie de artículos aprenderemos las principales características de vagrant y veremos la potencia de esta Herramienta wue nos ayuda a trabajar con maquinas virtuales de forma más simple y potente

----

*Recuerda que este tutorial forma parte de una cadena de tutoriales que puedes encontrar en los siguientes enlaces:*

- **Primeros pasos en Vagrant**
- [Imágenes en Vagrant](https://github.com/oscar-grc/blog/blob/articles/vagrant/imagenes_en_vagrant.md)
- [Configurando nuestra maquina virtual](https://github.com/oscar-grc/blog/blob/articles/vagrant/configurando_nuestra_maquina_virtual.md)
- [Configurando multiples maquinas](https://github.com/oscar-grc/blog/blob/articles/vagrant/configurando_multiples_maquinas_en_vagrant.md)
- [Taller: Configurando nuestro ambiente de desarrollo con Ansible y Vagrant](https://github.com/oscar-grc/blog/blob/articles/vagrant/taller_configurando_nuestro_ambiente_de_desarrollo_con_vagrant_y_ansible.md)


---

#### Indice de contenidos:

- [Entorno](#Entorno)
- [¿Que es Vagrant?](#¿Que-es-Vagrant?)
- [Instalando Vagrant](#instalando-webpack)
- [Instalando VirtualBox](#configurando-webpack)
- [Creando nuestra primer maquina vitual](#primer-bundle)
- [Conclusiones](#Conclusiones)

---

## Entorno 

- **Sistema Operativo:** Windows 10 - WSL2  
- **Oracle VirtualBox:** 6.1.22
- **Vagrant:** 2.2.16

---

## ¿Que es Vagrant?

Vagrant es una herramienta que nos
permite generar entornos de trabajo fácilmente configurables,
reproducibles y portables.

Vagrant esta pensado para crear y
gestionar escenarios virtuales sencillos, no suele usarse para montar
infraestructuras complejas en ambientes productivos.

``Vagrant`` nos ayuda a trabajar con
maquinas virtuales de diferentes proveedores, puede trabajar con
``VirtualBox``, ``VMWare``, ``AWS`` entre otras herramientas de virtualización,
además nos permite crear y destruir ambientes para pruebas de forma
sencilla gracias a que trabajamos con ``infraestructura como código.``

Una de las características interesantes
de Vagrant es que nos permite gestionar la configuración
integrándose con herramientas como   ``Chef``, ``Puppet`` o ``Ansible``,
permitiendo separar la ``gestión de la configuración`` y ``la
infraestructura como código``.

``Vagrant`` es mantenida por ``Hashicorp`` y esta pensada para escenarios sencillos, si se requiere realizar aprovisionamiento de escenarios complejos hashicorp nos proporciona la herramienta ``Terraform``.

## Instalando Vagrant

![Vagrant](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-vagrant-logotipo-vagrant.PNG)

Instalar Vagrant es realmente sencillo solo tenemos que ir a la pagina oficial [https://www.vagrantup.com/downloads](https://www.vagrantup.com/downloads) y seguir los pasos indicados de acuerdo a tu sistema operativo, en el caso de Windows lo vamos a instalar dentro de **WSL2** **“Windows Subsystem for Linux”** lo que nos permite realizar la instalación mediante **apt-get**

```
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch = amd64] https://apt.releases.hashicorp.com $ (lsb_release -cs) main"

sudo apt-get update && sudo apt-get install ``Vagrant``

````

Una vez realizada la instalación podemos verificar la versión

```
vagrant --version
```

![Vagrant Version](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-vagrant-version-vagrant.PNG)

Una de las cosas que es importante mencionar es que ``Vagrant`` nos proporciona ayuda general con el comando 

```
vagrant -h
```

![Vagrant Help](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-vagrant-ayuda-command.PNG)


Pero tambien nos puede dar ayuda más detallada a nivel de comando por ejemplo:

```
vagrant ssh -h
```

![Vagrant Help](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-vagrant-ayuda_command-dos.PNG)

Para poder ejecutar vagrant, es necesario tener un sistema de virtualización instalado. En nuestro caso utilizaremos ``VirtualBox``. 

## Instalando VirtualBox


![VirtualBox](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-vagrant-virtualbox-logotipo.PNG)

VirtualBox es una herramienta de virtualización que nos permite alojar distintos sistemas operativos invitados en nuestra maquina anfitrion mediante virtualización de recursos. Si quieres aprender más acerca de la virtualización y como VirtualBox lo maneja te recomiendo el siguiente articulo: [https://www.virtualbox.org/manual/ch01.html#virt-why-useful](https://www.virtualbox.org/manual/ch01.html#virt-why-useful)

Para descargar ``virtualBox`` solo tenemos que ira su sitio web: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads), y en la sección de descargas seleccionar la versión de nuestra maquina. ``VirtualBox`` esta soportado para distintos sistemas operativos, en nuestro caso instalaremos la version **6.1.22** para Windows

Descargará un ejecutable y al igual que la mayoria de programas en Windows tendremos que ir siguiendo las indicaciones del wizard.

![VirtualBox](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-vagrant-install-virtual-box.PNG)

## Creando nuestra primer maquina virtual


Para poder generar nuestro primer escenario, es importante recordar que por un lado necesitamos gestionar el sistema operativo de la maquina a generar y por otro lado gestionaremos los archivos con las configuraciónes especificas del escenario;  por lo que es necesario primero obtener la imágen del sistema opertivo de nuestra maquina (en Vagrant llamadas Box)Podemos ver un listado de boxes en [https://app.vagrantup.com/boxes/search](https://app.vagrantup.com/boxes/search) una vez seleccionado el box a utilizar necesitamos descargarlo con el
siguiente comando:

```
vagrany box add ubuntu/trusty64
```

![Vagrant box](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-vagrant-install-box.PNG)


Ahora bien, una vez que ya tenemos la imágen descargada, procedemos a generar nuestro escenario el cual estara definido en un archivo **Vagrantfile**, por lo que cada escenario debe de estar en una carpeta aislada para crear el escenario, es necesario utilizar el comando ``vagrant init`` e indicarle que box utilizaremos

```
vagrant init debian/jessie64
```
![Vagrant init](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-vagrant-init-vagrant.PNG)

Una vez realizado el comando podemos ver que se genero dentro de la carpeta un archivo **Vagrantfile**

![Vagrantfile](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-vagrant-vagrant-file.PNG)

Este archivo esta desarrollado en ``Ruby`` y contiene información propia del escenario, en este caso unicamente contiene la imagen del sistema operativo

Con este archivo generado, ya podemos arrancar la maquina virtual con el siguiente comando:

```
vagrant up
```
![Vagrant Up](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-vagrant-vagrant-up.PNG)

Vagrant realizara configuraciones propias y una vez finalizado podemos verificar el estado del escenario con el siguiente comando:

```
vagrant status
```
![Vagrant status](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-vagrant-status.PNG)

Vagrant genera un par de claves (Pública y privada) para poder acceder a la maquina solo es necesario utilizar el siguiente comando:

```
vagrant ssh
```
![Vagrant SSH](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-vagrant-vagrant-ssh.PNG)

En proximos articulos veremos más a detalle la configuración de nuestros escenarios, por ahora solo queda apagar y destruir la maquina generada:

```
vagrant halt 
```
![Vagrant Halt](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-vagrant-halt.PNG)

```
vagrant destroy
```
![Vagrant destroy](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-vagrant-destroy.PNG)


---
## Conclusiones

Con este primer acercamiento podemos ver la potencia y sencillez de ``Vagrant`` que nos facilita el uso de maquinas virtuales con `Virtual Box`. 

Ahora ya puedes comenzar a generar tus ambientes con `vagrant`, lo siguiente será ver de donde podemos obtener las imágenes para poder generar nuestras maquinas virtuales en el próximo articulo [Imágenes en Vagrant](https://github.com/oscar-grc/blog/blob/articles/vagrant/imagenes_en_vagrant.md)

si quieres profundizar más sobre `Vagrant` o `VirtualBox` puedes ir a los sitios oficiales

- https://www.vagrantup.com/
- https://www.virtualbox.org/
