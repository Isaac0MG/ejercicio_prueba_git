# Práctica guiada de Git

## 1. Primeros pasos

El primer paso en esta práctica guiada es crear el directorio que usaremos. Para crear un directorio se usa el comando `mkdir nombre_directorio`. En mi caso lo llamé como en el ejercicio **prueba_git**.

```
mkdir prueba_git
```

Después nos movemos al directorio creado con el comando `cd nombre_directorio`.

```
cd prueba_git
```

Lo siguiente que tendremos que hacer es crear 2 archivos, uno .txt y otro .sh, usando el comando nano. Yo soy más de touch, pero como nos piden insertar datos dentro de los archivos es mejor usar nano directamente.

```
nano texto.txt
```
```
nano script.sh
```

Dentro del editor de nano tendremos que presionar `Control + O` para guardar el contenido y también nos pedirá el nombre del archivo y luego para salir una vez guardado `Control + X`. 

> Por cierto el contenido del script.sh de la práctica guiada está desordenado, porque primero dices que el listado se ha completado y luego listas el directorio (página 11).

## 2. Inicializar el directorio y subir archivos

Para inicializar el directorio y crear el directorio .git tenemos que ejecutar el comando `git init`.

Podemos cambiar el nombre de la rama principal con el comando `git branch -m nombre`.

```
git branch -m master
```

Ahora mismo tenemos los dos archivos que están en el estado *untracked*, es decir que no forman parte del repositorio. Para añadir un archivo y que esté en el estado *tracked* y Git lo tenga en cuenta para añadirlo al repositorio tenemos que ejecutar el comando `git add nombreArchivo`. 

```
git add script.sh
```

Esto hará que Git detecte si hay cambios en el archivo desde la última versión pero aún no está subido al repositorio. Para subir el archivo al repositorio se ejecuta el comando `git commit -m "mensaje"`.

```
git commit -m "Mi primer archivo"
```

Esto subirá el archivo al repositorio local con el mensaje que le hemos dado. Otras personas aún no pueden clonar el repositorio y tener el archivo en sus ordenadores porque faltaría hacer el push.

Hacemos lo mismo con el **texto.txt** y ahora al ejecutar `git status` nos mostrará que el árbol de archivos está limpio y no hay nada untracked o pendiente de subir.

## 3. Modificar archivos

Si modificamos un archivo, por ejemplo **texto.txt**, y ejecutamos `git status` veremos que aparece **texto.txt** como modificado y en rojo, es decir, pendiente de subir la nueva versión del archivo al repositorio.

Lo interesante de git es que puedes preparar archivos para subirlos con el `git add nombreArchivo` pero no hacer el commit, porque a lo mejor te interesa no subir esa versión del archivo porque dentro de poco tendrás otra versión y aprovechas y subes la nueva y te ahorras un commit. 

Para ejemplificarlo lo haremos con **texto.txt**. Actualmente el archivo está modified pero aún no he hecho el add. Si ejecuto el comando `git add texto.txt` y lo añado para posteriormente hacerle el commit puedo, por ejemplo, revisar el archivo, ver si está todo correcto y hacer el commit. Pero imagina que me fijo en que en el archivo tengo una errata y la corrijo, es decir, modifico de nuevo el archivo **texto.txt**. 

Lo que sucede es que ahora tengo *2 versiones* del archivo, el que está en el add y el que está en modified. Ambos archivos son el mismo archivo (**archivo.txt**) pero el contenido es distinto. Por lo tanto, Git nos ofrece una gestión de versiones de los archivos bastante sólida y fácil de entender para gestionar nuestros commits de manera eficiente.

>Si queremos subir una versión modificada de un archivo al repositorio nos podemos saltar el paso de añadirlo y luego hacer el commit. El comando para hacer esto es `git commit -a -m "mensaje"`. Esto solo se puede hacer con archivos que existan previamente en el repositorio, si es un archivo nuevo el add se tiene que hacer por separado.

## 4. Información - Git Show y Git Log

Tenemos dos comandos que nos permiten ver información de commits anteriores. El primero es `git show` que nos muestra información sobre el último commit realizado. Con la salida de este comando podemos ver lo que se ha añadido (en verde) y lo que se ha eliminado (en rojo). Así como información básica sobre el autor, el mensaje del commit, la fecha y hora, etc.

Por otra parte tenemos el comando `git log` que nos muestra información de todos los commits, con información mucho más breve, no podemos ver los cambios de cada commit como con `git show` pero podemos visualizar el mensaje del commit, la fecha y hora, etc.

## 5. Diferencias - Git Diff

El comando `git diff` nos permite ver las diferencias entre un archivo modificado y la versión que existe en el repositorio. Por ejemplo, modifico **texto.txt**, le añado una línea más que pone seis y le quito la línea que pone tres. Al ejecutar el comando `git diff` puedo ver las líneas que se han eliminado y añadido o si se ha cambiado el nombre al archivo. 

## 6. Ignorar archivos - .gitignore

Existen situaciones en las que nos interesa que ciertos archivos no se suban al repositorio, como por ejemplo, los archivos .class de Java o las carpetas /bin y /obj en C#. Para ello necesitamos un archivo llamado `.gitignore`, e insertaremos el siguiente contenido en él.

