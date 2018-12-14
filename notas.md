# Git Workshop


## Breve historia de git

## VCS

Un sistema de control de versiones es una bbdd que guarda distintas revisiones de un proyecto

* La información de los archivos, nombre, fecha modificación y estructura de directorios
* Autor
* Commiter
* Fecha Commit
* Descripción
* Cero, uno o mas punteros a revisiones que preceden a esta

En Git un commit es una revisión

## Por qué

* Guardar todo
* Es fácil recuperar versiones
* Sirve de Documentacion

## Git Help
git help <comando> muestra ayuda de un determinado <comando>

## Setup inicial

### Gitconfig
La info la podemos tener en un archivo global, llamado `.gitconfig` o por proyecto. Si el equipo lo vamos a dedicar a MeLi exclusivamente, entonces podemos configurar el archivo global, ejecutando los comandos con el parámetro `--global`

```
git config user.name "Tu Nombre"
git config user.email tu@correo.com
```

Por defecto Git usa como editor a vi, por lo que si lo queremos cambiar, podemos usar el comando:

```
git config --global core.editor gedit
```

### Crear claves ssh

Esto no es parte de git, pero simplifica la tarea de autenticarse con los servidores remotos, como GitHub

###### Creación clave ssh

`ssh-keygen -t rsa -b 4096 -C "you@computer-name"`

Una vez creada, se puede asociar al usuario creado en GitHub (Bitbucket, Gitlab, etc)

## Áreas

    Hay 4 áreas principales:

##### Archivos sin trackear

Son archivos que todavía no le dijimos a git que tiene que versionar.

##### Área de trabajo

Archivos que han sido modificados, pero que aun no fueron commiteados

##### Área de staging

Archivos modificados que han sido marcados para ir en el próximo commit.

##### Directorio .git (repositorio)

Base de datos donde se almacenan todas las versiones de todos los archivos que estemos trackeando

## Primeros Pasos

#### Init repo

`git init`

#### Agregar Contenido

Agrega los archivos que indiquemos al área de `staging` para que este disponibles para el commit.

` git add hola-mundo.txt `

#### commit

` git commit `

* Todos los archivos modificados que se quieran commitear deben ser agregados al área de staging
* Ya que los archivos agregados son puestos en el área de staging `add` es sinónimo de `staging`


#### status
` git status `


#### .gitignore    

En el archivo `.gitignore` podemos poner nombres de archivos, directorios que queremos que git ignore a la hora mostrarnos el status o hacer operaciones.

Los archivos que ya se encuentran trackeados, van a seguir apareciendo, aun si los agregamos al gitignore.


## Diffs

Antes de commitear es una buena idea ver las modificaciones que se estan por persistir. Una forma de verlo es pedirle a git que nos muestra un diff
    Un diff consiste en las siguientes partes para cada archivo modificado

1. una pseudo linea de comando que comienza con `diff --git`
1. una linea describiendo la revisión anterior que comienza con `---`
1. una linea describiendo la próxima revisión que comienza con `+++`
1. varios bloques que empiezan con `@@ -<begin line>,<line count> +<begin line>,<line count>`
1. un bloque de lineas que empiezan con un espacio en las lineas iguales en ambas revisiones, un `-` en lineas de la revisión anterior y un `+` en las lineas de la nueva revisión

###### Generar y visualizar un cambio

` git diff `

###### ver el diff en colores

` git diff --color `


### Detalles internos

* Todo es un objeto
* Todo objeto tiene un tipo
* Todo objeto tiene un nombre que es calculado como una function hash, SHA-1, a partir del tipo y los contenidos. Es un número de 40 dígitos
* Todos los objetos son inmutables (cambiar algo cambia el nombre/hash)
* Archivos son guardados como `blobs`
* Los directorios son guardados como arboles
* Las revisiones son guardadas como commits

Para simplificar el uso, ademas del nombre largo, el hash de 40 caracteres, los commits se pueden referenciar como:
* nombre corto
* HEAD
* HEAD~<n>


