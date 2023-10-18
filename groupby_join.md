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

