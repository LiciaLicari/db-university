## query for database

## selezionare tutti gli studenti nati nel 1990 (160)

- SELECT * FROM `students` WHERE YEAR(date_of_birth) = 1990;

## Selezionare tutti i corsi che valgono più di 10 crediti (479)

- SELECT * FROM `courses` WHERE `cfu` > 10;

## Selezionare tutti gli studenti che hanno più di 30 anni

- SELECT * FROM students WHERE YEAR(date_of_birth) < 1993;

## Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di laurea (286)

- SELECT * FROM `courses` WHERE `period`='I semestre' AND `year` = 1;

## Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del 20/06/2020 (21)

- SELECT * FROM `exams` WHERE `date` = '2020-06-20' AND `hour` > '14:00:00';

## Selezionare tutti i corsi di laurea magistrale (38)

- SELECT * FROM `degrees` WHERE `level` = 'magistrale';

## Da quanti dipartimenti è composta l'università? (12)

- SELECT COUNT(`id`) FROM `departments`;

## Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

- SELECT * FROM `teachers` WHERE `phone` IS NULL;





## Join 1:
Selezionare tutti i corsi del Corso di Laurea in Informatica (22)

SELECT `courses`.`id`, `courses`.`name`, `courses`.`period`, `courses`.`year`, `courses`.`cfu`, `courses`.`website`, `degrees`.`name` AS `degree_name`
    FROM `courses`
    JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
    WHERE `degrees`.`name` = 'Corso di Laurea in Informatica';

## Join 2:
Selezionare le informazioni sul corso con id = 144, con tutti i relativi appelli d’esame

SELECT `courses`.`id` AS `course_id`, `courses`.`name`, `courses`.`description`, `courses`.`period`, `courses`.`cfu`, `courses`.`year`, `courses`.`website`, `exams`.`id` AS `exam_id`, `exams`.`date`, `exams`.`hour`, `exams`.`location`, `exams`.`address`
FROM `courses`
JOIN `exams` ON `courses`.`id` = `exams`.`course_id`
WHERE `courses`.`id` = 144;


## Join 3:
Selezionare a quale dipartimento appartiene il Corso di Laurea in Diritto
dell'Economia (Dipartimento di Scienze politiche, giuridiche e studi internazionali)

SELECT `departments`.* 
FROM `departments` 
JOIN `degrees` ON `departments`.`id` = `degrees`.`department_id` 
WHERE `degrees`.`name` = 'Corso di Laurea in Diritto dell\'Economia';


## Join 4:
Selezionare tutti gli appelli d'esame del Corso di Laurea Magistrale in Fisica del primo anno

SELECT `courses`.`name`, `courses`.`period`, `courses`.`cfu`, `exams`.`date`, `exams`.`location`, `exams`.`address` 
FROM `degrees` 
JOIN `courses` ON `courses`.`degree_id` = `degrees`.`id` 
JOIN `exams` ON `exams`.`course_id` = `courses`.`id` 
WHERE `courses`.`year` = 1 AND `degrees`.`name` = 'Corso di Laurea Magistrale in Fisica';


## Join 5:
Selezionare tutti i docenti che insegnano nel Corso di Laurea in Lettere (21)

SELECT DISTINCT `teachers`.*
FROM `teachers`     
JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
JOIN `courses` ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Lettere'
ORDER BY `teachers`.`surname` ASC;


## Join 6:
Selezionare il libretto universitario di Mirco Messina (matricola n. 620320)

SELECT `students`.`name`, `students`.`surname`, `students`.`registration_number`, `courses`.`id`, `courses`.`name`, `exams`.`date`, `exam_student`.`vote`
FROM `exam_student`
JOIN `exams` ON `exam_student`.`exam_id` =  `exams`.`id`
JOIN `students` ON `exam_student`.`student_id` = `students`.`id`
JOIN `courses` ON `exams`.`course_id` = `courses`.`id`
WHERE `students`.`name` = 'Mirco' AND `students`.`surname` = 'Messina'
AND `exam_student`.`vote` >= 18;