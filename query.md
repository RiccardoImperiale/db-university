1. Selezionare tutti gli studenti nati nel 1990 (160)
   
```sql
SELECT * FROM `students` WHERE `date_of_birth` LIKE '1990%';
```

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

```sql
SELECT * FROM `courses` WHERE `cfu` > 10;
```

3. Selezionare tutti gli studenti che hanno più di 30 anni

```sql  
SELECT * FROM `students` WHERE TIMESTAMPDIFF(YEAR, `date_of_birth`, CURDATE()) > 30;
```

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)

```sql  
SELECT * FROM `courses` WHERE `period` = 'I semestre' AND `year` = 1;
```

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)

```sql  
SELECT * FROM `exams` WHERE `date` = '2020-06-20' AND `hour` >= '14:00:00';
```

6. Selezionare tutti i corsi di laurea magistrale (38)

```sql  
SELECT * FROM `degrees` WHERE `level` = 'magistrale';
```

7. Da quanti dipartimenti è composta l'università? (12)

```sql  
SELECT COUNT(*) AS `departments_number` FROM `departments`;
```

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

```sql  
SELECT COUNT(*) AS `teachers_without_phone` 
FROM `teachers` 
WHERE `phone` IS NULL OR `phone` = '';
```


## Group by
1. Contare quanti iscritti ci sono stati ogni anno
   
```sql
SELECT COUNT(*) as `number_of_members`, `year`
FROM `courses`
GROUP BY `year`;
```

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

```sql
SELECT COUNT(*) as `number_of_teachers`, `office_address`
FROM `teachers`
GROUP BY `office_address`;
```

3. Calcolare la media dei voti di ogni appello d'esame
   
```sql
SELECT `id`, AVG(`cfu`) as `average_cfu`
FROM `courses`
GROUP BY `id`;
```

4. Contare quanti corsi di laurea ci sono per ogni dipartimento

```sql
SELECT `department_id`, COUNT(*) as `numbers_of_courses`
FROM `degrees`
GROUP BY `department_id`;
```

## Joins
1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

```sql
SELECT `students`.`id` as `student_id`, `students`.`name`, `students`.`surname`, `degrees`.`name`
FROM `students`
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';
```

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

```sql
SELECT `departments`.`id`, `departments`.`name`, `degrees`.`level`
FROM `departments`
JOIN `degrees` ON `departments`.`id` = `degrees`.`department_id`
WHERE `departments`.`name` = 'Dipartimento di Neuroscienze' AND `degrees`.`level` = 'magistrale';
```

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
   
```sql
SELECT `teachers`.`id`, `teachers`.`name`, `teachers`.`surname`, `courses`.`name`
FROM `courses`
JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`
WHERE `teachers`.`id` = 44;
```

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

```sql
SELECT `students`.`id`, `students`.`surname`, `students`.`name`, `degrees`.`name`, `degrees`.`level`, `degrees`.`website`
FROM `students`
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
JOIN `departments` ON `departments`.`id` = `degrees`.`department_id`
ORDER BY `students`.`surname` ASC, `students`.`name` ASC;
```

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

```sql
SELECT `degrees`.`id` AS `degree_id`, `degrees`.`name`, `degrees`.`level`, `degrees`.`address`, `degrees`.`email`, `courses`.`id` AS `course_id`, `courses`.`name`, `courses`.`description`, `courses`.`cfu`, `teachers`.`id` AS `teacher_id`, `teachers`.`name`, `teachers`.`surname`
FROM `degrees`
JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id`
JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`
```

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

```sql
SELECT `teachers`.`id` AS `teacher_id`, `teachers`.`name`, `teachers`.`surname`, `departments`.`name`
FROM `departments`
JOIN `degrees` ON `degrees`.`department_id` = `departments`.`id`
JOIN `courses` ON `courses`.`degree_id` = `degrees`.`id`
JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`
WHERE `departments`.`name` = 'Dipartimento di Matematica';
```

### BONUS: 
Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.