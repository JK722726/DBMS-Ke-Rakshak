5. Program / PL-SQL Code
(A) Create Sales Table
CREATE TABLE Sales (
sale_id NUMBER PRIMARY KEY,
customer_name VARCHAR2(50), 
amount NUMBER(10,2), 
sale_date DATE
);

(B) Stored Procedure to Add Sales Data
CREATE OR REPLACE PROCEDURE add_sale (
p_sale_id IN NUMBER, 
p_customer_name IN VARCHAR2,
p_amount IN NUMBER
) AS
BEGIN
INSERT INTO Sales(sale_id, customer_name, amount, sale_date) VALUES(p_sale_id, p_customer_name, p_amount, SYSDATE);
DBMS_OUTPUT.PUT_LINE('Sale record inserted successfully.'); 
END;
/

(C) Function to Calculate Total Sales
CREATE OR REPLACE FUNCTION get_total_sales 
RETURN NUMBER
AS
total NUMBER;
BEGIN
SELECT SUM(amount) INTO total FROM Sales; 
RETURN total;
END;
/

(D) Execute Procedure and Function
BEGIN
add_sale(101, 'Rahul Sharma', 4500); 
add_sale(102, 'Neha Verma', 3200);
END;
/

DECLARE
total_sales NUMBER; 
BEGIN
total_sales := get_total_sales(); 
DBMS_OUTPUT.PUT_LINE('Total Sales = ' || total_sales);
END;
/

