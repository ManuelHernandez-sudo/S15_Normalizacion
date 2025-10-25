# 🧩 Ejercicio Práctico: Normalización hasta 3NF

## Contexto

Una universidad necesita registrar información sobre los cursos que imparte, los profesores que los enseñan y los estudiantes que los inscriben.  
Actualmente, la información se guarda en una única tabla llamada **Registro_Cursos**, con la siguiente estructura:

| ID_Registro | Curso             | Profesor     | Email_Profesor     | Estudiantes              | Emails_Estudiantes                    | Créditos | Facultad     |
|--------------|------------------|--------------|--------------------|--------------------------|--------------------------------------|-----------|--------------|
| 1            | Bases de Datos    | Ana Torres   | ana@uni.edu        | Luis, María, Jorge       | luis@uni.edu, maria@uni.edu, jorge@uni.edu | 4         | Ingeniería   |
| 2            | Programación Web  | Carlos López | carlos@uni.edu     | Pedro, Ana, Lucía        | pedro@uni.edu, ana@uni.edu, lucia@uni.edu | 5         | Ingeniería   |

---

## 🔹 Análisis inicial (0NF)

### Problemas identificados

1. **Datos no atómicos:** Los campos *Estudiantes* y *Emails_Estudiantes* contienen múltiples valores separados por comas.  
2. **Redundancia:** Si un profesor enseña varios cursos o un estudiante se inscribe en varios cursos, su información se repetirá.  
3. **Dependencias múltiples:** Atributos como *Facultad* o *Créditos* dependen del curso, no del registro completo.  
4. **Dificultad para actualizar:** Si cambia el correo de un estudiante o profesor, habría que modificar varias filas.

> 💡 Por tanto, la tabla no cumple los principios de una base de datos relacional bien estructurada.

---

## 🔸 Primera Forma Normal (1NF)

### Reglas

Una tabla está en **1NF** si:
1. Todos los atributos contienen **valores atómicos** (sin listas o conjuntos).  
2. No hay grupos repetitivos de columnas.  
3. Cada registro es **único** y puede identificarse por una clave primaria.

### Transformación

Se separan los valores múltiples de *Estudiantes* y *Emails_Estudiantes* en filas independientes.

### Estructura resultante (1NF)

| ID_Registro | Curso             | Profesor     | Email_Profesor     | Estudiante | Email_Estudiante       | Créditos | Facultad     |
|--------------|------------------|--------------|--------------------|-------------|------------------------|-----------|--------------|
| 1            | Bases de Datos    | Ana Torres   | ana@uni.edu        | Luis        | luis@uni.edu           | 4         | Ingeniería   |
| 1            | Bases de Datos    | Ana Torres   | ana@uni.edu        | María       | maria@uni.edu          | 4         | Ingeniería   |
| 1            | Bases de Datos    | Ana Torres   | ana@uni.edu        | Jorge       | jorge@uni.edu          | 4         | Ingeniería   |
| 2            | Programación Web  | Carlos López | carlos@uni.edu     | Pedro       | pedro@uni.edu          | 5         | Ingeniería   |
| 2            | Programación Web  | Carlos López | carlos@uni.edu     | Ana         | ana@uni.edu            | 5         | Ingeniería   |
| 2            | Programación Web  | Carlos López | carlos@uni.edu     | Lucía       | lucia@uni.edu          | 5         | Ingeniería   |

✅ Ahora todos los datos son atómicos, pero persisten **redundancias** y **dependencias parciales**.

---

## 🔸 Segunda Forma Normal (2NF)

### Reglas

Una tabla está en **2NF** si:
1. Cumple con 1NF.  
2. Todos los campos **dependen completamente de la clave primaria**, no solo de una parte (sin dependencias parciales).

### Análisis

En la tabla 1NF, la **clave primaria compuesta** podría ser *(Curso, Estudiante)*.  
Sin embargo:
- *Profesor*, *Email_Profesor*, *Créditos* y *Facultad* dependen **solo del curso**, no del estudiante.  
- Esto crea **dependencias parciales**.

