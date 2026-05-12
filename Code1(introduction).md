SQL Commands
1.1
Create Database MySQL:
CREATE DATABASE AcademicDB;
USE AcademicDB;

1.2 Create Tables Student Table
CREATE TABLE Student ( 
StudentID INT PRIMARY KEY,
Name VARCHAR(100),
Email VARCHAR(100) UNIQUE, 
Age INT,
Address VARCHAR(200)
);

Instructor Table:
CREATE TABLE Instructor ( 
InstructorID INT PRIMARY KEY, 
Name VARCHAR(100),
Email VARCHAR(100) UNIQUE,
Department VARCHAR(100)
);

Course Table
CREATE TABLE Course ( 
CourseID INT PRIMARY KEY,
CourseName VARCHAR(100), 
Credits INT,
InstructorID INT,
FOREIGN KEY (InstructorID) REFERENCES Instructor(InstructorID)
);

Enrollment Table
CREATE TABLE Enrollment ( 
EnrollmentID INT PRIMARY KEY, 
StudentID INT,
CourseID INT, 
EnrollmentDate DATE,
FOREIGN KEY (StudentID) REFERENCES Student(StudentID), 
FOREIGN KEY (CourseID) REFERENCES Course(CourseID)
);

1.3 Insert Data into table:
INSERT INTO Student VALUES
(1,'Rahul','rahul@gmail.com',20,'Delhi'),
(2,'Anita','anita@gmail.com',21,'Mumbai');

INSERT INTO Instructor VALUES
(101,'Dr. Mehta','mehta@college.com','Computer Science'), 
(102,'Prof. Rao','rao@college.com','Mathematics');

INSERT INTO Course VALUES 
(501,'DBMS',4,101),
(502,'Calculus',3,102);

INSERT INTO Enrollment VALUES 
(1001,1,501,'2024-07-01'),
(1002,1,502,'2024-07-05'),
(1003,2,501,'2024-07-10');

List all students
SELECT * FROM Student;

List all courses taught by a specific instructor 
SELECT CourseName
FROM Course
WHERE InstructorID = 101;

Find all courses a student is enrolled in 
SELECT Student.Name, 
Course.CourseName 
FROM Enrollment
JOIN Student ON Enrollment.StudentID = Student.StudentID 
JOIN Course ON Enrollment.CourseID = Course.CourseID 
WHERE Student.StudentID = 1;