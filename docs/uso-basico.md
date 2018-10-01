# Uso básico de Git #

## Crear un proyecto ##

### Crear un programa "Hola Mundo"

Creamos un directorio donde colocar el código

```
     mkdir curso-de-git
     cd curso-de-git
```

Creamos un fichero `README.md` que muestre una descripción de nuestro proyecto.
```markdown
  #Curso de git ASL
```

### Crear el repositorio

Para crear un nuevo repositorio se usa la orden `git init`
```
    $ git init
    Inicializado repositorio Git vacío en /home/cyberh99/code/curso-de-git/.git/
```

### Añadir la aplicación

Vamos a almacenar el archivo que hemos creado para presentar el proyecto, este será siempre uno de los primeros _commits_ que tenemos que realizar.
```
     $ git add README.md
     $ git commit -m "initial commit"
    [master (commit-raíz) afdd3c3] initial commit
    1 file changed, 1 insertion(+)
    create mode 100644 README.md
```
### Comprobar el estado del repositorio
Creamos un fichero `hello.cpp` que muestre Hola Mundo, este será el primer archivo para desarrollar nuestro proyecto

```c++
#include <iostream>

int main(){
  std::cout<<"Hello World"<<std::endl;
  return 0;
}
```
Con la orden `git status` podemos ver en qué estado se encuentran los archivos de nuestro repositorio.
```
     $ git status
     En la rama master
 Archivos sin seguimiento:
   (usa "git add <archivo>..." para incluirlo a lo que se será confirmado)

 	hello.cpp

 no hay nada agregado al commit pero hay archivos sin seguimiento presentes (usa "git add" para hacerles seguimiento)
```
Si volvemos a agregar nuestro nuevo archivo a git, veremos como cambia la salida del comando
```
  $ git add hello.cpp
  $ git commit -m "Comenzamos el proyecto"
  [master 24a4f96] comenzamos el proyecto
 1 file changed, 6 insertions(+)
 create mode 100644 hello.cpp
```

```
 $ git status
 En la rama master
 nada para hacer commit, el árbol de trabajo esta limpio
```

Si modificamos el archivo `hola.cpp`:

```c++
#include <iostream>

int main(int argc, char* argv[]){
  if (argc < 2){
    std::cout<<"Usage: hello Nombre"<<std::endl;
  }
  std::cout<<"Hello World "<<argv[1]<<std::endl;
  return 0;
}
```

Y volvemos a comprobar el estado del repositorio:
```
    $ git status
    En la rama master
Cambios no rastreados para el commit:
  (usa "git add <archivo>..." para actualizar lo que será confirmado)
  (usa "git checkout -- <archivo>..." para descartar los cambios en el directorio de trabajo)

	modificado:     hello.cpp

sin cambios agregados al commit (usa "git add" y/o "git commit -a")
```

### Añadir cambios

Con la orden `git add` indicamos a git que prepare los cambios para que sean almacenados.

    $ git add hello.cpp
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #   modified:   hello.cpp
    #

### Confirmar los cambios

Con la orden `git commit` confirmamos los cambios definitivamente, lo que hace que se guarden permanentemente en nuestro repositorio.
```
    $ git commit -m "Parametrización del programa"
    [master 4a4ad0d] Agregados parametros
    1 file changed, 6 insertions(+), 3 deletions(-)
    $ git status
    # On branch master
    nothing to commit (working directory clean)
```
### Diferencias entre _workdir_ y _staging_.

Modificamos nuestra aplicación para que muestre otro mensaje y añadimos los cambios.

```c++
#include <iostream>

int main(int argc, char* argv[]){
  if (argc < 2){
    std::cout<<"Usage: hello Nombre"<<std::endl;
  }
  std::cout<<"Hello World "<<argv[1]<<"Bienvenido al curso de git"<<std::endl;
  return 0;
}
```

Este vez añadimos los cambios a la fase de _staging_ pero sin confirmarlos (_commit_).

    $ git add hello.cpp

Volvemos a modificar el programa para indicar con un comentario lo que hemos hecho.

```c++
#include <iostream>

int main(int argc, char* argv[]){
  //Funcion para comprobar los numeros de parametros
  if (argc < 2){
    std::cout<<"Usage: hello Nombre"<<std::endl;
  }

  std::cout<<"Hello World "<<argv[1]<<"Bienvenido al curso de git"<<std::endl;
  return 0;
}

```

Y vemos el estado en el que está el repositorio
```
    $ git status
    En la rama master
Cambios a ser confirmados:
  (usa "git reset HEAD <archivo>..." para sacar del área de stage)

	modificado:     hello.cpp

Cambios no rastreados para el commit:
  (usa "git add <archivo>..." para actualizar lo que será confirmado)
  (usa "git checkout -- <archivo>..." para descartar los cambios en el directorio de trabajo)

	modificado:     hello.cpp
```

Podemos ver como aparecen el archivo _hello.cpp_ dos veces. El primero está preparado
para ser confirmado y está almacenado en la zona de _staging_. El segundo indica
que el directorio hola.php está modificado otra vez en la zona de trabajo (_workdir_).

**warning**:
    Si volvieramos a hacer un `git add hello.cpp` sobreescribiríamos los cambios previos
    que había en la zona de _staging_.

Almacenamos los cambios por separado:
```
    $ git commit -m "Cambiado el mensaje por defecto"
    [master 506066a] Cambiado el mensaje por defecto
    1 file changed, 1 insertion(+), 1 deletion(-)
    $ git status
    En la rama master
  Cambios no rastreados para el commit:
    (usa "git add <archivo>..." para actualizar lo que será confirmado)
    (usa "git checkout -- <archivo>..." para descartar los cambios en el directorio de trabajo)

  	modificado:     hello.cpp

  sin cambios agregados al commit (usa "git add" y/o "git commit -a")

    $ git add .
    $ git status
    # En la rama master
Cambios a ser confirmados:
  (usa "git reset HEAD <archivo>..." para sacar del área de stage)

	modificado:     hello.cpp


    $ git commit -m "Agregamos comentarios"
    [master 8ca08af] Agregamos comentarios
     1 file changed, 2 insertions(+)
```

_**info**_
    El valor "." despues de `git add` indica que se añadan todos los archivos de forma recursiva.

_**warning**_
    Cuidado cuando uses `git add .` asegúrate de que no estás añadiendo archivos que no quieres añadir.

### Ignorando archivos

La orden `git add .` o `git add nombre_directorio` es muy cómoda, ya que nos permite añadir todos los archivos del proyecto o todos los contenidos en un directorio y sus subdirectorios. Es mucho más rápido que tener que ir añadiéndolos uno por uno. El problema es que, si no se tiene cuidado, se puede terminar por añadir archivos innecesarios o con información sensible.

Por lo general se debe evitar añadir archivos que se hayan generado como producto de la compilación del proyecto, los que generen los entornos de desarrollo (archivos de configuración y temporales) y aquellos que contentan información sensible, como contraseñas o tokens de autenticación. Por ejemplo, en un proyecto de `C/C++`, los archivos objeto no deben incluirse, solo los que contengan código fuente y los make que los generen.

Para indicarle a _git_ que debe ignorar un archivo, se puede crear un fichero llamado _.gitignore_, bien en la raíz del proyecto o en los subdirectorios que queramos. Dicho fichero puede contener patrones, uno en cada línea, que especiquen qué archivos deben ignorarse. El formato es el siguiente:

```
# .gitignore
dir1/           # ignora todo lo que contenga el directorio dir1
!dir1/info.txt  # El operador ! excluye del ignore a dir1/info.txt (sí se guardaría)
dir2/*.txt      # ignora todos los archivos txt que hay en el directorio dir2
dir3/**/*.txt   # ignora todos los archivos txt que hay en el dir3 y sus subdirectorios
*.o             # ignora todos los archivos con extensión .o en todos los directorios
```

Cada tipo de proyecto genera sus ficheros temporales, así que para cada proyecto hay un `.gitignore` apropiado. Existen repositorios que ya tienen creadas plantillas. Podéis encontrar uno en [https://github.com/github/gitignore](https://github.com/github/gitignore)

### Ignorando archivos globalmente

Si bien, los archivos que hemos metido en `.gitignore`, deben ser aquellos ficheros temporales o de configuración que se pueden crear durante las fases de compilación o ejecución del programa, en ocasiones habrá otros ficheros que tampoco debemos introducir en el repositorio y que son recurrentes en todos los proyectos. En dicho caso, es más útil tener un _gitignore_ que sea global a todos nuestros proyectos. Esta configuración sería complementaria a la que ya tenemos. Ejemplos de lo que se puede ignorar de forma global son los ficheros temporales del sistema operativo (`*~`, `.nfs*`) y los que generan los entornos de desarrollo.

Para indicar a _git_ que queremos tener un fichero de _gitignore_ global, tenemos que configurarlo con la siguiente orden:

```
git config --global core.excludesfile $HOME/.gitignore_global
```

Ahora podemos crear un archivo llamado `.gitignore_global` en la raíz de nuestra cuenta con este contenido:


```
# Compiled source #
###################
*.com
*.class
*.dll
*.exe
*.o
*.so

# Packages #
############
# it's better to unpack these files and commit the raw source
# git has its own built in compression methods
*.7z
*.dmg
*.gz
*.iso
*.jar
*.rar
*.tar
*.zip

# Logs and databases #
######################
*.log
*.sql
*.sqlite

# OS generated files #
######################
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db
*~
*.swp

# IDEs               #
######################
.idea
.settings/
.classpath
.project
```


## Trabajando con el historial

### Observando los cambios

Con la orden `git log` podemos ver todos los cambios que hemos hecho:
```
    $ git log
    commit 8ca08af5c30f5d715f85532592d2ddf99223907f (HEAD -> master)
    Author: cyberh99 <cyberh99@protonmail.com>
    Date:   Mon Oct 1 17:18:04 2018 +0200

        Agregamos comentarios

    commit 506066a73012aaa110eba39a8756fddb150a4bb8
    Author: cyberh99 <cyberh99@protonmail.com>
    Date:   Mon Oct 1 17:16:51 2018 +0200

        Cambiado el mensaje por defecto

    commit 4a4ad0d8b74b4e1ad91bb960ec28f81ffaa3e22c
    Author: cyberh99 <cyberh99@protonmail.com>
    Date:   Mon Oct 1 17:12:23 2018 +0200

        Agregados parametros

    commit 24a4f969086f4b3eaf8f33d1bc883590395c5f04
    Author: cyberh99 <cyberh99@protonmail.com>
    Date:   Mon Oct 1 17:06:18 2018 +0200

        comenzamos el proyecto

    commit afdd3c3d5ee6037ace39daa2fb9dbaceacb94347
    Author: cyberh99 <cyberh99@protonmail.com>
    Date:   Mon Oct 1 16:58:09 2018 +0200

        initial commit

```
También es posible ver versiones abreviadas o limitadas, dependiendo de los parámetros:
```    
    $ git log --oneline
    8ca08af (HEAD -> master) Agregamos comentarios
    506066a Cambiado el mensaje por defecto
    4a4ad0d Agregados parametros
    24a4f96 comenzamos el proyecto
    afdd3c3 initial commit
    git log --oneline --max-count=2
    git log --oneline --since='5 minutes ago'
    git log --oneline --until='5 minutes ago'
    git log --oneline --author=sergio
    git log --oneline --all
```
Una versión muy útil de `git log` es la siguiente, pues nos permite ver en que lugares está master y HEAD, entre otras cosas:
```

    $ git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short
    * 8ca08af 2018-10-01 | Agregamos comentarios (HEAD -> master) [cyberh99]
    * 506066a 2018-10-01 | Cambiado el mensaje por defecto [cyberh99]
    * 4a4ad0d 2018-10-01 | Agregados parametros [cyberh99]
    * 24a4f96 2018-10-01 | comenzamos el proyecto [cyberh99]
    * afdd3c3 2018-10-01 | initial commit [cyberh99]
```

### Crear alias

Como estas órdenes son demasiado largas, Git nos permite crear alias para crear nuevas órdenes parametrizadas. Para ello editaremos un archivo llamado `.gitconfig` que está en nuestro `$HOME` y le añadiremos estas líneas al final:

    [alias]
      hist = log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short


### Recuperando versiones anteriores

Cada cambio es etiquetado por un hash, para poder regresar a ese momento del estado del proyecto se usa la orden `git checkout`.
```
    $ git checkout e19f2c1
    Nota: actualizando el árbol de trabajo '4a4ad0d'.

    Te encuentras en estado 'detached HEAD'. Puedes revisar por aquí, hacer
    cambios experimentales y confirmarlos, y puedes descartar cualquier
    commit que hayas hecho en este estado sin impactar a tu rama realizando
    otro checkout.

    Si quieres crear una nueva rama para mantener los commits que has creado,
    puedes hacerlo (ahora o después) usando -b con el comando checkout. Ejemplo:

      git checkout -b <nombre-de-nueva-rama>

    HEAD está ahora en 4a4ad0d Agregados parametros
```
```
    $ cat hello.cpp
    #include <iostream>

    int main(int argc, char* argv[]){
      if (argc < 2){
        std::cout<<"Usage: hello Nombre"<<std::endl;
      }
      std::cout<<"Hello World"<<argv[1]<<std::endl;
      return 0;
    }

```
El aviso que nos sale nos indica que estamos en un estado donde no trabajamos en ninguna rama concreta. Eso significa que los cambios que hagamos podrían "perderse" porque si no son guardados en una nueva rama, en principio no podríamos volver a recuperarlos. Hay que pensar que Git es como un árbol donde un nodo tiene información de su nodo padre, no de sus nodos hijos, con lo que siempre necesitaríamos información de dónde se encuentran los nodos finales o de otra manera no podríamos acceder a ellos.

### Volver a la última versión de la rama master.

Usamos `git checkout` indicando el nombre de la rama:
```
    $ git checkout master
    La posición previa de HEAD era 4a4ad0d Agregados parametros
Cambiado a rama 'master'  
```
### Etiquetando versiones

Para poder recuperar versiones concretas en la historia del repositorio, podemos etiquetarlas, lo cual es más facil que usar un hash. Para eso usaremos la orden `git tag`.

    $ git tag v1

Ahora vamos a etiquetar la versión inmediatamente anterior como v1-beta. Para ello podemos usar los modificadores `^` o `~` que nos llevarán a un ancestro determinado. Las siguientes dos órdenes son equivalentes:

    $ git checkout v1^
    $ git checkout v1~1
    $ git tag v1-beta

Si ejecutamos la orden sin parámetros nos mostrará todas las etiquetas existentes.

    $ git tag
    v1
    v1-beta

Y para verlas en el historial:

    $ git hist master --all

### Borrar etiquetas

Para borrar etiquetas:

    git tag -d nombre_etiqueta

### Visualizar cambios

Para ver los cambios que se han realizado en el código usamos  la orden `git diff`. La orden sin especificar nada más, mostrará los cambios que no han sido añadidos aún, es decir, todos los cambios que se han hecho antes de usar la orden `git add`. Después se puede indicar un parámetro y dará los cambios entre la versión indicada y el estado actual. O para comparar dos versiones entre sí, se indica la más antigua y la más nueva. Ejemplo:
```
    $ git diff v1-beta v1

    index 0b00757..bb1c5b7 100644
    --- a/hello.cpp
    +++ b/hello.cpp
    @@ -1,11 +1,9 @@
     #include <iostream>

     int main(int argc, char* argv[]){
    -  //Funcion para comprobar los numeros de parametros
       if (argc < 2){
         std::cout<<"Usage: hello Nombre"<<std::endl;
       }
    -
       std::cout<<"Hello World "<<argv[1]<<"Bienvenido al curso de git"<<std::endl;
       return 0;
     }
```
