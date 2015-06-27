# Visualizar15 #

Visualizar15 se presentó esta edición de 2015 con el sugerente y atrevido título de [Datos para el Bien Común](http://medialab-prado.es/article/visualizar15convocatoriacolaboradores)

Para el simposio inicial contamos con la presencia de Greg Bloom [@greggish](https://www.twitter.com/greggish) del proyecto Open Referral, que pretende mejorar la disponibilidad y el intercambio de datos referentes a la asistencia sociosanitaria.

Aunque la realidad de EE.UU. es distinta a la española, creemos que el objetivo del proyecto, la necesidad de contar con herramientas de intercambio y de acceso de información y datos es compartida.

## Open Referral ##

Open Referral se refiere a desarrollar estándares de datos y plataformas abiertas para compartir y facilitar información sobre recursos comunitarios.

Referral se refiere a los servicios de atención socio sanitarios comunitarios en abierto:

<http://openreferral.org/faq/>

Una instalación de lo que ese pretende se puede ver en <https://ohana-web-search-demo.herokuapp.com/>

## Ohana ##

En la cultura hawaiana [Ohana](https://es.wikipedia.org/wiki/Ohana) significa familia y la familia "nunca te abandona ni te olvida, incluyendo parientes de sangre, adoptados o intencionales. Se enfatiza en que la familia están atados juntos y los miembros deben cooperar y recordarse entre sí".

Ohana es el nombre elegido por Open Referral para el desarrollo web y API de intercambio de datos.

## Fundación B. San Martín de Porres ##

Después de una reunión un tanto kamikaze en la mesa de salud del Distrito Centro, conseguimos que la persona que lleva el espacio, que pertenece a la Fundación B. San Martín de Porres, nos pusiera telefónicamente con el encargado de nuevas tecnologías de la fundación, con quien quedamos en reunirnos al día siguiente en sus oficinas.

Al día siguiente nos encontramos con Israel y hablamos de la necesidad de contar con información e intercambio de datos en estos temas que puede ser muy interesante para las entidades y las redes en las que participa:

### Participación Madrid ###

La fundación participa en la [Red FACIAM](http://faciam.org/) (Federación de Asociaciones de Centros para la Integración y Ayuda a Marginados) Fundación Asistencial a Personas Sin Hogar Madrid, en la que participan:

-   Apostólicas Del Corazón De Jesús
-   Asociación Albéniz
-   Asociación Cooperadora Del Centro STA. María De La Paz
-   Cáritas Diocesana De Madrid
-   Fundación Albergue Covadonga
-   Fundación B. San Martín De Porres
-   Programa Integral San Vicente De Paúl
-   San Juan De Dios

### Participación nacional ###

La Fundación San Martín de Porres participa en [fePsh](http://www.fepsh.org/es/), Federación de Entidades de Apoyo a Personas sin Hogar

## Participación internacional

La fundación trabaja en la [red FEANTSA](http://www.feantsa.org/?lang=es), Federación Europea de Organizaciones Nacionales que trabajan con Personas Sin Hogar

## Instalación ##

Seguimos el manual de instalación para un servidor propio:
<https://github.com/codeforamerica/ohana-api/blob/master/INSTALL.md>

### Requisitos previos ###

Antes de correr la API de Ohana necesitas los siguientes paquetes instalados en tu ordenador:

-   Git
-   Ruby2.1

### Problema de database user ###

    psql -h localhost

Devuelve

    psql: FATAL:  database "<user>" does not exist

Entonces he utilizado esta página:
<https://stackoverflow.com/questions/17633422/psql-fatal-database-user-does-not-exist>

Y he ejecutado

    createdb

Entonces ha funcionado la conexión con postgres:

    psql -h localhost

### Problema de extension plpgsql ###

Según nos ha contado Álvaro Santamaría, puede ser por cómo han preparado la base para ejecutarse

Daba un error así:

    could not execute query: ERROR: must be owner of extension plpgsql

Que hemos resuelto siguiendo esta página:
<https://stackoverflow.com/questions/10169203/postgresql-9-1-pg-restore-error-regarding-plpgsql>

Donde explican que `pg_restore` intenta recuperar datos que no nos pertenecen, por lo que sugiere que apliquemos la opción `-n public` para recuperar solo los contenidos del *schema public*

    pg_restore -U username -c -n public -d database_name

### Problema de Unicode ###

Lo resolvimos con este enlace:
<https://stackoverflow.com/questions/16736891/pgerror-error-new-encoding-utf8-is-incompatible>

Resulta que si `template1` está como `UNICODE`, daba error al crear la tabla porque no podía crearla en `UTF-8`

Para ello había que borrar la tabla `template1` y generarla de nuevo con `UTF8`:

Como los templates no se pueden borrar, primero tenemos que decirle a Postgres que es una bbdd ordinaria:

	UPDATE pg_database SET datistemplate = FALSE WHERE datname = 'template1';

Luego la borramos

    DROP DATABASE template1;

Creamos de nuevo `template1` desde `template0` con la nueva codificación:

    CREATE DATABASE template1 WITH TEMPLATE = template0 ENCODING = 'UNICODE';

Modificamos `template1` para decirle que efectivamente es un *template*

    UPDATE pg_database SET datistemplate = TRUE WHERE datname = 'template1';

Nos conectamos a `template1`

    \c template1

Actualizamos

    VACUUM FREEZE;

## Desarrollo ##

Amanda Sorribes

## Pasos adelante ##

### Alianzas ###

-   [ ] Definir relación con Fundación San Martín de Porres
-   [ ] Definir relación con Medialab-Prado
-   [ ] Definir relación con OKFN-España

### Eventos ###

-   [ ] Posible participación en 10ª Conferencia Europea de Investigación en Personas sin Hogar 25 septiembre Dublín.
    -   <https://www.facebook.com/photo.php?fbid=984860481533441>
    -   <http://fr.amiando.com/AEOHJTK.html>
-   [ ] Posible taller de la herramienta con entidades involucradas en infoPsh
-   [ ] Posible taller de la herramienta con más entidades involucradas en la atención sociosanitaria.
-   [ ] Visita de Greg en octubre
