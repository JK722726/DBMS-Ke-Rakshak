CREATE TABLE Emp (
    id INT PRIMARY KEY,
    name VARCHAR2(20),
    department VARCHAR2(10),
    salary NUMBER
);

CREATE TABLE Emp_Audit (
    emp_id INT,
    salary NUMBER
);

-- 2. Trigger Creation
CREATE OR REPLACE TRIGGER t1 
AFTER INSERT OR UPDATE ON Emp 
FOR EACH ROW 
BEGIN 
    IF INSERTING THEN 
        INSERT INTO Emp_Audit VALUES (:NEW.id, :NEW.salary); 
        DBMS_OUTPUT.PUT_LINE('Data Inserted Successfully'); 
    ELSIF UPDATING THEN 
        INSERT INTO Emp_Audit VALUES (:NEW.id, :NEW.salary); 
        DBMS_OUTPUT.PUT_LINE('Data Updated Successfully'); 
    END IF; 
END;
/


-- 3. Data Insertion and Initial Updates
INSERT INTO Emp VALUES (11, 'pqr', 'designing', 44400);
INSERT INTO Emp VALUES (12, 'xyz', 'develop', 540000);
INSERT INTO Emp VALUES (21, 'mno', 'hr', 50000);
INSERT INTO Emp VALUES (23, 'abc', 'it', 770000);

-- Note: ID 101 from your original query doesn't exist yet, 
--but included here for consistency with your logic
UPDATE Emp SET salary = 80000 WHERE id = 101; 

-- 4. PL/SQL Cursor Processing
SET SERVEROUTPUT ON;
DECLARE  
    CURSOR c1 IS SELECT id, name, department, salary FROM Emp; 
    n_id    Emp.id%TYPE; 
    n_name  Emp.name%TYPE; 
    n_dept  Emp.department%TYPE;
    n_sal   Emp.salary%TYPE; 
BEGIN 
    OPEN c1; 
    LOOP 
        FETCH c1 INTO n_id, n_name, n_dept, n_sal; 
        EXIT WHEN c1%NOTFOUND; 
        
        -- Logic for IT department (Reducing to 10% per your original logic)
        IF n_dept = 'it' THEN  
            UPDATE Emp 
            SET salary = n_sal * 0.1 
            WHERE id = n_id;
        END IF; 
    END LOOP; 
    CLOSE c1; 
    COMMIT;
END;
/

-- 5. Final Results
SELECT * FROM Emp;
SELECT * FROM Emp_Audit;
