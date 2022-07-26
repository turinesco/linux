## Descripción breve de la utilidad del comando
    [comment] Comentario extra
    [command] El comando completo
    [options]
    opcion1 : Descripción
    opcion2 : Descripción
!!! note: 
Reemplazar (& gt;) mayor qué y (& lt;) menor qué.
----

## Listar los procesos cuyos nombres son filtrados por {str} con opciones 
    [command] ps aux | grep -i {str}
    [options]
    a : Quitar la restricción "only yourself" en el BSD-style.
    u : Mostrar en un formato orientado al usuario.
    x : Quitar la restricción "must have a tty" en el BSD-style, junto a la opción "a" se enumera todos los procesos.
    -i : No sensible a mayusculas ni minusculas.
----

## Mostrar todos métodos de cifrado de nivel 2 y configurar el security level a 2.
    [command] openssl ciphers -s -v 'ALL:@SECLEVEL=2'
    [options]
    -s : 
    -v : 
----

## Generar un backup MariaDB/MySQL de forma 'segura' cifrado con aes-256 y comprimido.
    [comment] El más básico y funcionable
    [command] mysqldump -u {user} -p$MYSQL_PASSWORD_USER {database} | gzip | openssl enc -aes-256-cbc -md sha512 -pbkdf2 -iter 100000 -k $PASSWORD_BACKUP > {ruta_de_backups}/backup-[fecha].xb.enc
    [options]
    -pbkdf2 : 
    -iter : 
    -k : 

    [comment] El más completo que he usado con --add-drop-database
    [command] mysqldump -u {user} -p$MYSQL_PASSWORD_USER --databases --add-drop-database --single-transaction --triggers --routines {database} | gzip | openssl enc -aes-256-cbc -md sha512 -pbkdf2 -iter 100000 -k $PASSWORD_BACKUP > {ruta_de_backups}/backup-[fecha].xb.enc
    [options]
    --add-drop-database :
!!! note: Para no colocar el usuario y contraseña de mysqldump, se puede colocar una sección [mysqldump] con user=user y password=password en el my.cnf.
----


## Restaurar un backup MariaDB/MySQL de forma 'segura'.
    [command] openssl enc -aes-256-cbc -md sha512 -pbkdf2 -iter 100000 -k $PASSWORD_BACKUP -d -in {ruta_de_backups}/backup-[fecha].xb.enc | gzip -d | mysql -u {user} -p$MYSQL_PASSWORD_USER
    [options]
    -d : Extracción del comprimido del backup.
----


## Mover directorios de forma recursiva y forzada (sobreescribiendo). Copio el directorio y luego lo elimino.
    [command] cp -rf {nombre-del-directorio}
    [command] rm -rf {nombre-del-directorio}
    [options]
    -r : Recursiveando
    -f : Forzando
----


## Conectar por ssh a un servidor remoto usando un archivo .pem en mi carpeta security (read by root)
    [command] sudo ssh -i ~/security/clave.pem user@{ip | dominio}
    [options]
    -i : 
----


## Convertir a PDF desde un cuaderno de Jupyter (.ipynb)
    [command] jupyter nbconvert --to pdf {cuaderno-jupyter}
----


## Ver dirección ip (o direcciones) de un dominio
    [command] nslookup {dominio} | grep -i "address" | tail -1
----


## Configurar composer en un proyecto de PHP e instalar algún módulo de ejemplo
    [command] cd {ruta-proyecto}
    [command] php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
    [command] php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
    [command] php composer-setup.php
    [command] php -r "unlink('composer-setup.php');"
    [options]
    -r : 

    [comment] Instalando módulos de ejemplo
    [command] php composer.phar require cboden/ratchet
    [command] php composer.phar require google/cloud-dialogflow
----


## Recargar, reiniciar y ver el estado de un servicio
    [commad] systemctl {reload | restart | status} {nombre-servicio}
----


## Establecer entornos virtuales en python
    [command] cd {ruta-proyecto}
    [command] python3 -m venv {nombre-entorno-virtual}
    [command] pip3 list
    [options]
    venv : Creación de entornos virtuales

    [comment] Activar el entorno creado
    [command] {nombre-entorno-virtual}/bin/activate
    
    [comment] Desactivar el entorno
    [command] deactivate 
----


## Listar los puertos ocupados activos escuchadores y sus respectivas aplicaciones ejecutandose
    [command] sudo netstat -tunlp | grep LISTEN
    [options]
    -t :
    -u :
    -n :
----


## Clonar un repositorio remoto y establecer un repositorio remoto
    [command] git clone https://user:$GITHUB_TOKEN_USER@github.com/{repositorio-git}
    [command] git remote add {nombre-remoto} https:user:$GITHUB_TOKEN_USER@github.com/{repositorio-git}

    [comment] Ver, eliminar y cambiar nombre de los repositorios remotos
    [command] git remote -v 
    [command] git remote delete {nombre-remoto}
    [command] git remote rename {nombre-remoto} {nuevo-nombre-remoto}
    [options]
    -v :

    [comment] Colocar un dominio remoto
    [command] git remote set-url {nombre-remoto} git@{repositorio-remoto}:{user}/{nombre-repositorio}.git
----


## Ver la diferencia entre dos commits usando el HEAD
    [comment] HEAD~n : Commit n-veces antes del HEAD.
    [command] git diff HEAD~{n} HEAD~{m}
    
    [comment] Ir al commit HEAD~n pero de forma 'suave'
    [command] git checkout --soft HEAD~{n}
----


## Solucionar algunos incovenientes con Git
    [comment] Descartar los últimos n commits (usarlo con precaución)
    [command] git reset --hard HEAD~{n}
    
    [comment] Descargar cambios con unrelated-histories
    [command] git pull {nombre-remoto} {rama} --allow-unrelated-histories
   
    [comment] Subir forzadamente cambios (sirve cuando hice reset --hard a un commit específico y quiero que se aplique lo mismo en el remoto)
    [command] git push -f {nombre-remoto} {rama}
    
    [comment] Cambiar el nombre a la rama actual.
    [command] git branch -M nuevo-nombre-de-la-rama-actual
----



!!! note:
Def [ A | B | C] : Opcional A o B o C
Def { A | B | C} : Obligatorio A o B o C
Def str : "."





| | |
| -- | -- |
| !!! hint: autor | **felipeturing** |
