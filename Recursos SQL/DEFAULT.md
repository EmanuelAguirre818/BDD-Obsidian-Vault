- Si no se especifica un valor al momento de agregar un registro, se asigna un valor generico especificado.
```sql
CREATE TABLE Ordenes ( 
[...] 
fecha DATE DEFAULT CURRENT_DATE);
```