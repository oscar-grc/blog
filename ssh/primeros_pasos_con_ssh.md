![Extendiendo la funcionalidad de webpack](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/ssh-primeros-pasos-con-secure-shell.png)


> Por: **Oscar García** - 05/07/2021

### Primeros pasos con **Secure Shell** - SSH

###### En esta serie de artículos aprenderemos las principales características de openSSH y con algunos ejercicios mostraremos la potencia de esta Herramienta.

----

*Recuerda que este tutorial forma parte de una cadena de tutoriales que puedes encontrar en los siguientes enlaces:*

- **Primeros pasos con SSH**
- [Entorno de pruebas con SSH]()
- [Configurando nuestro agente SSH]()
- [Enviando archivos SSH]()
- [Creando y configurando túneles SSH]()


---

#### Indice de contenidos:

- [Entorno](#Entorno)
- [Primeros pasos con Secure Shell](#Primeros-pasos-con-Secure-Shell)
- [Un poco acerca de criptografía](#Un-poco-acerca-de-criptografía)
- [Funciones de HASH](#Funciones-de-HASH)
- [Métodos de autenticación](#Métodos-de-autenticación)
- [Fases dentro de SSH](#Fases-dentro-de-SSH)
- [Conclusiones](#Conclusiones)

<br />

---
<br />

## Entorno

- **Sistema Operativo:** Windows 10 - WSL2  
- **OpenSSH:** 2

<br />

----

## Primeros pasos con Secure Shell

**SSH** nace como mejora de herramientas como TELNET, RLOGIN o RSH, estas herramientas lo que hacían principalmente era abrir una terminar remota de conexión desde un equipo a otro, lo que nos permitía poder ingresar y controlar un equipo de forma remota.

El problema de estas herramientas es que la autenticación y comunicación no estaba cifrada, por lo cual eran vulnerables a ataques `man in the middle`

`Secure Shell` ó **SSH** creada en 1995 por "Tatu Ylonen" intenta solucionar este problema, por lo que **SSH** proporciona cifrado por todo el canal, desde la autenticación hasta la comunicación. 

En 1999 surge el proyecto **OPENSSH** el cual es desarrollado por la comunidad de **OPEN BSD**, escrito en C y se apoya en el proyecto **Libre SSL** 

## Un poco acerca de criptografía

### Qué es criptografía?

Es la ciencia y arte de escribir mensajes de forma cifrada que impide que receptores no autorizados puedan leer el mensaje. para poder hacer esto posible, se suelen usar diferentes algoritmos.

Un ejemplo muy conocido en criptografía es **El Cesar** generada por los Romanos que se basa en la sustitución, donde solo era necesario mover el orden del alfabeto un numero de posiciones por lo cual si no contabas con el numero de posiciones, el mensaje era ilegible

![Criptografía Cesar](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/ssh-primeros-pasos-cesar.PNG)

### Claves simétricas:

Se caracteriza principalmente por que receptor y emisor conocen la clave para cifrar y descifrar el mensaje, algunos algoritmos utilizados son:

- DES
- 3DES
- IDEA
- Blowfish
- AES

 

![Cirptografía Simétrica](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/clave-simetrica.PNG)

### Claves asimétricas:

En el cifrado asimétrico, también llamado de clave pública, se generan dos claves relacionadas, una pública y otra privada, donde solo la llave publica se comparte.

Cuando la clave publica cifra la información, la clave privada puede descifrarla y viceversa, algunos algoritmos utilizados:

- RSA
- DSA
- ECDSA
- Ed25519

![Criptografía asimétrica](https://ninjaaprendiendo.s3.us-east-2.amazonaws.com/clave-publica.PNG)

## Funciones de HASH

Son operaciones matemáticas que permite transforma una serie de datos con una longitud fija.

- Son unidireccionales
- No muestran información del origen
- Con cualquier modificación al origen por mínima que esta sea, el hash de salida cambia.
- Costo computacional bajo

Algunos algoritmos utilizados son:

- MD5
- Tiger
- SHA-256
- SHA-512
- SHA-3

## Métodos de autenticación

Como vimos anteriormente SSH nos permite tener acceso a maquinas remotas, pero ¿Cómo nos autenticamos?, SSH nos proporciona varios tipos o  métodos, a continuación listamos unos de ellos:

- Usuario y contraseña
- Clave pública/privada
- Kerberos

## Fases dentro de SSH

En este apartado veremos a grandes rasgos el proceso de conexión SSH consta de 2 fases diferentes:

### Fase 1: **Negociación**

El cliente intenta conectarse al servidor y ambos muestran la versión de SSH, una vez establecido con que versión realizarán la conexión, el servidor envía su clave publica para que el cliente pueda verificar la huella dentro de el histórico de conexiones, después de eso, si todo va bien, negocian que algoritmo y semilla se va a utilizar, cuando lo determinan ambos generan de forma paralela las claves de sesión y envían a su contraparte las claves publicas para validar que ambas son iguales.

### Fase 2: **Autenticación**

Una vez que se estableció la clave de sesión, el servidor ofrece en orden los métodos de autenticación disponibles, el cliente los va rechazando hasta que encuentra uno a utilizar, una vez que el cliente se autentica correctamente, se abre la sesión en el equipo remoto.


<br />

---
## Conclusiones

En este primer acercamiento podemos ver la potencia y sencillez de openSSH y como gracias a la criptografía podemos interactuar hacia un equipo remoto.

En el próximo articulo generaremos nuestro entorno de pruebas para comenzar a parcticar con esta herramienta [Entorno de pruebas con SSH]()

si quieres profundizar más sobre `SSH` te recomiendo los siguientes sitios:

- https://www.openssh.com/
- https://es.wikipedia.org/wiki/Secure_Shell
- https://www.freebsd.org/es/