##Def [ A | B | C] : Opcional A o B o C
##Def { A | B | C} : Obligatorio A o B o C
##Def str : "."

***
Descripcion: Listar los procesos cuyos nombres son filtrados por {str} con opciones

a : Quitar la restricción "only yourself" en el BSD-style

u : Mostrar en un formato orientado al usuario

x : Quitar la restricción "must have a tty" en el BSD-style, junto a la opción "a" se enumera todo los procesos.

-i : No sensible a mayusculas no minusculas.
#ps aux | grep -i {str}
***


***
Descripción: Mostrar todos métodos de cifrado de nivel 2 y configurar el security level a 2.

-s :

-v :
#openssl ciphers -s -v 'ALL:@SECLEVEL=2'
***

***
Descripción: Generar un backup MariaDB/MySQL de forma 'segura' cifrado con aes-256 y comprimido.

--add-drop-database :

--single-transaction :

-pbkdf2 : Usa el algoritmo PBKDF2 (Password-Based Key Derivation Function 2)

-iter : Permite el uso de -pbkdf2 para derivar la clave, los valores altos aumentan el tiempo necesario para aplicar fuerza bruta al archivo resultante. 

-k {password} : Contraseña del backup
## El más básico y funcionable
#mysqldump -u {user} -p$MYSQL_PASSWORD_USER {database} | gzip | openssl enc -aes-256-cbc -md sha512 -pbkdf2 -iter 100000 -k $PASSWORD_BACKUP > {ruta_de_backups}/backup-[fecha].xb.enc
## Sin --add-drop-database
#mysqldump -u {user} -p$MYSQL_PASSWORD_USER --databases --single-transaction --triggers --routines {database} | gzip | openssl enc -aes-256-cbc -md sha512 -pbkdf2 -iter 100000 -k $PASSWORD_BACKUP > {ruta_de_backups}/backup-[fecha].xb.enc
## El más completo que he usado
#mysqldump -u {user} -p$MYSQL_PASSWORD_USER --databases --add-drop-database --single-transaction --triggers --routines {database} | gzip | openssl enc -aes-256-cbc -md sha512 -pbkdf2 -iter 100000 -k $PASSWORD_BACKUP > {ruta_de_backups}/backup-[fecha].xb.enc
## Para no colocar el usuario y contraseña de mysqldump, se puede colocar una sección [mysqldump] con user=user y password=password en el my.cnf.
***

***
Descripción: Restaurar un backup MariaDB/MySQL de forma 'segura'.

-d : Extracción del backup
#openssl enc -aes-256-cbc -md sha512 -pbkdf2 -iter 100000 -k $PASSWORD_BACKUP -d -in {ruta_de_backups}/backup-[fecha].xb.enc | gzip -d | mysql -u {user} -p$MYSQL_PASSWORD_USER
***

***
Descripción: Mover directorios completos de forma recursiva y forzada (sobreescribiendo). Para esto primero copio el directorio y luego lo elimino.

-r : Recursiveando

-f : Forzando
#cp -rf {nombre-del-directorio}
#rm -rf {nombre-del-directorio}
***

***
Descripción: Conectar por ssh a un servidor remoto usando un archivo .pem en mi carpeta security (only read by root)

-i : 
#sudo ssh -i ~/security/clave.pem user@{ip | dominio}
***

***
Descripcion: Convertir a PDF desde un cuaderno de Jupyter (.ipynb)
#jupyter nbconvert --to pdf {cuaderno-jupyter}
***

***
Descripción: Ver dirección ip (o direcciones) de un dominio
#nslookup {dominio} | grep -i "address" | tail -1
***

***
Descripción: Configurar composer en un proyecto de PHP e instalar algún módulo de ejemplo
#cd {ruta-proyecto}
#php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
#php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
#php composer-setup.php
#php -r "unlink('composer-setup.php');"
## Instalando módulos de ejemplo
#php composer.phar require cboden/ratchet
#php composer.phar require google/cloud-dialogflow
***

***
Descripción: Recargar, reiniciar y ver el estado de un servicio
#systemctl {reload | restart | status} {nombre-servicio}
***

***
Descripción: Establecer entornos virtuales en python

venv : Creación de entornos virtuales
#cd {ruta-proyecto}
#python3 -m venv {nombre-entorno-virtual}
#pip3 list
***

***
Descripción: Listar los puertos ocupados activos escuchadores y sus respectivas aplicaciones ejecutandose

-t :

-u :

-n :
#sudo netstat -tunlp | grep LISTEN
***

***
Descripción: Clonar un repositorio remoto y establecer un repositorio remoto
#git clone https:user:$GITHUB_TOKEN_USER@github.com/{repositorio-git}
#git remote add {nombre-remoto} https:user:$GITHUB_TOKEN_USER@github.com/{repositorio-git}
## Ver, eliminar y cambiar nombre de los repositorios remotos
#git remote -v 
#git remote delete {nombre-remoto}
#git remote rename {nombre-remoto} {nuevo-nombre-remoto}
## Colocar un dominio remoto
#git remote set-url {nombre-remoto} git@{repositorio-remoto}:{user}/{nombre-repositorio}.git
***


***
Descripción: Ver la diferencia entre dos commits usando el HEAD

HEAD~n : Commit n-veces antes del HEAD.
#git diff HEAD~{n} HEAD~{m}
## Ir al commit HEAD~n pero de forma 'suave'
#git checkout --soft HEAD~{n}
***

***
Descripción: Descartar los últimos n commits (usarlo con precaución), descargar cambios con unrelated-histories, subir forzadamente cambios (sirve cuando hice reset --hard a un commit específico y quiero que se aplique lo mismo en el remoto) y cambiar el nombre a la rama actual.
#git reset --hard HEAD~{n}
#git pull {nombre-remoto} {rama} --allow-unrelated-histories
#git push -f {nombre-remoto} {rama}
#git branch -M nuevo-nombre-de-la-rama-actual
***
