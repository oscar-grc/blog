
![Primeros pasos en Webpack](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros_pasos_webpack.png)

> Por: **Oscar García** - 24/02/2021

### Primeros pasos con Webpack

###### En esta serie de artículos aprenderemos las principales características de webpack y veremos con un ejemplo la potencia de esta Herramienta de desarrollo.

----

*Recuerda que este tutorial forma parte de una cadena de tutoriales que puedes encontrar en los siguientes enlaces:*

- **Primeros pasos en webpack**
- [Aprendiendo a usar loaders en webpack](https://github.com/oscar-grc/blog/blob/articles/webpack/aprendiendo_a_usar_loaders.md)
- [Extendiendo la funcionalidad de webpack](https://github.com/oscar-grc/blog/blob/articles/webpack/extendiendo_la_funcionalidad_de_webpack.md)
- [Implementando hot reload en nuestras aplicaciones](https://github.com/oscar-grc/blog/blob/articles/webpack/implementando_hot_reload_en_nuestras_aplicaciones_con_webpack.md) 		

---

#### Indice de contenidos:

- [Entorno](#Entorno)
- [¿Que es Webpack?](#Que-es-Webpack)
- [Instalando Webpack](#instalando-webpack)
- [Configurando Webpack](#configurando-webpack)
- [¿Como funciona Webpack?](#como-funciona-webpack)
- [Conclusiones](#Conclusiones)

---

## Entorno 

- **Sistema Operativo:** MacOS - Catalina 
- **Editor:** Visual Studio Code
- **Webpack:** 5.21.2
- **Webpack-cli:** 4.5.0
- **Node JS:** v12.18.2
- **NPM:** 6.14.5

---

## ¿Que es Webpack?

Hoy día internet ya no es lo que era antes donde solo con `HTML`, `CSS` y un poco de `Javascript` podías montarte un sitio web completo, y no digo que ya no puedas, pero seguramente ya no lo harías únicamente con estas tecnologías y dada la cantidad de herramientas que existen hoy en internet, algunas veces se torna difícil su  gestión, es aquí es donde webpack nos puede ayudar a optimizar nuestros proyectos de forma fácil y sencilla.

¿Pero? ... ¿Que es webpack?.  Cómo  indican en la pagina oficial del proyecto, webpack es un  empaquetador de módulos,  aunque no solo se queda ahí y eso es lo que hace que webpack sea una excelente opción para utilizar en tus proyectos, ya que gracias a los plugins *ya sean propios de webpack o personalizados*, puedes dotar de diferentes funcionalidades al proyecto, una de ellas es la capacidad de  ejecutar distintas tareas.   

![Webpack](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-con-webpack-imagen-webpack.png)

Normalmente cuando tenemos un desarrollo web, este consta de varios archivos de distintos tipos, los cuales pueden ser archivos `html`, hojas de estilos en `CSS`, imágenes, typografias ó pre-procesadores como `LESS`, `SASS` y algunas veces inclusive podemos trabajar con transpiladores como `Typescript` etc, 
es aquí donde webpack nos puede ayudar, ya que el va ir navegando entre los módulos dado un punto de entrada y a partir de ahí comenzará a mapear un árbol de dependencias para poder empaquetar cada uno de esos módulos para despues poder generar un esquema de archivos que el navegador puede interpretar.

Webpack se basa en los siguientes componentes:  
- **Entry points:** *Punto de entrada a partir del cual webpack comienza a construir el árbol de dependencias*
- **Output:** *Punto de salida donde webpack colocara los componentes que el navegador puede entender*
- **Loaders:** *Son transformaciones que se pueden  hacer sobre los módulos ya que por defecto webpack solo puede leer archivos Javascript y JSON*
- **Plugins:** *Funcionalidades que pueden acceder a los estados del ciclo de vida de webpack*

---

## Instalando Webpack

Para este ejemplo necesitaremos descargar el runtime de `nodejs` que puedes encontrar en https://nodejs.org/es/ 

![NodeJS](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-con-webpack-node.png)

Una vez que este instalado podemos comprobar la versión escribiendo en la terminal 

```
node -v
npm -v 
```

Para instalar Webpack, utilizaremos el gestor de paquetes npm, **pero es importante recordar que es recomendable instalarlo de manera local**, así será independiente de cada proyecto que realicemos 

En este caso generaremos un proyecto y lo inicializaremos como un proyecto `node`

```
mkdir webpack-demo
cd webpack-demo && npm int 
```

Solo es necesario llenar los datos solicitados para iniciar el proyecto

![Terminal](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-con-webpack-iniciando-proyecto.png)

Para trabajar con webpack es necesario instalar dos dependencias `webpack` y `webpack-cli`

```
npm install webpack –save-dev  
npm install webpack-cli -save-dev
```
ó podemos utilizar la notación corta

```
npm i webpack -D
npm i webpack-cli -D
```

**Nota:** *El flag –save-dev ó -D, se utiliza para indicar que son dependencias que solo tienen sentido en ambiente de desarrollo por lo que cuando se despliegue a producción ya no es necesario usar webpack y la dependencia no subirá*

una vez realizados los pasos anteriores, al abrir el proyecto podemos ver que tenemos un archivo `package.json` donde se describen las dependencias así como un archivo `node_modules` donde se almacenan las mismas.



![Visual studio code](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/primeros-pasos-con-webpack-visual.png)

---

## Configurando Webpack

Por defecto `webpack` busca un archivo de configuración `webpack.config.js` por lo que será necesario que lo generemos en nuestro proyecto, en este ejemplo lo vamos a generar en el directorio raiz. 

Una de las propiedades que es bueno configurar es el ambiente de desarrollo ya que en activara algunas características que nos ayudaran a leer mejor las salidas cuando aún estamos en desarrollo.

Como mencionamos anteriormente `webpack` necesita un input de entrada donde comenzará a construir el árbol de depencias así como una salida, por lo cual configuraremos las rutas de entrada y de salida en el archivo de configuración.

````
const path = require('path');

module.exports = {
    mode: 'development',
    entry: path.resolve(__dirname, 'src', 'index.js'),
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
    }
}
````

En este caso nuestro archivo de entrada `index.js` esta en `src/` y nuestras salida se llamará `bundle.js` y se encontrará en la carpeta `dist/`

## ¿Como funciona Webpack?

`Webpack` primero intentara resolver a partir del `entrypoint` todos los archivos que importamos para poder generar el grafo de dependencias, en caso de no poder resolver algún archivo, se generara un error.

Una vez resuelto el grafo de dependencias, `webpack` aplicara las transformaciones con los `loaders` definidos, es aquí donde entran los `plugins` que mediante hooks pueden aplicar funcionamiento especifico de acuerdo al punto de anclaje del ciclo de vida de `webpack`.

Cuando `webpack` termina de transformar los datos, estos los empaquetará en un archivo de salida que el navegador pueda entender. 


---
## Conclusiones

Aunque fue un ejemplo sencillo en este primer acercamiento podemos ver la potencia y sencillez de esta herramienta, `webpack` nos ayuda a empaquetar de manera sencilla distintas tecnologías, frameworks, lenguajes o simples recursos. 

Ahora ya puedes configurar tus proyectos con `webpack`, lo siguiente será aprender a utilizar loaders personalizados en el próximo articulo [Aprendiendo a usar loaders](https://github.com/oscar-grc/blog/blob/articles/webpack/aprendiendo_a_usar_loaders.md)

si quieres profundizar más sobre `webpack` puedes ir al sitio oficial https://webpack.js.org/
