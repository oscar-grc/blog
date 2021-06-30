![Configurando nuestra maquina virtual](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-configurando-nuestra-maquina-virtual.png)

> Por: **Oscar García** - 30/06/2021

### Configurando nuestra maquina virtual

###### En esta serie de artículos aprenderemos las principales características de vagrant y veremos la potencia de esta Herramienta wue nos ayuda a trabajar con maquinas virtuales de forma más simple y potente

----

*Recuerda que este tutorial forma parte de una cadena de tutoriales que puedes encontrar en los siguientes enlaces:*

- [Primeros pasos en Vagrant](https://github.com/oscar-grc/blog/blob/articles/vagrant/primeros_pasos_con_vagrant.md)
- [Imágenes en Vagrant](https://github.com/oscar-grc/blog/blob/articles/vagrant/imagenes_en_vagrant.md)
- **Configurando nuestra maquina virtual**
- [Configurando multiples maquinas](https://github.com/oscar-grc/blog/blob/articles/vagrant/configurando_multiples_maquinas_en_vagrant.md)
- [Taller: Configurando nuestro ambiente de desarrollo con Ansible y Vagrant](https://github.com/oscar-grc/blog/blob/articles/vagrant/taller_configurando_nuestro_ambiente_de_desarrollo_con_vagrant_y_ansible.md)


---


#### Indice de contenidos:

- [Entorno](#Entorno)
- [Modificando el hardware de nuestras maquinas virtuales](#Modificando-el-hardware-de-nuestras-maquinas-virtuales)
- [Configurando puertos en nuestra maquina virtual](#Configurando-puertos-en-nuestra-maquina-virtual)
- [Ejemplo de redirección de puertos](#Ejemplo-de-redirección-de-puertos)
- [Suspender y lanzar maquinas virtuales](#Suspender-y-lanzar-maquinas-virtuales)
- [Conclusiones](#Conclusiones)

---

## Entorno 

- **Sistema Operativo:** Windows 10 - WSL2  
- **Oracle VirtualBox:** 6.1.22
- **Vagrant:** 2.2.16


---

<br />

## Modificando el hardware de nuestras maquinas virtuales

Anteriormente generamos un par de archivos de configuración `Vagrantfile`, pero lo hicimos de forma muy sencilla, solo configurando la imagen base de la maquina virtual algo que no es muy común ya que usualmente se especifican otras características como tamaño de **memoria ram** o **numero de cpus**

Para configurar el hardware de nuestra maquina virtual podemos hacerlo desde el archivo `Vagrantfile`, aunque es importante mencionar que las sentencias varían de acuerdo al proveedor que utilicemos, en nuestro caso utilizamos `VirtualBox` por lo que nuestro archivo quedaría de la siguiente manera:

![vagrant file](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/maquina-configurada-vagrantfile.PNG)

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "ninja-project"
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.provider "virtualbox" do |vb|
    vb.name = "ninja"
    vb.memory = "1024"
    vb.cpus = "2"
    vb.linked_clone = true
  end
end
```

Ahora podemos ver los cambios una vez que levantamos la maquina virtual con: 

```
 vagrant up
```

o en caso de que ya tengamos nuestra maquina previa podemos re lanzar nuestra maquina con:

```
vagrant reload
```

Ahora si abrimos la interfaz grafica de VirtualBox podemos ver que el nombre de la maquina virtual tiene el nombre y características indicadas

![virtuabox](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/maquina-vagrantfile-virtualbox.PNG)

También dentro de nuestra maquina podemos ver que el nombre del hostname también es el que nosotros indicamos:

![vagrant ssh](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/maquina-vagrantfile-vagrant-ssh.PNG)

### Configurando puertos en nuestra maquina virtual

Muchas veces necesitamos exponer algún puerto especifico dentro de la maquina invitada para que dentro de la maquina anfitriona podamos conectarnos, Esto es una situación muy habitual que ocurre cuando trabajamos con maquinas virtuales ya sea por que estamos trabajando con algún servidor web o por cualquier otra situación que requiere una redirección de puertos.

esto  lo podemos configurar con la siguiente sentencia:

```ruby
config.vm.network "forwarded_port", guest: 80, host: 8080
```

Nuestro archivo quedaría de la siguiente forma:

![vagrant ports](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/maquina-vagrantfile-puertos.PNG)

Esto hace que desde nuestra maquina anfitriona las comunicaciones que entren por el puerto 8080, sean redirigidas al puerto 80 de nuestra maquina virtual

Una vez que lanzamos nuevamente la maquina virtual podemos ver en la salida que ya se encuentran los puertos configurados:

![vagrant prots](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/maquina-vagrantfile-vagrant-puertos-8080.PNG)

### Ejemplo de redirección de puertos

Para ver claramente como funciona la redirección de puertos, vamos a instalar un servidor apache dentro de nuestra maquina, lo primero que tenemos que hacer es actualizar el gestor de paquetes **apt-get**

```
sudo apt-get update
```

Una vez actualizado, procedemos a instalar nuestro servidor apache:

```
sudo apt-get install apache2
```

![install apache](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/maquina-vagrantfile-install-apache.PNG)

Una vez terminada la instalación, podemos ver desde nuestro navegador la maquina anfitriona como nos redirige al puerto 80 de la maquina virtual.

![apache](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/maquina-vagrantfile-view-apache.PNG)

También es importante mencionar que si queremos ver cuales son los puertos soportados por nuestra maquina virtual, Vagrant nos proporciona el siguiente comando:

```
vagrant port 
```

![vagrant ports](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/maquina-vagrantfile-vagrant-port-cli.PNG)

### Suspender y lanzar maquinas virtuales

Vagrant nos permite apagar nuestras maquinas virtuales para evitar el consumo de recursos cuando no se están ocupando.

para poder ver el status de nuestra maquina virtual, podemos utilizar el siguiente comando: 

```
vagrant status
```

![vagrant status](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-status.PNG)

Si queremos suspender la maquina podemos utilizar el siguiente comando:

```
vagrant halt
```

![vagrant halt](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-halt.PNG)

Podemos ver que la maquina virtual se encuentra apagada

![virtualbox](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/virtual-box-halt.PNG)

Para volver a encender la maquina virtual podemos utilizar el comando `vagrant resume`, que solo encenderá la maquina sin revisar si hay algún cambio en el archivo Vagrantfile, si queremos que realice una verificación antes de encender la maquina podemos utilizar `vagrant up` nuevamente.

<br />

---
## Conclusiones

Con este primer acercamiento podemos ver la potencia y sencillez de `Vagrant` que nos facilita el uso de maquinas virtuales con `Virtual Box`. 

Ahora ya puedes comenzar a generar tus ambientes con `vagrant`, lo siguiente será ver como podemos configurar multiples maquinas virtuales en el próximo articulo [Configurando multiples maquinas](https://github.com/oscar-grc/blog/blob/articles/vagrant/configurando_multiples_maquinas_en_vagrant.md)

si quieres profundizar más sobre `Vagrant` o `VirtualBox` puedes ir a los sitios oficiales

- https://www.vagrantup.com/
- https://www.virtualbox.org/
- https://app.vagrantup.com/boxes/search
