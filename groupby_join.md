## Group by: 

#### Contare quanti iscritti ci sono stati ogni anno

- SELECT YEAR(`students`.`enrolment_date`), COUNT(*)
FROM `students`
GROUP BY YEAR(`students`.`enrolment_date`);


#### Contare gli insegnanti che hanno l'ufficio nello stesso edificio

SELECT `teachers`.`office_address`, COUNT(*)
FROM `teachers`
GROUP BY `teachers`.`office_address`;


#### Calcolare la media dei voti di ogni appello d'esame

SELECT `exams`.`id`, AVG(`exam_student`.`vote`) AS `average_vote`
FROM `exams`
JOIN `exam_student` ON `exam_student`.`exam_id` = `exams`.`id`
GROUP BY `exam_student`.`exam_id`;


#### Contare quanti corsi di laurea ci sono per ogni dipartimento

SELECT `degrees`.`department_id`, COUNT(*) AS `course_number`
FROM `degrees`
GROUP BY `degrees`.`department_id`;

#
#

## Joins:

#### Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

SELECT `students`.`id`, `students`.`name`, `students`.`surname`, `students`.`degree_id`, `degrees`.`name`
FROM `students`
JOIN `degrees` ON `degrees`.`id` = `students`.`degree_id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';


#### Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

SELECT `degrees`.id, `degrees`.`name` AS `course_name`, `departments`.`name` AS `department`
FROM `degrees`
JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
WHERE `departments`.`name` = 'Dipartimento di Neuroscienze' AND `degrees`.`level` = 'magistrale';


#### Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

SELECT `courses`.`id`,`courses`.`name`, `teachers`.`name`,`teachers`.`surname`
FROM `courses`
JOIN `course_teacher` ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`
WHERE `teachers`.`id` = 44;


#### Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

SELECT `students`.`registration_number`, `students`.`surname`,`students`.`name`,`degrees`.`name` AS `degree`,`departments`.`name` AS `department`
FROM `students`
JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
JOIN `departments` ON `departments`.`id` = `degrees`.`department_id`
ORDER BY `students`.`surname` ASC, `students`.`name`;


#### Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

SELECT `degrees`.`name` AS `degree_name`, `courses`.`name` AS `course_name`, `teachers`.`name` AS `teacher_name`,`teachers`.`surname` AS `teacher_surname`
FROM `degrees`
JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id`
JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`;


#### Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

SELECT DISTINCT `teachers`.`name` AS `teacher_name`, `teachers`.`surname` AS `teacher_surname`,`departments`.`name`
FROM `teachers`
JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
JOIN `courses` ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `degrees` ON `degrees`.`id` = `courses`.`degree_id`
JOIN `departments` ON `departments`.`id` = `degrees`.`department_id`
WHERE `departments`.`name` = 'Dipartimento di Matematica';

<!-- distinct per non ripetere piú volte lo stesso risultato -->


#### BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.