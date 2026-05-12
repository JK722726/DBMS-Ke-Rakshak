5. Program / SQL
5.1
Filtering Data with WHERE
a) Find students above age 20
SELECT StudentID, Name, Age 
FROM Student
WHERE Age > 20;

b) Filter courses taught by instructor 'Dr. Sharma'
SELECT C.CourseID, C.CourseName 
FROM Course C
JOIN Instructor I ON C.InstructorID = I.InstructorID 
WHERE I.Name = 'Dr. Sharma';

5.2 Sorting with ORDER BY
a) Sort students alphabetically
SELECT StudentID, Name, Age 
FROM Student
ORDER BY Name ASC;

b) Sort courses by credits (descending)
SELECT CourseID, CourseName, Credits 
FROM Course
ORDER BY Credits DESC;

5.3 Aggregation using GROUP BY
a) Student count per course
SELECT
C.CourseName,
COUNT(E.StudentID) AS Total_Students 
FROM Course C
LEFT JOIN Enrollment E ON C.CourseID = E.CourseID 
GROUP BY C.CourseName;

b) Average age of students
SELECT AVG(Age) AS Average_Student_Age 
FROM Student;

5.4 Nested Subqueries
a) Display students older than the average student age
SELECT *
FROM Student
WHERE Age > (SELECT AVG(Age) FROM Student);

b) Display courses having more enrollments than the average number of enrollments
SELECT C.CourseName 
FROM Course C
JOIN Enrollment E ON C.CourseID = E.CourseID 
GROUP BY C.CourseName
HAVING COUNT(E.StudentID) > 
(
SELECT AVG(Cnt) 
FROM (
SELECT COUNT(StudentID) AS Cnt
FROM Enrollment 
GROUP BY CourseID
) AS AvgTable
);
