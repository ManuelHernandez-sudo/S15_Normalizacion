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

>
