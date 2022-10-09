## Actividad entregable corte II.sql
### 2. Devuelve un listado de los profesores junto con el nombre del departamento al que están vinculados. El listado debe devolver cuatro columnas, primer apellido, segundo apellido, nombre y nombre del departamento. El resultado estará ordenado alfabéticamente de menor a mayor por los apellidos y el nombre.

```sql
SELECT p.apellido1, p.apellido2, p.nombre, dpto.nombre
FROM persona as p INNER JOIN profesor as pr ON pr.id_profesor = p.id
        	  INNER JOIN departamento AS dpto ON dpto.id = pr.id_departamento
ORDER BY p.apellido1, p.apellido2, p.nombre ASC;
```

### 9. Devuelve un listado con los datos de todas las alumnas que se han matriculado alguna vez en el Grado en Ingeniería Informática (Plan 2015).
```sql
SELECT DISTINCT p.*
FROM persona as p INNER JOIN alumno_se_matricula_asignatura as asma ON asma.id_alumno = p.id
		  INNER JOIN asignatura as asi ON asi.id = asma.id_asignatura
		  INNER JOIN grado ON grado.id = asi.id_grado
WHERE p.sexo = 'M' AND grado.nombre = 'Grado en Ingeniería Informática (Plan 2015)';
```
### 14. Devuelve un listado con todas las asignaturas ofertadas en el Grado en Ingeniería Informática (Plan 2015).
```sql
SELECT asi.*
FROM asignatura as asi RIGHT JOIN grado ON grado.id = asi.id_grado
WHERE grado.nombre = 'Grado en Ingeniería Informática (Plan 2015)';
```