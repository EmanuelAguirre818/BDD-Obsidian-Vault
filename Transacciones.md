- Conjunto indivisible de operaciones que deben ejecutarse en su totalidad o no se ejecutan en absoluto.
- En términos prácticos, la transacción por sí misma no altera la forma en la que se escribe una consulta `INSERT` o `UPDATE` , simplemente actúa como una **red de seguridad**. Aqui el `HANDLER` nos es de mucha utilidad para reaccionar ante determinadas condiciones\errores dentro del procedimiento, usando `COMMIT` para guardar los datos o `ROLLBACK` para volver atras en la operacion.

## Comandos basicos:
En MySql existen varias sentencias a la hora de trabajar con transaccion.
- **START TRANSACTION/BEGIN:** Inicia una nueva transaccion.
- **COMMIT:** Confirma todos los cambios realizados durante la transaccion.
- **ROLLBACK:** Deshace todos los cambios realizados durante la transaccion.
- **SET autocommit:** Controla si las operaciones se confirman automaticamente o requieren un **COMMIT** explicito.

```sql
DELIMITER //
CREATE PROCEDURE insertar_cliente(
	IN p_nombre VARCHAR(100),
	IN p_email VARCHAR(100),
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
 
 INSERT INTO clientes(nombre, email) VALUES (p_nombre, p_email);
 
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

CREATE PROCEDURE sp_registrar_reserva(
    IN sp_dni_cliente INT,
    IN sp_nombre_cancha VARCHAR(50),
    IN sp_fecha DATE,
    IN sp_hora TIME,
    OUT sp_resultado VARCHAR(100)
)
BEGIN
    DECLARE sp_cancha_disponible BOOLEAN;
    DECLARE sp_reserva_coincidente BOOLEAN;
    DECLARE sp_reserva_maxima BOOLEAN;
    DECLARE sp_id_cancha INT;
    DECLARE sp_id_cliente INT;

    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        SET sp_resultado = 'ERROR';
        ROLLBACK;
    END;

    START TRANSACTION;

    SET sp_cancha_disponible =
        fn_cancha_disponible(sp_nombre_cancha, sp_fecha, sp_hora);

    SET sp_reserva_coincidente =
        fn_existe_reserva_horario_coincidente(
            sp_fecha,
            sp_hora,
            sp_dni_cliente
        );

    SET sp_reserva_maxima =
        fn_reserva_maxima(sp_dni_cliente);

    SELECT id
    INTO sp_id_cancha
    FROM canchas
    WHERE nombre = sp_nombre_cancha;

    SELECT id
    INTO sp_id_cliente
    FROM clientes
    WHERE dni = sp_dni_cliente;

    IF sp_cancha_disponible
       AND NOT sp_reserva_coincidente
       AND NOT sp_reserva_maxima THEN

        INSERT INTO reservas(
            id_cliente,
            id_cancha,
            fecha,
            hora
        )
        VALUES (
            sp_id_cliente,
            sp_id_cancha,
            sp_fecha,
            sp_hora
        );

        COMMIT;

        SET sp_resultado = 'Reserva realizada correctamente';

    ELSE

        ROLLBACK;

        SET sp_resultado = 'No se pudo realizar la reserva';

    END IF;

END //

DELIMITER ;

```
## Propiedades: 
