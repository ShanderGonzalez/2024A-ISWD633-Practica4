# Dockerfile
Un Dockerfile es un archivo de texto plano que contiene una serie de instrucciones que Docker utiliza para construir una imagen de contenedor Docker. Este conjunto de instrucciones define cómo se debe configurar y construir una imagen de contenedor, incluyendo qué sistema operativo base usar, qué software instalar, qué archivos copiar en el contenedor y cómo configurar el entorno de ejecución.
 ![Dockerfile](imagenes/relacion.PNG)


Tradicionalmente, el archivo docker no tiene extensión. Simplemente se denomina Dockerfile sin ningún tipo de extensión. Adicionalmente, los Dockerfiles pueden ser creados usando la extensión .dockerfile. Esto se utiliza cuando hay una necesidad de almacenar múltiples archivos docker en un solo directorio.
Las instrucciones en un Dockerfile son simples y están diseñadas para ser leídas y comprendidas fácilmente. 

### FROM 
Especifica la imagen base que se utilizará como punto de partida para construir la nueva imagen de Docker. Por ejemplo, FROM nginx:alpine significa que la nueva imagen se basará en la imagen oficial de Nginx que está etiquetada como "alpine". Al utilizar una imagen base existente, se heredan todas las configuraciones y software incluidos en esa imagen, lo que facilita la creación de nuevas imágenes sin tener que empezar desde cero.

### RUN
Ejecuta comandos en el contenedor durante el proceso de construcción de la imagen. Puedes usar esta instrucción para instalar software, configurar el entorno, o realizar cualquier otra tarea necesaria para preparar la imagen. Por ejemplo, RUN apt-get update && apt-get install -y <paquete> instalará un paquete utilizando el administrador de paquetes apt en una imagen basada en Ubuntu.

### COPY
Copia archivos o directorios desde la máquina host al sistema de archivos del contenedor. Se utiliza para agregar archivos, scripts u otros recursos necesarios para la aplicación en la imagen de contenedor. Por ejemplo, COPY ./mi-app /app copiará el directorio mi-app desde la máquina host al directorio /app en el contenedor.

### ADD
Copia archivos o directorios desde el sistema de archivos de la máquina host al sistema de archivos del contenedor. Aunque es similar a la instrucción COPY, ADD tiene algunas características adicionales, como la capacidad de descomprimir automáticamente archivos y de copiar archivos desde una URL remota.

### CMD 
Define el comando predeterminado que se ejecutará cuando se inicie el contenedor. Puede haber solo una instrucción CMD en un Dockerfile. Si se proporciona más de una, solo la última tendrá efecto. Por ejemplo, CMD ["nginx", "-g", "daemon off;"] ejecutará el servidor web Nginx en modo daemon cuando se inicie el contenedor.

### ENTRYPOINT
Configura el comando o el script que se ejecutará cuando se inicie un contenedor basado en la imagen. A diferencia de la instrucción CMD, que especifica el comando predeterminado que se ejecutará y que puede ser sobrescrito desde la línea de comandos al iniciar el contenedor, ENTRYPOINT establece un comando que no se puede sobrescribir fácilmente al iniciar el contenedor.
Es importante destacar que, si se proporciona un comando en la línea de comandos al iniciar el contenedor (por ejemplo, docker run <imagen> <comando>), este comando se agregará como argumentos al comando especificado en ENTRYPOINT, en lugar de sobrescribirlo. Esto permite que el contenedor sea más versátil y se adapte a diferentes necesidades de uso.

### EXPOSE
Informa a Docker que el contenedor escuchará en un puerto específico en tiempo de ejecución. No abre realmente el puerto, solo documenta que la aplicación dentro del contenedor puede usar dicho puerto. Por ejemplo, EXPOSE 80 expone el puerto 80 en el contenedor, lo que permite que se acceda a la aplicación que se esté ejecutando en ese puerto desde fuera del contenedor.

### ENV
Define variables de entorno dentro del contenedor. Las variables de entorno definidas con ENV estarán disponibles para cualquier proceso en el contenedor durante su ejecución. Por ejemplo, ENV MYSQL_ROOT_PASSWORD=password define una variable de entorno llamada MYSQL_ROOT_PASSWORD con el valor password.

### VOLUME 
Esta instrucción se se utiliza para definir un punto de montaje para volúmenes dentro del contenedor.

##  Para construir una imagen de Docker a partir de un Dockerfile
```
docker build -t <nombre imagen>:<version> .
```
- **-t** esta opción se utiliza para etiquetar la imagen que se está construyendo con un nombre y una versión.
- <nombre_imagen>:<version> por ejemplo: myapp:1.0
- **.** este punto indica al comando docker build que busque el Dockerfile en el directorio actual, es decir especifica la ubicación del contexto de la construcción que incluye el Dockerfile y cualquier otro archivo necesario para la construcción de la imagen.

## Ejemplo
### Colocar las siguientes instrucciones en un Dockerfile, 
![Dockerfile](imagenes/Dockerfile.PNG)

- apachectl: Es el script de control para el servidor web Apache. Se utiliza para iniciar, detener y controlar el servidor web.
- -D FOREGROUND: Esta opción le dice a Apache que se ejecute en primer plano. Por defecto, Apache se ejecuta como un servicio en segundo plano. Sin embargo, en un contenedor Docker, es preferible que el proceso principal (en este caso, Apache) se ejecute en primer plano para que Docker pueda monitorear el estado del proceso. Si Apache se ejecutara en segundo plano, Docker no podría saber si el servidor web está funcionando correctamente o no.

 
### Ejecutar el archivo Dockerfile y construir una imagen en la versión 1.0

