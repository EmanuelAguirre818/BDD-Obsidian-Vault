Un ejemplo para entender como podemos utilizar un procedimiento almacenado junto a una transaccion, con su respectivo handler error.

```sql
DELIMITER //

CREATE PROCEDURE nombre_procedimiento(parametros)
BEGIN

    -- 1. Declarar variables
    DECLARE ...

    -- 2. Declarar HANDLER
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        ...
    END;

    -- 3. Iniciar transacción
    START TRANSACTION;

    -- 4. Hacer validaciones / lógica
    IF ... THEN

        -- 5. Si todo está bien
        INSERT / UPDATE / DELETE ...

        COMMIT;

    ELSE

        -- 6. Si no cumple las condiciones
        ROLLBACK;

    END IF;

END //

DELIMITER ;
```