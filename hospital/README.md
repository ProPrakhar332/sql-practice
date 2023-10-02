## hospital.db

The hospital.db has the following Entity Relationship Diagram which we have to keep in mind before solving the questions.

![erd](https://github.com/ProPrakhar332/sql-practice/blob/main/hospital/hospital-schema.jpg)

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

---

### Section2: Medium

---

<h3>1. Show unique birth years from patients and order them by ascending.
</h3>

```sql
SELECT  DISTINCT(YEAR(birth_date))

FROM    patients 

ORDER BY birth_date ASC;
```

<h3>2. Show unique first names from the patients table which only occurs once in the list.

For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.
</h3>

```sql
SELECT first_name

FROM   patients 

GROUP BY first_name

HAVING COUNT(first_name) = 1;
```

<h3>3. Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.
</h3>

Solution 1:-

```sql
SELECT patient_id, first_name

FROM   patients 

WHERE  LEN(first_name) >= 6 AND first_name LIKE "%s" AND first_name LIKE "S%";
```

Solution 2:-

```sql
SELECT patient_id, first_name

FROM   patients

WHERE  first_name LIKE 's%s' AND LEN(first_name) >= 6;
```

<h3>4. Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'. Primary diagnosis is stored in the admissions table.</h3>

```sql
SELECT p.patient_id, p.first_name, p.last_name

FROM   patients p LEFT JOIN admissions a

ON     p.patient_id = a.patient_id

WHERE  a.diagnosis LIKE "Dementia";
```

<h3>5. Display every patient's first_name.
Order the list by the length of each name and then by alphbetically</h3>

```sql
SELECT first_name

FROM   patients

ORDER BY LEN(first_name), first_name;
```

<h3>6. Show the total amount of male patients and the total amount of female patients in the patients table.
Display the two results in the same row.</h3>

Solution 1:-

```sql
SELECT SUM( CASE WHEN gender LIKE 'M' THEN 1 ELSE 0 END) AS male_count,
       SUM( CASE WHEN gender LIKE 'F' THEN 1 ELSE 0 END) as female_count
FROM   patients
```

Solution 2:-

```sql
SELECT SUM(Gender = 'M') as male_count,
       SUM(Gender = 'F') AS female_count
FROM   patients
```

<h3>7. Show first and last name, allergies from patients which have allergies to either 'Penicillin' or 'Morphine'. Show results ordered ascending by allergies then by first_name then by last_name.
</h3>

```sql
SELECT first_name, last_name, allergies

FROM   patients

WHERE  allergies IN ( 'Penicillin' , 'Morphine' )

ORDER BY allergies, first_name, last_name
```

<h3>8. Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.
</h3>

```sql
SELECT a1.patient_id, a1.diagnosis

FROM   admissions a1

JOIN   admissions a2

ON     a1.patient_id = a2.patient_id AND
       a1.diagnosis = a2.diagnosis AND
       a1.admission_date <> a2.admission_date

GROUP BY a1.patient_id;
```

<h3>9. Show the city and the total number of patients in the city.
Order from most to least patients and then by city name ascending.</h3>

```sql
SELECT city, COUNT(*) AS num_patients

FROM   patients

GROUP BY city

ORDER BY num_patients DESC, city ASC;

```

<h3>10. Show first name, last name and role of every person that is either patient or doctor.
The roles are either "Patient" or "Doctor"</h3>

```sql
SELECT first_name, last_name, 'Patient' AS role

FROM   patients

UNION ALL

SELECT first_name, last_name, 'Doctor'

FROM   doctors;
```

<h3>11. Show all allergies ordered by popularity. Remove NULL values from query.</h3>

```sql
SELECT allergies, COUNT(allergies) AS total_diagnosis

FROM   patients

WHERE allergies IS NOT NULL

GROUP BY allergies

ORDER BY total_diagnosis DESC;
```

<h3>12. Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.</h3>

```sql
SELECT first_name, last_name, birth_date 

FROM patients WHERE YEAR(birth_date) >= 1970 AND YEAR(birth_date) < 1980

ORDER BY birth_date;
```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```

<h3></h3>

```sql

```
