# Ramas o Branches

Su utilización está muy extendida ya que permite un mejor control del código, especialmente cuando su evolución se produce en paralelo.

Uno de sus usos más comunes es representar las diferentes versiones del software.

### Ver listado de ramas
`git branch`

### Crear una rama
`git branch nombre_rama`

 Git es consciente de la rama en la que nos encontramos en cada momento debido a la existencia de un apuntador especial denominado HEAD que apunta a la rama en la que nos encontramos.

### Cambiar a una rama
`git checkout nombre_rama`

 A través de este comando, el apuntador HEAD mencionado anteriormente pasa a apuntar a la rama 'nombre_rama'.

### Comparar ramas
`git diff nombre_rama1..nombre_rama2`

>Para listar los archivos que han cambiado se puede utilizar `git diff --name-status nombre_rama1..nombre_rama2`

### Renombrar ramas
`git branch -m nombre_antiguo nombre_nuevo`

### Eliminar ramas
~~~
git branch -d nombre_rama
git branch -D nombre_rama
~~~
>El primero de estos 2 comandos dará un error si la rama no es idéntica a la rama master. Con el segundo forzamos el borrado.

### Ver si una rama es igual que la rama en la que nos encontramos
`git branch --merged`

### Integrar una rama a la acutal
`git branch merge rama_a_integrar`

Con la ejecución de este comando, suelen producirse conflictos con los archivos de ambas ramas. Estos conflictos suelen resolverse manualmente realizando commmits con el mensaje "merge"

## Cambios temporales

### Almacenar cambios temporales
`git stash save "Mensaje"`

### Listar cambios
`git stash --list`

### Ver contenido de un cambio
`git stash show -p nombre_stash`

### Eliminar un cambio
`git stash drop nombre_stash`

### Aplicar cambio del stash
~~~
git stash apply nombre_stash
git stash pop nombre nombre_stash
~~~
