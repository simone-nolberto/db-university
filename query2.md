## Group by
- Contare quanti iscritti ci sono stati ogni anno
  - SELECT COUNT(`id`), enrolment_date FROM `students` GROUP BY enrolment_date;


- Contare gli insegnanti che hanno l'ufficio nello stesso edificio
  - SELECT COUNT(`id`), office_address FROM `teachers` GROUP BY office_address;   


- Calcolare la media dei voti di ogni appello d'esame
  - SELECT AVG(vote), exam_id FROM `exam_student` GROUP BY exam_id;

- Contare quanti corsi di laurea ci sono per ogni dipartimento
  - SELECT COUNT(`id`), department_id FROM `degrees` GROUP BY department_id;


## Joins
- Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia (id=53)
  - SELECT students.name, students.surname, degree_id FROM `students` INNER JOIN `degrees` ON `degrees`.id = `students`.degree_id WHERE degrees.id = 53 (totali 68);
  
- Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze(id=7)
  - SELECT * FROM `degrees` INNER JOIN `departments` ON departments.id = degrees.department_id WHERE departments.id = 7 and degrees.level = 'magistrale' (totale 1);
  
- Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
  - SELECT `course_id`, teacher_id FROM `course_teacher` INNER JOIN `teachers` ON teachers.id = course_teacher.teacher_id WHERE `teachers`.`id` = 44 (totale 11);

- Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
  - SELECT `students`.`name`, `students`.`surname`, `degrees`.`name` AS 'Iscritto in', `departments`.`name` AS 'Dipartimento'
    FROM `students`
    INNER JOIN `degrees` ON `degrees`.`id` = students.`degree_id`
    INNER JOIN `departments` ON `departments`.`id` = degrees.`department_id`  
    ORDER BY `students`.`name` ASC, `students`.`surname` ASC;
    (totale 5k);

- Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
  - SELECT `courses`.name, `teachers`.name , `teachers`.surname, `degrees`.name
    FROM `course_teacher`
    INNER JOIN `courses` ON courses.id = course_teacher.course_id 
    INNER JOIN `teachers` ON teachers.id = course_teacher.teacher_id
    INNER JOIN `degrees` ON degrees.id = courses.degree_id
    ORDER BY `degrees`.name;


- Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (5)
  - SELECT `teachers`.name , `teachers`.surname, `departments`.id, `departments`.name
    FROM `course_teacher`
    INNER JOIN `courses` ON courses.id = course_teacher.course_id 
    INNER JOIN `teachers` ON teachers.id = course_teacher.teacher_id
    INNER JOIN `degrees` ON degrees.id = courses.degree_id
    INNER JOIN `departments` ON departments.id = degrees.department_id
    WHERE `departments`.id = 5;


- BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.