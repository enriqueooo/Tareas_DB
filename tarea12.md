# Subconsultas
## Desarrollar los siguientes ejercicios usando subconsultas.

## DB INVOICE
1.- Número total de facturas realizadas por cada cliente:
- sentencia
```
SELECT c.full_name, c.address, (
                                SELECT COUNT (i.client_id) 
                                FROM invoice i WHERE i.client_id = c.id
                                )
FROM client c;
```
- captura
![image](https://github.com/enriqueooo/Tareas_DB/assets/148830588/4b3b92ba-b49f-44ad-bb35-c9dac2d88e07)

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
<img src="Capturas/Captura de pantalla 2024-07-02 200833.png" alt="drawing" width="500"/>

3.- Listar las facturas donde sus totales sean mayores al promedio de las facturas.
- sentencia
```
SELECT i.create_at, i.total 
FROM invoice i 
WHERE (SELECT AVG(i.total) FROM invoice i ) < i.total;
```
- captura
<img src="Capturas/Captura de pantalla 2024-07-02 201302.png" alt="drawing" width="500"/>

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
<img src="Capturas/Captura de pantalla 2024-07-02 203024.png" alt="drawing" width="500"/>

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
<img src="Capturas/Captura de pantalla 2024-07-02 205413.png" alt="drawing" width="500"/>
