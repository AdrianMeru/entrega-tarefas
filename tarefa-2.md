# Tarefa 2

# INSTALAR MARIADB EN LINUX

Lo unico que tenemos que hacer es usar la pagina oficial de MAriaDB para ver los comandos necesarios.
Lo primeiro que tenemos que hacer es cargar el repositorio con los siguientes comandos:
```sql
sudo apt-get install software-properties-common
sudo apt-key adv --fetch-keys 'https://mariadb.org/mariadb_release_signing_key.asc'
sudo add-apt-repository 'deb [arch=amd64] http://ftp.yz.yamagata-u.ac.jp/pub/dbms/mariadb/repo/10.4/ubuntu eoan main'
```

Despues de cargarlo toca pasar a la instalacion:
```sql
sudo apt update
sudo apt install mariadb-server
```

Para finalizar solo tenemos que iniciar el programa:
```sql
sudo mysql
```
![funcionando](/img/5.PNG)
