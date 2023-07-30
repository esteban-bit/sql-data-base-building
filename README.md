 [![LinkedIn][linkedin-shield]][linkedin-url]
<!-- PROJECT LOGO -->

<div align="center">
    <img src= "/Users/esteban/Phyton/sql-data-base-building/Data/images/sql.PNG" alt="Logo" width="1000" height="250">
  </a>

  <h3 align="center" style="font-size: 25px">Procesamiento de datos relacional con Python - SQL</h3>
    <br />
  <p align="center">
    Se analizan los siguientes archivos CSV:
    
 ``* Actor``
 ``* Category``
 ``* Films``
 ``* Inventory``
 ``* language``
 ``* old_HDD``
 ``* rental``
    <br />
    <br />
      <a>
        <img src="/Users/esteban/Phyton/sql-data-base-building/Data/images/palomitas.PNG" alt="Logo" width="450" height="200">
    </a>
    <br />
    <a >Próxima inauguración VIDEOHACK</a>
  </p>
</div> 

## Objetivo

 El cliente nos facilita archivos CSV de una antigua tienda de videoclub, el proyecto se basa en crear una nueva base de datos para la inaguracion de una tienda.

  * Limpiar datos
  * Diseñar relaciones funcionales.


## Proceso

Se realiza el proceso con las herramientas de Python para la limpieza de datos y SQL para crear las relaciones.

Se analiza el conjunto de datos obtenido por cliente para entender y comenzar a trabajar.

## 1. Instalamos las librerias e importamos :
```
%pip install ipython
import pandas as pd
import numpy as np
```

# 2. Se procede a cargar los datos descargados.

Se cargan todos los datos uno a uno para analizar y entender con que tipo de datos estamos trabajando. 

* actor
* films
* language
* rental
* category
* inventory
* films
* oldhhd

# 3. Limpieza de Valores nulos.

Empezamos limpiando tabla por tabla


## 3.1 Actor
* Se realiza una actualizacion de la columna last_update
* Se conservan datos.
```<class 'pandas.core.frame.DataFrame'>
RangeIndex: 200 entries, 0 to 199
Data columns (total 4 columns):
 #   Column       Non-Null Count  Dtype 
---  ------       --------------  ----- 
 0   actor_id     200 non-null    int64 
 1   first_name   200 non-null    object
 2   last_name    200 non-null    object
 3   last_update  200 non-null    object
dtypes: int64(1), object(3)
memory usage: 6.4+ KB 
```

## 3.2 Films
- Se realiza una actualizacion de la columna last_update
- Se elimina columna de nulos 
- Tras revisar inventario vemos que no tenemos todas las peliculas, por lo cual se elimina.
- Se conservan dos archivos:
    1. Para cliente 
    2. Para analisis.
```<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1000 entries, 0 to 999
Data columns (total 13 columns):
 #   Column                Non-Null Count  Dtype  
---  ------                --------------  -----  
 0   film_id               1000 non-null   int64  
 1   title                 1000 non-null   object 
 2   description           1000 non-null   object 
 3   release_year          1000 non-null   int64  
 4   language_id           1000 non-null   int64  
 5   original_language_id  0 non-null      float64
 6   rental_duration       1000 non-null   int64  
 7   rental_rate           1000 non-null   float64
 8   length                1000 non-null   int64  
 9   replacement_cost      1000 non-null   float64
 10  rating                1000 non-null   object 
 11  special_features      1000 non-null   object 
 12  last_update           1000 non-null   object 
dtypes: float64(3), int64(5), object(5)
memory usage: 101.7+ KB
```

## 3.3 Language

- Aunque observamos que en peliculas no tenemos otros idiomas, se decide mantener la tabla para el futuro.
- Se actualiza fecha last_update.

``` <class 'pandas.core.frame.DataFrame'>
RangeIndex: 6 entries, 0 to 5
Data columns (total 3 columns):
 #   Column       Non-Null Count  Dtype 
---  ------       --------------  ----- 
 0   language_id  6 non-null      int64 
 1   name         6 non-null      object
 2   last_update  6 non-null      object
dtypes: int64(1), object(2)
memory usage: 272.0+ bytes 
```

## 3.4 Rental

- Con la tabla rental se procede a dejar los datos a 0, para darle al cliente 
- Conservo el backup para analizar
- Creamos otras dos tablas customer y staff para las relaciones en SQL.
  
```<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1000 entries, 0 to 999
Data columns (total 7 columns):
 #   Column        Non-Null Count  Dtype 
---  ------        --------------  ----- 
 0   rental_id     1000 non-null   int64 
 1   rental_date   1000 non-null   object
 2   inventory_id  1000 non-null   int64 
 3   customer_id   1000 non-null   int64 
 4   return_date   1000 non-null   object
 5   staff_id      1000 non-null   int64 
 6   last_update   1000 non-null   object
dtypes: int64(4), object(3)
memory usage: 54.8+ KB
```

## 3.5 Category

- Se actualiza fecha, last_update.
- Se mantienen datos.
```<class 'pandas.core.frame.DataFrame'>
RangeIndex: 16 entries, 0 to 15
Data columns (total 3 columns):
 #   Column       Non-Null Count  Dtype 
---  ------       --------------  ----- 
 0   category_id  16 non-null     int64 
 1   name         16 non-null     object
 2   last_update  16 non-null     object
dtypes: int64(1), object(2)
memory usage: 512.0+ bytes
```
## 3.6 Inventory

- El cliente ya no tiene las dos tiendas, por lo cual eliminamos la fila store.
- Se actualiza fecha last_update.
- Se filtan los valores unicos para comparar con el archivo de peliculas.
  
``` <class 'pandas.core.frame.DataFrame'>
RangeIndex: 1000 entries, 0 to 999
Data columns (total 4 columns):
 #   Column        Non-Null Count  Dtype 
---  ------        --------------  ----- 
 0   inventory_id  1000 non-null   int64 
 1   film_id       1000 non-null   int64 
 2   store_id      1000 non-null   int64 
 3   last_update   1000 non-null   object
dtypes: int64(3), object(1)
memory usage: 31.4+ KB
```

## 3.7 Old HDD 

- Se decide mantener tabla para poder hacer relacion many to many en SQL.
- Se decide eliminar columnas first_name, last_name.
- Se realiza la funcion merge para unir los datos y sustituir por numeros, ya que es mas eficiente.
 ```class 'pandas.core.frame.DataFrame'>
RangeIndex: 1000 entries, 0 to 999
Data columns (total 5 columns):
 #   Column        Non-Null Count  Dtype 
---  ------        --------------  ----- 
 0   first_name    1000 non-null   object
 1   last_name     1000 non-null   object
 2   title         1000 non-null   object
 3   release_year  1000 non-null   int64 
 4   category_id   1000 non-null   int64 
dtypes: int64(2), object(3)
memory usage: 39.2+ KB 
```
# 4. Se procede a guardar CSV
 ```ctor.to_csv('actor_v1.csv', index= False)
films_filtrados.to_csv('films_v1.csv', index= False)
language.to_csv('language_v1.csv', index= False)
rental_v1.to_csv('rental_v1.csv', index= False)
category.to_csv('category_v1.csv',index= False)
inventory.to_csv('inventory_v1.csv', index= False)
old_HDD_v1.to_csv('old_HDD_v1.csv', index= True)
```