Defininimos la *integridad en base de datos* como un conjunto de reglas, procesos y normas con el fin de mantener completos, coherentes y seguros a lo largo del ciclo de su vida. Ademas, define la Consistencia en Bases de Datos como la garantía de que los datos son precisos y uniformes, manteniendo la base de datos en un estado válido bajo el modelo ACID. 
#### Veremos como aplicar restricciones como: 
- [[NOT NULL]].
- [[UNIQUE]]
- [[PK y FK]].
- [[TRIGGERS]].
- [[SENTENCIA SIGNAL]].
- [[CHECK]].
- [[ENUM]].
- [[DEFAULT]].
### Tipos de integridad
- Dominio:
	- Definido por el conjunto de valores aceptados, cantidad, restricciones y reglas que pueden aceptar las columnas de una tabla.
- Identidad:
	- Basado en claves y valores unicos para identificar datos. Asegura que no haya datos duplicados.
- Referencial:
	- Son procesos que garantizan que los datos son almacenados y utilizados de manera coherente. Agrega ademas relaciones entre tablas para evitar registros huerfanos y mantener la coherencia de los datos en la bdd.
- Definida por el usuario:
	- Reglas y restricciones de negocio planteadas por el usuario/cliente. Las especificaciones son unicas, no garantizan la seguridad de los datos. 

## Ejemplo Integral:
```sql
CREATE TABLE Pedidos (
ID_pedido INT PRIMARY KEY, -- Identificador único del pedido
ID_cliente INT, -- Cliente que realizó el pedido
ID_producto INT, -- Producto que fue pedido
Cantidad INT CHECK (Cantidad > 0), -- La cantidad debe ser mayor a 0
Fecha DATE NOT NULL, -- La fecha del pedido no puede ser nula

-- Claves foráneas
FOREIGN KEY (ID_cliente) REFERENCES Clientes(ID_cliente)
ON DELETE CASCADE, -- Si un cliente se borra, sus pedidos también se eliminan
FOREIGN KEY (ID_producto) REFERENCES Productos(ID_producto)
ON DELETE SET NULL -- Si un producto se borra, los pedidos quedan sin producto
);
```


```sql
DELIMITER //
CREATE TRIGGER validar_stock_pedido
BEFORE INSERT ON Pedidos
FOR EACH ROW
BEGIN
DECLARE stock_actual INT;
-- Verificar el stock disponible
SELECT Stock INTO stock_actual
FROM Productos
WHERE ID_producto = NEW.ID_producto;
-- Si no hay suficiente stock, lanzar un error
IF stock_actual < NEW.Cantidad THEN
SIGNAL SQLSTATE '45000'
SET MESSAGE_TEXT = 'No hay suficiente stock para este pedido.';
END IF;
END;

DELIMITER ;
```