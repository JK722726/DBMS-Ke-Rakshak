5.1 Creating Base Tables
CREATE TABLE Student ( StudentID INT PRIMARY KEY, Name VARCHAR(100),
Email VARCHAR(100), Age INT,
Address VARCHAR(200)
);

CREATE TABLE Course ( CourseID INT PRIMARY KEY,
CourseName VARCHAR(100), Credits INT, InstructorID INT
);

CREATE TABLE Instructor ( InstructorID INT PRIMARY KEY, Name VARCHAR(100),
Email VARCHAR(100),
Department VARCHAR(100)
);

CREATE TABLE Enrollment ( EnrollmentID INT PRIMARY KEY, StudentID INT,
CourseID INT, EnrollmentDate DATE,
FOREIGN KEY(StudentID) REFERENCES Student(StudentID),
FOREIGN KEY(CourseID) REFERENCES Course(CourseID)
);

5.2 Creating a View for Student-Course Report
CREATE VIEW StudentCourseView AS SELECT
S.StudentID, S.Name AS StudentName, C.CourseName, C.Credits,
I.Name AS InstructorName FROM Student S
JOIN Enrollment E ON S.StudentID = E.StudentID JOIN Course C ON E.CourseID = C.CourseID
JOIN Instructor I ON C.InstructorID = I.InstructorID;

5.3 Viewing the Report
SELECT * FROM StudentCourseView;

5.4 Updating Records Using a View (If Updatable)
UPDATE StudentCourseView SET Credits = 5
WHERE CourseName = 'Data Structures';

5.5 Creating Indexes
CREATE INDEX idx_student_name ON Student(Name); CREATE INDEX idx_course_name ON Course(CourseName);
CREATE INDEX idx_enrollment_student ON Enrollment(StudentID);

5.6 Checking Query Performance
Before Indexing:
EXPLAIN SELECT * FROM Student WHERE Name = 'John';
After Indexing:
EXPLAIN SELECT * FROM Student WHERE Name = 'John';


1. Display total number of students enrolled in each course.
SELECT
C.CourseName,
COUNT(E.StudentID) AS Total_Students 
FROM Course C
LEFT JOIN Enrollment E ON C.CourseID = E.CourseID 
GROUP BY C.CourseName;

2. Display youngest student enrolled under each instructor.
SELECT
I.InstructorID,
I.Name AS InstructorName, 
S.StudentID,
S.Name AS StudentName, 
S.Age
FROM Instructor I
JOIN Course C ON I.InstructorID = C.InstructorID 
JOIN Enrollment E ON C.CourseID = E.CourseID 
JOIN Student S ON S.StudentID = E.StudentID
WHERE S.Age = ( 
    SELECT MIN(S2.Age)
FROM Course C2
JOIN Enrollment E2 ON C2.CourseID = E2.CourseID
JOIN Student S2 ON S2.StudentID = E2.StudentID WHERE C2.InstructorID = I.InstructorID);

3. Display number of courses taught in each department with department name.
SELECT
I.Department,
COUNT(C.CourseID) AS Total_Courses 
FROM Instructor I
LEFT JOIN Course C ON I.InstructorID = C.InstructorID 
GROUP BY I.Department;

4. Display student details sorted by age in increasing order.
SELECT
StudentID, Name, Email, Age, Address 
FROM Student
ORDER BY Age ASC;

5. Show records of students older than 20 in each course.
SELECT
S.StudentID, S.Name, S.Age, 
C.CourseName
FROM Student S
JOIN Enrollment E ON S.StudentID = E.StudentID 
JOIN Course C ON C.CourseID = E.CourseID 
WHERE S.Age > 20
ORDER BY C.CourseName;