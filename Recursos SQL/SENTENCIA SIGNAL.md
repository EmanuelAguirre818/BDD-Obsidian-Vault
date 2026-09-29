- Es usado para devolver un error durante tiempo de ejecución.  
- Se utiliza para triggers, funciones y procedimientos almacenados. 
```sql
IF stock_actual < NEW.Cantidad THEN
	SIGNAL SQLSTATE '45000'
	SET MESSAGE_TEXT = 'No hay suficiente stock para este pedido.';
END IF;
```
