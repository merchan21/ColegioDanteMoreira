# ColegioDanteMoreira
use ColegioDante

Create table estudiante
(
cedula_e varchar (20),
nombre varchar (25),
apellido varchar (25),
genero varchar (5),
fecha_n varchar(25),
direccion varchar (20),
telelefono varchar (11),
primary key (cedula_e)
);

insert into estudiante values
('1310446552','Ronald Fabian','Merchan Anchundia','M','05-08-1997','Los Geranios','0995830143'),
('1313769935','Bryan Carlos','Quimiz Nunez','M','15-11-1998','Los Bajos','0994828528'),
('1204765525','Karla Manuela','Delgado Garcia','F','23-02-2000','Montecristi','0985324569'),
('1321151817','Benjamin Franklin','Bautista Burgos','M','27-03-1999','San Pedro','0979632545');


Create table representante
(
cedula_r varchar (20),
cedula_e varchar (20),
nombre varchar (25),
apellido varchar (25),
genero_r varchar (15),
r_activo varchar (10),
correo varchar (35),
direccion varchar (20),
telefono varchar (11),
primary key (cedula_r)
);

insert into representante values
('3040556445','3050446552','Teofilo Roberto','Merchan Alay','M','Papa','luisr@gmail.com','Los Esteros','0983215645'),
('1212658824','1313769935','Bryan Javier','Quimiz Herrera','M','Papa','bryanj@hotmail.com','Los Bajos','0972350465'),
('1103654414','1204765525','Vanessa Maria','Garcia Polo','F','Mama','vane_m@gmail.com','Montecristi','0991023663'),
('1012543303','1321151817','Anthony Benjamin','Bautista Acosta','M','Ambos','thony_b@hotmai.com','San Pedro','0980112552');


Create table periodo_a
(
fecha_i_p varchar (20),
fecha_f_p varchar (20),
periodo int,
id_p int,
primary key (id_p)
);

insert into periodo_a values
('07-04-2021','08-10-2021',1,101),
('15-11-2021','08-04-2022',2,102);

Create table docente
(
cedula_d varchar (20),
nombre varchar (25),
apellido varchar (25),
direccion varchar (20),
telefono varchar (11),
fecha_ingreso varchar (20),
primary key (cedula_d)
);

insert into docente values
('1456324154','Ronald','Merchan','Los Bajos','0965322060','05-08-2018'),
('1586524369','Robert','Moreira','Tarqui','0961235896','03-05-2016'),
('1689745632','William','Delgado','La 13','0984568969','15-03-2015');


Create table curso
(
id_c varchar (10),
cedula_d varchar (20),
cedula_e varchar (20),
materia varchar (20),
id_p int,
primary key (id_c)
);

insert into curso values
('A1','1456324154','3050446552','Quimica',101),
('B2','1586524369','1313769935','Fisica',101),
('B3','1689745632','1204765525','Matematica',102);


Create table calificacion
(cedula_r varchar (20),
cedula_d varchar (20),
cedula_e varchar (20),
nota_1 int,
nota_2 int,
nota_recu int,
final varchar (20),
id_p int,
primary key (cedula_r)
);


insert into calificacion values
('1310556445','1456324154','1310446552',8,8,0,'Aprobado',101),
('1212658824','1586524369','1313769935',6,5,7,'Reprobado',101),
('1103654414','1689745632','1321151817',5,10,0,'Aprobado',102);


Select e.cedula_e as [Cedula del estudiante], e.nombre, e.apellido, e.genero, e.fecha_n, r.cedula_r, r.nombre, r.apellido, r.direccion, r.telefono from estudiante e, representante r where e.cedula_e = r.cedula_e

---PRIMERA CONSULTA---
----¿ESTUDIANTES REPRESENTADOS POR EL PAPÁ?-----
select e.cedula_e as [Cedula de estudiante], e.nombre as [Nombre de estudiante], e.apellido [Apellido del estudiante],
		e.genero as [Genero del estudiante], 
		r.nombre as [Nombre del representante], r.apellido as [Apellido del representante],
		r.genero_r as [Género], r_activo as [Representante activo] from estudiante as e inner join representante as r 
			on e.cedula_e=r.cedula_e where r_activo like 'Papa'

