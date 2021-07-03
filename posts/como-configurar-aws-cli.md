![¿Cómo configurar AWS CLI?](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/como-configurar-aws-cli.PNG)

> Por: **Oscar García** - 04/07/2021

### ¿Cómo configurar AWS CLI?

###### En esta articulo aprenderemos a instalar y configurar AWS CLI.
	
---

#### Indice de contenidos:

- [Entorno](#Entorno)
- [Instalando AWS CLI](#Instalando-AWS-CLI)
- [Configurando AWS CLI](#Configurando-AWS-CLI)
- [Subiendo imágenes desde AWS CLI](#Subiendo-imágenes-desde-AWS-CLI)
- [Conclusiones](#Conclusiones)

---

## Entorno

- **Sistema Operativo:** MacOS - Catalina
- **aws cli:** 2.2.16

<br />

-----

<br />



### Instalando AWS CLI

La interfaz de linea de comandos de AWS, nos permite interactuar con los servicios de Amazon Webservices (AWS) desde nuestra terminal mediante lineas de comando, esto nos facilita mucho la creacioón, modificación y eliminación de recursos de forma sencilla sin necesidad de acceder a la consola mediante el nageador web.

Lo primero que vamos a hacer es descargar AWS CLI, En nuestro caso utilizaremos la versón para MACOS por lo que iremos a la siguiente url:

https://docs.aws.amazon.com/es_es/cli/latest/userguide/install-cliv2-mac.html


![aws cli](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-descarga-macos.png) 

una vez que descargamos el apk, el instalador nos va ir indicando los pasos a seguir para su correcta instalación (next, next ... next )

![aws cli install](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-instalacion.png)

Terminando el proceso de instalación podemos validar que la instalación fue exitosa

![aws cli version](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-version.png)

### Configurando AWS CLI

Una vez que instalamos AWS CLI debemos configurar nuestras credenciales para poder interactuar con **AWS**

Para poder realizar la configuración antes necesitaremos generar nuestras claves de accesos desde la consola de administración de AWS, por lo cual nos dirigimos a nuestra consola:

![aws console](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-dashboard-inicio.png)

Dentro de la consola, podemos ver que en la sección de IAM, en el menu usuarios, se listan los usuarios que tenemos activos, en nuestro caso solo tenemos el usuario **cloud_user** 

![aws list users](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-dashboard-usuario.png)

Si accedemos a nuestro usuario (que en nuestro caso es **cloud_user),** vemos que ya tenemos un par de claves generadas, para este ejemplo eliminaremos las claves por defecto y generaremos un par de claves nuevas, pero para poder eliminarlas primero vamos a desactivarlas:

![aws credentiasl off](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-dashboard-usuario-clave-desactivada.png)

Una vez desactivadas, procedemos a eliminarlas

![aws credentials remove](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-dashboard-usuario-clave-eliminada.png) 

Una vez realizado esto, pasamos a generar nuestras nuevas claves:

![aws credentials](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-dashboard-usuario-clave-nueva.png)

Es importante que descarguemos las claves, ya que las utilizaremos después para la configuración.

**Recuerda que estas claves son privadas y nunca debes compartirlas con nadie, ya que podrían hacer mal uso de ellas y estarías exponiendo tu cuenta.**   

Ahora pasamos a nuestra terminal y con el siguiente comando podemos comenzar a configurar nuestras credenciales:

```
aws configure
```

![aws configure](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-configure.png)

Podemos ver que nos pide los datos descargados previamente y adicionalmente también nos solicita una region por defecto.

En proximos articulos explicaremos que son las regiones a detalle, pero por el momento entendamos que una region es cada zona geografica donde AWS tiene presencia, a esta zona se le asigna un nombre el cual puedes obtener en la siguiente url: https://aws.amazon.com/es/about-aws/global-infrastructure/ 

![aws cli regions console](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-dashboard-regiones.png)

En nuestro caso utilizaremos el Norte de Virgina (us-east-1).

Una vez configuradas nuestras credenciales podemos acceder a ellas en la ruta `~/.aws/credentials`

![aws cli credentials](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-configure-credentials.png)

En este caso, vemos que solo hay configuradas un par de claves, pero en caso de tener más claves configuradas, estas se listaran en el archivo.

Vamos a validar que tenemos acceso a la consola de AWS desde nuestra terminal con el siguiente comando que lista los usuarios registrados en nuestra cuenta:

```
aws iam list-users
```

si todo va bien, deberiamos poder ver los usuarios registrados:

![aws iam list-users](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-iam-list.png)

### Subiendo imágenes desde AWS CLI

Para demostrar que es sumamente sencillo trabajar con AWS desde la terminal gracias a AWS CLI, vamos a generar un bucket S3, que como sabemos, S3 son las siglas de `Simple Storage Service`, que no es otra cosa más que un repositorio donde podemos alojar archivos de forma sencilla.

desde la terminal creamos un bucket S3 con el siguiente comando:

```
aws s3 mb s3://{nombre del bucket}
```

recuerda que el nombre del bucket es unico a nivel de AWS por lo que si alguien ya utilizo el nombre que quieres ocupar, sera necesario que intentes con otro nombre. 

![aws s3 mb ](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-s3-create.png)

Podemos validar desde la consola de AWS que el bucket se genero correctamente:

![aws s3 console](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-s3-dashboard.png)

Para el ejercicio,  solo subiremos una imagen al bucket que acabamos de crear.

```
aws s3 cp {nombre_archivo.extension} s3://{nobre del bucket}/{nobre_archivo.extension}
```

![aws s3 cp](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-s3-copy-assets.png)

Desde la terminal, podemos ver que el archivo se subio correctamente, ahora podemos validar desde la consola de aws.

![aws s3](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-s3-assets-dashboard.png)

Ahora vemos que la imagen demo, se subio al bucket con solo un par de comandos.

para finalizar el ejercicio, solo queda borrar el bucket ya que solo lo generamos para el ejemplo.

```
aws s3 rb s3://{nombre del bucket} --force
```

![aws s3 bucket rb](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aws-cli-s3-delete.png)

-----

## Conclusiones 


Ahora ya puedes puedes empezar a interactuar con los servicios de AWS de forma más sencilla mediante su interfaz de linea de comandos (AWS CLI).

si quieres profundizar más sobre `AWS` puedes ir a los sitios oficiales

- https://docs.aws.amazon.com/
- https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html
- https://docs.aws.amazon.com/cli/latest/reference/
- https://aws.amazon.com/es/about-aws/global-infrastructure/regions_az/