# QUERY GROUP BY:

## 1. Contare quanti iscritti ci sono stati ogni anno
```
SELECT COUNT(`id`) AS `Iscritti_per_anno`, YEAR(`enrolment_date`)
FROM `students`
GROUP BY YEAR(`enrolment_date`);
```

## 2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
```
SELECT COUNT(`id`) AS `numero_insegnanti`, `office_address`
FROM `teachers`
GROUP BY `office_address`
```

## 3. Calcolare la media dei voti di ogni appello d'esame
```
SELECT AVG(`exam_student`.`vote`) AS `media_voti`, `exams`.`date` AS `data_esame`, `courses`.`name` AS `nome_corso`
FROM `exam_student`
JOIN `exams` ON `exams`.`id` = `exam_student`.`exam_id`
JOIN `courses` ON `courses`.`id` = `exams`.`course_id`
GROUP BY `exams`.`id`
```

## 4. Contare quanti corsi di laurea ci sono per ogni dipartimento
```
SELECT COUNT(`degrees`.`id`) AS `n_corsi`, `departments`.`name` AS `nome_dipartimento` 
FROM `degrees`
JOIN `departments` ON `departments`.`id` = `degrees`.`department_id`
GROUP BY `departments`.`id`
ORDER BY `n_corsi` ASC
```