- Conjunto indivisible de operaciones que deben ejecutarse en su totalidad o no se ejecutan en absoluto.
- En términos prácticos, la transacción por sí misma no altera la forma en la que se escribe una consulta `INSERT` o `UPDATE` , simplemente actúa como una **red de seguridad**. Le delega el control total al `HANDLER` para que decida las operaciones a realizar, usando `COMMIT` para guardar los datos o `ROLLBACK` para volver atras en la operacion.

## Comandos basicos:
En MySql existen varias sentencias a la hora de trabajar con transaccion.
- **START TRANSACTION/BEGIN:** Inicia una nueva transaccion.
- **COMMIT:** Confirma todos los cambios realizados durante la transaccion.
- **ROLLBACK:** Deshace todos los cambios realizados durante la transaccion.
- **SET autocommit:** Controla si las operaciones se confirman automaticamente o requieren un **COMMIT** explicito.

```sql
DELIMITER //
CREATE PROCEDURE insertar_cliente(
	IN p_nombre VARCHAR(100)
	IN p_email VARCHAR(100)
	OUT p_resultado VARCHAR(100)
)
BEGIN
--handler para capturar errores
 DECLARE exit handler for SQLEXCEPTION
 BEGIN
	 SET p_resultado = "Error al insertar cliente";
	 ROLLBACK;
 END;
 
 --Iniciar transaccion
 START TRANSACTION;
 -- Operaciones convencionales con SET, UPDATE, INSERT.
 
 INTO INTO clientes(nombre, email) VALUES (p_nombre, p_email);
 
 SET p_resultado = 'Cliente insertado correctamente';
 
 COMMIT;
 END//
 DELIMITER ;
```

- En transacciones veremos sentencias tales como:
	- [[ERROR HANDLERS]]: Para reaccionar ante errores tecnicos o sintacticos. 
	- [[UPDATE, INSERT, SET]]: Para modificar los datos.
	- [[CONDICIONALES Y BUCLES]]: Para validar reglas de negocio y disparar una [[SENTENCIA SIGNAL]] o bien instanciar un `ROLLBACK` o un `COMMIT`. 
	- [[DECLARE]]: Para instanciar variables de control de error o de flujos.

Otro ejemplo: 

```sql 
DELIMITER //

CREATE PROCEDURE TransferirDineroConValidacion(
    IN p_CuentaOrigen INT,
    IN p_CuentaDestino INT,
    IN p_Monto DECIMAL(18,2)
)
BEGIN
    -- 1. Variables para controlar el flujo y el saldo actual
    DECLARE v_Error INT DEFAULT 0;
    DECLARE v_SaldoActual DECIMAL(18,2) DEFAULT 0.00;
    
    -- Manejador de errores técnicos y también de nuestros errores provocados (SIGNAL)
    DECLARE CONTINUE HANDLER FOR SQLEXCEPTION SET v_Error = 1;

    -- 2. Obtener el saldo actual de la cuenta de origen para poder validarlo
    SELECT Saldo INTO v_SaldoActual 
    FROM Cuentas 
    WHERE IdCuenta = p_CuentaOrigen;

    -- 3. VALIDACIÓN DE NEGOCIO: ¿Tiene saldo suficiente?
    IF v_SaldoActual < p_Monto THEN
        -- Si no tiene saldo, disparamos un error personalizado (SQLSTATE '45000' es para errores definidos por el usuario)
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = 'Transacción rechazada: Saldo insuficiente en la cuenta de origen.';
    END IF;

    -- 4. Si pasó la validación, iniciamos la transacción física
    START TRANSACTION;

    -- Paso A: Restar el dinero
    UPDATE Cuentas 
    SET Saldo = Saldo - p_Monto 
    WHERE IdCuenta = p_CuentaOrigen;

    -- Paso B: Sumar el dinero
    UPDATE Cuentas 
    SET Saldo = Saldo + p_Monto 
    WHERE IdCuenta = p_CuentaDestino;

    -- 5. Evaluación final del Handler
    IF v_Error = 1 THEN
        ROLLBACK;
        SELECT 'La transacción fue cancelada (por error técnico o saldo insuficiente).' AS Resultado;
    ELSE
        COMMIT;
        SELECT 'Transferencia realizada con éxito.' AS Resultado;
    END IF;

END //

DELIMITER ;

```
## Propiedades: 
