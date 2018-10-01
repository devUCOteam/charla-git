# Uso avanzado de Git

## Deshacer cambios

### Deshaciendo cambios antes de la fase de staging.

Volvemos a la rama máster y vamos a modificar el comentario que pusimos:

    $ git checkout master
    Previous HEAD position was 3283e0d... Se añade un parámetro por defecto
    Switched to branch 'master'

Modificamos _hello.cpp_ de la siguiente manera:

```c++
#include <iostream>

int main(int argc, char* argv[]){
  //Me he equivocado en el comentario, pero la funcion hace eso :-)
  if (argc < 2){
    std::cout<<"Usage: hello Nombre"<<std::endl;
  }

  std::cout<<"Hello World "<<argv[1]<<"Bienvenido al curso de git"<<std::endl;
  return 0;
}

```

Y comprobamos:
```
    $ git status
    En la rama master
Cambios no rastreados para el commit:
  (usa "git add <archivo>..." para actualizar lo que será confirmado)
  (usa "git checkout -- <archivo>..." para descartar los cambios en el directorio de trabajo)

	modificado:     hello.cpp

sin cambios agregados al commit (usa "git add" y/o "git commit -a")
```

El mismo Git nos indica que debemos hacer para añadir los cambios o para deshacerlos:
```
    $ git checkout hola.php
    $ git status
    En la rama master
    nada para hacer commit, el árbol de trabajo esta limpio

    $ cat hola.php
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

### Deshaciendo cambios antes del commit

Vamos a hacer lo mismo que la vez anterior, pero esta vez sí añadiremos el cambio al _staging_ (sin hacer _commit_).
Así que volvemos a modificar _hello.cpp_ igual que la anterior ocasión:

```c++
#include <iostream>

int main(int argc, char* argv[]){
  //Enserio, sigo fallando en comentarios. Help me please :/
  if (argc < 2){
    std::cout<<"Usage: hello Nombre"<<std::endl;
  }

  std::cout<<"Hello World "<<argv[1]<<"Bienvenido al curso de git"<<std::endl;
  return 0;
}

```

Y lo añadimos al _staging_
```
    $ git add hello.cpp
    $ git status
    En la rama master
  Cambios a ser confirmados:
    (usa "git reset HEAD <archivo>..." para sacar del área de stage)

  	modificado:     hello.cpp

```
De nuevo, Git nos indica qué debemos hacer para deshacer el cambio:
```
    $ git reset HEAD hello.cpp
    Cambios fuera del área de stage tras el reset:
    M	hello.cpp

    $ git status
    En la rama master
Cambios no rastreados para el commit:
  (usa "git add <archivo>..." para actualizar lo que será confirmado)
  (usa "git checkout -- <archivo>..." para descartar los cambios en el directorio de trabajo)

	modificado:     hello.cpp

sin cambios agregados al commit (usa "git add" y/o "git commit -a")

    $ git checkout hello.cpp
```
Y ya tenemos nuestro repositorio limpio otra vez. Como vemos hay que hacerlo en dos pasos:
uno para borrar los datos del _staging_ y otro para restaurar la copia de trabajo.

### Deshaciendo commits no deseados.

Si a pesar de todo hemos hecho un commit y nos hemos equivocado, podemos deshacerlo con la orden `git revert`.
Modificamos otra vez el archivo como antes:

```c++
#include <iostream>

int main(int argc, char* argv[]){
  // 3º vez que fallo en el comentario, me voy a cambiar de carrera
  if (argc < 2){
    std::cout<<"Usage: hello Nombre"<<std::endl;
  }

  std::cout<<"Hello World "<<argv[1]<<"Bienvenido al curso de git"<<std::endl;
  return 0;
}

```

Pero ahora sí hacemos commit:
```
    $ git add hello.cpp
    $ git commit -m "Fallo en el comentario y lloro"
    [master 09e9f8b] Fallo en el comentario y lloro
     1 file changed, 1 insertion(+), 1 deletion(-)
```

Bien, una vez confirmado el cambio, vamos a deshacer el cambio con la orden `git revert`:
```
    $ git revert HEAD --no-edit
    [master 41f3998] Revert "Fallo en el comentario y lloro"
    Date: Mon Oct 1 18:20:00 2018 +0200
    1 file changed, 1 insertion(+), 1 deletion(-)
    $ git hist
    * 41f3998 2018-10-01 | Revert "Fallo en el comentario y lloro" (HEAD -> master) [cyberh99]
    * 09e9f8b 2018-10-01 | Fallo en el comentario y lloro [cyberh99]
    * 8ca08af 2018-10-01 | Agregamos comentarios [cyberh99]
    * 506066a 2018-10-01 | Cambiado el mensaje por defecto [cyberh99]
    * 4a4ad0d 2018-10-01 | Agregados parametros [cyberh99]
    * 24a4f96 2018-10-01 | comenzamos el proyecto [cyberh99]
    * afdd3c3 2018-10-01 | initial commit [cyberh99]
