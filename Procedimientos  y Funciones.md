## Procedimientos: 
Es un conjunto de instrucciones logicas que se almacenan en el servidor de la base de datos y pueden ser invocados por su nombre, pueden recibir parametros.

**CREACION:**
```sql
DELIMITER ///
CREATE PROCEDURE nombre_procedimiento (|tipo parametro| nombre_parametro)
BEGIN
	---Instrucciones
END ///
DELIMITER ;
```

**INVOCACION:**
```SQL
CALL nombre_procedimiento(parametros_ingresados)
```

**Tipos de parametros:**
- **IN:** Parámetros de entrada, es el valor por defecto si no se especifica.
- OUT: Parametro de salida que devuelve valores.
- INOUT: Funciona tanto para entrada como para salida.  
## Funciones: 
Las funciones son similares a los procedimientos pero estan pensadas pero calcular y devolver un unico valor. Es comun verlas en expresiones SQL.
Hay 3 instrucciones obligatorias en funciones:
- Returns: Indica que tipo de valor retornara la funcion.
- Deterministic: Le indica a SQL que la funcion siempre devolvera el mismo valor si se le indican los mismos valores a los parametros.
- Return: El valor que retorna la funcion luego de ser ejecutada.

**CREACION:**
```SQL
DELIMITER //
CREATE FUNCTION nombre_funcion (paramteros)
RETURNS |tipo_de_dato| ---OBLIGATORIO.
DETERMINISTIC ---OBLIGATORIO.
BEGIN
	---Instrucciones SQL.
	RETURN valor; ---OBLIGATORIO.
END ///
DELIMITER ;
```
**USO:**
```SQL
SELECT nombre_funcion(parametros) as alias;
```

**LISTAR PROCEDIMIENTOS O FUNCIONES:**
```SQL
SHOW |PROCEDURE|FUNCTION| STATUS WHERE Db = 'nombre_base_datos';

```

**ELIMINAR PROCEDIMIENTOS Y FUNCIONES:**
```SQL
DROP |FUNCTION|PROCEDURE| IF EXIST nombre_funcion; 
```

En ambos recursos es normal encontrarnos con instrucciones como:
- [[DECLARE]]
- [[UPDATE, INSERT, SET]]
- [[CONDICIONALES Y BUCLES]]
- [[Transacciones]]

En transacciones podemos usar [[ERROR HANDLERS]]:
