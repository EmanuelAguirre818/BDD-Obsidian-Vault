- Mecanismo por el cual sql protegre la integridad de los datos frente a errores durante la ejecucion de un procedimiento. 
- Este detecta un error, detiene la ejecucion, borra los cambios y avisa al sistema.
- Se declara al principio del procedimiento almacenado con transacciones.

```sql
DECLARE exit handler for SQLEXCEPTION
BEGIN
	set p_resultado = "Error"
	ROLLBACK;
END;
```