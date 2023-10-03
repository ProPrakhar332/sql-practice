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

<h3>5. Each admission costs $50 for patients without insurance, and $10 for patients with insurance. All patients with an even patient_id have insurance.

Give each patient a 'Yes' if they have insurance, and a 'No' if they don't have insurance. Add up the admission_total cost for each has_insurance group.</h3>

```sql
SELECT (CASE WHEN patient_id%2==0 THEN "Yes" ELSE "No" END) AS has_insurance,
       SUM(CASE WHEN patient_id%2==0 THEN 10 ELSE 50 END) AS cost_after_insurance

FROM   admissions

GROUP BY has_insurance;
```

<h3>6. Show the provinces that has more patients identified as 'M' than 'F'. Must only show full province_name
</h3>

```sql
SELECT P.province_name

FROM

(SELECT province_id,
        COUNT(CASE WHEN gender="M" THEN patient_id ELSE NULL END) AS males,
        COUNT(CASE WHEN gender="F" THEN patient_id ELSE NULL END) AS females
FROM patients
GROUP BY province_id) D

LEFT JOIN province_names P ON P.province_id = D.province_id

WHERE males > females;
```

<h3>7. We are looking for a specific patient. Pull all columns for the patient who matches the following criteria:
- First_name contains an 'r' after the first two letters.
- Identifies their gender as 'F'
- Born in February, May, or December
- Their weight would be between 60kg and 80kg
- Their patient_id is an odd number
- They are from the city 'Kingston'</h3>

```sql
SELECT *

FROM   patients

WHERE  first_name like "__r%"

       AND gender = "F"
       
       ANd MONTH(birth_date) IN (2,5,12)
       
       AND weight BETWEEN 60 AND 80
       
       AND patient_id % 2 = 1
       
       AND city = 'Kingston';
```

<h3>8. Show the percent of patients that have 'M' as their gender. Round the answer to the nearest hundreth number and in percent form.</h3>

```sql
SELECT ROUND( SUM( gender = "M" ) * 100.0 / COUNT(*),2) || "%"

FROM   patients;
```

<h3>9. For each day display the total amount of admissions on that day. Display the amount changed from the previous date.
</h3>

```sql
SELECT admission_date, COUNT(admission_date) AS admission_day,
       COUNT(admission_date) - LAG(COUNT(admission_date)) OVER (ORDER BY admission_date) AS admission_count_change

FROM   admissions

GROUP BY admission_date;
```

<h3>10. Sort the province names in ascending order in such a way that the province 'Ontario' is always on top.
</h3>

```sql
SELECT province_name

FROM   province_names

ORDER BY (province_name = 'Ontario') DESC, province_name ASC;
```

<h3>11. We need a breakdown for the total amount of admissions each doctor has started each year. Show the doctor_id, doctor_full_name, specialty, year, total_admissions for that year.
</h3>

```sql
SELECT D.doctor_id, D.first_name || " " || D.last_name AS doctor_name, D.specialty, YEAR(A.admission_date) AS selected_year, COUNT(*) AS total_admissions

FROM admissions A LEFT JOIN doctors D

ON

A.attending_doctor_id = D.doctor_id

GROUP BY A.attending_doctor_id, selected_year;
```
