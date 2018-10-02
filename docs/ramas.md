# Ramas
## ¿Qué son las ramas?
Es la forma para separar la lı́nea actual de desarrollo con respecto a la principal. Normalmente representan versiones del software que posteriormente son integradas a la lı́nea principal.

![branches](img/branches.png)  

## ¿Cómo me puedo aprovechar de ellas?
Cuando vamos a trabajar en una nueva funcionalidad, es conveniente hacerlo en una nueva rama, para no modificar la rama principal y dejarla inestable.

### preparacion del ejemplo.
Para poder llevar a acabo el ejemplo, haremos, (en un directorio vacío) lo siguiente:  

	git init && touch .gitignore && git add * && git commit -m "initial commit"

### Vamos a crear una rama.

!!! info
	Si usamos git branch sin ningún argumento, nos devolverá la lista de ramas disponibles.

Una vez hemos comprobado que no existe una rama 'test' podemos proceder a su creación.

	$ git chechout -b hola
	Switched to branch 'hola'

<!-- TODO: match the previous file and modify it -->

### Modificamos la nueva rama.
para esto añadiremos un simple hola mundo en SH, creamos un documento nuevo, llamado "HelloWorld.sh" en el cual escribiremos:

	#!/bin/sh
	printf "hello, world\n"

### Salvamos las modificaciones a la rama.
	$ git add HelloWorld.sh
	$ git commit -m "Añadido script hola mundo"
	[hola daae9ca] Añadido script hola mundo
	1 file changed, 3 insertions(+)
	create mode 100644 HelloWorld.sh

### Merge al master.
Una vez provado el nuevo cambio, vemos que no da errores, entonces decidimos implementarlo al master, para ello, nos iremos al master:

	$ git checkout master

y haremos mergin desde la rama hola, a la rama master.

	$ git merge hola
	Updating daae9ca..787590a
	Fast-forward
	HelloWorld.sh | 2 +-
	1 file changed, 1 insertion(+), 1 deletion(-)

### mergin

A continuación forzaremos una situacion de conflicto entre ramas, para ello modificaremos el script en las dos ramas a la vez.  

en la rama master, editaremos el script añadiendo una exclamación al final del printf.

	#!/bin/sh
	printf "hello, world!\n"

A continuacion haremos un commit de lo modificado

	$ git commit -am "added \!"


en la rama hola, añadiremos una interrogación, para ello, nos cambiamos a la rama hola:

	$ git checkout hola

y editamos el script:

	#!/bin/sh
	printf "hello, world?\n"

A continuacion haremos un commit de lo modificado

	$ git commit -am "added ?"


ahora volveremos a la rama master e intentaremos hacer mergin.

	$ git merge hola
	Auto-merging HelloWorld.sh
	CONFLICT (content): Merge conflict in HelloWorld.sh
	Automatic merge failed; fix conflicts and then commit the result.

Esto indica que el mergin fallo en le archivo HelloWorld.sh y que debemos arreglarlo a mano, para ello, abriremos el script de nuevo, esta vez observaremos que unas marcas se han añadido:

	#!/bin/sh
	<<<<<<< HEAD
	printf "hello, world!\n"
	=======
	printf "hello, world?\n"
	>>>>>>> hola


Estas marcas lo que indican el trozo de código que ha entrado en conflicto, para arreglarlo eliminaremos uno u otro, y procederemos a hacer commit de lo realizado, para este problema, escogeremos el contenido de la rama hola.

	#!/bin/sh
	printf "hello, world?\n"


	$ git commit -am "fixed mergin problem with branch \"hola\""
