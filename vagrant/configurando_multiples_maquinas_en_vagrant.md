![Configurando múltiples maquinas en Vagrant](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/configurando-multiples-maquinas-en-vagrant.png)

> Por: **Oscar García** - 02/07/2021

### Configurando múltiples maquinas en Vagrant

###### En esta serie de artículos aprenderemos las principales características de vagrant y veremos la potencia de esta Herramienta wue nos ayuda a trabajar con maquinas virtuales de forma más simple y potente

----

*Recuerda que este tutorial forma parte de una cadena de tutoriales que puedes encontrar en los siguientes enlaces:*

- [Primeros pasos en Vagrant](https://github.com/oscar-grc/blog/blob/articles/vagrant/primeros_pasos_en_vagrant.md)
- [Imágenes en Vagrant](https://github.com/oscar-grc/blog/blob/articles/vagrant/imagenes_en_vagrant.md)
- [Configurando nuestra maquina virtual](https://github.com/oscar-grc/blog/blob/articles/vagrant/configurando_nuestra_maquina_virtual.md)
- **Configurando múltiples maquinas**
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

### Redes en Vagrant

Por lo general cuando trabajamos con maquinas virtuales, necesitamos que estén conectadas a  internet bien sea para poder instalar software o clonar repositorios. `Vagrant` por defecto proporciona un sistema de redes muy sencillo que cumple las necesidades básicas para ambientes de desarrollo, pero pese a esto, algunas veces es necesario poder configurar redes de forma especifica ya que en entornos de múltiples maquinas algunas veces queremos estas maquinas tengan acceso entre ellas mediante un rango especifico, o que tengan configuradas ips privadas en segmentos personalizados.

`Vagrant` nos proporciona dos sistemas de redes:

- **Red privada**: Nos permite conectar la maquina virtual con un rango de  redireccionamiento privado, se utiliza principalmente cuando desde la maquina anfitriona queremos conectarnos a la maquina virtual mediante un puerto especifico ó cuando queremos conectar varias maquinas virtuales.
- **Red pública**: Nos permite conectar nuestra maquina virtual en modo puente a la maquina anfitriona mediante una tarjeta de red lógica o inalámbrica.

### Redes privadas

Vagrant nos permite agregar redes privadas de una marea muy sencilla, solo debemos agregar la siguiente sentencia:

```ruby
Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network "private_network", ip: "x.x.x.x"
end
```

En donde colocaremos la **ip privada** que queremos asociar, `Vagrant` soporta redireccionamiento de tipo **ipv4 estático**, **ipv4 dinámico**, y también **ipv6**.

Ahora, podemos modificar el archivo `Vagranfile` que hemos estado trabajando en artículos anteriores y quedará de la siguiente manera:

```ruby
Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.hostname = "ninja-project"
    config.vm.network "forwarded_port", guest: 80, host: 8080
    config.vm.network "private_network", ip: "198.168.33.10"
    config.vm.provider "virtualbox" do |vb|
      vb.name = "ninja"
      vb.memory = "1024"
      vb.cpus = "2"
      vb.linked_clone = true
    end
  end
```

Re lanzamos la maquina virtual y podemos ver los siguiente:

```ruby
vagrant reload
```

![](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-private-ip-cli.PNG)

Podemos ver que ahora tiene dos adaptadores, la ip que agregamos se muestra como **hostonly**

De igual forma podemos verificar entrando a la maquina virtual: 

Podemos  entrar a la maquia virtual es con el siguiente comando:

```ruby
vagrant ssh
```

Una vez dentro, podemos listar la información de nuestras ips con:

```ruby
ip -a
```
![](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-private-ip-a-maquina-virtual.PNG)

y podemos ver que la ip privada que agregamos en el archivo de configuración `Vagrantfile` ya se encuentra configurada correctamente.

Hasta ahora solo hemos accedido a nuestra maquina virtual mediante el comando `vagrant ssh`, pero esto es posible por que `Vagrant` genera un par de claves publica y privada que permite que por medio de `secure shell` o `ssh` nos conectemos a la maquina, la llave privada se resguarda en la siguiente ruta: **.vagrant/machines/default/virtualbox/private_key**

![](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-private-key.PNG)

Ahora que sabemos donde esta la llave privada podemos intentar conectarnos directamente utilizando la ip privada que definimos anteriormente:

```ruby
ssh -i .vagrant/machines/default/virtualbox/private_key vagrant@198.168.33.10 
```

Podemos ver que pudimos acceder a la maquina virtual sin problema.

![](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-ssh-private-ip.PNG)

 

### Redes públicas

### Simulando entorno real

Normalmente suele usarse `Vagrant` para replicar de manera local nuestros entornos productivos donde solemos tener varios nodos, por lo que trataremos de hacer un ejemplo sencillo donde se pueda entender de manera clara como levantar un par de maquinas:

Lo primero que tenemos que hacer es modificar el archivo de Configuración `Vagrantfile` para definir los nodos o maquinas que vamos a configurar:

```ruby
Vagrant.configure("2") do |config|
  config.vm.define "front" do |front|
    front.vm.box = "ubuntu/trusty64"
    front.vm.hostname = "ninja-front"
  end
  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/trusty64"
    db.vm.hostname = "ninja-back"
  end  
end
```

 

 Al momento de levantar las maquinas virtuales nuevamente podemos ver que se configuran ambos nodos.

![](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-multi-cli.PNG)

Podemos ver en la interfaz grafica de Virtualbox como ahora existen dos maquinas corriendo. 

![](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-multi-virtualbox.PNG)

Ahora, ¿Qué pasa si queremos acceder a las maquinas?, como vimos anteriormente, cuando queremos acceder a una maquina virtual solo utilizábamos el comando `vagrant ssh`

Podemos ver que si intentamos acceder, nos muestra el siguiente error:

![Error vagrant ssh](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-multi-cli-error.PNG)

Tenemos que recordar que ahora no solo estamos trabajando con una solo maquina, por lo que tenemos que indicar a que nodo queremos conectarnos:

```ruby
vagrant ssh front
```

Ahora ya podemos acceder sin problema.

![error nodo](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/vagrant-multi-cli-ssh.PNG)

---
## Conclusiones

Con este pequeño articulo podemos ver lo facil que es montar un entorno con múltiples maquinas.

Ahora ya puedes comenzar a generar tus ambientes con `vagrant`, lo siguiente será ver con un ejercicio práctico como podemos montar ambientes con múltiples maquinas automatizando sus configuración y actualización con ayuda de ansible en el próximo articulo [Imágenes en Vagrant](https://github.com/oscar-grc/blog/blob/articles/vagrant/imagenes_en_vagrant.md)

si quieres profundizar más sobre `Vagrant` o `VirtualBox` puedes ir a los sitios oficiales

- https://www.vagrantup.com/
- https://www.virtualbox.org/
