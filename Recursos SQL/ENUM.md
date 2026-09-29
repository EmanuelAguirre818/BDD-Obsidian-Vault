- Es un objeto de strings que restringe la entrada de datos a valores predefinidos como opciones.
- MySQL permite definir tres tipos de atributos para ENUM:
	- **NOT NULL:** Agregarlo si no queremos ningún  valor nulo.
	- **NULL:** Sinonimo de DEFAULT NULL.
	- DEFAULT: Por defecto, el tipo de dato de ENUM es NULL si el usuario no ingresa ningún dato.

```sql
CREATE TABLE Student_grade(  
    id INT PRIMARY KEY AUTO_INCREMENT,  
    Grade VARCHAR(250) NOT NULL,  
    priority ENUM('Low', 'Medium', 'High') NOT NULL  
);
```
## Recuperar valores en columnas con ENUM:
Las siguientes consultas funcionan de igual manera:
```sql

SELECT * FROM Student_grade  
WHERE priority = 'High';
```

```sql
SELECT * FROM Student_grade  
WHERE priority = 3;
```

![mysql enum example output|523](https://media.geeksforgeeks.org/wp-content/uploads/20201219185233/tw-660x158.png "Click to enlarge")

Esto sucede ya que el motor devuelve lo que hay en ese indice de ENUM.
Esto también puede ser usado para ordenar

```sql
SELECT Grade, priority FROM Student_grade  
ORDER BY priority DESC;
```

![](https://media.geeksforgeeks.org/wp-content/uploads/20201219191933/t3-660x253.png "Click to enlarge")