*Puedes copiar y ejecutar directamente. No olvides verificar en qué directorio se encuentra el archivo Dockerfile*
```
docker build -t dockerfile:1.0 .
```
![image](https://github.com/ShanderGonzalez/2024A-ISWD633-Practica4/assets/94009521/52cac541-b360-4f23-a4d7-a9dacf3f4fde)


**¿Cuántos pasos se han ejecutado?**

Se han ejecutado 10 pasos:

1. Cargar la definición de construcción desde Dockerfile:
```=> [internal] load build definition from Dockerfile```

2. Cargar metadatos para centos:7:
```=> [internal] load metadata for docker.io/library/centos:7```

3. Autenticación para library/centos:pull en registry-1.docker.io:
```=> [auth] library/centos:pull token for registry-1.docker.io```

4. Cargar .dockerignore:
```=> [internal] load .dockerignore```

5. Cargar el contexto de construcción:
```=> [internal] load build context```

6. Descargar y extraer la imagen base centos:7:
```=> [1/4] FROM docker.io/library/centos:7@sha256:be65f488b7764ad3638f236b7b515b3678369a5124c47b8d32916d6487418ea4```

7. Actualizar el sistema operativo:
```=> [2/4] RUN yum -y update```

8. Instalar Apache HTTP Server:
```=> [3/4] RUN yum -y install httpd```

9. Copiar el contenido del directorio ./web al contenedor:
```=> [4/4] COPY ./web /var/www/html```

10. Exportar la imagen resultante:
```=> exporting to image```


### Inspeccionar la imagen creada
![image](https://github.com/ShanderGonzalez/2024A-ISWD633-Practica4/assets/94009521/4265a18f-4ccb-4020-8420-5f092f49f294)

**Modificar el archivo index.html para incluir su nombre**
![image](https://github.com/ShanderGonzalez/2024A-ISWD633-Practica4/assets/94009521/cfa92d6b-f6c8-4e60-b410-710731d1f428)

**¿Cuántos pasos se han ejecutado? ¿Observa algo diferente en la creación de la imagen?**

Se han ejecutado 10 pasos:

1. Cargar la definición de construcción desde Dockerfile
2. Cargar metadatos para centos:7
3. Autenticación para library/centos:pull en registry-1.docker.io
4. Cargar .dockerignore
5. Cargar el contexto de construcción
6. Descargar y extraer la imagen base centos:7
7. Actualizar el sistema operativo
8. Instalar Apache HTTP Server
9. Copiar el contenido del directorio ./web al contenedor
10. Exportar la imagen resultante

En cuanto a la creacion de la imagen, se tiene que en los pasos 7 y 8 (actualización del sistema operativo e instalación de Apache HTTP Server) se marcaron como CACHED, lo que significa que Docker detectó que no hubo cambios en estos pasos desde la última construcción y reutilizó las capas anteriores. El uso de la cache en Docker ofrece beneficios como una construcción más rápida, mayor eficiencia al reducir el tiempo y los recursos necesarios, y consistencia al asegurar que las capas intermedias no cambien si los pasos de construcción permanecen iguales. En resumen, aunque se utilizaron 10 pasos, la construcción fue significativamente más rápida gracias a la cache.

## Mecanismo de caché
Docker usa un mecanismo de caché cuando crea imágenes para acelerar el proceso de construcción y evitar la repetición de pasos que no han cambiado. Cada instrucción en un Dockerfile crea una capa en la imagen final. Docker intenta reutilizar las capas de una construcción anterior si no han cambiado, lo que reduce significativamente el tiempo de construcción.

- Instrucción FROM: Si la imagen base ya está en el sistema, Docker la reutiliza.
- Instrucciones de configuración (ENV, RUN, COPY, etc.): Docker verifica si alguna instrucción ha cambiado. Si no, reutiliza la capa correspondiente de la caché.
- Instrucción COPY y ADD: Si los archivos copiados no han cambiado, Docker reutiliza la capa de caché correspondiente.
![mapeo](imagenes/dockerfile-cache.PNG)

### Crear un contenedor a partir de las imagen creada, mapear todos los puertos
```
docker run -d -P --name contenedor1 dockerfile:1.0
```

### ¿Con que puerto host se está realizando el mapeo?
```
docker port <nombre del contenedor>
```
![image](https://github.com/ShanderGonzalez/2024A-ISWD633-Practica4/assets/94009521/b1bd0132-9cf4-47ed-a6ab-7e74b0dd0bfd)
![image](https://github.com/ShanderGonzalez/2024A-ISWD633-Practica4/assets/94009521/7e3ab5bb-eb58-4451-8c2e-a805a0a4157e)

**¿Qué es una imagen huérfana?**

Una imagen huérfana en Docker es aquella que carece de etiquetas y no está en uso por ningún contenedor ni servicio activo. Estas imágenes pueden acumularse si se eliminan contenedores sin eliminar las imágenes subyacentes, ocupando espacio innecesario en el sistema. Es recomendable identificar y eliminar regularmente estas imágenes usando comandos como docker image prune para mantener un entorno Docker eficiente y organizado.

### Identificar imágenes huérfanas
```
docker images -f "dangling=true"
```

### Listar los IDS de las imágenes huérfanas
```
docker images -f "dangling=true" -q
```

### Eliminar imágenes huérfanas
Este comando eliminará todas las imágenes que no estén asociadas a ningún contenedor en ejecución. Antes de ejecutarlo, asegúrate de revisar las imágenes que serán eliminadas para evitar la pérdida de imágenes importantes. 
```
docker image prune
```

### Para Ejecutar un archivo Dockerfile que tiene otro nombre
```
docker build -t <nombre imagen>:<version> -f <ruta y nombre del Dockerfile> .
```

## Por ejemplo
docker build -t imagen:1.0 -f Dockerfile-custom .



