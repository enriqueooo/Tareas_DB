## Desarrollar los siguientes ejercicios usando subconsultas.

## DB INVOICE
1.-Crear un función y un trigger para validar que el numero de cedula del cliente tenga 10 números (no letras) en la tabla cliente.

```
CREATE OR REPLACE FUNCTION validar_cedula_func()
RETURNS TRIGGER AS $$
BEGIN
    IF LENGTH(NEW.cedula) <> 10 OR NEW.cedula !~ '^[0-9]+$' THEN
        RAISE EXCEPTION 'La cédula debe tener 10 números.';
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

```
```
CREATE TRIGGER trg_validar_cedula
BEFORE INSERT OR UPDATE ON cliente
FOR EACH ROW
EXECUTE PROCEDURE validar_cedula_func();

```
- captura
<img src="Capturas/6.png" alt="drawing" width="500"/>
<img src="Capturas/7.png" alt="drawing" width="500"/>

2.- Listar nombre y correo de los clientes junto a su compra mas cara realizada.
- sentencia
```
SELECT C.full_name, c.email, (
                              SELECT MAX (i.total) AS compra_cara 
                              FROM invoice i WHERE i.client_id = c.id
                              )
FROM client c;
```
- captura
<img src="Capturas/2.png" alt="drawing" width="500"/>

3.- Listar las facturas donde sus totales sean mayores al promedio de las facturas.
- sentencia
```
SELECT i.create_at, i.total 
FROM invoice i 
WHERE (SELECT AVG(i.total) FROM invoice i ) < i.total;
```
- captura
<img src="Capturas/Captura 3.png" alt="drawing" width="500"/>

---

## DB EVENT     
1.- Listar una lista de conferencias, incluyendo el número total de asistentes registrados en cada conferencia.
- sentencia
```
SELECT m.id, m.full_name, m.email, m.age 
FROM member m WHERE m.id IN (
                              SELECT r.member_id FROM register r JOIN conference c ON r.conference_id = c.id 
                              WHERE c.total_attendees > 50
                            );
```
- captura
<img src="Capturas/Captura 4.png" alt="drawing" width="500"/>

2.- Seleccionar el id, full_name, email y age de los miembros que se han registrado en eventos con más de 100 participantes totales.
- sentencia
```
SELECT m.id, m.full_name, m.email, m.age 
FROM member m 
WHERE m.id IN (
    SELECT r.member_id 
    FROM register r 
    JOIN event e ON r.conference_id = e.id 
    WHERE e.total_attendees > 100
);
```
- captura
<img src="Capturas/Captura 5.png" alt="drawing" width="500"/>