######  Inspeccionar la historia

` git log `

######  Inspeccionar la historia de manear gráfica

` gitk `

######  Inspeccionar la historia con diffs

` git log -p `

######  Inspeccionar los últimos 5 commits de la historia con diffs

` git log -p -5 `

######  Inspeccionar un commit especifico

` git show HEAD~5 `

######  Inspeccionar commits que afectan un o mas archivos

` git log README `

Para que no haya ambigüedades a la hora de definir los archivos sobre los cuales vamos a operar, usamos `--` antes de los nombres.

` git log HEAD -5 -- doc `

## Ramas

Qué son las ramas y por qué las necesitamos.

Una rama es un puntero con nombre en el gráfico de commits. La rama principal se llama `master`. Ya que son punteros, son creadas muy fácilmente.

Cuando hacemos un commit en una rama, avanza ese puntero, si necesitamos identificar un commit especifico, podemos usar un `tag`. Los nombre de las ramas deben ser relativamente simples, ya que no pueden contener espacios o caracteres raros.

Las ramas se usan para organizar cambios por tópicos o features a desarrollar. Permite que los cambios queden aislados hasta que estamos listos para mezclarlos con el código principal.

Depende del flujo de trabajo elegido, pero en general las ramas dependen de los tickets o tareas a realizar.

###### Crear una rama

` git checkout -b nueva-rama `

###### Cambiar a otra rama

` git checkout master `

###### Ver ramas locales

` git branch `

###### Volver a la rama anterior

` git checkout @{-1} `


## Qué significa "Distribuido"

Hasta este punto el repositorio es local al directorio de trabajo. Git se puede sincronizar con otros repositorios.

Normalmente hay un repositorio designado como principal. Si queres incializar uno en un directorios de trabajo, usa `git clone`. No es necesario que un repositorio tenga un directorios de trabajo, si este es el caso, se llaman `bare`.

###### Clonar un repositorios

` git clone git://github.com/git/hello-world `

###### Clonar un repositorio de un disco o file server

` git clone /path/to/hello-world `


###### Prepara un repo bare para usa compartido

` git init --bare --shared=group /path/to/fileserver/my.git `

## Merge

Cuando se trabaja con otros, sus cambios (incluyendo toda la historia de commits) necesita ser integrada en otra rama. Este proceso se denonima `merge`.
Para lograr esto, las versiones de los archivos son puestos en el area de staging (el `index`) y todos los cambios que no se pisen son resueltos automaticamente. Los cambios que se solapan no son resueltos, sino que se marcan como conflictos.


###### Inicializar y commitear un archivo

```
echo Hello > hello.txt
git add hello.txt
git commit -m initial
```

###### Inicializar, una nueva rama y comitear

```
git checkout -b tut-anch-amun
echo Boooh > hello.txt
git add hello.txt
git commit -m "On branch"
```

###### Cambiar de rama y modificar el mismo archivo

```
git checkout @{-1}
echo Hi > hello.txt
git add hello.txt
git commit -m "On original branch"
```

######  Merge

` git merge otra-rama `


#### Conflictos de merge

Al crear un merge, podemos encontrarnos con conflictos, lineas que fueron modificadas en las ramas que queremos unificar.

En este caso tenemos distintas estrategias que podemos usar. La mas común es abrir el archivo, corregir el problema, mezclando el codigo, agregando el archivo al area de staging y terminando el commit.


## Rebase


### Qué son los reflogs

Cada repositorio tiene su propia historia. Esto se guarda en `reflogs`, los cuales son eliminados periódicamente, pero pueden ser útiles.
Los reflogs se pueden acceder agregar `@{numero}` o `@{fecha}` a un nombre de rama o `HEAD`

###### Ver como estaba la revisión hace 5 minutos

` git show HEAD@{5.minutes.ago} `

###### Ver la historia de reflog

` git log -g `


## Qué es el stash

En algunos casos es necesario guardar las modificaciones y volver a un estado limpio, pero sin perder los cambios. Esto se logra usando el `stash`.

