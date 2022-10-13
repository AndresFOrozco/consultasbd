## Actividad entregable corte II.sql

### 1. Devuelve un listado con todos los alumnos que se han matriculado en alguna asignatura durante el curso escolar 2018/2019.

```sql
SELECT DISTINCT p.*
FROM persona as p JOIN alumno_se_matricula_asignatura AS asma ON asma.id_alumno = p.id
	          JOIN curso_escolar AS ce ON ce.id = asma.id_curso_escolar
WHERE ce.anyo_inicio = 2018 AND ce.anyo_fin = 2019;
```

### 2. Devuelve un listado de los profesores junto con el nombre del departamento al que están vinculados. El listado debe devolver cuatro columnas, primer apellido, segundo apellido, nombre y nombre del departamento. El resultado estará ordenado alfabéticamente de menor a mayor por los apellidos y el nombre.

```sql
SELECT p.apellido1, p.apellido2, p.nombre, dpto.nombre
FROM persona as p INNER JOIN profesor as pr ON pr.id_profesor = p.id
        	  INNER JOIN departamento AS dpto ON dpto.id = pr.id_departamento
ORDER BY p.apellido1, p.apellido2, p.nombre ASC;
```

### 3. Devuelve un listado con los departamentos que no tienen profesores asociados.

```sql
select * from departamento
left join profesor on departamento.id = profesor.id_departamento
where profesor.id_profesor is null
```

### 4 .Devuelve el listado de las asignaturas que se imparten en el primer cuatrimestre, en el tercer curso del
grado que tiene elidentificador 7.

```sql
select asignatura.nombre, curso, cuatrimestre, grado.id
from asignatura
join grado
on grado.id = asignatura.id_grado
where grado.id = 7 and asignatura.cuatrimestre =1 and curso =3;
```
### 5 .Devuelve un listado que muestre cuántos alumnos se han matriculado de alguna
asignatura en cada uno de los cursos escolares. El resultado deberá mostrar dos columnas,
una columna con el año de inicio del curso escolar y otra con el número de alumnos matriculados.

```sql
select curso_escolar.anyo_inicio, count (alumno_se_matricula_asignatura.id_alumno) as numero_alumnos_matriculados
from alumno_se_matricula_asignatura
join asignatura on asignatura.id = alumno_se_matricula_asignatura.id_asignatura
join curso_escolar on curso_escolar.id = alumno_se_matricula_asignatura.id_curso_escolar
group by curso_escolar.anyo_inicio
```

### 6. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno, de los grados que tengan más de 40 asignaturas asociadas.
```sql
SELECT gr.nombre, COUNT(asi.nombre) AS cantAsignatura
FROM grado AS gr INNER JOIN asignatura AS asi ON asi.id_grado = gr.id
GROUP BY gr.nombre HAVING COUNT(asi.nombre) > 40;
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

### 11. Devuelve un listado con las asignaturas que no tienen un profesor asignado.
```sql
SELECT nombre, id_profesor
FROM asignatura WHERE id_profesor IS NULL;
```
### 12.	Devuelve un listado con el número de asignaturas que imparte cada profesor. El listado debe tener en cuenta aquellos profesores que no imparten ninguna asignatura. El resultado mostrará cinco columnas: id, nombre, primer apellido, segundo apellido y número de asignaturas. El resultado estará ordenado de mayor a menor por el número de asignaturas.
```sql
SELECT pr.id_profesor, p.nombre, p.apellido1 AS primerApellido, p.apellido2 AS segundoApellido, COUNT(asi.id) AS cantAsignaturas
FROM profesor AS pr LEFT JOIN asignatura AS asi ON pr.id_profesor = asi.id_profesor
	            RIGHT JOIN persona AS p ON p.id = pr.id_profesor
WHERE p.tipo = 'profesor'					   
GROUP BY pr.id_profesor, p.nombre, p.apellido1, p.apellido2 ORDER BY cantAsignaturas DESC;
```
### 13. Devuelve un listado con el primer apellido, segundo apellido y el nombre de todos los alumnos. El listado deberá estar ordenado alfabéticamente de menor a mayor por el primer apellido, segundo apellido y nombre. revisar
```sql
SELECT DISTINCT p.apellido1, p.apellido2, p.nombre
FROM alumno_se_matricula_asignatura AS asma INNER JOIN persona AS p ON p.id = asma.id_alumno
ORDER BY p.apellido1, p.apellido2, p.nombre DESC;
```

### 14. Devuelve un listado con todas las asignaturas ofertadas en el Grado en Ingeniería Informática (Plan 2015).
```sql
SELECT asi.*
FROM asignatura as asi RIGHT JOIN grado ON grado.id = asi.id_grado
WHERE grado.nombre = 'Grado en Ingeniería Informática (Plan 2015)';
```

### 16. Devuelve el número total de alumnas que hay.
```sql
SELECT DISTINCT COUNT(p.id) AS totalAlumnas
FROM persona AS p
WHERE p.sexo = 'M' AND p.tipo = 'alumno';
```

### 17. Devuelve un listado que muestre el nombre de los grados y la suma del número total de créditos que hay para cada tipo de asignatura. El resultado debe tener tres columnas: nombre del grado, tipo de asignatura y la suma de los créditos de todas las asignaturas que hay de ese tipo. Ordene el resultado de mayor a menor por el número total de créditos.
```sql
SELECT grado.nombre, asi.tipo, sum(asi.creditos) AS totalCreditos
FROM grado INNER JOIN asignatura as asi ON asi.id_grado = grado.id
GROUP BY grado.nombre, asi.tipo ORDER BY totalCreditos DESC;
```

### 18.	Devuelve un listado con todos los departamentos que tienen alguna asignatura que no se haya impartido en ningún curso escolar. El resultado debe mostrar el nombre del departamento y el nombre de la asignatura que no se haya impartido nunca.
```sql
SELECT dpto.nombre, asi.nombre
FROM departamento AS dpto LEFT JOIN profesor AS pr ON pr.id_departamento = dpto.id
		          LEFT JOIN asignatura AS asi ON asi.id_profesor = pr.id_profesor
			  LEFT JOIN alumno_se_matricula_asignatura AS asma ON asma.id_asignatura = asi.id
WHERE asma.id_asignatura IS NULL AND asi.nombre IS NOT NULL;
```

### 20. Devuelve un listado con el nombre de las asignaturas, año de inicio y año de fin del curso escolar del alumno con nif 26902806M.
```sql
SELECT asi.nombre, ce.anyo_inicio, ce.anyo_fin
FROM persona AS p INNER JOIN alumno_se_matricula_asignatura AS asma ON asma.id_alumno = p.id
                  INNER JOIN asignatura AS asi ON asi.id = asma.id_asignatura
		  INNER JOIN curso_escolar AS ce ON ce.id = asma.id_curso_escolar
WHERE p.nif = '26902806M';
```

### 21. Devuelve un listado con todos los departamentos y el número de profesores que hay en cada uno de ellos. Tenga en cuenta que pueden existir departamentos que no tienen profesores asociados. Estos departamentos también tienen que aparecer en el listado.
```sql
SELECT dpto.nombre, COUNT(pr.id_profesor) as cantProfesores
FROM departamento AS dpto INNER JOIN profesor AS pr ON pr.id_departamento = dpto.id
GROUP BY dpto.nombre ORDER BY cantProfesores DESC;
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
