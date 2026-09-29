- Agrega una condicion a la entrada del valor del atributo, si esta no coincide, se disparara un error. 
```sql
CREATE TABLE Estudiantes ( 
[...] 
edad INT CHECK (edad >= 18) );
```

