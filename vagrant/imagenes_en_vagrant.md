![Imágenes en Vagrant](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-imagenes-en-vagrant.png)

> Por: **Oscar García** - 30/06/2021

### Imágenes en Vagrant

###### En esta serie de artículos aprenderemos las principales características de vagrant y veremos la potencia de esta Herramienta wue nos ayuda a trabajar con maquinas virtuales de forma más simple y potente

----

*Recuerda que este tutorial forma parte de una cadena de tutoriales que puedes encontrar en los siguientes enlaces:*

- [Primeros pasos en Vagrant](https://github.com/oscar-grc/blog/blob/articles/vagrant/primeros_pasos_con_vagrant.md)
- **Imágenes en Vagrant**
- [Configurando nuestra maquina virtual](https://github.com/oscar-grc/blog/blob/articles/vagrant/configurando_nuestra_maquina_virtual.md)
- [Configurando multiples maquinas](https://github.com/oscar-grc/blog/blob/articles/vagrant/configurando_multiples_maquinas_en_vagrant.md)
- [Taller: Configurando nuestro ambiente de desarrollo con Ansible y Vagrant](https://github.com/oscar-grc/blog/blob/articles/vagrant/taller_configurando_nuestro_ambiente_de_desarrollo_con_vagrant_y_ansible.md)


---

#### Indice de contenidos:

- [Entorno](#Entorno)
- [Imágenes en Vagrant](#Imágenes-en-Vagrant)
- [Buscando imágenes para Vagrant](#Buscando-imágenes-para-Vagrant)
- [Trabajando con imágenes en Vagrant](#Trabajando-con-imágenes-en-Vagrant)
- [Conclusiones](#Conclusiones)

---

## Entorno 

- **Sistema Operativo:** Windows 10 - WSL2  
- **Oracle VirtualBox:** 6.1.22
- **Vagrant:** 2.2.16

<br/>

---
<br/>

## Imagenes en Vagrant
<br />


Continuando con esta serie de artículos acerca de Vagrant, veremos que son las imágenes en Vagrant y  aunque en Vagrant no se llaman imágenes como tal, y en su lugar son llamadas `cajas` o `boxes`, en este articulo asumiremos el termino `imágenes` ya que es más común este nombre en maquinas virtuales.

Las imágenes en Vagrant son archivos con ciertas configuraciones compatibles con el proveedor de virtualización `"En nuestro caso Vitualbox"`, que además de contener la imagen del sistema a virtualizar contiene un par de formatos más que indican configuraciones y metadatos propios de Vagrant lo que permite que se pueda trabajar directamente desde Vagrant en nuestra terminal y no necesariamente desde la interfaz grafica del proveedor.

Cualquier persona puede generar estas imágenes y compartirlas para su uso, pero lo más habitual es realizarlo mediante el repositorio que nos brinda  `Hashicorp`

<br/>

## Buscando imagenes para Vagrant

<br/>

`Hashicorp` nos brinda de un repositorio donde podemos realizar búsquedas de las imágenes oficiales y no oficiales que se encuentran en su repositorio.

https://app.vagrantup.com/boxes/search

Dentro del sitio, podemos realizar búsquedas de nuestras imágenes y no solo eso, también podemos filtrar por el proveedor de virtualización que estemos manejando. 

- Virtualbox
- vmware
- libvirt
- openstack 

entre otros ...

![hashicorp search](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/repositorio-de-imagenes-hashicorp.PNG)

Para nuestro ejemplo utilizaremos la imagen oficial de ubuntu (las imágenes oficiales son distribuidas por los desarrolladores del proyecto)

![ubuntu](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/repositorio-de-imagenes-hashicorp-ubuntu.PNG)

Una vez que elegimos la imagen o box a utilizar, podemos ver que nos indica como configurar nuestro Vagrant file

![ubuntu vagrant file](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/repositorio-de-imagenes-hashicorp-vagranfile.PNG)

En nuestro ejemplo la imagen a utilizar se indica de la siguiente forma:

```
config.vm.box = "ubuntu/trusty64"
```

La imagen se identifica de la forma ``usuario/nombre-de-imagen``

Nota: Recuerda intentar descargar siempre imágenes oficiales, y en caso de no ser así poner especial atención a la imagen.

<br />

## Trabajando con imagenes en Vagrant

<br />


### Instalando imagenes en local:
<br />
Una vez que identificamos cual es la imagen que queremos utilizar como vimos anteriormente, podemos simplemente agregarla al repositorio local con el siguiente comando:

```
vagrant add ubuntu/trusty64
```

![vagrant add](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-add.PNG)

También podemos levantar la maquina virtual (Una vez definido el archivo de configuración Vagrantfile) y se descargará de forma automática la imagen a utilizar

```
vagrant up
```

### Listando imagenes instaladas:

<br />

Cuando instalamos una imagen en nuestra maquina, estas se resguardan en el siguiente directorio ``~/.vagrant.d/boxes``

![list boxes](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/box-list.PNG)

Pero si solo queremos listar que imágenes que tenemos instaladas, podemos hacerlo directamente desde el siguiente comando de Vagrant:

```
vagrant box list
```

![vagrant box list](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-box-list.PNG)

Si observamos los datos dentro de las imágenes instaladas, podemos ver que no tenemos ningún archivo empaquetado ya que cuando vagrant realiza la instalación de la imagen, esta se descomprime y se aloja en el directorio:

![ubuntu detail](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/imagen-metadata.PNG)

Pero si requerimos empaquetar la imagen para poder compartirla o cualquier otra situación Vagrant nos proporciona el siguiente comando:

```
vagrant box repackage ubuntu/trusty64 virtualbox xxx.x
```

### Acutalizando imagenes:
<br />

También podemos verificar si la imagen utilizada es la mas actual o tiene actualizaciones

```
vagrant box outdated --global
```

![Vagrant box outdated](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-outdated.PNG)

En caso de existir cambios, podemos actualizar la imagen con el siguiente comando:

``` 
vagrant box update
```

### Eliminar imagenes:
<br />

Vagrant también nos permite eliminar las imágenes de forma sencilla una vez que ya no son utilizadas 

```
vagrant box remove ubuntu/trusty64
```

![Vagrant remove](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-remove.PNG)


<br />

---

<br />

## Conclusiones
<br />
En este articulo vimos un acercamiento más a detalle de como se gestionan las imágenes o ``boxes`` dentro de `Vagrant` lo que nos ayudará a trabajar de forma más sencilla con nuestras maquinas virtuales.

Ahora ya puedes comenzar a descargar, instalar, actualizar y borrar imágenes para tus ambientes con `vagrant`, en el próximo articulo vamos a configurar y poner en marcha nuestras primeras maquinas virtuales [Configurando nuestra maquina virtual](https://github.com/oscar-grc/blog/blob/articles/vagrant/configurando_nuestra_maquina_virtual.md)

si quieres profundizar más sobre `Vagrant` o `VirtualBox` puedes ir a los sitios oficiales

- https://www.vagrantup.com/
- https://www.virtualbox.org/
- https://app.vagrantup.com/boxes/search
