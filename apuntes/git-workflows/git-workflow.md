# Flujos de trabajo Git #

En este tema vamos a hacer un repaso de los conceptos fundamentales de
Git y a presentar los flujos de trabajo más habituales que se utilizan
para gestionar el trabajo de equipos de desarrollo usando esta
herramienta de control de versiones.

En muchas diapositivas e imágenes aparece `master`. Hemos cambiado a
`main`.

**Enlace a cómo cambiar en GitHub master por main**

## Git ##

Lo primero que nos dicen siempre de Git es que es un sistema de
control de versiones (VCS en inglés, [_Version Control
System_](https://en.wikipedia.org/wiki/Version_control))
distribuido. ¿Qué significa esto?

Todos conocemos la utilidad de los sistemas de control de
versiones. Es un sistema que registra los cambios a lo largo del
tiempo en un fichero o un conjunto de ficheros, de forma que es
posible recuperar más tarde versiones específicas.

<img src="imagenes/vcs.png" width="700px"/>

Cuando trabajamos en equipo en un proyecto software, el sistema de
control de versiones nos va a permitir cosas como: 

- Revertir un conjunto de ficheros a un estado previo.
- Comparar cambios a lo largo del tiempo.
- Consultar quién ha sido el último que ha modificado algo en algún
  fichero que está causando problemas. 
- Abrir una rama independiente del tronco principal en la que realizar
  un desarrollo que se integra después cuando esté terminado y
  probado. 
- Recuperar un estado anterior estable si hemos roto algo en los
  ficheros que estás tocando.

Un VCS es el elemento fundamental de prácticas de desarrollo más
avanzadas. Cualquier sistema o metodología de desarrollo en equipo
tiene como prerrequisito la utilización de un sistema de control de
versiones.

Ejemplos de utilización del VCS en el desarrollo de software:

- Distribución de tareas entre miembros del equipo e integración posterior del código.
- Revisión de código.
- Sistema de integración continua, como el que se muestra en la imagen.
- Mantenimiento de distintas versiones del producto desarrollado.
- Compartir código con desarrolladores externos.

### Historia de Git ###

Git nace para gestionar el desarrollo del kernel de Linux en 2005. La
comunidad de Linux, y en especial su creador Linus Torvalds, lo
desarrolla en esa fecha para sustituir un software de control de
versiones propietario que no cubría las necesidades del equipo.

Entre los objetivos que se persiguen:

- Velocidad
- Diseño simple
- Soporte para el desarrollo de miles de ramas simultáneas
- Completamente distribuido
- Capacidad de manejar proyectos grandes (en tamaño y tiempo) como el kernel de Linux

Desde su nacimiento en 2005, Git ha evolucionado y madurado,
convirtiéndose en un sistema increíblemente rápido, muy eficiente con
proyectos grandes y con una enorme facilidad de gestionar cientos de
ramas simultáneamente.


### Sistemas de control de versiones distribuidos ###

Los sistemas de control de versiones modernos como Git y Mercurial son distribuidos.

Cada desarrollador tiene su propio repositorio y su copia de trabajo
en su máquina. 

Después de hacer un commit de tus cambios, los demás no tienen acceso
a ellos hasta que los subes (push) al repositorio remoto central. 

Para obtener los cambios del repositorio central hay que bajarlos
(fetch) al repositorio local y actualizar la copia de trabajo. 

El ciclo de trabajo básico es:

- Haces uno o varios commits de tus cambios en tu repositorio local.
- Cuando quieres que los compañeros vean todos los cambios haces un
push al repositorio central. 
- Los compañeros hacen un fetch para actualizar su repositorio local.
- Después actualizan su copia de trabajo.

Hacer notar que la confirmación y actualización sólo mueven los
cambios entre la copia de trabajo y el repositorio local. Y al
contrario, push y fetch suben y bajan cambios del repositorio local al
repositorio remoto.  Sistemas de control de versiones centralizados

<img src="imagenes/sistema-distribuido.png" width="450px" />



### Repositorios y copias de trabajo ###

<img src="imagenes/repository.png" width="150px" align="right"/>

Un sistema de control de versiones usa un repositorio (una
recopilación de todos los cambios) y una copia de trabajo (el
directorio actual) donde haces tu trabajo.

La copia de trabajo es tu copia personal de todos los ficheros del
proyecto. Puedes realizar los cambios que desees en esta copia
personal sin afectar a la historia. Cuando estás contento con el
resultado, confirmas (commit en inglés) los cambios en el repositorio
y añades un commit a la historia.

<img src="imagenes/git-areas.png" width="200px" align="left"/>

Podemos entender mejor el funcionamiento de los comandos git add y git
commit introduciendo el concepto de zona de stage.

En Git no es obligatorio introducir todos los cambios en el siguiente
commit, sino que es conveniente seleccionar los cambios que queremos
introducir en el commit.

El comando git add añade a la zona de stage los cambios que queremos
incluir en el nuevo commit.

Además de usarlo para añadir nuevos ficheros que antes no estaban
trackeados, también hay que usarlo incluir cambios que queremos que
vayan en el siguiente commit.

El comando git commit añade los cambios que están en la zona de stage
al repositorio.

Hay dos posibles formas de eliminar y renombrar ficheros en un
repositorio:

- Los eliminamos y renombramos con los comandos del sistema operativo
y después hacemos un git add para incluir esos cambios en el stage.
- Los eliminamos y renombramos con los comandos propios de git:

git rm <fichero> para eliminar un fichero (o un patrón)
git mv <nombre-antiguo> <nuevo-nombre> para renombrar un fichero del
repositorio.

<img src="imagenes/grafo-commits.png" width="600px" />

La figura muestra un grafo de commits de un repositorio.

El espacio de trabajo se encuentra en HEAD, en el commit que acabamos
de crear.

Las ramas master y origin/master (rama remota) se siguen encontrando
en el mismo sitio, el último commit de la rama principal.

Podemos usar tags para referirnos a un commit concreto.

git tag

### Trabajar con la historia en Git ###

#### Movernos a un commit pasado ####

<img src="imagenes/git-checkout.png" width="400px" />

```text
$ git checkout 0bf82cd
# Hacemos cambios
$ git add .
$ git commit -m "Cambios sobre un commit pasado"
```

#### Cambiar el último commit en local ####

Si todavía no hemos subido el commit al repositorio remoto tenemos
varias opciones para cambiar el último commit.

Si queremos cambiar sólo el mensaje del commit:

```text
$ git commit --amend -m "<nuevo mensaje>"
```

Si queremos deshacer el commit, pero no los cambios introducidos en
él: 

```text
$ git reset HEAD^
```

El espacio de trabajo no cambia, pero el commit se ha desecho. 

`HEAD^` significa "el commit anterior a HEAD". Es equivalente a poner
el número de commit anterior al actual: 

```text
$ git reset <commit-anterior>
```

Si queremos eliminar los cambios del último commit del espacio de
trabajo y empezar de nuevo:

```
$ git reset --hard HEAD^
```

#### Deshacer el último commit cuando se ha publicado ####

Si ya hemos publicado el commit en el repositorio remoto otras
personas pueden habérselo descargado, con lo que el commit ya no es
solo nuestro, sino que es parte de la historia pública del proyecto.

Es posible revertir los cambios del commit que queremos eliminar con
el comando git revert <commit>.

El comando introduce un commit con exactamente los cambios contrarios
al commit indicado.

Por ejemplo, el siguiente comando crea un commit que revierte el último commit

```text
$ git revert HEAD
```

Para revertir los cambios realizados hace 3 commits:

```text
$ git revert HEAD~3
```

Para revertir sin commitear los cambios realizados por el quinto
último commit en master (incluido) hasta el tercer último commit en
master (incluido), usando la opción -n:

```text
$ git revert -n master~5..master~2
```


### Trabajo con ramas en Git ###

Supongamos que comenzamos con la siguiente historia (por simplificar,
en las imágenes no mostramos la etiqueta `head`).

<img src="imagenes/ramas-1.png" width="350px"/>

Vamos a crear una rama. Lo podemos hacer con los comandos:

```text
$ git branch iss53
$ git checkout iss53
```

Los comandos anteriores son equivalentes al más usual:

```text
$ git checkout -b iss53
```

El resultado es el siguiente:

<img src="imagenes/ramas-2.png" width="350px"/>

Ahora escribimos algo de código en la rama y hacemos un commit.

```text
# escribimos código
$ git commit -am "Añadido nuevo font"
```

El resultado (`head` estaría apuntando a `iss53`):

<img src="imagenes/ramas-3.png" width="400px"/>

Ahora nos vamos otra vez a `master` y creamos allí otra rama en la que
hacemos un hotfix:

```text
$ git checkout master
$ git checkout -b hotfix
# corregimos el error
$ git commit -am "Corregido el enlace erróneo"
```

El resultado (`head` estaría apuntando a `hotfix`):

<img src="imagenes/ramas-4.png" width="400px"/>

Ahora integramos el hotfix en `master`:

```text
$ git checkout master
$ git merge hotfix
```

Al estar `master` y `hotfix` en la misma línea de commits, git no
tiene que crear ningún commit de merge, sino que hace un _fast
forward_ y adelanta la rama de master para que apunte a `C4` (`head`
estaría apuntando a `master`):

<img src="imagenes/ramas-5.png" width="400px"/>

Ahora borramos la rama `hotfix` y hacemos un nuevo commit en `iss53`:

```text
$ git branch -d hotfix
$ git checkout iss53
# Cambiamos más cosas
$ git commit -am "Finalizado el nuevo aspecto"
```

El resultado es el siguiente (`head` estaría apuntando a `iss53`):

<img src="imagenes/ramas-6.png" width="450px"/>

Y ahora mezclamos la rama con `master`:

```text
$ git checkout master
$ git merge iss54
```

Para hacer el merge, git utiliza una estrategia denominada
_recursiva_. Consiste en buscar el commit antecesor común a ambas
ramas (`C2`), calcular los cambios desde ese commit hasta el commit
que se quiere mezclar (cambios desde `C2` hasta `C5`) y hacer un
_patch_ de esos cambios en el commit en el que se quiere hacer la
mezcla (`C4`):

<img src="imagenes/ramas-7.png" width="450px"/>

Si no hay ningún conflicto, el resultado de la mezcla se consolida
automáticamente en un nuevo commit y se avanza la rama `master`:

<img src="imagenes/ramas-8.png" width="500px"/>

En el caso en que hubiera conflicto, el merge no se realiza y se queda
abierto, hasta que se confirmen cuáles son los cambios correctos o se
deshaga el merge.

Los conflictos suceden cuando hay cambios incompatibles entre la rama
que se quiere mezclar y la rama en la que se está mezclando. En este
caso, podría deberse a haber tocado las mismas líneas de un mismo
fichero en los commits `C4` y los commits `C3` y `C5`. En el caso en
que los cambios toquen distintas líneas del mismo fichero, git
identifica los cambios como compatibles y no lo marca como un
conflicto.

Estrictamente, un conflicto sucede cuando el orden en que se aplican
los cambios de la mezcla dan lugar a ficheros distintos. En el
ejemplo, si tenemos un fichero que se ha modificado en el commit `C4`
y en el commit `C3`, Git detecta un conflicto cuando el fichero
resultante de aplicar los cambios `C4`+`C3` es distinto al de aplicar
los cambios `C3`+`C4`.

La solución de un conflicto es sencilla. En los ficheros en los que
hay conflicto, Git introducirá los textos de los commits que entran en
conflicto y tendremos que editarlo y dejarlo como nos interesa. Una
vez editado

<img src="imagenes/no-conflicto.png" width="500px"/>


**Los cambios en el repositorio remoto son ramas**

Se bajan haciendo fetch y se pueden examinar los cambios, etc. y
después integrarlos.



### Pull requests ###

### Solución de conflictos en pull requests ###






## Trunk-based development ##

## Short-lived branches ##

## GitFlow ##
