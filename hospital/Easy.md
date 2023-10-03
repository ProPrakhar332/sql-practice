---

### Section1: Easy

---

<h3>1. Show first name, last name, and gender of patients who's gender is 'M'.</h3>

```sql
SELECT first_name, last_name, gender

FROM   patients

WHERE  gender = 'M';
```

<h3>2. Show first name and last name of patients who does not have allergies.</h3>

```sql
SELECT first_name, last_name

FROM   patients

WHERE  allergies IS NULL;
```

<h3>3. Show first name of patients that start with the letter 'C'.</h3>

Solution 1:-

```sql
SELECT first_name

FROM   patients

WHERE  first_name LIKE "C%";
```

Solution 2:-

```sql
SELECT first_name

FROM   patients

WHERE  SUBSTRING( first_name, 1, 1 ) = 'C';
```

<h3>4. Show first name and last name of patients that weight within the range of 100 to 120 (inclusive).</h3>

Solution 1:-

```sql
SELECT first_name,last_name

FROM   patients

WHERE  weight >= 100 AND weight <= 120;
```
Solution 2:-

```sql
SELECT first_name, last_name

FROM   patients

WHERE  weight BETWEEN 100 AND 120;
```

<h3>5. Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'.</h3>

```sql
UPDATE patients

SET    allergies = 'NKA'

WHERE  allergies IS NULL
```

<h3>6. Show first name and last name concatinated into one column to show their full name.</h3>

```sql
SELECT CONCAT( first_name, " " ,last_name ) AS full_name

FROM   patients;
```

<h3>7. Show first name, last name, and the full province name of each patient.

Example: 'Ontario' instead of 'ON'</h3>

```sql
SELECT p.first_name, p.last_name, pn.province_name

FROM   patients p LEFT JOIN province_names pn

ON     p.province_id = pn.province_id;
```

<h3>8. Show how many patients have a birth_date with 2010 as the birth year.</h3>

```sql
SELECT COUNT(*)

FROM   patients p

WHERE  YEAR(birth_date) = 2010;
```

<h3>9. Show the first_name, last_name, and height of the patient with the greatest height.</h3>

Solution 1:-

```sql
SELECT first_name, last_name, MAX(height) AS height

FROM   patients;
```

Solution 2:-

```sql
SELECT first_name, last_name, height

FROM   patients p

WHERE  height IN (SELECT max(height) FROM patients);
```

<h3>10. Show all columns for patients who have one of the following patient_ids:
1,45,534,879,1000</h3>

```sql
SELECT *

FROM   patients

WHERE  patient_id in (1,45,534,879,1000);
```

<h3>11. Show the total number of admissions.</h3>

```sql
SELECT COUNT(*)

FROM   admissions;
```

<h3>12. Show all the columns from admissions where the patient was admitted and discharged on the same day.</h3>

```sql
SELECT *

FROM   admissions

WHERE  admission_date = discharge_date;
```

<h3>13. Show the patient id and the total number of admissions for patient_id 579.</h3>

```sql
SELECT patient_id, COUNT(admission_date)

FROM   admissions

WHERE  patient_id = 579;
```

<h3>14. Based on the cities that our patients live in, show unique cities that are in province_id 'NS'?</h3>

```sql
SELECT DISTINCT(city) 

FROM   patients

WHERE  province_id = 'NS';
```

<h3>15. Write a query to find the first_name, last name and birth date of patients who has height greater than 160 and weight greater than 70.</h3>

```sql
SELECT first_name, last_name, birth_date

FROM   patients

WHERE  height > 160 AND weight > 70;
```

<h3>16. Write a query to find list of patients first_name, last_name, and allergies from Hamilton where allergies are not null.
</h3>

```sql
SELECT first_name, last_name, allergies 

FROM   patients 

WHERE  city = 'Hamilton' AND allergies IS NOT NULL;
```

<h3>17. Based on cities where our patient lives in, write a query to display the list of unique city starting with a vowel (a, e, i, o, u). Show the result order in ascending by city.
</h3>

```sql
SELECT DISTINCT(city)

FROM   patients 

WHERE  SUBSTRING( city , 1 , 1 ) in ( 'A' , 'E' , 'I' , 'O' , 'U' )

ORDER BY city;
```
