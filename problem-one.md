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