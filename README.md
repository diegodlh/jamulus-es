# jamulus-es-wiki

**Octubre 2020**: Nos mudamos a https://jamulus.io/es/

Este repositorio aloja la versión en español de la wiki de Jamulus. Ésta es fundamentalmente una traducción de la [documentación oficial en inglés](https://github.com/corrados/jamulus/wiki), gentilmente contribuida por el usuario @ignotus666, quien la mantiene junto a otrxs colaboradrxs.

Esto fue discutido [acá](https://github.com/corrados/jamulus/issues/77#issuecomment-687216471).

## Procedimiento
Para iniciar esta wiki, se siguieron estos pasos:
1. Se creó una Home vacía en la wiki usando la interfaz web, para habilitar el repositorio especial `jamulus-es.wiki.git`.
2. Se deshabilitó la opción "Restrict editing to collaborators only" en la configuración del repositorio, para permitir que cualquier usuarix de Github pueda modificar la wiki (como ocurre con la versión en inglés).

Luego, ejecutaron los siguientes comandos en bash:
```
git clone https://github.com/corrados/jamulus.wiki.git
mv jamulus.wiki jamulus-es.wiki
cd jamulus-es.wiki
git remote set-url origin https://github.com/diegodlh/jamulus-es.wiki.git
git pull --allow-unrelated-histories
```
En este punto fue necesario solucionar los conflictos entre la Home vacía creada en el punto 1 y la Home original.

A continuación, se iteró a través de los archivos de la wiki para (i) agregar un título (porque la wiki Github toma como título el nombre del archivo, que se prefirió dejar como en la versión original en inglés, para facilitar el mantenimiento), (ii) agregar un selector de idioma, y (iii) reemplazar el contenido por una leyenda de "Traducción pendiente..."
```
# primero, eliminar líneas extra al comienzo de uno de los archivos
tail --lines=+9 Server-FreeBSD.md > temp && mv temp Server-FreeBSD.md

# todos los archivos, excepto los que comienzan con _
for file in `ls -I "_*"`
do
  # eliminar extensión .md
  filename=${file%.md}
  # reemplazar uno o más guiones por espacio
	title=${filename//+(-)/ }
	# construir encabezado con título y selector de idioma
	heading="# $title\n( [en](https://github.com/corrados/jamulus/wiki/$filename) | es )\n\n"
	# obtener la primera línea del archivo original
	firstline=`head -1 $file`
	# generar el placeholder para la traducción
	echo -e $heading$firstline > temp && mv temp $file
	# agregar la leyenda de traducción pendiente
	echo -e "\nTraducción pendiente..." >> $file
done
```
Finalmente, se hizo commit y push de los cambios.
