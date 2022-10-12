## Actividad entregable corte II.sql
### 2. Devuelve un listado de los profesores junto con el nombre del departamento al que están vinculados. El listado debe devolver cuatro columnas, primer apellido, segundo apellido, nombre y nombre del departamento. El resultado estará ordenado alfabéticamente de menor a mayor por los apellidos y el nombre.

```sql
SELECT p.apellido1, p.apellido2, p.nombre, dpto.nombre
FROM persona as p INNER JOIN profesor as pr ON pr.id_profesor = p.id
        	  INNER JOIN departamento AS dpto ON dpto.id = pr.id_departamento
ORDER BY p.apellido1, p.apellido2, p.nombre ASC;
```

### 7. Devuelve un listado con los profesores que no están asociados a un departamento.
```sql
SELECT (p.nombre||''||p.apellido1||''||p.apellido2) AS nom_completo, pr.id_departamento
FROM profesor AS pr INNER JOIN persona AS p ON p.id = pr.id_profesor
WHERE pr.id_departamento IS NULL;
```

### 9. Devuelve un listado con los datos de todas las alumnas que se han matriculado alguna vez en el Grado en Ingeniería Informática (Plan 2015).
```sql
SELECT DISTINCT p.*
FROM persona as p INNER JOIN alumno_se_matricula_asignatura as asma ON asma.id_alumno = p.id
		  INNER JOIN asignatura as asi ON asi.id = asma.id_asignatura
		  INNER JOIN grado ON grado.id = asi.id_grado
WHERE p.sexo = 'M' AND grado.nombre = 'Grado en Ingeniería Informática (Plan 2015)';
```

### 12.	Devuelve un listado con el número de asignaturas que imparte cada profesor. El listado debe tener en cuenta aquellos profesores que no imparten ninguna asignatura. El resultado mostrará cinco columnas: id, nombre, primer apellido, segundo apellido y número de asignaturas. El resultado estará ordenado de mayor a menor por el número de asignaturas.
```sql
SELECT pr.id_profesor, p.nombre, p.apellido1 AS primerApellido, p.apellido2 AS segundoApellido, COUNT(asi.id) AS cantAsignaturas
FROM profesor AS pr LEFT JOIN asignatura AS asi ON pr.id_profesor = asi.id_profesor
	            RIGHT JOIN persona AS p ON p.id = pr.id_profesor
WHERE p.tipo = 'profesor'					   
GROUP BY pr.id_profesor, p.nombre, p.apellido1, p.apellido2 ORDER BY cantAsignaturas DESC;
```
### 14. Devuelve un listado con todas las asignaturas ofertadas en el Grado en Ingeniería Informática (Plan 2015).
```sql
SELECT asi.*
FROM asignatura as asi RIGHT JOIN grado ON grado.id = asi.id_grado
WHERE grado.nombre = 'Grado en Ingeniería Informática (Plan 2015)';
```
### 17. Devuelve un listado que muestre el nombre de los grados y la suma del número total de créditos que hay para cada tipo de asignatura. El resultado debe tener tres columnas: nombre del grado, tipo de asignatura y la suma de los créditos de todas las asignaturas que hay de ese tipo. Ordene el resultado de mayor a menor por el número total de créditos.
```sql
SELECT grado.nombre, asi.tipo, sum(asi.creditos) AS totalCreditos
FROM grado RIGHT JOIN asignatura as asi ON asi.id_grado = grado.id
GROUP BY grado.nombre, asi.tipo ORDER BY totalCreditos;
```

### 23.	Calcula cuántos profesores hay en cada departamento. El resultado sólo debe mostrar dos columnas, una con el nombre del departamento y otra con el número de profesores que hay en ese departamento. El resultado sólo debe incluir los departamentos que tienen profesores asociados y deberá estar ordenado de mayor a menor por el número de profesores.
```sql
SELECT dpto.nombre, COUNT(pr.id_profesor) as cantProfesores
FROM departamento AS dpto INNER JOIN profesor AS pr ON pr.id_departamento = dpto.id
GROUP BY dpto.nombre ORDER BY cantProfesores DESC;
```

### 26. Devuelve el listado de profesores que no han dado de alta su número de teléfono en la base de datos y además su nif termina en K.
```sql
SELECT p.*
FROM profesor as pr INNER JOIN persona AS p ON p.id = pr.id_profesor
WHERE p.telefono IS NULL AND p.nif LIKE '%K';
```
