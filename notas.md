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

# EXPLICAR AREAS


### Primeros Pasos

#### Init repo

`git init`

#### Agregar Contenido

Agrega los archivos que indiquemos al área de `staging` para que este disponibles para el commit.

    
```
git add hello-world.txt
```

#### commit

```
git commit
```

* Todos los archivos modificados que se quieran commitear deben ser agregados al área de staging
* Ya que los archivos agregados son puestos en el área de staging `add` es sinónimo de `staging`


#### status
```
git status
```


#### .gitignore    


#### gitconfig


#### diffs

Antes de commitear es una buena idea ver las modificaciones que se estan por persistir. Una forma de verlo es pedirle a git que nos muestra un diff
Un diff consiste en las siguientes partes para cada archivo modificado

1. una pseudo linea de comando que comienza con `diff --git`
1. una linea describiendo la revisión anterior que comienza con `---`
1. una linea describiendo la próxima revisión que comienza con `+++`
1. varios bloques que empiezan con `@@ -<begin line>,<line count> +<begin line>,<line count>`
1. un bloque de lineas que empiezan con un espacio en las lineas iguales en ambas revisiones, un `-` en lineas de la revisión anterior y un `+` en las lineas de la nueva revisión

##### Tareas:

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


##### Tareas:

######  Inspeccionar la historia
    
` git log `

######  Inspeccionar la historia de manear gráfica

` gitk `

######  Inspeccionar la historia con diffs

` git log -p `

######  Inspeccionar los últimos 5 commits de la historia con diffs
    
` git log -p -5 `

######  Inspeccionar un commit especifico
    
git show HEAD~5

######  Inspeccionar commits que afectan un o mas archivos
    
` git log README `

Para que no haya ambigüedades a la hora de definir los archivos sobre los cuales vamos a operar, usamos `--` antes de los nombres.

` git log HEAD -5 -- doc `

## Ramas

Qué son las ramas y por qué las necesitamos.

Una rama es un puntero con nombre en el gráfico de commits. La rama principal se llama `master`. Ya que son punteros, son creadas muy fácilmente.

Cuando hacemos un commit en una rama, avanza ese puntero, si necesitamos identificar un commit especifico, podemos usar un `tag`. Los nombre de las ramas deben ser relativamente simples, ya que no pueden contener espacios o caracteres raros.

Las ramas se usan para organizar cambios por tópicos.

###### Crear una rama

` git checkout -b my-new-branch `

###### Cambiar a otra rama

` git checkout master `

###### Ver ramas locales

` git branch `

###### Volver a la rama anterior

` git checkout @{-1} `


## Qué significa "Distribuido"

What does "distributed" mean with regards to Git?

So far, the repository is local to the working directory. But Git can also synchronize with multiple other repositories.

Typically, there is one designated main repository. If you want to initialize a working directory from a given repository, use git clone.

A main repository usually does not need a working directory, in which case it is called a bare repository.
Hands-on

    Clone a repository

?
1
    
git clone git://github.com/git/hello-world

    Clone a repository from USB disk or file server

?
1
    
git clone /path/to/hello-world

    Set up a main repository on the file server

?
1
    
git init --bare --shared=group /path/to/fileserver/my.git
The merge concept of Git (and what is this "index" all about?)

When working with others, or with topic branches, the changes (including their complete commit history) need to be integrated into another branch, typically master. This integration is called merging.

In order to perform a merge, the file versions are put into a staging area (the index), and all non-overlapping changes are resolved automatically. Overlapping (read: contradicting) changes are not resolved, but marked as merge conflicts.

Please refer to the Fiji's page on Git Conflicts for a detailed explanation how to resolve merge conflicts.
Hands-on

    Initialize and commit a file, say, hello.txt

?
1
2
3
    
echo Hello > hello.txt
git add hello.txt
git commit -m initial

    Initialize a new branch and modify hello.txt

?
1
2
3
4
    
git checkout -b tut-anch-amun
echo Boooh > hello.txt
git add hello.txt
git commit -m "On branch"

    Switch back to old branch, modify hello.txt

?
1
2
3
4
    