### Transformación

Se separan los datos en tres tablas.

#### Tabla: CURSO
| ID_Curso | Nombre_Curso      | Créditos | Facultad     | ID_Profesor |
|-----------|------------------|-----------|--------------|--------------|
| 1         | Bases de Datos    | 4         | Ingeniería   | 1            |
| 2         | Programación Web  | 5         | Ingeniería   | 2            |

#### Tabla: PROFESOR
| ID_Profesor | Nombre_Profesor | Email_Profesor     |
|--------------|----------------|--------------------|
| 1            | Ana Torres     | ana@uni.edu        |
| 2            | Carlos López   | carlos@uni.edu     |

#### Tabla: ESTUDIANTE_CURSO
| ID_Curso | Nombre_Estudiante | Email_Estudiante   |
|-----------|------------------|--------------------|
| 1         | Luis             | luis@uni.edu       |
| 1         | María            | maria@uni.edu      |
| 1         | Jorge            | jorge@uni.edu      |
| 2         | Pedro            | pedro@uni.edu      |
| 2         | Ana              | ana@uni.edu        |
| 2         | Lucía            | lucia@uni.edu      |

✅ Ahora cada tabla depende completamente de su clave primaria.

---

## 🔸 Tercera Forma Normal (3NF)

### Reglas

Una tabla está en **3NF** si:
1. Cumple con 2NF.  
2. No existen **dependencias transitivas**, es decir, ningún campo depende de otro atributo que no sea la clave primaria.

### Análisis

En la tabla **ESTUDIANTE_CURSO**, los campos *Nombre_Estudiante* y *Email_Estudiante* describen a los estudiantes, no a la relación del curso.  
Por tanto, se debe crear una tabla independiente para **estudiantes**.

### Transformación (3NF final)

#### Tabla: PROFESOR
| ID_Profesor | Nombre_Profesor | Email_Profesor     |
|--------------|----------------|--------------------|
| 1            | Ana Torres     | ana@uni.edu        |
| 2            | Carlos López   | carlos@uni.edu     |

#### Tabla: CURSO
| ID_Curso | Nombre_Curso      | Créditos | Facultad     | ID_Profesor |
|-----------|------------------|-----------|--------------|--------------|
| 1         | Bases de Datos    | 4         | Ingeniería   | 1            |
| 2         | Programación Web  | 5         | Ingeniería   | 2            |

#### Tabla: ESTUDIANTE
| ID_Estudiante | Nombre_Estudiante | Email_Estudiante   |
|----------------|------------------|--------------------|
| 1              | Luis             | luis@uni.edu       |
| 2              | María            | maria@uni.edu      |
| 3              | Jorge            | jorge@uni.edu      |
| 4              | Pedro            | pedro@uni.edu      |
| 5              | Ana              | ana@uni.edu        |
| 6              | Lucía            | lucia@uni.edu      |

#### Tabla: INSCRIPCIÓN
| ID_Curso | ID_Estudiante |
|-----------|----------------|
| 1         | 1              |
| 1         | 2              |
| 1         | 3              |
| 2         | 4              |
| 2         | 5              |
| 2         | 6              |

✅ Ahora no hay redundancia, cada tabla representa una sola entidad y las relaciones se manejan mediante claves foráneas.

---

## 🧠 Conclusión

El proceso de normalización transformó una tabla desorganizada y redundante en un **modelo relacional limpio y eficiente**:

- En **0NF**, los datos estaban agrupados en una sola tabla con valores repetidos y no atómicos.  
- En **1NF**, se separaron los valores múltiples en filas individuales.  
- En **2NF**, se eliminaron las dependencias parciales al separar cursos, profesores y estudiantes.  
- En **3NF**, se eliminaron las dependencias transitivas, creando una estructura modular y fácil de mantener.

### Beneficios del modelo final:
- Evita redundancias.
- Facilita las actualizaciones.
- Garantiza integridad referencial.
- Representa correctamente las relaciones entre **Cursos, Profesores y Estudiantes**.
