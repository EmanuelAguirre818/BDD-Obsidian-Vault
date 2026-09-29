- INSERT: Usado para ingresar datos nuevos en una tabla.

```sql
INSERT INTO usuarios (nombre, email, fecha_registro) 
VALUES (p_nombre, p_email, NOW());
```

- UPDATE: Modifica datos existentes.
```sql
UPDATE productos 
SET stock = stock - p_cantidad_vendida 
WHERE id = p_producto_id;
```
 