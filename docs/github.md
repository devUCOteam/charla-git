# Github

## ¿Qué es github?

Git es un control de versiones que nos permite trabajar comodamente con múltiples versiones de nuestro código, sin embargo, cuando estamos trabajando en el mundo normal, lo general es estar colaborando con otras personas en el mismo proycto. Esta funcionalidad se realiza de forma más cómoda mediante Github.

**Github es un proyecto independiente a git**

Debemos tener clara la anterior afirmación, puesto que Github simplemente es un repositorio donde podemos encontrar múltiples proyectos cuyo control de versiones es _git_, es una plataforma muy útil de usar para la comunicación entre los diferentes equipos.

Antes de continuar será neceseario obtener nuestra cuenta de [Github](https://www.github.com)

<p align="center"><img src="https://solanch96.files.wordpress.com/2017/06/github.gif?w=500&h=236&crop=1"></p>

## Sincronización de Github y Git

Cuando estamos trabajando con git estamos haciendo repositorios locales, dichos repositorios estan alojados en nuestro equipo aislados del resto del mundo. Al querer trabajar con Github, será necesario sincronizar los cambios que hagamos en local con los cambios en remoto (dentro de nuestro repositorio Github) para ello tenemos dos opciones de trabajar:

* Autenticación mediante HTTP
* Utilizar claves SSH

### Autenticación HTTP

Consiste en introducir nuestro usuario y contraseña en el caso de que queramos realizar operaciones en remoto ( como pueden ser obtener los cambios o subir nuevos cambios), para ello simplemente tenemos que configurar nuestra cuenta de Github.

En el caso de estar trabajando con claves HTTP, para ahorrar tiempo podemos introducir la contraseña en caché de forma que no la requiera cada vez que tengamos que realizar operaciones remotas, para ello es necesario activar la caché mediante el siguiente comando:
```
    git config --global credential.helper 'cache --timeout=3600'
```

### Autenticación con SSH

Github tiene una opción muy útil que consiste en poder sincronizar las claves [ssh](https://en.wikipedia.org/wiki/Ssh) con Github de forma que podamos realizar cambios sin necesidad de introducir nuestro usuario y contraseña de Github.

Si queremos configurar nuestra clave ssh dentro del repositorio de Github, necesitaremos generarla, para ello introduciremos el siguiente comando en la terminal.

```ssh-keygen -b 4096 ```

De forma que por defecto (aunque podemos cambiarlo dentro del asistente de generación) se guardarán los archivos __id_rsa__ y __id_rsa.pub__ dentro de la carpeta __~/.ssh__ de nuestro usuario, pudiendo así copiar el contenido del **último archivo** a la configuración de nuestro perfil de Github:

<p align="center"><img src="https://jdblischak.github.io/2014-09-18-chicago/novice/git/img/github-ssh-keys.png"> </p>

## Convertir un repositorio en remoto

Como se ha mencionado con anterioridad, cuando estamos trabajando con git lo estamos haciendo de forma local, sin embargo cuando queremos sincronizar dichos cambios con Github, simplemente tenemos que indicarle a git a que repositorio de git tiene que enviarlo, para ello tenemos que ejecutar el siguiente comando:

```
    git  remote  add  origin  url
```

**Tenemos que tener en cuenta que la url, será http si estamos trabajando con este método de autenticación o ssh en el caso de tener configurada la clave pública, ambas opciones nos la facilita Github cuando creamos un nuevo repositorio**

## Clonar un repositorio

Al hablar de Github mencionamos que existían múltiples repositorios creados por numerosas personas, todos usando el _git_ como control de versiones. _¿Qué pasa si quiero obtener el repositorio de una persona?_ Esta pregunta muy frecuente podemos solucionarla con la opción _clone_ de git.

Esta opción nos permite obtener le repositorio (con todos los cambios que se han realizado) sobre cualquier proyecto **público** de Github. Un aspecto a tener en cuenta es que esta operación es que si constamos como colaborador en dicho proyecto podemos realizar todas las operaciones (puesto que se obtienen la configuración del mismo)

Para clonar un repositorio cualquiera simplemente tenemos que ejecutar:

```
    git clone url
```

## Ver repostorios remotos

Cuando estamos trabajando en un proyecto, generalmente subimos todos los cambios a un solo repositorio de Github, sin embargo, es posible que estemos trabajando en diferentes repositorios por diversos motivos. Uno de estos motivos puede ser que estemos obteniendo el código de un repositorio determinado y subiendo nuestros cambios a uno propio.

Para poder llevar el control de los repositorio remotos asociados a nuestro repositorio local, utilizamos el siguiente comando:

```
    git remote -v
```

Si queremos eliminar cualquier repositorio remoto de nuestro repositorio local simplemente podemos borrar el reositiorio haciendo uso del comando

```
  git remote rm origin
```
<p align="center"><img src="http://www.cantabriatic.com/wp-content/uploads/2015/07/Github.png"/></p>

## Obteniendo y aplicando datos en un repositorio remoto

Cuando hemos terminado de trabajar en nuestro repositorio local, realizados todos nuestros _commits_ y agregados los archivos pertinentes, pasaremos a aplicar los cambios en el repositorio remoto, para ello usaremos el siguiente comando:

```
  git push -u origin master
```
Teniendo en cuenta que _origin_ hará referencia al repositorio remoto con el que estamos trabajando, mientras que _master_ hace referencia a la rama en la que estamos trabajando en nuestro repositorio local.

Para **obtener los cambios** realizados por nosotros (u otras personas) en el repositorio remoto utilizamos:

```
  git pull origin master
```

Haciendo referencia de nuevo a _master_ como la rama en la que queremos obtener los cambios.

**Debemos tener en cuenta que antes de realizar cualquier _git push_ es recomendable hacer un _git pull_ para evitar posibles errores a la hora de subir los cambios al repositorio remoto**

## Trabajando con ramas remotas

Las ramas (de lo cual se ha hablado con anterioridad), es un elemento de _git_ que puede ser trasladado a _Github_ sin ningún problema. Esto permite a los usuarios trabajar en remoto sin problema alguno. Podemos ver las ramas remotas de nuestro repositorio con:

```
  git branch -r
```

### Operaciones con ramas remotas

Las mismas operaciones (crear,modificar,eliminar) que podíamos hacer con las ramas locales podrémos hacerlas con las remotas sin nigún tipo de problema, simplemente tendrémos que haber creado las ramas remotas previamente y aplicar los diferentes cambios que hagamos en local.

#### Crear y modficiar ramas remota
Para crear una rama remota, tenemos que seguir los siguientes pasos:

1. Crear una rama en local
 * _git branch nueva rama_
2. Modificar la rama local
3. Hacer commit
4. Crear la rama remota
  * _git push origin rama1_


De nuevo hacemos referencia a _origin_ como la rama remota y _rama1_ como la *rama* local en la que estamos trabajando y queremos sincronizar con el repositorio remoto

Si llevamos cierto tiempo trabajando con una rama en nuestro repositorio local, podemos copiar dicha rama al repositorio remoto usando:

```
  git checkout -b rama_local rama_remota
```
#### Eliminar ramas remotas
Al igual podemos eliminar una rama del repositorio remoto:

```
  git push origin --delete rama_remota
```
#### Obtener datos de ramas remotas

Una vez que tenemos contenido en ramas locales,sería de utilidad poder traerlo a nuestro repositorio local de forma que podamos explorar y probar las nuevas funcionalidades implementadas por cualquier compañero (como ejemplo personal). Para poder obtener *todos los cambios remotos* utilizamos el comando:
```
git fetch
```
Si estamos trabajando en una rama local y queremos fusionarla con las ramas remotas tendrémos que usar el comando
```
  git merge origin/master
```
De forma que estamos sincronizando completamente nuestr rama local _master_ con nuestra rama remota (_master en el repositorio remoto_), en el caso de que tengamos problemas de sincronización Github tiene los denominados _pull merge requests_ que quedarán registrados como un commit cuando un usuario decide fusionar su rama local con la remota.

Cuando queremos obtener los cambios *en la rama en la que estamos trabajando* simplemente tenemos que recurrir al comando visto con anterioridad:

```
  git pull
```


## Trabajo dinámico con repositorio remotos

<p align="center"><img src="https://user-images.githubusercontent.com/9693472/41194624-35b0dc8c-6c3c-11e8-9223-fa1a6f3cb887.png"/></p>

Cuando estamos explorando repositorios remotos, podemos encontrar algún proyecto que te guste modificar o un trabajo que quieras comprobar como funciona. En este caso Github implementa lo denomidado _fork_ que consiste en el clonado de un repositorio remoto (creado por otra persona) a un repositorio remoto (creado por nosotros) de forma que podamos modificar libremente **una copia exacta del repositorio ajeno**

Una vez que hemos copiado el remoto ajeno a uno personal podemos trabajar libremente como mencionamos anteriormente.
