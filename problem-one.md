### Creating the Tables

##### Students
```
CREATE TABLE students (
	id SERIAL PRIMARY KEY,
	first_name TEXT,
	last_name TEXT,
	email TEXT,
	school_enrollment_date DATE
);
```
##### Professors
```
CREATE TABLE professors (
	id SERIAL PRIMARY KEY,
	first_name TEXT,
	last_name TEXT,
	department TEXT
);
```
##### Courses
```
CREATE TABLE courses (
	id SERIAL PRIMARY KEY,
	course_name TEXT,
	course_description TEXT,
	professor_id INTEGER,
	FOREIGN KEY (professor_id) REFERENCES professors(id)
);
```
##### Enrollments
```
CREATE TABLE enrollments (
	student_id INTEGER,
	course_id INTEGER,
	enrollment_date DATE,
	PRIMARY KEY (student_id, course_id),
	FOREIGN KEY (student_id) REFERENCES students(id),
	FOREIGN KEY (course_id) REFERENCES courses(id)
);
```
### Insert the data
##### Students Data
```
INSERT INTO students (first_name, last_name, email, school_enrollment_date)
VALUES
	('John', 'Marston', 'marston.john@redemption.ca', '2023-08-11'),
	('Arthur', 'Morgan', 'morgan.arthur@redemption.ca', '2022-01-12'),
	('Jack', 'Marston', 'marston.jack@redemption.ca', '2021-04-21'),
	('Niko', 'Bellic', 'bellic.niko@liberty.ca', '2024-01-15'),
	('Abigail', 'Roberts', 'roberts.abigail@redemption.ca', '2022-09-20');
```
##### Professors Data
```
INSERT INTO professors (first_name, last_name, department)
VALUES
	('Jim', 'Halpert', 'Advanced Math'),
	('Pam', 'Halpert', 'Art & Music'),
	('Mike', 'Scott', 'Science'),
	('Todd', 'Packer', 'Physical Education');
```
##### Courses Data
```
INSERT INTO courses (course_name, course_description, professor_id)
VALUES
	('Physics 101', 'Introduction to Physics', (SELECT id FROM professors WHERE first_name = 'Mike' AND last_name = 'Scott')),
	('Math 201', 'Introduction to Advanced Math', (SELECT id FROM professors WHERE first_name = 'Jim' AND last_name = 'Halpert')),
	('Art 101', 'Introduction to Painting', (SELECT id FROM professors WHERE first_name = 'Pam' AND last_name = 'Halpert'));
```
##### Enrollments Data
```
INSERT INTO enrollments (student_id, course_id, enrollment_date)
VALUES
	((SELECT id FROM students WHERE first_name = 'John' AND last_name = 'Marston'), (SELECT id FROM courses WHERE course_name = 'Physics 101' AND course_description = 'Introduction to Physics'), '2023-09-01'),
	((SELECT id FROM students WHERE first_name = 'Jack' AND last_name = 'Marston'), (SELECT id FROM courses WHERE course_name = 'Physics 101' AND course_description = 'Introduction to Physics'), '2023-09-01'),
	((SELECT id FROM students WHERE first_name = 'John' AND last_name = 'Marston'), (SELECT id FROM courses WHERE course_name = 'Art 101' AND course_description = 'Introduction to Painting'), '2024-01-01'),
	((SELECT id FROM students WHERE first_name = 'Niko' AND last_name = 'Bellic'), (SELECT id FROM courses WHERE course_name = 'Art 101' AND course_description = 'Introduction to Painting'), '2024-01-01'),
	((SELECT id FROM students WHERE first_name = 'Abigail' AND last_name = 'Roberts'), (SELECT id FROM courses WHERE course_name = 'Physics 101' AND course_description = 'Introduction to Physics'), '2023-09-01');
 ```
 ### Write SQL Queries
##### Query 1
```
SELECT first_name || ' ' || last_name AS full_names FROM students
JOIN enrollments ON students.id = enrollments.student_id
JOIN courses ON enrollments.course_id = courses.id
WHERE courses.course_name = 'Physics 101';
```
##### Query 2
```
SELECT professors.first_name || ' ' || professors.last_name AS full_names, courses.course_name
FROM courses
JOIN professors ON courses.professor_id = professors.id;
```
##### Query 3
- I had to research SELECT DISTINCT so that the courses with students in them wouldnt show up 2-3 different times (https://www.w3schools.com/sql/sql_distinct.asp)
```
SELECT DISTINCT courses.course_name FROM courses 
JOIN enrollments ON courses.id = enrollments.course_id;
```
##### Update Data
```
UPDATE students
SET email = 'john.marston@redemption.ca'
WHERE first_name = 'John' AND last_name = 'Marston';
```
##### Delete Data
```
DELETE FROM enrollments
WHERE student_id = 1 AND course_id = 1;
```