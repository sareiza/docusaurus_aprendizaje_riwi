# Actividad Progresiva SQL — Subiendo de Nivel
## Ejercicios SQL Robinson

## Nivel 1 — Fundamentos (Exploración básica)

### --->Listar todos los usuarios
```
SELECT * from users;
```
:::note
En SQL, el asterisco (*) en la consulta SELECT * FROM users; es un comodín que significa "todas las columnas". Indica al motor de la base de datos que devuelva cada campo o columna existente en la tabla users para todas las filas coincidentes.
:::
### ---> Mostrar solo first_name, last_name, email
```
SELECT first_name, last_name, email FROM users;
```
### ---> Filtrar usuarios cuyo role sea 'admin'
```
SELECT * FROM `users` WHERE role = "admin";
```
### ---> Filtrar usuarios con document_type = 'CC'
```
SELECT * FROM `users` WHERE document_type = "CC";
```
### ---> Mostrar usuarios mayores de 18 años (calcular edad desde birth_date)
```
SELECT * FROM users WHERE birth_date <= '2008-02-24';
```
:::danger Ojo con la lógica de las fechas!
Aquí hay dos puntos clave: la edad se analiza según la fecha de nacimiento y usaremos el operador WHERE con sus comparativos clásicos `=, >, <, >=, y <=.` Lo curioso es que, para identificar a los mayores de edad, buscamos fechas que se alejen hacia el pasado. Por eso, aunque busquemos personas mayores de edad, en la base de datos filtramos por fechas anteriores (menores) a la de hace 18 años. 
No olvides que la fecha debe ir en formato 'AAAA-MM-DD' y debe estar entre comillas simples.
:::

### ---> Mostrar usuarios cuyo ingreso sea mayor a 5,000,000
```
SELECT * FROM `users` WHERE monthly_income>5000000;
```
:::danger ¡Cuidado con los Números y el Formato! 
Para que tu consulta no lance un error (o peor, te traiga datos que no son), ten en cuenta estas reglas de oro al manejar valores numéricos o sueldos:

¡Cero adornos!: Olvídate de los puntos para indicar miles o las comas para separar millones. En SQL, las cifras se ponen "crudas" y completitas. Si vas a escribir diez mil, pones 10000, no 10.000.

El punto es el rey de los decimales: Si necesitas precisión decimal, el único signo permitido es el punto (.). Nada de comas aquí, que eso confunde al motor de la base de datos.

Sin "disfraces" (comillas) en el codigo de busqueda: A diferencia de los nombres o textos (strings) que siempre van elegantes entre comillas, los números van desnudos. Si los encierras en comillas, el sistema creerá que son texto y no podrás hacer cálculos matemáticos con ellos.
:::
### ---> Mostrar usuarios cuyo nombre empiece por "A"
```
SELECT * FROM users WHERE first_name LIKE 'a%';
```
:::note 
Hay dos comodines que suelen utilizarse junto con el operador LIKE:
El signo de porcentaje % que representa cero, uno o varios caracteres.
El signo de subrayado _ representa un solo carácter
:::
### ---> Mostrar usuarios que no tengan company
```
SELECT * FROM `users` WHERE company IS NULL;
```
:::note ¡Cuidado con confundir company = NULL y company IS NULL!
¿Vas a FILTRAR/BUSCAR? Usa IS NULL o IS NOT NULL.
¿Vas a ASIGNAR/CAMBIAR un valor a vacío? Usa NULL. (Esto solo se usa cuando se quiere modificar un valor de una casilla por medio del UPDATE)
Piensa que IS es el verbo que define el estado (como el 'is' en inglés), mientras que NULL es el concepto del vacío.
:::
Aquí ya aprendiste SELECT, WHERE, operadores lógicos y NULL.

## Nivel 2 — Combinación de condiciones

### --->Usuarios mayores de 25 años que sean 'employee'
```
SELECT * FROM `users` WHERE birth_date < '2001-02-25' AND role = 'employee';
```

