#Aspectos básicos de Git#

## Instalación ##

### Instalando en Linux ###

Si quieres instalar Git en Linux a través de un instalador binario, en general puedes hacerlo a través de la herramienta básica de gestión de paquetes que trae tu distribución. Si estás en Fedora, puedes usar yum:

```
     yum install git-core
```
O si estás en una distribución basada en Debian como Ubuntu, prueba con apt-get:
```
     apt-get install git
```
### Instalando en Windows ###

Instalar Git en Windows es muy fácil. El proyecto msysGit tiene uno de los procesos de instalación más sencillos. Simplemente descarga el archivo exe del instalador desde la página de GitHub, y ejecútalo:

[http://msysgit.github.com/](http://msysgit.github.com/)

Una vez instalado, tendrás tanto la versión de línea de comandos (incluido un cliente SSH que nos será útil más adelante) como la interfaz gráfica de usuario estándar. Se recomienda no modificar las opciones que trae por defecto el instalador.

### Instalando en MacOS

En MacOS se recomienda tener instalada la herramienta [homebrew](https://brew.sh/). Después, es tan fácil como ejecutar:
```
     brew install git
```

## Configuración del entorno git ##



Git tiene ciertos valores por defecto que nos permite comenzar a trabajar después  de la instalación, sin embargo, cuando estamos trabajando en conjunto con otras personas es necesario identificarnos dentro de git, de forma que podamos identificar los commits que estemos realizado. Para ello modificaremos los siguientes parámetros.

### Configuración de usuario

```
    git config --global user.name "Nombre de usuario"
```

```
    git config --global user.email "correo_usuario@gmail.com"
```

Con estos sencillos comandos tenemos configurados tanto nuestro nombre de usuario como nuestro correo electrónico de forma que al subir nuestros cambios al repositorio remoto aparezca nuestro nombre en él.

### Configuración de un editor por defecto

Cuando estamos trabajando con git, es posible que obtengamos dieferentes mensajes de error, dichos mensajes de error serán mostrados en un editor de texto (que por defecto es _vim_), podemos cambiar este editor con el siguiente comando.

```
    git config --global core.editor editorFavorito
```

Algunos de los editores que podemos poner son:
* emacs
* vim
* nano

### Configuración de commits por defecto

En un entorno de trabajo grande, estaremos haciendo una gran cantidad de commits, es posible tener preconfigurado una plantilla para nuestro commit, para ello usaremos:

```
    git config --global commit.template ~/.gitmessage.txt
```

Siendo _~/.gitmessage.txt_ el archivo que contiene la plantilla
