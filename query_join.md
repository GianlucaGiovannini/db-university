# QUERY JOINS:

## 1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
```
SELECT `students`.`name`, `students`.`surname`, `students`.`registration_number`
    FROM `students`
    JOIN `degrees` ON `degrees`.`id` = `students`.`degree_id`
    WHERE `degrees`.`name` = 'Corso di Laurea in Economia'
```

## 2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
```
SELECT `degrees`.`name` AS `nome_corso`, `degrees`.`level`, `departments`.`name` AS `dipartimento`
FROM `degrees`
JOIN departments ON `departments`.`id` = `degrees`.`department_id`
WHERE `departments`.`name` = 'Dipartimento di Neuroscienze' AND `degrees`.`level` = 'magistrale'
```

## 3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
```
SELECT `teachers`.`name`, `teachers`.`surname`, `courses`.`name` AS `nome_corso`
FROM `course_teacher`
JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`
JOIN `courses` ON `courses`.`id` = `course_teacher`.`course_id`
WHERE `teachers`.`name` = 'Fulvio' AND `teachers`.`surname` = 'Amato' 
```

## 4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
```
SELECT `students`.`name`, `students`.`surname`, `students`.`registration_number`, `degrees`.`name` AS `corso_laurea`, `departments`.`name` AS `dipartimento`
FROM `students`
JOIN `degrees` ON `degrees`.`id` = `students`.`degree_id` 
JOIN departments ON `departments`.`id` = `degrees`.`department_id` 
ORDER BY `students`.`surname`, `students`.`name`
```

## 5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
```
SELECT `degrees`.`id` AS `id_corso_laurea`, `degrees`.`name` AS `nome_corso_laurea`, `degrees`.`level`, `courses`.`id` AS `id_corso`, `courses`.`name` AS `nome_corso`, `courses`.`cfu`, `courses`.`period`,  `teachers`.`name`, `teachers`.`surname`, `teachers`.`id` AS `id_insegnante`
  FROM `degrees`
    JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id`
    JOIN `course_teacher` ON `course_teacher`.`course_id` = `courses`.`id`
    JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`
    ORDER BY `degrees`.`name`
```

## 6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
```
SELECT DISTINCT `teachers`.`id` AS `id_insegnante`, `teachers`.`name` AS `nome_insegnante`, `teachers`.`surname` AS `cognome_insegnante`, `departments`.`name` AS `dipartimento`
FROM `course_teacher`
    JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`
    JOIN `courses` ON `courses`.`id` = `course_teacher`.`course_id`
    JOIN `degrees` ON `degrees`.`id` = `courses`.`degree_id`
    JOIN `departments` ON `departments`.`id` = `degrees`.`department_id`
    WHERE `departments`.`name` = 'Dipartimento di Matematica';
```

## 7. BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per superare ciascuno dei suoi esami
```
     SELECT `students`.`id`, `students`.`name` AS 'nome', `students`.`surname` AS 'cognome', `courses`.`name` AS 'corso', MAX(`exam_student`.`vote`) AS 'voto_maggiore', COUNT(`vote`) AS 'Tentativi'
    FROM `exam_student`
    JOIN `students` ON `students`.`id` = `exam_student`.`student_id`
    JOIN `exams` ON `exams`.`id` = `exam_student`.`exam_id`
    JOIN `courses` ON `courses`.`id` = `exams`.`course_id`
    GROUP BY `students`.`id`, `courses`.`id`
    HAVING `voto_maggiore` >= 18
```