El `stash` es una pseudo-rama especial. Su historia esta en el reflog.

###### Hacer un cambio y crear un `stash`

```
echo Shalom > hello.txt
git stash
```

###### Recuperarlo

` git stash apply `

###### Stash solo los cambios que no se hayan agregado a git todavia

```
echo Howdy > hello.txt
git add hello.txt
echo Hey hey > hello.txt
git stash -k
```


###### Obtener una lista de cambios en el stash


` git stash list `


###### Stash con un mensaje personalizado

` git stash save This is my description `

###### Quitar el ultimo cambio del stash


` git stash pop `


## BBDD de objetos

La principal forma de ver los commits es usando `git show`. Puede mostrar `tags`, `commits`, `trees` y `blobs`.

Para acceder a mas info de los commits, usar el parametro `--pretty=fuller`

Para acceso de nivel mas bajo, usar `git cat-file`

###### Mostrar el mensaje de commit con el diff

` git show --pretty=raw HEAD `

###### Mostrar el árbol de un commit

` git show HEAD: `

###### Mostrar un blob

` git show HEAD:Documentation/README `

###### Acceso de bajo nivel a un commit  

` git cat-file commit HEAD `

## Checkout y Reset

Checkout permite cambiar entre ramas e inicializar nuevas.

Desafortunadamente, checkout también es el comando para recuperar una versión de archivo de otra rama, sin cambiarla. Estos archivos son automáticamente agregados al `stage` para ser commiteados.

Para quitar los cambios del área de staging, se puede usar el comando `reset`. Sin argumentos, resetea todos los archivos que git concoce.

El comando `reset` también se puede usar para resetear el área de trabajo

###### Buscar una versión de un archi de otra rama

` git checkout other-branch README `


###### Quitar del stage todos los cambios

Revierte todo los cambios listos para ser commiteados al estado `modificado`

` git reset `


###### Eliminar todos los cambios

Funciona como `stash`, pero no se guardan, los cambios se pierden

` git reset --hard `


## Repositorios Remotos

Ademas del repositorios clonado, otros repositorios puede ser enlazados, usando el comando `remote`. Esos repos son llamados repositorios remotos o `remotos`

El repositorio del cual clonamos se denomina `origin`

La interacción con el repositorio remoto se realiza usando:

* `git fetch` que genera copias locales de las ramas del repositorio remoto, incluyendo todos los objetos requerido.
* `git push` que actualiza el repositorio remoto con los cambios locales

Las copias locales de las ramas remotas viven en un espacio de nombre especifico `refs/remotes/<name>`. Por ejemplo, la rama `master` del remoto llamado `origin` sera guardado en `refs/remotes/origin/master`. Para que no hay ambigüedades podemos referirnos como `origin/master` 

A tener en cuenta, si tenemos un proyecto con multiples remotos y hacemos un `fetch` git lo va aplicar sin preocuparle cual es el origen.

Hay un comando que resume el proceso de hacer `fetch` y `merge` para traer los cambios al repo local:`pull`  

###### Agregar un remoto:

` git remote add hello git://github.com/git/hello-world `

###### Traer info de un remoto

` git fetch hello `


###### Listar copias locales de las ramas remotas

` git branch -r `

###### Push de una rama

` git push origin master `


###### Push de la rama actual

` git push origin HEAD `


###### Push de una rama con un nombre distinto del nombre local

` git push origin my-new-branch:master `

###### Borrar una rama de repo remoto

` git push --delete origin blabla-branch `


## Tags

Las etiquetas o `tags` nos permiten marcar un commit especifico, son un puntero al estado del repo en el momento del commit al que apuntan. Esto es útil para marcar versiones o deploys

Una etiqueta anotada es una parte de la historia del repo que no puede ser cambiada.

###### Etiqueta Lightweight

` git tag etiqueta_simple `

###### Etiqueta anotada

` git tag -a v1.0 -m ‘Version 1.0’ `

###### Subir las etiquetas al repositorio remoto

` git push origin --tags `
