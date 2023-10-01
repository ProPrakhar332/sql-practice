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
SELECT  first_name, last_name

FROM    patients

WHERE   allergies IS NULL;
```

<h3>3. Show first name of patients that start with the letter 'C'.</h3>

Solution 1:-

```sql
SELECT  first_name

FROM    patients

WHERE   first_name LIKE "C%";
```

Solution 2:-

```sql
SELECT   first_name

FROM     patients

WHERE    SUBSTRING( first_name, 1, 1 ) = 'C';
```

<h3>4. Show first name and last name of patients that weight within the range of 100 to 120 (inclusive).</h3>

Solution 1:-

```sql
SELECT   first_name,last_name

FROM     patients

WHERE    weight >= 100 AND weight <= 120;
```
Solution 2:-

```sql
SELECT    first_name, last_name

FROM      patients

WHERE     weight BETWEEN 100 AND 120;
```

<h3>5. Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'.</h3>

```sql
UPDATE    patients

SET       allergies = 'NKA'

WHERE     allergies IS NULL
```
