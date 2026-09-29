 - Objeto de la base de datos esta asociado a una tabla y que sucede cuando se produce un evento particular en ella.
- Pueden ser usados para validar informacion.
## Estructura básica: 
- **Llamada de activación:** es la sentencia que permite “disparar” el código a ejecutar.
- **Restricción:** es la condición necesaria para ejecutar el código. Esta restricción puede ser de tipo condicional o de tipo nulidad. 
- **Acción a ejecutar:** es la sentencia de instrucciones a ejecutar una vez que se han cumplido las condiciones iniciales.

```sql
DELIMITER //

CREATE TRIGGER nombre_trigger
[BEFORE|AFTER] [INSERT|UPDATE|DELETE] ON tabla
FOR EACH ROW
BEGIN
    -- Restricción aplicada con un IF
    IF (NEW.atributo < 0) THEN
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = 'mensaje';
    END IF;
END //

DELIMITER ;

```

#### Tipos eventos de disparadores: 
- INSERT, DELETE Y UPDATE.
- Estos pueden activarse:
	-   Antes (BEFORE) o despues (AFTER) del evento en cuestion.

###  Tipos de disparadores:
- Row Triggers: utilizan la clausula `FOR EACH ROW` . Se ejecutan por acada fila afectada.
- Statement Triggers: Se ejecuta solo una vez, independientemente de la cantidad de filas afectadas. **Importante**: Este tipo de trigger no está disponible para MySQL.
  
## Limitaciones:
Los disparadores no pueden referirse a ninguna tabla por su nombre. Para esto empleamos la palabra clave `OLD/NEW`.

- `OLD` se refiere a un registro existente que va a borrarse o que va a actualizarse antes de que esto ocurra. 
- `NEW` se refiere a un registro nuevo que se insertará o a un registro modificado luego de que ocurra la modificación.
### Consideraciones:
- Los disparadores **BEFORE** se activados, la operacion en el registro correspondiente no se efectua.
- Los disparadores **AFTER** son activados solamente si la operacion se ejecuta correctamente.
- En una transaccion, la activacion de un disparador, debe causar un `ROLLBACK` de todos los cambios generados por la sentencia. En tablas no transaccionales, cualquier cambio realizado antes del error no se ve afectado.

Si por alguna razón, quisiéramos que el Trigger no se ejecute más, debemos eliminarlo. Para eliminar un Trigger, se utiliza la sentencia DROP TRIGGER indicando el nombre del Trigger a borrar.

### Trigger vs Check:
- Cuando la condición de validación es directa y se aplica sobre una columna, afectando a todos los registros de la tabla, se recomienda usar `CHECK`
- Para validaciones más complejas, que involucran datos de otras tablas, o necesitan considerar múltiples filas para determinar la validez, se debe recurrir a los `TRIGGER`. 
Siempre que sea posible, se debe optar por CHECK en lugar de TRIGGER, ya que los CHECK son generalmente más eficientes y tienen un menor impacto en el rendimiento de la base de datos.