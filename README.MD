# LDD
```mermaid
flowchart LR
LDD(LDD)
CR(CREATE)
DR(DROP)
ALT(ALTER)
LDD-->CR
LDD-->DR
LDD-->ALT
T1(TABLE)
V1(VIEW)
I1(INDEX)
T2(TABLE)
V2(VIEW)
I2(INDEX)
T3(TABLE)
V3(VIEW)
I3(INDEX)
CR-->T1
CR-->V1
CR-->I1
DR-->T2
DR-->V2
DR-->I2
ALT-->T3
ALT-->V3
ALT-->I3
```
## CREATE TABLE
Ejemplo de comando create table
```sql
CREATE TABLE tabla (col1 tipo_de_datos[NOT NULL][RESTRICCIONES])
```
### RESTRICCIONES 
* IDENTITY:
  Proporciona una valor incremental que va sumando por cada fila que se crea se suele usar en las PRIMARY KEY POR EJ:<BR>
  ```sql
  Personid int IDENTITY([numero en el que empieza],[numero que se incrementa]) ,
  ```
  EJ REAL:
  ```sql
  CREATE TABLE Persons (
    Personid INT IDENTITY(1,1) PRIMARY KEY,
    LastName VARCHAR(255) NOT NULL,
    FirstName VARCHAR(255),
    Age INT
    );
  ```
* PRIMARY KEY:<br> 
Es una clave que identifica a la tabla **DEBE SER UNICO** y es recomendable que no lo cree el usuario y que se cree automáticamente:
ej
    ```sql
    Personid int PRIMARY KEY,
    ```
    ejemplo real:
  ```sql
  CREATE TABLE Persons (
    Personid INT IDENTITY(1,1) PRIMARY KEY,
    LastName VARCHAR(255) NOT NULL,
    FirstName VARCHAR(255),
    Age INT
  );
    ```
* FOREIGN KEY:<br>
  Es cuando un dato es otra tabla por lo que se usa la clave de la otra tabla <br>
  ej manera 1:
  ```sql
  c2 INT FOREIGN KEY REFERENCES tabla_objetivo(c1)
  ```
  ej real manera 1:
  ```sql
  CREATE TABLE Orders (
    OrderID int NOT NULL PRIMARY KEY,
    OrderNumber int NOT NULL,
    PersonID int FOREIGN KEY REFERENCES Persons(PersonID)
    );
  ```
  ej real manera 2:
  ```sql
  CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    CONSTRAINT FK_PersonOrder FOREIGN KEY (PersonID)
    REFERENCES Persons(PersonID)
  );
  ```
* REGLAS DE BORRADO  Y DE ACTUALIZACIÓN:<BR>
  -  Indica en la definición de la
    FOREIGN KEY qué debe ocurrir en la tabla referencial cuando hay un intento de
    modificar o borrar un valor de PRIMARY KEY<BR>
       - NO ACTION(Valor por defecto):<br>
        NO ACTION define que no hace nada<br>
        ej update:
          ```sql 
          CREATE TABLE padres (
            id INT PRIMARY KEY
          );

          CREATE TABLE hijos (
            id INT PRIMARY KEY,
            padre_id INT,
            FOREIGN KEY (padre_id) REFERENCES padres(id) ON UPDATE NO ACTION
          );
          ```
          ej delete:
          ```sql
            CREATE TABLE padres (
              id INT PRIMARY KEY,
              nombre VARCHAR(50)
            );
            CREATE TABLE hijos (
              id INT PRIMARY KEY,
              nombre VARCHAR(50),
              padre_id INT,
              FOREIGN KEY (padre_id) REFERENCES padres(id) ON DELETE NO ACTION
            );
          ```
      - CASCADE:<BR>
        El `CASCADE` hace la acción en cascada es decir al hacerlo en la tabla donde lo haces también lo borras en las tablas referidas<br>
        ej de update
        ```sql
        CREATE TABLE padres (
          id INT PRIMARY KEY,
          nombre VARCHAR(50)
        );

        CREATE TABLE hijos (
          id INT PRIMARY KEY,
          nombre VARCHAR(50),
          padre_id INT,
          FOREIGN KEY (padre_id) REFERENCES padres(id) ON DELETE CASCADE
        );
        ```
        ej de delete:
        ```sql
        CREATE TABLE padres (
          id INT PRIMARY KEY,
          nombre VARCHAR(50)
        );

        CREATE TABLE hijos (
          id INT PRIMARY KEY,
          nombre VARCHAR(50),
          padre_id INT,
          FOREIGN KEY (padre_id) REFERENCES padres(id) ON DELETE NO ACTION
        );
        ```
      - SET NULL:<BR>
        el `SET NULL` dice que cuando se haga la acción lo pondrá a nulo:<br>
        ej update:
        ```sql
        CREATE TABLE departamentos (
          id INT PRIMARY KEY,
          nombre VARCHAR(100) NOT NULL
        );

        CREATE TABLE empleados (
          id INT PRIMARY KEY,
          nombre VARCHAR(100) NOT NULL,
          departamento_id INT,
          FOREIGN KEY (departamento_id) REFERENCES departamentos(id)
          ON UPDATE SET NULL
        );
        ```
        ej delete:
        ```sql
          CREATE TABLE departamentos (
              id INT PRIMARY KEY,
              nombre VARCHAR(100) NOT NULL
          );
          CREATE TABLE empleados (
              id INT PRIMARY KEY,
              nombre VARCHAR(100) NOT NULL,
              departamento_id INT,
              FOREIGN KEY (departamento_id) REFERENCES departamentos(id)
              ON DELETE SET NULL
        );
        ```
      - SET DEFAULT:<BR>
        El `set default` hace que cuando que al hacer la acción pone el valor por defecto(si no hay valor por defecto y admite nulo pone null)<br>
        ej update
        ```sql
          CREATE TABLE departamentos (
            id INT PRIMARY KEY,
            nombre VARCHAR(100) NOT NULL
          );

          CREATE TABLE empleados (
            id INT PRIMARY KEY,
            nombre VARCHAR(100) NOT NULL,
            departamento_id INT DEFAULT 99,
            FOREIGN KEY (departamento_id) REFERENCES departamentos(id)
            ON UPDATE SET DEFAULT
        );
        ```
        ej delete
        ```sql
          CREATE TABLE departamentos (
            id INT PRIMARY KEY,
            nombre VARCHAR(100) NOT NULL
          );

          CREATE TABLE empleados (
              id INT PRIMARY KEY,
              nombre VARCHAR(100) NOT NULL,
              departamento_id INT DEFAULT 99,  -- El valor por defecto para empleados sin departamento
              FOREIGN KEY (departamento_id) REFERENCES departamentos(id)
              ON DELETE SET DEFAULT
          );
        ```
* DEFAULT:<BR>
  Los valores predeterminados suministran un valor cuando no se especifica ninguno<br>
  ej:
  ```sql
  c2 [nombre de variable] DEFAULT [valor por defecto]
  ```
  ej real:
  ```sql
  CREATE TABLE tabla1(
    c1 INT PRIMARY KEY,
    c2 nombres* DEFAULT 'Málaga'
  );
  ````
  - *como crear un tipo:<br>
    Para facilitar las cosas se puede crear un tipo con lo siguiente:<br>
    ```sql
    sp_addtype [nombre de tipo], [tipo]
    ```
    ej real:
    ```sql
    sp_addtype nombres, 'VARCHAR(30)','NOT NULL';
    Constraints.
    CREATE TABLE tabla1(
      c1 INT PRIMARY KEY,
      c2 nombres DEFAULT 'Málaga'
    );
    ```



  
    



