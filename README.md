# Test de automatización del build de create-react-app ~~y despligue en localhost~~ usando GitHub Actions

## Runner
El runner es la aplicación de GitHbub que escucha si hay cambios en el repo (un nuevo push o pull request)

Cuando el runner detecta un nuevo cambio crea un nuevo build ~~en el repositorio de GitHub~~ en una subruta local (o del servidor) del directorio donde hemos instalado el runner, machacando así al build anterior. 

## Pasos

## Crear el cascarón de create-react-app y de un repo de prueba
-En local: 

npx create-react-app myapp && cd myapp

-En la cuenta de GitHub crear un repo de prueba, ej: automationTest

-En local: push del cascarón vacío de create-react-app al repo 


## Crear el fichero .yml para el runner
En el repo:

* Menú superior horizontal > Actions

* Debajo de "Suggested for this repo", en Node.js, pinchar en **Configure** (debe aparecer un fichero editable)

Contenido del fichero .yml:

**On**: define cuando el runner hará el build (por defecto, cada vez que se haga un push o un pull request)

**Jobs: build** se refiere a todos los pasos para hacer el build: descargar Node, instalarlo, instalar dependencias y hacer el build cada vez que el runner detecte un cambio en el repo

**Steps**: todas estos pasos por separado

## Modificar el fichero .yml
-Para esta prueba modificamos lo siguiente: 

**runs-on: self-hosted** (en vez del valor que haya por defecto, ej: "ubuntu-latest"), con esto el runner correrá en nuestra máquina (o en su momento en el servidor) y no en una máquina virtual de GitHub

**node-version: [14.x]** (poner preferiblemente solo una versión de Node)

**run: npm i** (en vez de ci)

-pinchar en el botón verde 'start commit' (o algo así)

## Instalar y ejecutar el runner en local
-En la cuenta de Github > repo de prueba > settings (menú superior horizontal) 

-En Settings: en el menú lateral izquierdo, en el desplegable Actions
seleccionar Runners

-Pinchar en **new-self-hosted-runner**

-Seleccionar el SO y la architectura del procesador 


-En la pantalla anterior seguir la lista de comandos para descargar, instalar y configurar el runner en un directorio nuevo en local que debe ser DISTINTO al directorio **myapp**

Nota: esta configuración es lo que conecta al runner en nuestra máquina (o en el servidor) con el repo

Nota: decir que sí (o aceptar el default) en las preguntas de configuración del runner hasta que aparezca: 

```sh
√ Runner successfully added

√ Runner connection is good

√ Settings Saved
```

-Ejecutar el runner (última instrucción de la lista)

Debe aparecer:

```sh
√ Connected to GitHub
Listening for Jobs
Running job: build (14.x)
```
-Volvemos al repo > Settings > Actions

En **All Workflows** debe aparecer el runner con el nombre del repo con un círculo verde (el runner está ya escuchando)

## ~~Despliegue~~ Creación automatizada del build en el repo al hacer un push en local (CORRECCIONES)

-En local abrimos una nueva consola

-Hacemos cualquier cambio visible en el código, ej: cambiar el texto de algún elemento HTML en el fichero App.js

-Hacemos git add y git commit

-Al hacer el push automáticamente se crea un nuevo build ~~en el repo de GitHub que sobreescribe el anterior~~ en el directorio 
donde se ha instalado el runner (ej: ~/actions-runner)

-Situarse en la ruta donde se crea/machaca el build: ~/actions-runner/_work/ subdirectorio_con_el_nombre_del_repo/ subdirectorio_con_el_nombre_del_repo (sí, otra vez)/build

-Comprobar que el index.html refleja los cambios del último push

Nota: El despliegue en local no tiene mucho sentido, por tanto, este debería probarse directamente en el servidor 

## Despliegue en el servidor

-Seguir los pasos anteriores de instalación del runner (menos el último) para que quede como un servicio permanente del 
sistema

-En vez del último paso (ejecución), hacemos lo siguiente:

```sh
sudo ./svc sh install 
sudo ./svc sh start 
```
## Identificar la ruta donde el runner está haciendo el build

-Ir al subdirectorio donde está instalado el runner/_work/ subdirectorio_con_el_nombre_del_repo/ subdirectorio_con_el_nombre_del_repo/build

-cd a la ruta anterior

-hacemos pwd (y copiamos toda la ruta)

## Cambiar la configuración de nginx en sites available

-En el fichero default de la configuración de nginx (/etc/nginx/sites-available) y poner lo siguiente: 

root nombredelaruta (en vez de /var/www/html)



