### PRIMARY KEY
Garantiza que cada registro en una tabla tenga un identificador único. Las claves primarias deben contener valores UNIQUE y no pueden contener valores NULL.
```sql
CREATE TABLE Usuarios ( 
id INT PRIMARY KEY,
nombre VARCHAR(50),
  [...]
); 
```

### FOREIGN KEY
Garantiza la correcta relacion entre una tabla y otra.
- Para su correcta implementacion, debemos crear un atributo el cual luego declararemos como FK de la tabla padre. 

```sql
CREATE TABLE Pedidos ( 
[...]
 usuario_id INT, # Atributo que luego sera declarado como FK.
[...],
FOREIGN KEY (usuario_id) REFERENCES Usuarios(id) 
);          #Referencia             #Referencia a la 
			al atributo.            tabla padre.
```
