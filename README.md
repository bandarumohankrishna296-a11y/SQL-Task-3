DROP TABLE IF EXISTS Enrollments;
DROP TABLE IF EXISTS Students;
DROP TABLE IF EXISTS Courses;
CREATE TABLE Students (
    student_id INTEGER PRIMARY KEY,
    student_name TEXT
);
CREATE TABLE Courses (
    course_id INTEGER PRIMARY KEY,
    course_name TEXT
);
CREATE TABLE Enrollments (
    student_id INTEGER,
    course_id INTEGER,
    grade INTEGER,
    FOREIGN KEY (student_id) REFERENCES Students(student_id),
    FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);
INSERT INTO Students VALUES
(1,'Rahul'),
(2,'Priya'),
(3,'Amit'),
(4,'Sneha');

INSERT INTO Courses VALUES
(101,'DBMS'),
(102,'Java'),
(103,'Python');

INSERT INTO Enrollments VALUES
(1,101,85),
(1,102,90),
(2,101,78),
(2,103,88),
(3,101,95),
(3,102,65),
(4,102,70),
(4,103,92);
SELECT c.course_name, s.student_name, e.grade
FROM Enrollments e
JOIN Students s ON e.student_id = s.student_id
JOIN Courses c ON e.course_id = c.course_id
WHERE e.grade = (
    SELECT MAX(e2.grade)
    FROM Enrollments e2
    WHERE e2.course_id = e.course_id
);
SELECT c.course_name,
       COUNT(*) AS Total_Students,
       SUM(CASE WHEN e.grade >= 40 THEN 1 ELSE 0 END) AS Passed_Students,
       ROUND(SUM(CASE WHEN e.grade >= 40 THEN 1 ELSE 0 END) * 100.0 / COUNT(*),2) AS Pass_Rate
FROM Enrollments e
JOIN Courses c ON e.course_id = c.course_id
GROUP BY c.course_name;
SELECT s.student_name,
       SUM(e.grade) AS Total_Marks
FROM Students s
JOIN Enrollments e ON s.student_id = e.student_id
GROUP BY s.student_id, s.student_name
ORDER BY Total_Marks DESC
LIMIT 1;
SELECT s.student_name,
       COUNT(e.course_id) AS Total_Courses
FROM Students s
JOIN Enrollments e ON s.student_id = e.student_id
GROUP BY s.student_id, s.student_name
HAVING COUNT(e.course_id) > 1;

-- 4. Students Enrolled in Multiple Courses
SELECT s.student_name,
COUNT(e.course_id) AS Total_Courses
FROM Students s
JOIN Enrollments e ON s.student_id = e.student_id
GROUP BY s.student_id, s.student_name
HAVING COUNT(e.course_id) > 1;
