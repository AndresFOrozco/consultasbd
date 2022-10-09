## Ejercicio 2.sql
### 2. Devuelve un listado de los profesores junto con el nombre del departamento al que están vinculados. El listado debe devolver cuatro columnas, primer apellido, segundo apellido, nombre y nombre del departamento. El resultado estará ordenado alfabéticamente de menor a mayor por los apellidos y el nombre.

```sql
SELECT p.apellido1, p.apellido2, p.nombre, dpto.nombre
FROM persona as p INNER JOIN profesor as pr ON pr.id_profesor = p.id
        	  INNER JOIN departamento AS dpto ON dpto.id = pr.id_departamento
ORDER BY p.apellido1, p.apellido2, p.nombre ASC;
```