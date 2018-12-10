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
    All modified files should be added before calling git commit
    Since the added files are staged for commit, the command git stage is a synonym for git add'
    making use of the git status output
    using a .gitignore file
    adding a Sign-off (AKA S-O-B)
    git init creates .git/ in which all of the repository's files live
    git config accesses .git/config, ~/.gitconfig and /etc/gitconfig

What are diffs?

Before committing, it is a good idea to see what modifications are about to be committed. One mode to look at them is to let Git show a diff. A diff consists of the following parts for every modified file:

    a pseudo command line starting with diff --git
    a line describing the previous revision, starting with ---
    a line describing the next revision, starting with +++
    one or more hunks starting with @@ -<begin line>,<line count> +<begin line>,<line count>
    a corresponding block of lines starting with a space for lines in both the previous and next revision, a minus for lines only in the previous one (removed line) and a plus for lines only in the next revision (added line). A modified line will show up as removed and added.

Hands-on

    Looking at a diff:

?
1
    
git diff

    Looking at a diff in color:

?
1
    
git diff --color

    Changing the default to color diffs:

?
1
    
git config --global diff.color auto
A word on the data model of Git (and how to reference objects)

See Git for computer scientists.

    Everything is an object
    Every object has a type
    Every object has a name that is calculated by a one-way function (the hash function SHA-1) from the type and the contents
    It is a 40-digit hex number
    Every object is immutable (changing something changes the name).
    Plain files are encoded as blobs
    Directories are encoded as trees
    Revisions are encoded as commits

For ease of use, in addition to their long name (40 hex characters, quite klunky but precise) commits can be referred to by

    short name
    HEAD
    HEAD~<n>

Hands-on

    Inspecting the version history

?
1
    
git log

    Inspecting the version history in a graphical way

?
1
    
gitk

    Inspecting the version history with diffs

?
1
    
git log -p

    Inspecting the latest 5 revisions with diffs

?
1
    
git log -p -5

    Inspecting just one commit

?
1
    
git show HEAD~5

    Looking at all commits touching a specific set of files/directories

?
1
    
git log README

Note: to disambiguate between start commit and files, put a -- between commit and/or options and files/directories, e.g. git log HEAD -5 -- doc
What are branches? Why do I need them?

A branch is a named pointer into the commit graph. The main branch is called 'master' (Subversion's trunk, Mercurial's default). Since branches are just pointers, they are very easily created.

Committing while on a branch advances that pointer (if you need something that cannot advance, you need to use tags).

Note: branch names must be simple, i.e. not contain spaces or wild characters (although minus & underscore is okay).

Branches can be used to organize sets of changes by topic. Compare also Fiji's tutorial on topic branches.
Hands-on

    Creating a branch

?
1
    
git checkout -b my-new-branch

    Switching to another branch

?
1
    
git checkout master

    Seeing all (local) branches (the current one is prefixed by a star)

?
1
    
git branch

    Switching back to the previous branch

?
1
    
git checkout @{-1}
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

