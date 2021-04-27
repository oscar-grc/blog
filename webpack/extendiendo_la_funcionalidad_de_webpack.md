![Extendiendo la funcionalidad de webpack](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/extendiendo-la-funcionalidad-de-webpack.png)


> Por: **Oscar García** - 21/04/2021

### Extendiendo la funcionalidad de Webpack

###### En esta serie de artículos aprenderemos las principales características de webpack y veremos con un ejemplo la potencia de esta Herramienta de desarrollo.

----

*Recuerda que este tutorial forma parte de una cadena de tutoriales que puedes encontrar en los siguientes enlaces:*

- [Primeros pasos en webpack](https://github.com/oscar-grc/blog/blob/articles/primeros_pasos_en_webpack.md)
- [Aprendiendo a usar loaders en webpack](https://github.com/oscar-grc/blog/blob/articles/webpack/aprendiendo_a_usar_loaders.md)
- **Extendiendo la funcionalidad de webpack**
- [Implementando hot reload en nuestras aplicaciones](https://github.com/oscar-grc/blog/blob/articles/webpack/implementando_hot_reload_en_nuestras_aplicaciones_con_webpack.md) 		

---

#### Indice de contenidos:

- [Entorno](#Entorno)
- [¿Cómo extender la funcionalidad de Webpack?](#¿Cómo-extender-la-funcionalidad-de-Webpack?)
- [¿Cómo utilizamos los plugins?](#¿Cómo-utilizamos-los-plugins?)
- [Lanzando nuestro plugin](#Lanzando-nuestro-plugin)
- [Conclusiones](#Conclusiones)

---

## Entorno

- **Sistema Operativo:** MacOS - Catalina
- **Editor:** Visual Studio Code
- **Webpack:** 5.21.2
- **Webpack-cli:** 4.5.0
- **Node JS:** v12.18.2
- **NPM:** 6.14.5


## ¿Cómo extender la funcionalidad de Webpack?

Como aprendimos en el articulo anterior los loaders te brindan la capacidad de realizar transformaciones de ciertos módulos pero existen funcionalidades que no pueden hacerse con loaders y es aquí donde entran los plugins ya que estos pueden acceder a las distintas fases del ciclo de vida de un proyecto en webpack

Existe una gran variedad de plugins que podemos usar pero también podemos crear nuestros propios plugins.

## ¿Cómo utilizamos los plugins?

Si queremos utilizar plugins, debemos de definirlos en el archivo de configuración, a continuación listamos una serie de pasos que son necesarios para instalar plugins en nuestro proyecto webpack:

**1. Instalar el plugin a utilizar:**

Una vez que tenemos identificado que plugin podría servir para nuestro proyecto es necesario instalarlo, en este caso utilizaremos ``npm`` como los ejemplos anteriores.

Para este ejemplo utilizaremos el plugin  **HtmlWebpackPlugin**  *pero puedes encontrar una lista de [plugins](https://webpack.js.org/plugins/) en la web oficial*

```
npm install --save-dev html-webpack-plugin
```

**2. Instanciar el plugin:**

Una vez instalado, procedemos a importarlo y configurarlo en el archivo de configuración:

Es importante recordar que para hacer uso del plugin es necesario crear una instancia del plugin dentro del array de plugins que se encuentra en el archivo de configuración, en nuestro ejemplo en ``./webpack.config.js``

```JS

const HtmlWebpackPlugin = require('html-webpack-plugin');

module.export = {
	...
	plugins: [
		new HtmlWebpackPlugin()
	]
}
````

**3. Configurar el plugin:**

Ahora pasamos a configurar el plugin de acuerdo a la documentación del mismo, es importante revisar la documentación que se proporciona por cada plugin ya que la configuración puede variar de un plugin a otro:

para nuestro ejemplo la documentación puedes encontrarla aquí [HtmlWebpackPlugin Documentación](https://github.com/jantimon/html-webpack-plugin#options)

```JS
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

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
    },
    plugins:[
        new HtmlWebpackPlugin({
            title: 'Ejemplo con Webpack'
        })
    ]
}
```

## Lanzando nuestro plugin

Ahora una vez que instalamos y configuramos el ejemplo anterior expliquemos un poco que es lo que realiza este plugin.

Como recordaremos en el capitulo anterior [Aprendiendo a usar loaders en webpack](https://github.com/oscar-grc/blog/blob/articles/webpack/aprendiendo_a_usar_loaders.md) para poder ver nuestro ``bundle`` en acción era necesario generar manualmente un archivo ``index.html`` en la carpeta ``./dist/`` en la cual embebimos mediante la etiqueta script el ``bundle`` generado por webpack, esto pese a ser sencillo no es algo práctico ya que si consideramos que durante la fase de desarrollo constantemente estaremos reseteando o eliminando la carpeta ``dist`` para hacer nuestras pruebas vemos que generar el script deja de ser recomendado.

**Html Webpack Plugin** se encarga de simplificar la creación de estos archivos ``HTML``y nos permite servir de manera sencilla nuestros ``bundles`` ya se dejando que el plugin genere el archivo ``index.html` y añada el ``bundle`` de forma automática o mediante plantillas propias definidas. 

Si quieres aprender más acerca del funcionamiento de **Html Webpack Plugin** puedes consultar la [Documentación oficial](https://github.com/jantimon/html-webpack-plugin#options)

Una vez explicado el funcionamiento del plugin, vamos a proceder a lanzar nuestro proyecto:

**Nota:** *En caso de que estes siguiendo este articulo desde los artículos anteriores, es necesario borrar los archivos que se generaron en el articulo anterior en la carpeta ``./dist/``*


Ahora solo ejecutamos desde terminal nuestro proyecto

```
npm run build
```

Una vez que lanzamos el comando ``npm run build`` podemos ver que webpack comienza a realizar el empaquetado y si todo sale bien podremos ver el siguiente mensaje.

![Extendiendo la funcionalidad de webpack](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/extendiendo-webpack-success.png)

También podemos ver que se generaron dos archivos en la carpeta ``./dist/``, el archivo bundle que contiene nuestro componente web y un archivo ``index.html`` el cual anteriormente no se generaba y teníamos que crearlo manualmente para poder ver el ``bundle`` en el navegador.


![Extendiendo la funcionalidad de webpack](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/extendiendo-webpack-archivos-generados-en-dist.png)


Si abrimos en archivo ``index.html`` podemos ver que en este caso contiene el bundle embebido de forma automática y también contiene en la etiqueta ``title``el titulo que definimos en la configuración del plugin.

![Extendiendo la funcionalidad de webpack](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/extendiendo-webpack-index-html.png)

Nuevamente podemos ir a nuestro navegador, y veremos los estilos definidos.


![Extendiendo la funcionalidad de webpack](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/extendiendo-webpack-navegador.png)


## Conclusiones 


Ahora ya puedes puedes empezar a incluir plugins en tus proyectos con `webpack`, lo siguiente será aprender a utilizar **HOT RELOAD** en el próximo articulo [Implementando hot reload en nuestras aplicaciones](https://github.com/oscar-grc/blog/blob/articles/webpack/implementando_hot_reload_en_nuestras_aplicaciones_con_webpack.md) 

si quieres profundizar más sobre `webpack` puedes ir al sitio oficial https://webpack.js.org/