```
# ignora los archivos terminados en .class
*.class
# ignora los archivos terminados en ~
*~
# pero no importante~, aun cuando había ignorado los archivos
terminados en ~ en la línea anterior
!importante~
```

 Por ejemplo creo un archivo .java muy sencillo, que lo llamo Hola.java e imprime por pantalla la frase **Hello to the Java World**, lo compilo con `javac Hola.java` y me genera el .class, es decir, el archivo compilado de la clase. Ahora crearemos dos archivos que terminen en virgulilla, a uno lo llamaremos odioJavaScript~ y a otro importante~ para ver todos los casos posibles de nuestro .gitignore.

Si ejecutamos `git status` veremos solo el archivo .java e importante~ y comprobaremos que el archivo `.gitignore` está funcionando de manera correcta. 

## 7. Tags y gestión de versiones - Git Tag y Git Checkout

Si queremos ver los datos de un commit específico que se realizó en el pasado tenemos el comando `git show idCommit`. Los ids de los commits podemos verlos con el comando `git log`, aparecerá a la derecha de la palabra commit y está en la primera línea. Como podemos observar, el número identificativo es bastante largo y puede ser algo tedioso si se quiere compartir con otras personas. Es aquí donde entra `git tag`. Este comando nos permite ponerle un tag tanto a la versión actual como a versiones anteriores de forma que es más fácil reconocer distintas versiones además de tenerlo todo más organizado.

>Si queremos ver el historial de commits con información más resumida podemos ejecutar el comando `git log --oneline`, que nos mostrará los commits realizados con su tag y mensaje del commit.

Agregar un tag a un commit nos permite simplificar el uso del comando `git show idCommit` pudiendo usar el tag en vez del id del commit `git show tag`, lo cual nos facilita mucho la vida.

```
git show aadb7c6993c6c011dc17d85f8f3e8c708353f1d7

git show v0.3
```


Por último tenemos el comando `git checkout idCommit` o `git checkout tag` para ir a una versión del proyecto anterior. Actualmente no es recomendable hacer esto porque estamos en la rama principal y podemos fastidiar el repositorio. Para ello es mejor crear una rama nueva y trabajar desde ella para que los cambios no afecten de manera directa a la rama principal.

```
git checkout aadb7c6993c6c011dc17d85f8f3e8c708353f1d7

git checkout v0.3
```

>Para volver a la rama principal sería con `git checkout nombreRama`, que en mi caso es master. Este comando también servirá para cambiar entre ramas en un futuro.

## 8. Ramas

Las ramas son una herramienta que permite realizar pruebas sin afectar directamente a la rama principal. Una ventaja de ellas es que se pueden crear de manera local sin afectar al proyecto que existe en remoto. Para crear una rama es con el comando `git branch nombreRama`.

```
git branch Prueba
```

 Si ejecutamos ahora el comando `git log --oneline` veremos que nos aparecen dos ramas en la versión actual del repositorio, la principal y la que acabamos de crear. La rama que se está usando tiene la palabra **HEAD** apuntando al nombre de la rama. Otra forma de visualizarlo es con `git branch` para ver las ramas existentes y en verde la que se está usando.

 Para cambiar entre ramas se usa el comando `git switch nombreRama` o `git checkout nombreRama` como comentamos anteriormente. Si ahora creamos una rama nueva llamada Prueba, nos movemos a ella con `git switch Prueba`, modificamos el archivo java y le añadimos cualquier tontería y hacemos un `git commit -a -m "Llegan los no sé que a Java"` se hará el commit pero en la rama de Prueba. 
 
 Si volvemos a la rama principal con `git checkout master` (o el nombre que le tengas a la rama principal), podemos ver que el archivo .java no ha cambiado su contenido, para poder visualizar dichos cambios tendríamos que hacer un `git merge Prueba`, y por suerte, al ser un ejemplo simple no nos dará ningún tipo de error o conflicto y nos saldrá por pantalla que el archivo Hola.java se ha modificado correctamente.

 ## 9. Eliminar archivos

Para eliminar archivos ejecutamos el comando `git rm nombreArchivo` y posteriormente hacer un `git commit -m "mensaje"`.

## 10. Repositorios remotos - GitHub

GitHub es un proveedor de repositorios Git. Hasta ahora hemos estado trabajando con un repositorio local, si quisiéramos subir el repositorio a GitHub, tenemos que seguir los siguientes pasos:

1. Crear el proyecto en tu cuenta de GitHub.
2. Ejecutar el comando `git remote add origin enlaceAlRepositorio`
3. Ejecutar el comando `git push -u origin nombreRamaPrincipal`. Esto sirve para subir la rama main o master, depende de cómo la hayas llamado, al proyecto de GitHub.

 >Si quisieras subir otras ramas ejecutaríamos el comando `git push -u origin nombreRama`.

Si queremos clonar un repositorio de GitHub, ejecutamos el comando `git clone enlaceGithub`. Esto descargará el repositorio en el directorio en el que te encuentres. Para editar el repositorio y subir o modificar archivos necesitarías permisos sobre el repositorio.