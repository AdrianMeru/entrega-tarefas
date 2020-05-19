# Tarefa 1

# DDL

## ÍNDICE 
- [Que es DDL](#Que-es-DDL)
- [CREATE](#CREATE)
  -  [CREATE SCHEMA](#CREATE-SCHEMA)
  -  [CREATE DOMAIN](#CREATE-DOMAIN)
   -  [CREATE TABLE](#CREATE-TABLE)
 - [CONSTRAINT](#CONSTRAINT)
   -  [PRIMARY KEY](#PRIMARY-KEY)
   -  [FOREIGN KEY](#FOREIGN-KEY)
   -  [CHECK](#CHECK)
   -  [NOT NULL mais UNIQUE](#NOT-NULL-mais-UNIQUE)
- [ALTER](#ALTER)
- [DROP](#DROP)

## Que es DDL

Un lenguaje de base de datos o lenguaje de definición de datos (Data Definition Language, DDL por sus siglas en inglés) es un 
lenguaje proporcionado por el sistema de gestión de base de datos que permite a los programadores de la misma llevar a cabo las 
tareas de definición de las estructuras que almacenarán los datos así como de los procedimientos o funciones que permitan consultarlos.

## CREATE

A continuación vamos a ver los tres tipos diferentes de uso que tiene **create**

### CREATE SCHEMA

Para crear una bases de datos usaremos la la sentencia **SCHEMA**, se recomienda usar **IF NOT EXISTS** para evitar crear dos bases de datos con el mismo nombre

```sql
CREATE SCHEMA [IF NOT EXISTS] nombrebasesdedatos
```
### CREATE DOMAIN

Si queremos crear un dominio de base lo unico que necesitaremos es el nombre el dominio y el tipo de dato. Los domios se usan para facilitar el trabajo, ya que si se va a repetir un tipo de dato en varias partes de la base datos nos creamos un dominio y nos ahorramos especificar el dato en divesas veces,  y ademas en caso de que ese dato se modifique solo lo tendriamos que modificar en el dominio.

```sql
CREATE DOMAIN nombredominio VARCHAR(10)
```

### CREATE TABLE

Las tablas se crean de manera individual, fuera ponemos el nombre de la tabla y si queremos especificar que pertenece a una base de datos que no es en la que no estamos situados ponemos el nombre de esa base de datos entre **[]**. El contenido de la tabla se especifica todo entre **()** , de base solo se necesita el nombre del atributo y el tipo de dato. Si se requiere se le añadiran las **CONSTRAINT** las cuales veremos mas adelante.

```sql
CREATE TABLE nombre tabla(
nombreatributo tipodato [CONSTRAINT
);
```

## CONSTRAINT

Las **CONSTRAINT** se utilizan en el create table y con el alter el cual veremos mas adelante. Se utilizan para especificar las primary key, las foreing key, unique, not null y check.

### PRIMARY KEY

Sirve para especificar cual es la clave pricipal de una tabla, la  cual segun las reglas de entidad relacion no pueden ser nulas y sus valores no pueden repetirse. Se puede hacer en la linea del atributo o fuera.

**Dentro**
```sql
CREATE TABLE nombretabla(
nombreatributo tipodato PRIMARY KEY,
);
```

**Fuera**
```sql
CREATE TABLE nombretabla(
nombreatributo tipodato,
nombreatributo2 tipodato2,
...
PRIMARY KEY (<nombreatributo1>[, <nombreatributo2>, ...])
);
```

### FOREIGN KEY

Se utiliza cuando un atributo de una tabla es la clave principal de otra. Las claves ajenas deben tener el mismo tipo de atributo o dominio que tiene en la tabla en la que es principal. Se puede hacer de dos maneras para claves compuestas y no compuestas.

**No compuestas**
```sql
CREATE TABLE nombretabla1(
1nombreatributo1 dominio1 PRIMARY KEY,
1nombreatributo2 dominio2,
);
CREATE TABLE nombretabla2(
2nombreatributo1 dominio1 PRIMARY KEY,
2nombreatributo2 dominio2 REFERENCES nombretabla1 (1nombreatributo1),
);
```
**Compuesta**
```sql
CREATE TABLE nombretabla1(
1nombreatributo1 dominio1,
1nombreatributo2 dominio2,
[CONSTRAINT <PK_nombretabla1>]
  PRIMARY KEY (1nombreatributo1[, 1nombreatributo2])
);
CREATE TABLE nombretabla2(
2nombreatributo1 dominio1 PRIMARY KEY,
2nombreatributo2 dominio2 REFERENCES nombretabla1 (1nombreatributo1),
[CONSTRAINT <PK_nombretabla2>]
  FOREIGN KEY (2nombreatributo1[, 2nombreatributo2]
  REFERENCES 1nombretabla (1nombreatributo1[, 1nombreatributo2])
  ON DELETE [CASCADE | NO ACTION | SET NULL | SET DEFAULT <valordefecto>]
  ON UPDATE [CASCADE | NO ACTION | SET NULL | SET DEFAULT <valordefecto>]
);
```
El **ON DELETE Y EL ON UPDATE** son el criterio que se usa para el borrado o la actualizacion de datos presentes en varias tablas.
El **NO ACTION** se utiza para no modificar los datos durate el proceso(opcion predefinida).
El **CASCADE** se realiza en forma de cascada en todas las tablas donde aparezca el dato.
El **SET NULL** se utiliza para eliminar datos nulos.
El **SET DEFAULT** añade datos y no se recomienda usarla porque puede comprometer  la integridad de la tabla.

### CHECK

Los **CHECK** se utilizan para evitar valores invalidos en ciertos campos. Estos tambien se pueden crear en la linea del atributo o fuera de ella.

**Dentro**
```sql
CREATE TABLE nombretabla(
nombreatributo tipodato CHECK(<nombreatributo> > 25)
nombreatributo2 tipodato2,
);
```
**Fuera**
```sql
CREATE TABLE nombretabla(
nombreatributo tipodato,
nombreatributo2 tipodato2,
...
CHECK (<nombreatributo> < <nombreatributo2>)
);
```

### NOT NULL mais UNIQUE

La utilización de estas dos CONSTRAINT sirve para declarar las claves alternativas.

```sql
CREATE TABLE nombretabla(
nombreatributo tipodato PRIMARY KEY,
nombreatributo2 tipodato2 UNIQUE NOT NULL,
...
);
```

## ALTER

Utilizamos ALTER TABLE para modificar columnas y restricciones. Ademas nos permite cambiar el nombre a ciertos elemnetos y para vincular tablas de outra bases de datos.

```sql
ALTER TABLE [IF EXISTS] <nombretabla>
	    [RENAME TO <nuevonombretabla>],
	    [RENAME [COLUMN | CONSTRAINT] <nombreoriginal> TO <nuevonombre>],
	    [SET SCHEMA <nuevo_Schema>],
	    [ADD | DROP [COLUMN | CONSTRAINT] <nombre>]
```

## DROP

El DROP TABLE se utiliza para eliminar tabla con todo su contenido y tambien para eliminiar bases de datos enteras.

**BASES DE DATOS**
```sql
DROP SCHEMA <nombreSchema>
	[RESTRICT | CASCADE]
;
```

**TABLA**
```sql
DROP TABLE <nombretabla>
	[RESTRICT | CASCADE]
;
```
