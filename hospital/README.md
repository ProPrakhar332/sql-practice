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

<h3>13. We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in decending order
EX: SMITH,jane</h3>

```sql
SELECT CONCAT( UPPER(last_name),",",LOWER(first_name)) AS new_name_format

FROM   patients

ORDER BY first_name desc;

```

<h3>14. Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.
</h3>

```sql
SELECT province_id, SUM(height) AS sum_height

FROM   patients

GROUP BY province_id

HAVING sum_height >= 7000;
```

<h3>15. Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'
</h3>

```sql
SELECT (SELECT MAX(weight) FROM patients WHERE last_name = 'Maroni')
     - (SELECT MIN(weight) FROM patients WHERE last_name = 'Maroni') AS weight_data;
```

<h3>16. Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.
</h3>

```sql
SELECT DAY(admission_date) AS day_number , COUNT(*) AS number_of_admissions

FROM   admissions

GROUP BY day_number

ORDER BY number_of_admissions DESC;
```

<h3>17. Show all columns for patient_id 542's most recent admission_date.
</h3>

Solution 1:-

```sql
SELECT *

FROM   admissions

WHERE  patient_id = 542

ORDER BY admission_date desc

LIMIT 1;
```

Solution 2:-

```sql
SELECT *

FROM   admissions

GROUP BY patient_id

HAVING patient_id = 542 AND
       MAX(admission_date);
```

<h3>18. Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria:
<br>a. patient_id is an odd number and attending_doctor_id is either 1, 5, or 19.
<br>b. attending_doctor_id contains a 2 and the length of patient_id is 3 characters.</h3>

```sql
SELECT patient_id, attending_doctor_id, diagnosis

FROM   admissions

WHERE

(patient_id % 2 = 1 AND attending_doctor_id IN (1,5,19))

OR

(LEN(patient_id) = 3 AND (attending_doctor_id LIKE "%2%") );
```

<h3>19. 
Show first_name, last_name, and the total number of admissions attended for each doctor.

Every admission has been attended by a doctor.</h3>

```sql
SELECT D.first_name, D.last_name, COUNT(*) 

FROM   admissions A LEFT JOIN doctors D

ON     A.attending_doctor_id = D.doctor_id

GROUP BY D.doctor_id;
```

<h3>20. For each doctor, display their id, full name, and the first and last admission date they attended.
</h3>

```sql
SELECT D.doctor_id, D.first_name || " " || D.last_name AS full_name,
       MIN(A.admission_date) AS first_admission_date,
       MAX(A.admission_date) AS last_admission_date

FROM admissions A LEFT JOIN doctors D

ON A.attending_doctor_id = D.doctor_id

GROUP BY A.attending_doctor_id;
```

<h3>21. Display the total amount of patients for each province. Order by descending.
</h3>

```sql
SELECT PN.province_name,COUNT(*) AS patient_count

FROM   patients P LEFT JOIN province_names PN

ON     P.province_id = PN.province_id

GROUP BY PN.province_name

ORDER BY patient_count DESC;
```

<h3>22. For every admission, display the patient's full name, their admission diagnosis, and their doctor's full name who diagnosed their problem.
</h3>

```sql
SELECT  P.first_name || " " || P.last_name AS patient_name,
        A.diagnosis,
        D.first_name || " " || D.last_name as doctor_name

FROM   admissions A LEFT JOIN patients P, doctors D

ON     A.patient_id = P.patient_id AND
       A.attending_doctor_id = D.doctor_id;
```

<h3>23. Display the number of duplicate patients based on their first_name and last_name.
</h3>

```sql
SELECT first_name, last_name, COUNT(*) AS num_of_duplicates

FROM   patients

GROUP BY first_name, last_name

HAVING num_of_duplicates > 1;
```

<h3>24. Display patient's full name,
height in the units feet rounded to 1 decimal,
weight in the unit pounds rounded to 0 decimals,
birth_date,
gender non abbreviated.

Convert CM to feet by dividing by 30.48.
<br>Convert KG to pounds by multiplying by 2.205.</h3>

```sql
SELECT first_name || " " || last_name AS patient_name,
       ROUND(height/30.48,1) AS height,
       ROUND(weight*2.205,0) aAS weight,
       birth_date,
       CASE WHEN gender = "M" THEN "MALE" ELSE "FEMALE" END AS gender_type

FROM   patients;
``` 

<h3>25. Show patient_id, first_name, last_name from patients whose does not have any records in the admissions table. (Their patient_id does not exist in any admissions.patient_id rows.)
</h3>

```sql
SELECT patient_id, first_name, last_name

FROM   patients

WHERE patient_id NOT IN (SELECT DISTINCT(patient_id) FROM admissions);
```

---

### Section3: Hard

---

<h3>1. Show all of the patients grouped into weight groups.
Show the total amount of patients in each weight group.
Order the list by the weight group decending.

For example, if they weight 100 to 109 they are placed in the 100 weight group, 110-119 = 110 weight group, etc.</h3>

```sql
SELECT COUNT(*) AS patients_in_group, (weight/10) * 10 AS weight_group

FROM   patients

GROUP BY weight_group

ORDER BY weight_group DESC;
```

<h3>2. Show patient_id, weight, height, isObese from the patients table.

Display isObese as a boolean 0 or 1.

Obese is defined as weight(kg)/(height(m)2) >= 30.

weight is in units kg.

height is in units cm.</h3>

```sql
SELECT patient_id, weight, height,
       CASE WHEN weight/(POWER(height/100.0,2)) >= 30 THEN 1 ELSE 0 END AS isObese

FROM   patients;
```

<h3>3. Show patient_id, first_name, last_name, and attending doctor's specialty.
Show only the patients who has a diagnosis as 'Epilepsy' and the doctor's first name is 'Lisa'

Check patients, admissions, and doctors tables for required information.</h3>

```sql
SELECT A.patient_id, P.first_name, P.last_name, "Obstetrician/Gynecologist"

FROM   admissions A LEFT JOIN patients P

ON     A.patient_id = P.patient_id

WHERE A.diagnosis = "Epilepsy" AND A.attending_doctor_id = 21;
```

<h3>4. All patients who have gone through admissions, can see their medical documents on our site. Those patients are given a temporary password after their first admission. Show the patient_id and temp_password.

The password must be the following, in order:
1. patient_id
2. the numerical length of patient's last_name
3. year of patient's birth_date</h3>

```sql
SELECT A.patient_id, CONCAT(P.patient_id, LEN(P.last_name), YEAR(P.birth_date)) AS temp_password

FROM   admissions A LEFT JOIN patients P

ON     A.patient_id = P.patient_id

GROUP BY A.patient_id;
```

<h3></h3>

```sql

```

<h3></h3>

```sql

```