```


### Borrar commits de una rama

El anterior apartado revierte un commit, pero deja huella en el historial de cambios. Para hacer que no aparezca hay que usar la orden `git reset`.
```
    $ git reset --hard v1
    HEAD está ahora en 506066a Cambiado el mensaje por defecto
    $ git hist
    * 506066a 2018-10-01 | Cambiado el mensaje por defecto (HEAD -> master) [cyberh99]
    * 4a4ad0d 2018-10-01 | Agregados parametros [cyberh99]
    * 24a4f96 2018-10-01 | comenzamos el proyecto [cyberh99]
    * afdd3c3 2018-10-01 | initial commit [cyberh99]
```

El resto de cambios no se han borrado (aún), simplemente no están accesibles porque git no sabe como referenciarlos. Si sabemos su hash podemos acceder aún a ellos. Pasado un tiempo, eventualmente Git tiene un recolector de basura que los borrará. Se puede evitar etiquetando el estado final.

!!! danger
    La orden _reset_ es una operación delicada. Debe evitarse si no se sabe bien lo que se está haciendo,
    sobre todo cuando se trabaja en repositorios compartidos, porque podríamos alterar la historia de cambios
    lo cual puede provocar problemas de sincronización.

### Modificar un commit

Esto se usa cuando hemos olvidado añadir un cambio a un commit que acabamos de realizar. Tenemos
nuestro archivo _hello.cpp_ de la siguiente manera:

```c++
#include <iostream>

//Coded by god
//or by @cyberh99 who knows?
int main(int argc, char* argv[]){
  if (argc < 2){
    std::cout<<"Usage: hello Nombre"<<std::endl;
  }

  std::cout<<"Hello World "<<argv[1]<<"Bienvenido al curso de git"<<std::endl;
  return 0;
}

```

Y lo confirmamos:
```
    $ git commit -a -m "Añadido el autor del programa"
    [master c9a1ea1] Agregados comentarios súper útiles
   1 file changed, 3 insertions(+)
```

**tip**
    El parámetro `-a` hace un `git add` antes de hacer _commit_ de todos los archivos modificados
     o borrados (de los nuevos no), con lo que nos ahorramos un paso.

Ahora nos percatamos que se nos ha olvidado poner el correo electrónico. Así que volvemos a modificar nuestro archivo:

```c++
#include <iostream>

//Coded by god
//or by @cyberh99 who knows?
//okay... it's @cyberh99
int main(int argc, char* argv[]){
  if (argc < 2){
    std::cout<<"Usage: hello Nombre"<<std::endl;
  }

  std::cout<<"Hello World "<<argv[1]<<"Bienvenido al curso de git"<<std::endl;
  return 0;
}

```

Y en esta ocasión usamos `commit --amend` que nos permite modificar el último estado confirmado, sustituyéndolo por el estado actual:
```
    $ git add hola.php
    $ git commit --amend -m "Añadido el autor del programa y su email"
    [master 80919aa] Añadido comentario aclarativo
    Date: Mon Oct 1 18:23:25 2018 +0200
    1 file changed, 4 insertions(+)
    $ git hist
    * 80919aa 2018-10-01 | Añadido comentario aclarativo (HEAD -> master) [cyberh99]
    * 506066a 2018-10-01 | Cambiado el mensaje por defecto [cyberh99]
    * 4a4ad0d 2018-10-01 | Agregados parametros [cyberh99]
    * 24a4f96 2018-10-01 | comenzamos el proyecto [cyberh99]
    * afdd3c3 2018-10-01 | initial commit [cyberh99]
```
**danger**
    Nunca modifiques un _commit_ que ya hayas sincronizado con otro repositorio o
    que hayas recibido de él. Estarías alterando la historia de cambios y provocarías
    problemas de sincronización.

## Moviendo y borrando archivos

### Mover un archivo a otro directorio con git

Para mover archivos usaremos la orden `git mv`:
```
    $ mkdir src
    $ git mv hello.cpp src
    $ git status
    En la rama master
  Cambios a ser confirmados:
    (usa "git reset HEAD <archivo>..." para sacar del área de stage)

  	renombrado:     hello.cpp -> src/hello.cpp

```

### Mover y borrar archivos.

Podíamos haber hecho el paso anterior con la órden del sistema _mv_ y el resultado hubiera sido el mismo. Lo siguiente es a modo de ejemplo y no es necesario que lo ejecutes:

    $ mkdir src
    $ mv hello.cpp src
    $ git add src/hello.cpp
    $ git rm hello.cpp

Y, ahora sí, ya podemos guardar los cambios:
```
    $ git commit -m "Movido hola.php a lib."
    [master 2bfbc0d] Moivido a src
   1 file changed, 0 insertions(+), 0 deletions(-)
   rename hello.cpp => src/hello.cpp (100%)
```
