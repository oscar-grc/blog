
![¿Cómo instalar y configurar sonarlint con sonarqube?](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/sonarlint-como-instalar-y-configurar-sonarlint-con-sonarqube.png)

> Por: **Oscar García** - 08/06/2021

### ¿Cómo instalar y configurar SonarLint con Sonarqube™?

###### En esta articulo aprenderemos a instalar Sonarqube™ y SonarLint que nos permiten revisar nuestro código para detectar errores, vulnerabilidades y codigo sucio.

----

#### Indice de contenidos:

- [Entorno](#Entorno)
- [¿Que es Sonarqube™?](#¿Que-es-Sonarqube™?)
- [Instalando Sonarqube™](#Instalando-Sonarqube™)
- [Configurando Sonarqube™](#Configurando-Sonarqube™)
- [¿Que es SonarLint?](#¿Que-es-SonarLint?)
- [Instalando SonarLint en IntelliJ IDEA](#Instalando-SonarLint-en-IntelliJ-IDEA)
- [Configurando SonarLint](#Configurando-SonarLint)
- [Generando primer proyecto](#Generando-primer-proyecto)
- [Conclusiones](#Conclusiones)

---

## Entorno 

- **Hardware:** Surface Pro 7 - core i7 - 16GB
- **Sistema Operativo:** Windows 10 - WSL2  
- **Sonarqube™:** 8.9.0
- **SonarLint:** 3.45
- **IDE:** IntelliJ IDEA - Community 2020.2
- **Jdk:** 11.0.2 
- **Apache Maven:** 3.6.3

---

## ¿Que es Sonarqube™?

![¿Que es Sonarqube™?](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/download-sonarqube.PNG)

``Sonarqube™`` es una potente herramienta que nos permite realizar revisión automática de código para detectar errores, vulnerabilidades y código sucio sin necesidad de ejecutarlo, esto es comúnmente llamado "análisis de código estático" 

``Sonarqube™`` nos permite analizar código activamente durante todo el proceso y con eso  mantener nuestro código con la más alta la calidad y seguridad.

Es importante mencionar que ``Sonarqube™`` no está limitado a un único lenguajes, se pueden hacer análisis de diferentes lenguajes, hoy día soporta más de 20 lenguajes de programación.


## Instalando Sonarqube™

Para instalar ``Sonarqube™`` solo es necesario ir a la pagina oficial https://www.sonarqube.org/downloads/ y en la sección de descargas podemos seleccionar la opción mas adecuada, en nuestro caso utilizaremos la edición "Community"

Una vez que descargamos el .zip, procedemos a extraerlo en la ruta de nuestra elección para poder ejecutarlo, ``en nuesto ejempo lo dejaremos en la carpeta C://Program Files/sonarqube)`` 


## Configurando Sonarqube™

Ahora que ya tenemos ``Sonarqube™`` dentro de la carpeta especifica, es necesario realizar un par de configuraciones antes de poder ejecutar nuestra herramienta.

#### Instalar JAVA 11

Hay que recordar que la versión que descargamos **8.9.0** de ``Sonarqube™`` solo soporta java 11, por lo que es necesario que en caso de no tener instalada la vesion del jdk 11 vayamos al sitio oficial y realicemos la descarga https://jdk.java.net/archive/

#### Configurar versión de Java

Una vez que tenemos instalada la versión de java solicitada, tenemos que ir nuestro Sonarqube™, en la carpeta ``conf`` abrimos el archivo ``wrapper.conf`` y comentamos la linea ``wrapper.java.command=java``  y colocamos la ruta donde se encuentra instalado nuestro jdk versión 11

En nuestro caso el colocamos la siguiente linea:

```

wrapper.java.command=C:\Program Files\java\jdk-11.0.2\bin\java.exe

```

![Configurando versión de java en Sonarqube™](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/wrapper-config.PNG)

#### Ejecutando Sonarqube™

Si ya tenemos las configuraciones anteriores, ahora solo queda ejecutar ``Sonarqube™`` en la siguiente ruta ``C:\Program Files\sonarqube\sonarqube-8.9.0.43852\bin\windows-x86-64`` ejecutamos el programa StarSonar.bat

![Ejecutando Sonarqube™](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/starbat-sonarqube.PNG)

Ahora podemos ir a nuestro navegador favorito y abrir la consola de ``Sonarqube™`` en 

```

http://localhost:9000/

``` 

![Sonarqube™](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/sonarqube-localhost.PNG)

Cuando entras por primera vez, te pedirá un usuario y contraseña, por defecto son las siguientes:

- **user:** admin
- **password:** admin

Ahora procedemos a instalar **SonarLint**

## ¿Que es SonarLint?

![sonarlint](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/sonarlint.PNG)

SonarLint es una extensión que nos ayuda a detectar y solucionar problemas mientras escribimos nuestro código.

SonarLint nos avisa cuando detecta alguna vulnerabilidad en nuestro código, código repetido o código sucio y así de forma fácil puedas revisar las recomendaciones y solucionar en el momento los problemas. 

SonarLint soporta a los principales IDE's de desarrollo.

## Instalando SonarLint en IntelliJ IDEA

En nuestro articulo vamos a trabajar con un proyecto spring-boot dentro de IntelliJ IDEA.

Para instalar la extensión de SonarLint vamos a la sección de ``File > Settings >> Plugins ``

y buscamos el plugin de SonarLint y procedemos a instalar

![Instalando plugin SonarLint en IntelliJ IDEA Community](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/ideaj-install-plugin.PNG)

Una vez instalado, te solicitara reiniciar el programa para poder configurar el plugin.


## Configurando SonarLint

Ahora que esta instalado SonarLint puedes ver que comienza a notificarte en caso de encontrar recomendaciones:

![SonarLint issues](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/sonarlint-idea-issues.PNG)

Pero nos interesa conectar SonarLint con la herramienta ``Sonarqube™`` que acabamos de instalar.

Nuevamente vamos a la sección de **settings** en ``File > Settings `` pero esta vez, en la parte correspondiente a **Tools > SonarLint** y generamos una nueva conexión.

Agregamos el nombre para la conexión  y colocamos la url ``Sonarqube™``

```

http://localhost:9000/

```

![Agregar nueva conexión en SonarLint](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/sonarlint-configuration-conection.PNG)


Para poder realizar la conexión nos va a solicitar un método de acceso, para nuestro ejemplo utilizaremos el tipo **Token**, y al dar click en la opción **Generate** nos llevara a ``Sonarqube™`` a la sección de ``Administrator >> Security ``

En caso de que no sea así, podemos ir a ``Sonarqube™`` y en la parte superior derecha en "Account", seleccionamos  **Administrator** 

![Administracion de cuenta en Sonarqube™](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/my-account.PNG)

Y en la sección de seguridad, generamos un token (Es importante que guardes el token por que no se volverá a mostrar):

![Seguridad en Sonarqube™](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/sonarlint-token.PNG)


Nuevamente en IntelliJ Idea colocamos el Token generado en ``Sonarqube™`` y procedemos a realizar la conexión

![Realizando conexión en SonarLint](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/sonarlint-token-idea.PNG)

Verificamos que la conexión este creada y finalizamos la configuración

![Realizando conexión en SonarLint](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/sonarlint-connection-success.PNG)


## Generando primer proyecto

Una vez que realizamos la conexión vamos a generar un proyecto en ``Sonarqube™`` para poder tener un análisis mas detallado del componente a probar

![Generando nuevo proyecto](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/sonarqube-localhost.PNG)

Solo es necesario indicar el nombre del proyecto y continuar:

![Nombrando el proyecto](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/sonarqube-generate-project.PNG)

Agregamos el Token generado o podemos generar uno nuevo, para nuestro ejemplo utilizaremos el mismo.

En nuestro caso es un proyecto spring-boot gestionado por maven, por lo que elegimos la primer opción

![maven en sonarqube](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/sonar-qube-maven-project.PNG)

Sonarqube nos proporciona el comando de maven necesario para poder ejecutar el análisis del proyecto completo.

En la sección de **configurations** de **IntelliJ IDEA** generamos un nuevo comando para maven y colocamos la linea que nos proporciona ``Sonarqube™``

```

sonar:sonar \ -Dsonar.projectKey=techU \ -Dsonar.host.url=http://localhost:9000 \ -Dsonar.login=9ae1372601df4834cf79232830ce6834d83d67a7

```

![maven en Sonarqube™](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/idea-maven-command.PNG)

Después de compilar el proyecto, podemos ir a Sonarqube™ y veremos que ya esta el análisis generado en el proyecto

![Sonarqube™ análisis](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/sonar-qube-maven-project-results.PNG)

Dentro del proyecto también podemos analizar los **issues** en los que debemos trabajar. 

![Sonarqube™ issues](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/articulos/sonar-qube-maven-project-issues.PNG)

---

## Conclusiones

Ahora ya puedes puedes empezar a utilizar SonarLint y Sonarqube™ en tus proyectos, en próximos artículos integraremos sonar en nuestros flujos de integración continua.


si quieres profundizar más sobre las herramientas utilizadas en el articulo puedes ir a los sitios oficiales 

- https://docs.sonarqube.org/latest/
- https://www.sonarlint.org/intellij
- https://maven.apache.org/



