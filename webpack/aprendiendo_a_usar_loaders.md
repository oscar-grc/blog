![Aprendiendo a usar loaders](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aprendiendo-a-usar-loaders.png)

> Por: **Oscar García** - 21/04/2021

## Aprendiendo a usar Loaders

###### En esta serie de artículos aprenderemos las principales características de webpack, en este articulo veremos que son los loaders y por que su importancia en Webpack

*Recuerda que este tutorial forma parte de una cadena de tutoriales que puedes encontrar en los siguientes enlaces:*

- [Primeros pasos en webpack](https://github.com/oscar-grc/blog/blob/articles/webpack/primeros_pasos_en_webpack.md)
- **Aprendiendo a usar loaders en webpack**
- [Extendiendo la funcionalidad de webpack](https://github.com/oscar-grc/blog/blob/articles/webpack/extendiendo_la_funcionalidad_de_webpack.md)
- [Implementando hot reload en nuestras aplicaciones](https://github.com/oscar-grc/blog/blob/articles/webpack/implementando_hot_reload_en_nuestras_aplicaciones_con_webpack.md) 

---

## Indice de contenidos:

- [Entorno](#Entorno)
- [¿Que son los loaders?](#¿Que-son-los-loaders?)
- [Cargando nuestro primer loader](#cargando-loader)
- [CSS loaders](#css-loaders)
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

## ¿Que son los loaders?

Los loaders son una de las partes más importantes dentro de webpack ya que gracias a ellos podremos utilizar ficheros que por defecto no son soportados y de otra forma no podríamos trabajar con ellos.

Uno de los detalles que es importante mencionar con respecto a webpack, es que por defecto solo interpreta archivos de tipo ``Javascript`` y ``Json`` por lo que si necesitamos poder trabajar con cualquier otro tipo de recursos deberemos utilizar **“Loaders”** estos son componentes de código que le indican a webpack como procesar y convertir los archivos de otras extensiones a módulos que pueden ser consumidos dentro de la aplicación.


## Cargando nuestro primer loader


Webpack nos permite cargar los loaders de varias formas 

- Dentro del archivo de configuración
- De forma inline en cada archivo mediante import 
- Mediante comandos webpack-cli

*En este ejercicio veremos como se configuran mediante un archivo de configuración ya que es la forma más habitual de utilizarlos.*

Para poder trabajar con Loaders, es necesario colocarlos dentro del archivo de configuración en la propiedad **modules** y básicamente definiremos las reglas a considerar por webpack para poder ir transformando todos esos archivos con los que queremos trabajar:

**./webpack.config.js**
```JS
...

module:{
    rules:[
        {
            test: /\.css$/,
            use:[
                'style-loader',
                'css-loader'
            ]
        }
    ]
}

...

```

Dentro de la configuración de los loaders es necesario definir dos propiedades:

- **TEST:** indica que tipos de archivos se van a transformar 
- **USE:** indica que loader será utilizado para adaptar los archivos


## CSS loaders

Por defecto webpack no interpreta CSS, por lo que requerimos utilizar un loader para poder trabajar con hojas de estilos dentro de nuestro proyecto.

Para trabajar con CSS utilizaremos dos loaders:

- **CSS loader:** Se encarga de interpretar los imports y urls para poder agragar css a nuestro proyecto 
- **Style loader:** Nos ayuda a inyectar el css en nuestro DOM de forma transparente mediante etiquetas style

Lo primero que vamos a necesitar es instalar los loaders:

```
npm i -D css-loader style-loader 
```

Una vez instalados, vamos a proceder a configurarlos dentro del  archivo de configuración.

**./webpack.config.js**
```JS
const path = require('path');

module.exports = {
    mode: 'development',
    entry: path.resolve(__dirname, 'src', 'index.js'),
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
    },
    module:{
        rules:[
            {
                test: /\.css$/,
                use:[
                    'style-loader',
                    'css-loader'
                ]
            }
        ]
    }
}
```

para no tener que ejecutar webpack desde la carpeta localizada en ``node_modules`` como en el ejercicio anterior, vamos a configurar un script en el archivo package.json que nos ayudara a ejecutar webpack de forma mas sencilla con ayuda de npm 

**./package.json**
``` JSON
...

"scripts": {
    "build": "webpack --mode development"
},

...
```
Ahora gracias a este comando podemos correr webpack con el siguiente comando:

```
 npm run build
```

una vez que realizamos los pasos anteriores, vamos a generar una hoja de estilos sencilla donde solo modificaremos el color del nuestra plantilla html, esto para mantener el ejemplo lo más simple posible, y poder entender el funcionamiento de los loaders.

**./src/css/global.css**
```CSS
body{
    background: rgb(144, 205, 255);
    color:rgb(70, 70, 240);
}
```

Dentro del archivo index, que en nuestro ejemplo es el punto de entrada para webpack, es necesario agregar la siguiente linea de código para importar los estilos.

**./src/index.js**
```JS
import message from './grettings';
import './css/global.css';

message();
```
**Nota:** 
Como podemos observar, en este caso que el tipo de archivo es diferente de javascript, **es importante colocar la extension del mismo.**

Una vez que lanzamos el comando ``npm run build``  podemos ver que webpack comienza a realizar el empaquetado y si todo sale bien podremos ver el siguiente mensaje.

![Aprendiendo a usar loaders terminal](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aprendiendo-a-usar-loaders-success.png)

Nuevamente podemos ir a nuestro navegador, y veremos los estilos definidos.

![Aprendiendo a usar CSS loaders](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/aprendiendo-a-usar-loaders-mozilla.png)


## Conclusiones 


Ahora ya puedes puedes empezar a incluir loaders en tus proyectos con `webpack`, lo siguiente será aprender a utilizar plugins en el próximo articulo [Extendiendo la funcionalidad de webpack](https://github.com/oscar-grc/blog/blob/articles/extendiendo_la_funcionalidad_de_webpack.md)

si quieres profundizar más sobre `webpack` puedes ir al sitio oficial https://webpack.js.org/