### --->Usuarios con 'CC' que estén activos
```
SELECT * FROM `users` WHERE document_type = 'CC' and is_active = 1;
```
:::note
Cuando ponemos el simbolo = nos referimos a que la información dentro de la casilla que estamos analisanso va a ser exactamente a lo que pongamos despues dentro de las comillas simples, para poder que la comparación sea correcta y se verifique el criterio de igualdad con exactitud.
:::
### ---> Usuarios mayores de edad sin empleo
```
SELECT * FROM `users` WHERE birth_date <= '2008-02-25' AND (company IS NULL OR monthly_income IS NULL);
```
:::note
Para este ejercicio, despues del análisis de los datos, se evidenció que todo usuario que no tenia compañia no estaba percibiendo ingresos mensuales, por lo que para este caso especifico se pudo filtrar de estas dos maneras.
siempre que se van a utilizar dos booleanos, el primero que es el condicional principal va afuera de parentesis y el secundario que representa una u otra opcion debe ir entre parentesis para que no vaya a romper la logica y pueda realizar la busqueda de manera correcta. Aquí tambien puedes utilizar el operador LIKE.
:::
### ---> Usuarios con empleo y con ingresos mayores a 3,000,000
```
SELECT * FROM `users` WHERE company IS NOT NULL AND monthly_income >3000000;
```
:::note
Para este ejercicio se utilizó la lógica anterior pero al contrario, se verificó si todos los usuarios que se asociaban a una compañia estaban percibiendo ingresos mensuales y visceversa, se encontro que en todos los casos siempre habia una relación de ingresos y compañia. se utilizó el siguiente código:
```
SELECT * FROM `users` WHERE company IS NOT NULL OR monthly_income IS NOT NULL;
```
:::danger
No se utilizó el rol employee para la verificacion del estado de empleado debido a que estos estaban asociados a una empresa, y si hacia el filtro por medio de employee habian empleados asociados a compañias con rol de admin que no se presentarían en la lista filtrada haciendo asi que la busqueda este incompleta.
:::
### ---> Usuarios casados con al menos 1 hijo
```
SELECT * FROM `users` WHERE marital_status = 'Casado' and children_count >= 1; 
```
### ---> Usuarios entre 30 y 40 años
```
SELECT * FROM `users` WHERE birth_date BETWEEN '1986-02-25' AND '1996-02-25';
```
### ---> Usuarios 'admin' verificados mayores de 25 años
```
SELECT * FROM `users` WHERE role = 'admin' AND birth_date < '2000-02-26' AND is_verified =1;
```
Aquí combinamos múltiples condiciones y lógica booleana.
:::tip ¡Pregunta para Robin!
¿Si en este caso las fechas son estaticas, pero si yo voy a dejar que el sistema siempre este calculando la mayoria de edad para cada dia que pasa o la mayoria de cierrta edad, debo poner un contador en esto par aque se vaya actualizando o como se realizaría?
:::

## Nivel 3 — Introducción a análisis (Agregaciones)

### ---> Contar usuarios por role
```
SELECT role, COUNT(*) AS total_usuarios FROM `users`GROUP BY role;
```
:::tip Pregunta para Robin.
Preguntarle a Robin por que no entendí esta vaina
:::
### ---> Contar usuarios por document_type
```
SELECT document_type, COUNT(*) AS total_usuarios FROM `users`GROUP BY document_type;
```
:::tip Pregunta para Robin.
Preguntarle a Robin por que tampoco entendí esta vuelta.
:::
### ---> Contar cuántos usuarios están desempleados.
```
SELECT COUNT(*) AS total_desempleados FROM `users` WHERE company IS NULL;
```
:::note
Para este ejercicio se utilizo el contar, para que recorriera toda la tabla, luego de ello, el AS que seria el pseudonimo de esa tabla, o mejor dichjo el nombre de la tabla temprotal que creo donde averiguo la infor.  finalmente se agrega el FROM para indicar en que tabla, donde la compañia es null, que como lo analizamos en ejercicios pasados, si no tenia compañia o ingreso concluiamos que estaba desempleado. 
:::
### ---> Calcular el promedio general de ingresos.
```
SELECT AVG(monthly_income) FROM `users`;
```
:::note
La función AVG ( ) ignora los valores NULL de la columna que este analizando. 
:::
### ---> Calcular el promedio de ingresos por role
```
SELECT role, AVG(monthly_income) FROM `users` GROUP BY role;
```
Ahora ya no estás consultando individuos, estás leyendo patrones

## Nivel 4 — Pensamiento analítico

### ---> Mostrar profesiones con más de 10 personas
```
SELECT profession, COUNT(*) AS Number_of_Users FROM `users` GROUP BY profession HAVING COUNT(profession) > 10;
```
:::warning
Siempre debe ir en este orden: primero se menciona el SELECT seguido del nombre de la columna donde se van a tomar los datos, luego se le indica que va a a contrar COUNT (*) es para que recorra toda las casillas incluyendo la NULL luego de ello se pone AS seguido el nombre que le vamos a dar a esa columna que vamos a crear donde se mostraran los datos (si son mas de dos palabras, separalas con guion bajo, es mejor) luego de eso, se le indica el lugar que es la tablas osea FROM `users` , luego se pone el GROUP BY  que es para que ponga todos en grupos que tengan algo en común. finalmente, finalmente el HAVING COUNT (y adentro del aprentesis el nombre de las profesiones que es lo que va a poner en grupos) y el valor si quiere mayor o menor. y cierra. 
:::
### ---> Mostrar la ciudad con más usuarios.
```
SEL
```





