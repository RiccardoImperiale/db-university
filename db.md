DB_NAME: university

-Dipartimenti
-Corsi di laurea
-Corsi
-Insegnanti
-Appelli
-Studenti

## table name: departments
un dipartimento ha tanti corsi di laurea
un corso di laurea appartiene a un dipartimento

- id | BIGINT | INDEX | AI | UNIQUE | PK | NOTNULL
- name | VARCHAR(20) | NOTNULL
- location | VARCHAR(50) | NULL
- website | VARCHAR(255) | DEFAULT("N/A")

## table name: degree_course
un corso può avere tanti studenti
uno studente può essere iscritto a un solo corso di laurea

- id | BIGINT | INDEX | AI | UNIQUE | PK | NOTNULL
- department_id | FK
- name | VARCHAR(20) | NOTNULL
- courses_number | TINYINT
- tot_cfu | TINYINT

## table name: courses

- id | BIGINT | INDEX | AI | UNIQUE | PK | NOTNULL
- degree_course_id | FK
- name | VARCHAR(100) | NOTNULL 
- description | TEXT ! NULL
- start_date | DATE | NULL
- end_date | DATE | NULL
- cfu | TINYINT | NOTNULL

## table name: teachers
- id | BIGINT | INDEX | AI | UNIQUE | PK | NOTNULL
- name | VARCHAR(30) | NOTNULL 
- lastname | VARCHAR(30) | NOTNULL 
- email | VARCHAR(255) | NOTNULL 

## table name: course_teacher
- id | BIGINT | INDEX | AI | UNIQUE | PK | NOTNULL
- course_id | FK
- teacher_id | FK

## table name: examcall
- id | BIGINT | INDEX | AI | UNIQUE | PK | NOTNULL
- course_id | FK
- date | DATETIME
- room | VARCHAR(20)


## table name: students
- id | BIGINT | INDEX | AI | UNIQUE | PK | NOTNULL
- degree_course_id | FK
- registration_number | CHAR(15)
- name | VARCHAR(30) | NOTNULL
- lastname | VARCHAR(30) | NOTNULL
- email | VARCHAR(255) | NOTNULL

## table name: examcall_student
- id | BIGINT | INDEX | AI | UNIQUE | PK | NOTNULL
- examcall_id | FK
- student_id | FK
- vote | VARCHAR(10)