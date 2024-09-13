1. Selezionare tutti gli studenti nati nel 1990 (160)
2. Selezionare tutti i corsi che valgono più di 10 crediti (479)
3. Selezionare tutti gli studenti che hanno più di 30 anni
4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
   laurea (286)
5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
   20/06/2020 (21)
6. Selezionare tutti i corsi di laurea magistrale (38)
7. Da quanti dipartimenti è composta l'università? (12)
8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)
9. Inserire nella tabella degli studenti un nuovo record con i propri dati (per il campo
   degree_id, inserire un valore casuale)
10. Cambiare il numero dell’ufficio del professor Pietro Rizzo in 126
11. Eliminare dalla tabella studenti il record creato precedentemente al punto 9

<!-- PRIMA QUERY -->

SELECT \*
FROM `students`
WHERE YEAR(`date_of_birth`) = 1990;

<!-- SECONDA QUERY -->

SELECT \*
FROM `courses`
WHERE `cfu` > 10;

<!-- TERZA QUERY -->

SELECT \*
FROM `students`
WHERE TIMESTAMPDIFF(YEAR, `date_of_birth`, CURRENT_DATE ) > 30;

<!-- QUARTA QUERY -->

SELECT \*
FROM `courses`
WHERE `year` = '1' AND `period` = 'I semestre';

<!-- QUINTA QUERY -->

SELECT \*
FROM `exams`
WHERE `hour` >= '14:00:00' AND DATE = "2020-06-20";

<!-- SESTA QUERY -->

SELECT \*
FROM `degrees`
WHERE `level` = 'magistrale';

<!-- SETTIMA QUERY -->

SELECT COUNT(\*) AS 'NumberOfDepartments'
FROM `departments`;

<!-- OTTAVA QUERY -->

SELECT COUNT(\*) as 'teachersWithoutPhone'
FROM `teachers`
WHERE `phone` IS null;

<!-- NONA QUERY -->

INSERT INTO `students`(`degree_id`, `name`, `surname`, `date_of_birth`, `fiscal_code`, `enrolment_date`, `registration_number`, `email`) VALUES ('75','Simone','Durso','1998-10-21','Codice fiscale a caso','2019-11-30','625000','simone.durso98@gmail.com');

<!-- DECIMA QUERY -->

UPDATE `teachers` SET `office_number`='126'
WHERE `id` = '58';

<!-- UNDICESIMA QUERY -->

DELETE
FROM `students`
WHERE `id` = '5003';

<!-- QUERY CON GROUP BY -->

1. Contare quanti iscritti ci sono stati ogni anno
2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
3. Calcolare la media dei voti di ogni appello d'esame
4. Contare quanti corsi di laurea ci sono per ogni dipartimento

<!-- PRIMA QUERY CON GROUP BY -->

SELECT COUNT(\*) AS 'numero_iscritti', YEAR(`enrolment_date`) as 'anno'
FROM `students`
GROUP BY `anno`;

<!-- SECONDA QUERY CON GROUP BY -->

SELECT COUNT(\*) as 'insegnanti', `office_address`
FROM `teachers`
GROUP BY `office_address`;

<!-- TERZA QUERY CON GROUP BY -->

SELECT AVG(`vote`) as 'media_voti', `exam_id`
FROM `exam_student`
GROUP BY `exam_id`;

<!-- QUARTA QUERY CON GROUP BY -->

SELECT COUNT(\*) as `numero_corsi`, `department_id`
FROM `degrees`
GROUP BY `department_id`;

<!-- QUERY CON JOIN -->

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
   Neuroscienze
3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
   sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
   nome
5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
6. Selezionare tutti i docenti che insegnano nel Dipartimento di
   Matematica (54)
7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
   per ogni esame, stampando anche il voto massimo. Successivamente,
   filtrare i tentativi con voto minimo 18.

<!-- PRIMA QUERY CON JOIN -->

SELECT `students`.`name` as `nome_studente`,`students`.`surname` as `cognome_studente`, `degrees`.`name` as `nome_corso`
FROM `students`
JOIN `degrees` ON `degrees`.`id` = `students`.`degree_id`
WHERE `degrees`.`name` = "Corso di Laurea in Economia";

<!-- SECONDA QUERY CON JOIN -->

SELECT `degrees`.`name` as `nome_corso`,`degrees`.`level` as `livello_laurea`, `departments`.`name` as `nome_dipartimento`
FROM `degrees`
JOIN `departments` ON `departments`.`id` = `degrees`.`department_id`
WHERE `degrees`.`level` = "magistrale"
AND `departments`.`name` = "Dipartimento di Neuroscienze";

<!-- TERZA QUERY CON JOIN -->

SELECT `courses`.`name` as `nome_corso`, `teachers`.`name` as `nome_insegnante`, `teachers`.`surname` as `cognome_insegnante`
FROM `courses`
JOIN `course_teacher` ON `course_teacher`.`course_id` = `courses`.`id`
JOIN `teachers`ON `teachers`.`id` = `course_teacher`.`teacher_id`
WHERE `teachers`.`id` = 44;

<!-- QUARTA QUERY CON JOIN -->

SELECT `students`.`name`, `students`.`surname`, `degrees`.`name` as `nome_corso_di_Laurea`,`departments`.`name` as `nome_dipartimento`
FROM `students`
JOIN `degrees` ON `degrees`.`id`= `students`.`degree_id`
JOIN `departments` ON `departments`.`id` = `degrees`.`department_id`
ORDER BY `students`.`surname` ASC, `students`.`name` ASC;
