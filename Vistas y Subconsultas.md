## Vistas: 
Es un tabla virtual basada en el resultado de una consulta, que tiene nombre y logica propia, Son reutilizable y persistentes.

Creacion:
```sql
CREATE |OR REPLACE| VIEW nombre_vista AS SELECT columna1, columna2  
FROM tabla
WHERE condicion
WITH |CASCADED | LOCAL| CHECK OPTION; -- Esto no es obligatorio.
```

- Usaremos `WITH |CASCADED | LOCAL| CHECK OPTION` para verificar que cualquier insercion o actualizacion cumpla con la condiciona WHERE de la vista.

Eliminacion:
```sql
DROP VIEW IF EXISTS nombre_vista
```

- Las vistas puede ser actualizables mediante [[UPDATE, INSERT, SET]] solo si:
	- No debe haber uniones entre tablas.
	- No debe haber funciones de agregacion ni ordenamiento.
	- No debe haber subconsultas.

Ejemplo: 

```sql
CREATE VIEW ingenieros AS
SELECT id, nombre, apellido, email, salario
FROM empleados
WHERE departamento_id = 3;
```

```sql
INSERT INTO ingenieros(nombre, apellido, email, salario)
VALUES ('Juan', 'Perez', 'jperez@gmail.com', 6000);

UPDATE ingenieros SET salario = 65000 WHERE id = 101;

DELETE FROM ingenieros WHERE id = 102;
```