-----SEGUNDA CONSULTA---
----¿QUE ESTUDIANTES SON REPRESENTADOS POR LA MADRE?-----
select e.cedula_e as [Cedula de estudiante], e.nombre as [Nombre de estudiante], e.apellido [Apellido del estudiante],
		e.genero as [Genero del estudiante], 
		r.nombre as [Nombre del representante], r.apellido as [Apellido del representante],
		r.genero_r as [Género], r_activo as [Representante activo] from estudiante as e inner join representante as r 
			on e.cedula_e=r.cedula_e where r_activo like 'Mama'
---OPCIONAL CONSULTA---
----¿ESTUDIANTES REPRESENTADOS POR AMBOS?-----
select e.cedula_e as [Cedula de estudiante], e.nombre as [Nombre de estudiante], e.apellido [Apellido del estudiante],
		e.genero as [Genero del estudiante], 
		r.nombre as [Nombre del representante], r.apellido as [Apellido del representante],
		r.genero_r as [Género], r_activo as [Representante activo] from estudiante as e inner join representante as r 
			on e.cedula_e=r.cedula_e where r_activo like 'Ambos'

			--------- TERCERA CONSULTA----------
			--------------- ¿DATOS DEL TUTOR DE CADA CURSO EN CADA PERIODO Y ALUMNOS QUE ESTEN EN DICHO CURSO?-------------
Select d.nombre as [Nombre del Tutor], d.apellido as [Apellido del Tutor], d.cedula_d as [Cedula del Tutor], p.periodo as [Periodo Academico], c.id_c as [Curso], Count (c.cedula_d) as [Cantidad de Alumnnos en dicho Curso] from docente d, curso c, periodo_a p  where d.cedula_d = c.cedula_d and p.id_p = c.id_p
group by d.nombre, d.apellido, d.cedula_d, p.periodo, c.id_c, c.cedula_d
-------------------- Cantidad de Estudiantes en cada Periodo---------------------
SELECT p.periodo,
       COUNT(c.cedula_e) AS [CANTIDAD de Estudiantes]
FROM curso c
     INNER JOIN periodo_a p ON c.id_p = p.id_p group by c.cedula_e, p.periodo
-------------------------------------Cantidad de Estudiantes en Cada curso-----------------------------------
SELECT c.id_c as [Curso],
       COUNT(c.cedula_e) AS [CANTIDAD de Estudiantes]
FROM curso c group by c.id_c, c.cedula_e



--------- CUARTA CONSULTA----------
			--------------- ¿ESTUDIANTES APROBADOS Y REPROBADOS POR PERIODO?-------------


select e.nombre as [Nombre del Estudiante], e.apellido as [Apellido del Estudiante], c.nota_1 as [Primera Nota], c.nota_2 as [Segunda Nota], c.nota_recu as [Nota Recuperacion] , c.final as [Nota Final], p.periodo as [Periodo Academico] from estudiante e, calificacion c, periodo_a p where e.cedula_e = c.cedula_e and c.id_p = p.id_p 
------------------------Cantidad de Estudiantws Aprobados por Periodo-------------------------------
select p.periodo as [Periodo Academico], count (c.final) as [Cantidad de Estudiantes Aprobados por Periodo Academico] from calificacion c, periodo_a p where c.id_p = p.id_p and c.final = 'Aprobado' group by p.periodo, c.final
--------------------------Cantidad de Estudiantws Aprobados y Reprobados por Periodo---------------------------------------------------------------
select p.periodo as [Periodo Academico], count (c.final) as [Cantidad de Estudiantes Reprobados por Periodo Academico] from calificacion c, periodo_a p where c.id_p = p.id_p and c.final = 'Reprobado' group by p.periodo, c.final



select count (final) as [Aprobados y Reprobados] from calificacion where final = 'Aprobado' union all select count (*) from calificacion where final = 'Reprobado'
select * from calificacion where final = 'Aprobado'
select * from calificacion where final = 'Reprobado'