git checkout @{-1}
echo Hi > hello.txt
git add hello.txt
git commit -m "On original branch"

    Merge

?
1
    
git merge tut-anch-amun
What are reflogs? How can they help me?

We looked at the commit history previously. But every repository has its own individual history; for example, yesterday at noon, a specific branch with a specific revision was the current branch. This is stored in the reflogs (for space efficiency, reflogs are not available eternally but are pruned at some stage).

You can access the reflogs by appending @{<number>} or @{<date>} to a branch name or to HEAD.
Hands-on

    See what revision was current five minutes ago

?
1
    
git show HEAD@{5.minutes.ago}

    See the reflog history

?
1
    
git log -g
What is the stash?

Sometimes one needs to store away all modifications and go back to a clean state, but keep the modifications accessible. This is done using the stash.

Note: the stash is a special pseudo-branch, living in refs/stash. Their history is in the reflog.
Hands-on

    Make a change and stash it

?
1
2
    
echo Shalom > hello.txt
git stash

    Get it back

?
1
    
git stash apply

    Stashing only the changes that have not been git added yet

?
1
2
3
4
    
echo Howdy > hello.txt
git add hello.txt
echo Hey hey > hello.txt
git stash -k

    Getting a list of stashed changes

?
1
    
git stash list

    Stash with a custom message

?
1
    
git stash save This is my description

    Remove the latest changes from the stash

?
1
    
git stash pop
Accessing parts of the object database

The primary way to look at commits is by using git show. It can show tags, commits, trees and blobs.

To access more information about commits, use the parameter --pretty=fuller.

For more low-level access, use git cat-file.
Hands-on

    Show the raw commit message, with the diff

?
1
    
git show --pretty=raw HEAD

    Show a commit's corresponding top-level tree

?
1
    
git show HEAD:

    Show a blob

?
1
    
git show HEAD:Documentation/README

    Low-level access to a commit

?
1
    
git cat-file commit HEAD
What meanings do "checkout", "reset" have in Git?

We already saw that checkout can switch between branches and even initialize new branches.

Unfortunately, checkout is also the command to pick file versions from other branches without switching the branches. Thusly picked file versions are automatically added, i.e. staged for the next commit.

To unstage changes, one can use the reset command (without arguments, it resets all files which HEAD knows about currently).

The reset command can further be convinced to reset not only the index (staging area), but also the working directory.
Hands-on

    pick a file version from another branch

?
1
    
git checkout other-branch README

    Unstage all changes, i.e. revert all files from "to-be-committed" status to "modified"

?
1
    
git reset

    Get rid of all changes (like git stash but the changes are not stored)

?
1
    
git reset --hard
Git's concept of "remote repositories"

In addition to the repository from which we cloned, other repositories can be linked, too, using the git remote command. Such repositories are called remote repositories or simply remotes.

The repository from which we cloned is called origin.

Interaction with remote repositories is performed using

    git fetch, which local copies of the branches of the remote repository including all required objects (but being a bit clever about avoiding to get objects we have already) and
    git push, which updates remote repositories' branches to the local branches' state.

The local copies of remote branches live in a specific namespace, refs/remotes/<name>. For example, the master branch of the remote called origin will be stored in refs/remotes/origin/master. If unambiguous, it can be referred to as origin/master, too.

Note: you need to make sure that you do not mix two different projects in the same working directory's repository. Git will happily fetch from wherever into your local repository, even if it does not make sense.

Note: there is a shortcut for fetch & merge: pull. If you create a new branch from a local copy of a remote branch, you can even set it up such that git pull without further parameters will fetch from the correct remote and merge the correct branch: git checkout --track origin/cool-feature
Hands-on

    Add a remote

?
1
    
git remote add hello git://github.com/git/hello-world

    Fetch from a remote

?
1
    
git fetch hello

    Listing all local copies of the remotes' branches

?
1
    
git branch -r

    Push a branch

?
1
    
git push origin master

    Push the current branch

?
1
    
git push origin HEAD

    Push to a branch with a name differing from the local name

?
1
    
git push origin my-new-branch:master

    Delete a branch from a remote repository

?
1
    
git push --delete origin blabla-branch

