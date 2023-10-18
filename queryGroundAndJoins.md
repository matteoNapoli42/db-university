# Group by:

- ## Contare quanti iscritti ci sono stati ogni anno
    - SELECT COUNT(*) AS `enrolled_students`, YEAR(`enrolment_date`) FROM `students` GROUP BY YEAR(`enrolment_date`);

- ## Contare gli insegnanti che hanno l'ufficio nello stesso edificio
    - SELECT COUNT(*) AS `teachers_count`, `office_address` FROM `teachers` GROUP BY `office_address`;

- ## Calcolare la media dei voti di ogni appello d'esame
    - SELECT AVG(`vote`) AS `Media voti` , exam_id AS `esame` FROM `exam_student` GROUP BY `exam_id`;

- ## Contare quanti corsi di laurea ci sono per ogni dipartimento
    - SELECT COUNT(*) AS `degrees_count`, `department_id` FROM `degrees` GROUP BY `department_id`;


# Joins:

- ## Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
    - SELECT * FROM `students` JOIN `degrees` WHERE `degrees`.`name` = 'Corso di Laurea in Economia';

- ## Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
    - SELECT  *
      FROM `degrees`
      JOIN `departments` ON `department_id` = `departments`.`id`
      WHERE `departments`.`name` = 'Dipartimento di Neuroscienze' AND `degrees`.`level` = 'magistrale'

- ## Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
    -   SELECT `courses`.`name`, `teachers`.`name`, `teachers`.`surname`
        FROM `course_teacher`
        JOIN `courses` ON `course_id` = `courses`.`id`
        JOIN `teachers` ON `teacher_id` = `teachers`.`id`
        WHERE `teacher_id` = 44;

- ## Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
    - SELECT * FROM `students` JOIN `degrees` ON `degree_id` = `degrees`.`id` JOIN `departments` ON `department_id` = `departments`.`id` ORDER BY `students`.`name`, `students`.`surname` ASC;

- ## Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
    - SELECT `degrees`.`id`, `degrees`.`name` ,`degrees`.`level`,`courses`.`id` , `courses`.`name` ,`teachers`.`id`,`teachers`.`name`,`teachers`.`surname` 
    FROM `courses` 
    JOIN `course_teacher` 
    JOIN `teachers` 
    JOIN `degrees` 
    ON `courses`.`id` = `course_teacher`.`course_id` 
    AND `course_teacher`.`teacher_id` = `teachers`.`id`
    AND `degree_id`=`degrees`.`id`;

- ## Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
    - SELECT DISTINCT `teachers`.`id`,`teachers`.`name`,`teachers`.`surname`, `departments`.`id`, `departments`.`name` 
      FROM `courses` 
      JOIN `course_teacher`
      JOIN `teachers`
      JOIN `degrees`
      JOIN `departments`
      ON `courses`.`id` = `course_teacher`.course_id
      AND `course_teacher`.`teacher_id` = `teachers`.`id`
      AND `degree_id`= `degrees`.`id`
      AND `department_id`= `departments`.`id`
      WHERE `departments`.`name` = 'Dipartimento di Matematica'

- ## BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.
     - SELECT COUNT(`exam_id`) , `students`.`name` , `students`.`surname` , `courses`.`name` , `vote` FROM `exam_student` JOIN `students` ON `student_id` = `students`.`id` JOIN `exams` ON `exam_id` = `exams`.`id` JOIN `courses` ON `course_id` = `courses`.`id` WHERE `vote` > 18 GROUP BY `students`.`name`, `students`.`surname`, `courses`.`name`, `vote`;
