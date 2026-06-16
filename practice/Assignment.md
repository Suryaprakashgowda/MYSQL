Assignment:



##### 1.Get employee details whose salary is the highest in the city mumbai.



SELECT \*

FROM EMPS

WHERE LID = (

&#x20;   SELECT LID

&#x20;   FROM LOCATIONS

&#x20;   WHERE CITY = 'MUMBAI'

)

ORDER BY SAL DESC

LIMIT 1;





##### 2.Find employees who handled only Delivered orders.



SELECT \*

FROM EMPS

WHERE EID IN (

&#x20;   SELECT EID

&#x20;   FROM ORDERS

&#x20;   WHERE STATUS = 'Delivered');







##### 3\. Find employees who handled both Delivered and Cancelled orders.



SELECT \*

FROM EMPS

WHERE EID IN (

&#x20;   SELECT EID

&#x20;   FROM ORDERS

&#x20;   WHERE STATUS = 'Delivered')

AND EID IN (

&#x20;   SELECT EID

&#x20;   FROM ORDERS

&#x20;   WHERE STATUS = 'Cancelled');







##### 4.Get employee names who handled more orders than the average number of orders handled by employees.



SELECT FNAME

FROM EMPS

WHERE EID IN (

&#x20;   SELECT EID

&#x20;   FROM ORDERS

&#x20;   GROUP BY EID

&#x20;   HAVING COUNT(\*) > (

&#x20;       SELECT AVG(order\_count)

&#x20;       FROM (

&#x20;           SELECT COUNT(\*) AS order\_count

&#x20;           FROM ORDERS

&#x20;           GROUP BY EID

&#x20;       ) AS X ));



##### 5.Find employees whose commission is greater than the average commission of all employees.



SELECT \*

FROM EMPS

WHERE COMM > (

&#x20;   SELECT AVG(COMM)

&#x20;   FROM EMPS

);







##### 6.List employees whose DOB is earlier than all employees in the DELIVERY job.



SELECT \*

FROM EMPS

WHERE DOB < (

&#x20;   SELECT MIN(DOB)

&#x20;   FROM EMPS

&#x20;   WHERE JOB = 'DELIVERY');







##### 7.Find customers who have placed orders from restaurants located in the same city as the customer.



SELECT NAME

FROM CUSTOMERS

WHERE ORDER\_ID IN (

&#x20;   SELECT O.ORDER\_ID

&#x20;   FROM ORDERS O, RESTAURANTS R

&#x20;   WHERE O.RESTAURANT\_ID = R.RESTAURANT\_ID

&#x20;   AND R.CITY = (

&#x20;       SELECT CITY

&#x20;       FROM LOCATIONS L

&#x20;       WHERE L.LID = CUSTOMERS.LID )

);





##### 8\. Find customers who never placed any order.



SELECT \*

FROM CUSTOMERS

WHERE CID NOT IN (

&#x20;   SELECT CID

&#x20;   FROM CUSTOMERS

&#x20;   WHERE ORDER\_ID IS NOT NULL

);





##### 9\. Get customers whose order amount is higher than the average payment amount.



SELECT \*

FROM CUSTOMERS

WHERE ORDER\_ID IN (

&#x20;   SELECT ORDER\_ID

&#x20;   FROM PAYMENTS

&#x20;   WHERE AMOUNT > (

&#x20;       SELECT AVG(AMOUNT)

&#x20;       FROM PAYMENTS )

);





##### 10\. Find customers who placed orders but payment status is Pending or Failed.



SELECT \*

FROM CUSTOMERS

WHERE ORDER\_ID IN (

&#x20;   SELECT ORDER\_ID

&#x20;   FROM PAYMENTS

&#x20;   WHERE STATUS IN ('Pending', 'Failed')

);





##### 11\. List customers who placed more than one order.



SELECT \*

FROM CUSTOMERS

WHERE CID IN (

&#x20;   SELECT CID

&#x20;   FROM ORDERS

&#x20;   GROUP BY CID

&#x20;   HAVING COUNT(\*) > 1

);







##### 12\. Find restaurants whose average rating is higher than the overall average rating.



SELECT NAME

FROM RESTAURANTS

WHERE RESTAURANT\_ID IN (

&#x20;   SELECT RESTAURANT\_ID

&#x20;   FROM REVIEWS

&#x20;   GROUP BY RESTAURANT\_ID

&#x20;   HAVING AVG(RATING) > (

&#x20;       SELECT AVG(RATING)

&#x20;       FROM REVIEWS

&#x20;   )

);





##### 13.List restaurants that never received any review.



SELECT \*

FROM RESTAURANTS

WHERE RESTAURANT\_ID NOT IN (

&#x20;   SELECT RESTAURANT\_ID

&#x20;   FROM REVIEWS

);





##### 14\. Find restaurants where total order amount is maximum.



SELECT \*

FROM RESTAURANTS

WHERE RESTAURANT\_ID IN (

&#x20;   SELECT RESTAURANT\_ID

&#x20;   FROM MENU\_ITEMS

&#x20;   GROUP BY RESTAURANT\_ID

&#x20;   HAVING SUM(PRICE) = (

&#x20;       SELECT MAX(TOTAL)

&#x20;       FROM (

&#x20;           SELECT SUM(PRICE) AS TOTAL

&#x20;           FROM MENU\_ITEMS

&#x20;           GROUP BY RESTAURANT\_ID

&#x20;       ) X )

);



##### 15\. Get restaurants that have menu items priced higher than the average menu price.



SELECT \*

FROM RESTAURANTS

WHERE RESTAURANT\_ID IN (

&#x20;   SELECT RESTAURANT\_ID

&#x20;   FROM MENU\_ITEMS

&#x20;   WHERE PRICE > (

&#x20;       SELECT AVG(PRICE)

&#x20;       FROM MENU\_ITEMS )

);





##### 16\. Find restaurants that received only 5-star ratings.



SELECT \*

FROM RESTAURANTS

WHERE RESTAURANT\_ID IN (

&#x20;   SELECT RESTAURANT\_ID

&#x20;   FROM REVIEWS

&#x20;   GROUP BY RESTAURANT\_ID

&#x20;   HAVING MIN(RATING) = 5

&#x20;   AND MAX(RATING) = 5

);





##### 17\. Find orders whose payment amount is greater than the average payment amount.



SELECT \*

FROM ORDERS

WHERE ORDER\_ID IN (

&#x20;   SELECT ORDER\_ID

&#x20;   FROM PAYMENTS

&#x20;   WHERE AMOUNT > (

&#x20;       SELECT AVG(AMOUNT)

&#x20;       FROM PAYMENTS

&#x20;   )

);





##### 18\. Display orders that are Delivered but payment is not Completed.



SELECT \*

FROM ORDERS

WHERE ORDER\_ID IN (

&#x20;   SELECT ORDER\_ID

&#x20;   FROM PAYMENTS

&#x20;   WHERE STATUS != 'Completed'

)

AND STATUS = 'Delivered';





##### 19\. Find orders that contain more than one menu item.



SELECT \*

FROM ORDERS

WHERE ORDER\_ID IN (

&#x20;   SELECT ORDER\_ID

&#x20;   FROM MENU\_ITEMS

&#x20;   GROUP BY ORDER\_ID

&#x20;   HAVING COUNT(\*) > 1

);







##### 20\. Get orders where order date is later than all orders handled by employee 2.



SELECT \*

FROM ORDERS

WHERE ORDER\_DATE > (

&#x20;   SELECT MAX(ORDER\_DATE)

&#x20;   FROM ORDERS

&#x20;   WHERE EID = 2

);





##### 21.Find orders whose total menu item price is higher than the average order value.



SELECT \*

FROM ORDERS

WHERE ORDER\_ID IN (

&#x20;   SELECT ORDER\_ID

&#x20;   FROM MENU\_ITEMS

&#x20;   GROUP BY ORDER\_ID

&#x20;   HAVING SUM(PRICE) > (

&#x20;       SELECT AVG(TOTAL)

&#x20;       FROM (

&#x20;           SELECT SUM(PRICE) AS TOTAL

&#x20;           FROM MENU\_ITEMS

&#x20;           GROUP BY ORDER\_ID

&#x20;       ) X )

);







##### 22\. Find locations where both employees and customers exist.



SELECT \*

FROM LOCATIONS

WHERE LID IN (

&#x20;   SELECT LID

&#x20;   FROM EMPS

)

AND LID IN (

&#x20;   SELECT LID

&#x20;   FROM CUSTOMERS

);





##### 23\. List locations where number of customers is greater than number of employees.



SELECT \*

FROM LOCATIONS

WHERE LID IN (

&#x20;   SELECT C.LID

&#x20;   FROM CUSTOMERS C

&#x20;   GROUP BY C.LID

&#x20;   HAVING COUNT(\*) > (

&#x20;       SELECT COUNT(\*)

&#x20;       FROM EMPS E

&#x20;       WHERE E.LID = C.LID

&#x20;   )

);





##### 24\. Find locations where no orders were delivered.



SELECT \*

FROM LOCATIONS

WHERE LID NOT IN (

&#x20;   SELECT C.LID

&#x20;   FROM CUSTOMERS C

&#x20;   WHERE C.ORDER\_ID IN (

&#x20;       SELECT ORDER\_ID

&#x20;       FROM ORDERS

&#x20;       WHERE STATUS = 'Delivered'

&#x20;   )

);





##### 25\. Find employees who handled orders from all restaurants.





SELECT \*

FROM EMPS

WHERE EID IN (

&#x20;   SELECT EID

&#x20;   FROM ORDERS

&#x20;   GROUP BY EID

&#x20;   HAVING COUNT(DISTINCT RESTAURANT\_ID) = (

&#x20;       SELECT COUNT(\*)

&#x20;       FROM RESTAURANTS

&#x20;   )

);



##### 26.Find customers who ordered from the same restaurant as customer ‘KIRAN RAJ’.



SELECT \*

FROM CUSTOMERS

WHERE ORDER\_ID IN (

&#x20;   SELECT ORDER\_ID

&#x20;   FROM ORDERS

&#x20;   WHERE RESTAURANT\_ID = (

&#x20;       SELECT RESTAURANT\_ID

&#x20;       FROM ORDERS

&#x20;       WHERE ORDER\_ID = (

&#x20;           SELECT ORDER\_ID

&#x20;           FROM CUSTOMERS

&#x20;           WHERE NAME = 'KIRAN RAJ'

&#x20;       )

&#x20;   )

);





##### 27\. Find employees who handled orders with payment method = UPI more than average.



SELECT \*

FROM EMPS

WHERE EID IN (

&#x20;   SELECT EID

&#x20;   FROM ORDERS

&#x20;   WHERE ORDER\_ID IN (

&#x20;       SELECT ORDER\_ID

&#x20;       FROM PAYMENTS

&#x20;       WHERE METHOD = 'UPI'

&#x20;   )

&#x20;   GROUP BY EID

&#x20;   HAVING COUNT(\*) > (

&#x20;       SELECT AVG(CNT)

&#x20;       FROM (

&#x20;           SELECT COUNT(\*) AS CNT

&#x20;           FROM ORDERS

&#x20;           WHERE ORDER\_ID IN (

&#x20;               SELECT ORDER\_ID

&#x20;               FROM PAYMENTS

&#x20;               WHERE METHOD = 'UPI'

&#x20;           )

&#x20;           GROUP BY EID

&#x20;       ) X

&#x20;   )

);



##### 28\. Find restaurants whose orders were handled by more than one employee.



SELECT \*

FROM RESTAURANTS

WHERE RESTAURANT\_ID IN (

&#x20;   SELECT RESTAURANT\_ID

&#x20;   FROM ORDERS

&#x20;   GROUP BY RESTAURANT\_ID

&#x20;   HAVING COUNT(DISTINCT EID) > 1

);





##### 29\. Get customers whose city has the highest number of orders.



SELECT \*

FROM CUSTOMERS

WHERE LID IN (

&#x20;   SELECT LID

&#x20;   FROM CUSTOMERS

&#x20;   WHERE ORDER\_ID IS NOT NULL

&#x20;   GROUP BY LID

&#x20;   HAVING COUNT(\*) = (

&#x20;       SELECT MAX(CNT)

&#x20;       FROM (

&#x20;           SELECT COUNT(\*) AS CNT

&#x20;           FROM CUSTOMERS

&#x20;           WHERE ORDER\_ID IS NOT NULL

&#x20;           GROUP BY LID

&#x20;       ) X

&#x20;   )

);





##### 30\. Find employees whose salary is in the top 3 salaries using subquery.



SELECT \*

FROM EMPS E1

WHERE 3 > (

&#x20;   SELECT COUNT(DISTINCT E2.SAL)

&#x20;   FROM EMPS E2

&#x20;   WHERE E2.SAL > E1.SAL

);





##### 31\. Find restaurants that have menu items but no orders.



SELECT \*

FROM RESTAURANTS

WHERE RESTAURANT\_ID IN (

&#x20;   SELECT RESTAURANT\_ID

&#x20;   FROM MENU\_ITEMS

)

AND RESTAURANT\_ID NOT IN (

&#x20;   SELECT RESTAURANT\_ID

&#x20;   FROM ORDERS

);





##### 32\. Find customers who reviewed a restaurant but never ordered from it.



SELECT \*

FROM CUSTOMERS

WHERE CID IN (

&#x20;   SELECT CID

&#x20;   FROM REVIEWS

&#x20;   WHERE RESTAURANT\_ID <> (

&#x20;       SELECT RESTAURANT\_ID

&#x20;       FROM ORDERS

&#x20;       WHERE ORDER\_ID = (

&#x20;           SELECT ORDER\_ID

&#x20;           FROM CUSTOMERS C

&#x20;           WHERE C.CID = REVIEWS.CID

&#x20;       )

&#x20;   )

);







##### 33\. Find employees whose job role salary average is greater than overall employee average salary.



SELECT \*

FROM EMPS E

WHERE JOB IN (

&#x20;   SELECT JOB

&#x20;   FROM EMPS

&#x20;   GROUP BY JOB

&#x20;   HAVING AVG(SAL) > (

&#x20;       SELECT AVG(SAL)

&#x20;       FROM EMPS

&#x20;   )

);





##### 34\. Find restaurants where total revenue is less than the average restaurant revenue.



SELECT \*

FROM RESTAURANTS

WHERE RESTAURANT\_ID IN (

&#x20;   SELECT RESTAURANT\_ID

&#x20;   FROM MENU\_ITEMS

&#x20;   GROUP BY RESTAURANT\_ID

&#x20;   HAVING SUM(PRICE) < (

&#x20;       SELECT AVG(TOTAL)

&#x20;       FROM (

&#x20;           SELECT SUM(PRICE) AS TOTAL

&#x20;           FROM MENU\_ITEMS

&#x20;           GROUP BY RESTAURANT\_ID

&#x20;       ) X

&#x20;   )

);





##### 35\. Get orders where payment amount equals the maximum payment made by any customer.



SELECT \*

FROM ORDERS

WHERE ORDER\_ID IN (

&#x20;   SELECT ORDER\_ID

&#x20;   FROM PAYMENTS

&#x20;   WHERE AMOUNT = (

&#x20;       SELECT MAX(AMOUNT)

&#x20;       FROM PAYMENTS

&#x20;   )

);





##### 36\. Find customers who placed orders in multiple cities.



SELECT \*

FROM CUSTOMERS

WHERE CID IN (

&#x20;   SELECT CID

&#x20;   FROM ORDERS O, RESTAURANTS R

&#x20;   WHERE O.RESTAURANT\_ID = R.RESTAURANT\_ID

&#x20;   GROUP BY CID

&#x20;   HAVING COUNT(DISTINCT R.CITY) > 1

);







































































































































