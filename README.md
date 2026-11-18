--PATIENTS TABLE
CREATE TABLE patients ( 
	patient_id VARCHAR PRIMARY KEY, 
	first_name VARCHAR(50), 
	last_name VARCHAR(50), 
	gender VARCHAR(25), 
	date_of_birth DATE, 
	blood_group VARCHAR(20), 
	marital_status VARCHAR(50), 
	phone_number VARCHAR(50), 
	email VARCHAR(100), 
	city VARCHAR(50), 
	state VARCHAR(50), 
	registration_date DATE, 
	insurance_id VARCHAR(50) 
); 

--DOCTORS TABLE  
CREATE TABLE doctors ( 
	doctor_id VARCHAR PRIMARY KEY, 
	doctor_name VARCHAR(50), 
	gender VARCHAR(20), 
	specialization VARCHAR(50), 
	department_id VARCHAR(50),
	hire_date DATE, 
	consultation_fee NUMERIC, 
	years_experience INTEGER, 
	branch_name VARCHAR(50) 
); 

--APPOINTMENTS TABLE 
CREATE TABLE appointments ( 
	appointment_id VARCHAR PRIMARY KEY, 
	patient_id VARCHAR(50), 
	doctor_id VARCHAR(50), 
	appointment_date TIMESTAMP, 
	appointment_status VARCHAR(50), 
	appointment_type VARCHAR(50), 
	waiting_time_minutes INTEGER 
); 

--TREATMENTS TABLE 
CREATE TABLE treatments ( 
	Treatment_id VARCHAR(50) PRIMARY KEY, 
	Patient_id VARCHAR(50), 
	Doctor_id VARCHAR(50), 
	Treatment_date DATE, 
	Treatment_type VARCHAR(50), 
	Treatment_cost NUMERIC, 
	Outcome_status VARCHAR(50) 
); 

--BILLING TABLE 
CREATE TABLE billing ( 
	Bill_id VARCHAR(50) PRIMARY KEY, 	
	Patient_id VARCHAR(50), 
	Treatment_id VARCHAR(50), 
	Total_amount NUMERIC, 
	Insurance_covered NUMERIC, 
	Patient_paid NUMERIC, 
	Payment_status VARCHAR(50), 
	Payment_date DATE
); 

--ADMISSIONS TABLE 
CREATE TABLE admissions ( 
	Admission_id VARCHAR(50) PRIMARY KEY, 	
	Patient_id VARCHAR(50), 
	Admission_date DATE, 
	Discharge_date DATE, 
	Ward_type VARCHAR (50), 
	Room_number VARCHAR (50),
	Diagnosis VARCHAR (50), 
	Admission_status VARCHAR(50)
); 

--DEPARTMENTS TABLE
CREATE TABLE departments (
    department_name VARCHAR (50),
	department_id VARCHAR (50),
	branch_name VARCHAR (50),
	department_head VARCHAR (50)
);

--INSURANCE TABLE
CREATE TABLE insurance (
    insurance_id VARCHAR (50),
	provider_name VARCHAR (50),
	plan_type VARCHAR (50),
	coverage_percentage NUMERIC 
);

--LAB RESULTS TABLE
CREATE TABLE lab_results (
    lab_result_id VARCHAR (50),
	patient_id VARCHAR (50),
	test_name VARCHAR (50),
	test_result VARCHAR (50),
	test_date DATE,
	laboratory_branch VARCHAR (50)
);

--PRESCRIPTIONS TABLE
CREATE TABLE prescriptions (
    prescription_id VARCHAR (50),
	patient_id VARCHAR (50),
	doctor_id VARCHAR (50),
	medication_name VARCHAR (50),
	dosage VARCHAR (50),
	duration_days NUMERIC,
	prescription_date DATE
);
	
SELECT * FROM admissions;
SELECT * FROM appointments;
SELECT * FROM billing;
SELECT * FROM departments;
SELECT * FROM doctors;
SELECT * FROM insurance;
SELECT * FROM lab_results;
SELECT * FROM patients;
SELECT * FROM prescriptions;
SELECT * FROM treatments;


--Retrieve patients name and gender
SELECT first_name,
	   last_name,
	   gender
FROM patients;

--Retrieve doctor name, doctor id and specialisation
SELECT doctor_id,
       doctor_name,
	   specialization
FROM doctors;

--The hospital wants us to retrieve all information for female patients
SELECT *
FROM patients
WHERE gender ='Female';

--The hospital wants us to retrieve all patients that registered after 2024
SELECT *
FROM patients
WHERE registration_date > '2024-12-31';

--The hospital management wants us to retrieve all information for doctors working in cardiology
SELECT*
FROM doctors
WHERE specialization = 'Cardiology';

--Retrieve the first name and last name of patients from Lagos
SELECT first_name,
       last_name,
	   city
FROM patients
WHERE city = 'Lagos';

--Retrieve treatments that cost above 300,000
SELECT treatment_id,
       treatment_type,
	   treatment_cost
FROM treatments
WHERE treatment_cost > 300000;

--Inspect and identify completed appointments
SELECT *
FROM appointments
WHERE appointment_status = 'Completed';

--Retrieve appointment information with waiting time, from highest to lowest waiting time
SELECT appointment_id,
       appointment_date,
	   appointment_type,
	   waiting_time_minutes
FROM appointments
ORDER BY waiting_time_minutes DESC;

--The management wants a report showing each doctor's name, specialisation and consultation fee starting with the highest consultation fee.
SELECT doctor_name,
       specialization,
	   consultation_fee 
FROM doctors
ORDER BY consultation_fee DESC;

--The hospital lab manager wants to review lab test starting from the most recent performed test
SELECT *
FROM lab_results
ORDER BY test_date DESC;

--The hospital pharmacy department wants to review the prescription with the shortest duration
SELECT*
FROM prescriptions
ORDER BY duration_days ASC;

--The hospital wants to identify the top 5 doctors with the highest consultation fees
SELECT *
FROM doctors
ORDER BY consultation_fee DESC 
LIMIT 5;

--Top 5 insurance plans with the highest coverage percentage
SELECT *
FROM insurance
ORDER BY coverage_percentage DESC
LIMIT 5;

--Total patients in the hospital database
SELECT COUNT(*) AS Total_Patients
FROM patients;

--The hospital wants to know the average treatment cost
SELECT ROUND(AVG(treatment_cost),3) AS Average_treatment_cost
FROM treatments;

--Highest treatment cost
SELECT MAX(treatment_cost) AS Highest_treatment_cost
FROM treatments;

--Lowest treatment cost
SELECT MIN(treatment_cost) AS Lowest_treatment_cost
FROM treatments;

--Total hospital revenue (total amount billed)
SELECT SUM(total_amount) AS Total_revenue
FROM billing;

--GROUP BY
--Total patients by gender
SELECT gender, COUNT(*) AS Total_patients
FROM patients
GROUP BY gender;

--The hospital wants to retrieve revenue by payment status
SELECT payment_status, SUM (total_amount) AS Total_revenue
FROM billing
GROUP BY payment_status;

--Average treatment cost by treatment type
SELECT treatment_type,
	   ROUND(AVG(treatment_cost),2) AS Avg_Treatment_cost
FROM treatments
GROUP BY treatment_type;

--HAVING CLAUSE
--The hospital wants to show treatment type with average cost above 400000
SELECT treatment_type,
	   ROUND(AVG(treatment_cost),2) AS Avg_treatment_cost
FROM treatments
GROUP BY Treatment_type
HAVING ROUND(AVG(treatment_cost),2) >400000;

--The hospital management wants to identify the departments with more than 3 doctors
--(Corrected: groups by department through department_id, not by specialization)
SELECT dp.department_name,
       COUNT(d.doctor_id) AS Total_doctors
FROM departments dp
INNER JOIN doctors d
ON dp.department_id = d.department_id
GROUP BY dp.department_name
HAVING COUNT(d.doctor_id) > 3;

--Identify insurance types with average coverage above 70%
SELECT plan_type,
       ROUND(AVG(coverage_percentage),3) AS Average_Coverage
FROM insurance
GROUP BY plan_type
HAVING ROUND(AVG(coverage_percentage),3) > 70;

--Medical director wants to identify diagnosis with less than 100 admission cases
SELECT diagnosis,
       COUNT (*) AS Total_diagnosis
FROM admissions
GROUP BY diagnosis
HAVING COUNT (*) <100 
ORDER BY total_diagnosis ASC;

--DATE FUNCTION
--Calculate patients age
SELECT first_name,
       last_name,
	   AGE(date_of_birth) AS patient_age
FROM patients;

--Extracting appointment year FROM APPOINTMENT DATE
SELECT appointment_id,
EXTRACT (YEAR FROM appointment_date) AS appointment_year
FROM appointments;

--Monthly revenue trends
SELECT DATE_TRUNC('month',payment_date) AS month,
	   SUM (total_amount) AS monthly_revenue
FROM billing
GROUP BY MONTH
ORDER BY MONTH;

--CASE STATEMENT
--The hospital wants to categorise treatment cost
--Treatment cost above 500000 is high treatment cost
--Treatment cost above 200000 is medium treatment cost
--Treatment cost below 200000 is low treatment cost
SELECT TREATMENT_ID,
       TREATMENT_COST,
	   CASE 
	   		WHEN TREATMENT_COST > 500000 THEN 'HIGH COST'
			  WHEN TREATMENT_COST > 200000 THEN 'MEDIUM COST'
			  ELSE 'LOW COST'
			  END AS COST_CATEGORY
FROM treatments;

--JOINS
--INNER JOIN
--Hospital management wants us to identify patients with their appointment dates
SELECT p.patient_id,
	   p.first_name,
	   p.last_name,
	   a.appointment_date
FROM patients p
INNER JOIN appointments a
ON p.patient_id = a.patient_id;

--The management wants to show each treatment with the specialisation of the doctor who gave it
SELECT t.treatment_id,
	   t.treatment_type,
	   d.specialization
FROM treatments t
INNER JOIN doctors d
ON t.doctor_id=d.doctor_id;	   
	   
--The management wants to see patient bills with their payment information
SELECT p.first_name,
	   p.last_name,
	   b.total_amount,
	   b.payment_status
FROM billing b
INNER JOIN patients p
ON b.patient_id=p.patient_id;

--The management wants to see doctors and their departments
SELECT d.doctor_name,
	   dp.department_name
FROM doctors d
INNER JOIN departments dp
ON d.department_id=dp.department_id;	 

--The management wants to show patients' names together with their doctors' names
SELECT p.first_name,
	   p.last_name,
	   d.doctor_name
FROM appointments a
INNER JOIN patients p
ON a.patient_id=p.patient_id
INNER JOIN doctors d
ON a.doctor_id=d.doctor_id;

--Treatments that doctors have handled including doctors that have not treated anybody
SELECT d.doctor_name,
	   t.treatment_type,
	   t.treatment_cost
FROM doctors d
LEFT JOIN treatments t
ON d.doctor_id=t.doctor_id;

--Operations dept wants to review all patients 
--together with their admission records including patients who were never admitted
SELECT p.first_name,
	   p.last_name,
	   a.admission_date,
	   a.admission_status,
	   a.diagnosis
FROM patients p 
LEFT JOIN admissions a
ON p.patient_id=a.patient_id;

--Patients with insurance and patients without insurance
SELECT i.provider_name,
	   p.first_name,
	   p.last_name
FROM patients p
LEFT JOIN insurance i
ON i.insurance_id=p.insurance_id;
	   
--RIGHT JOIN
--The lab dpt wants to view all patients with their lab test 
--results including without test records
SELECT p.first_name,
	   p.last_name,
	   l.test_name,
	   l.test_result
FROM lab_results l
RIGHT JOIN patients p
ON l.patient_id=p.patient_id;

--RIGHT JOIN
--all treatment records including treatments that may not YET HAVE ASSIGNED DOCTORS
SELECT d.doctor_name,
	   t.treatment_type,
	   t.treatment_cost
FROM doctors d
RIGHT JOIN treatments t
ON d.doctor_id=t.doctor_id;

--Pharmacy dept wants to see all prescription including 
--all not matching doctors prescription
SELECT d.doctor_name,
	   pr.medication_name,
	   pr.duration_days
FROM doctors d
RIGHT JOIN prescriptions pr
ON d.doctor_id=pr.doctor_id;
	   
--FULL JOIN
--Management wants to review all patients and all appointments 
SELECT p.first_name,
	   p.last_name,
	   a.appointment_date,
	   a.appointment_status
FROM patients p
FULL JOIN appointments a
ON p.patient_id=a.patient_id;

--All patients and their prescriptions
SELECT p.first_name,
	   p.last_name,
	   pr.medication_name,
	   pr.duration_days
FROM patients p
FULL JOIN prescriptions pr
ON p.patient_id=pr.patient_id;

--Finance dept wants to review all billing and treatments records
SELECT b.total_amount,
	   b.payment_status,
	   tr.treatment_type,
	   tr.outcome_status
FROM billing b
FULL JOIN treatments tr
ON b.treatment_id=tr.treatment_id;

SELECT b.*,
	   tr.*
FROM billing b
FULL JOIN treatments tr
ON b.treatment_id=tr.treatment_id;

--AGG, JOIN AND GROUPING
--Management wants to know the total number of appointments handled by each doctor
SELECT d.doctor_name,
	   d.doctor_id,
	   COUNT(a.appointment_id) AS Total_appointments
FROM doctors d
INNER JOIN appointments a
ON d.doctor_id=a.doctor_id
GROUP BY d.doctor_id, d.doctor_name;

--Average consultation fee for each department
SELECT dp.department_name,
	   ROUND(AVG(d.consultation_fee),2) AS Avg_consultation_fee
FROM departments dp
INNER JOIN doctors d
ON dp.department_id=d.department_id
GROUP BY dp.department_name;

--Total billing amount for each patient
SELECT p.patient_id,
	   p.first_name,
	   p.last_name,
	   SUM(b.total_amount) AS Total_billed
FROM patients p
INNER JOIN billing b
ON p.patient_id=b.patient_id
GROUP BY p.patient_id, p.first_name, p.last_name
ORDER BY Total_billed DESC;

--Doctors who have handled more than 10 completed appointments
SELECT d.doctor_name,
	   a.appointment_status,
	   COUNT(a.appointment_id) AS Completed_appointments
FROM doctors d
INNER JOIN appointments a
ON d.doctor_id=a.doctor_id
WHERE a.appointment_status = 'Completed'
GROUP BY d.doctor_id, d.doctor_name, a.appointment_status
HAVING COUNT(a.appointment_id) >10
ORDER BY Completed_appointments DESC;

--Doctors whose consultation fees are higher than the average consultation fee
--SUB QUERY
SELECT doctor_name,
	   specialization,
	   consultation_fee
FROM doctors
WHERE consultation_fee > (SELECT ROUND(AVG(consultation_fee),2)FROM doctors);

--Patients whose conditions are marked as critical
SELECT patient_id,
	   first_name,
	   last_name
FROM patients
WHERE patient_id IN (
	SELECT patient_id
	FROM lab_results
	WHERE test_result = 'Critical'
);

--DATA ACCURACY CHECKS (accounting reconciliation)
--1. Every bill's insurance share plus patient share should equal the bill total
SELECT COUNT(*) AS Bills_not_balancing
FROM billing
WHERE ABS(total_amount - insurance_covered - patient_paid) > 1;

--2. Total billed should reconcile with total treatment cost
SELECT (SELECT SUM(total_amount) FROM billing) AS Total_billed,
       (SELECT SUM(treatment_cost) FROM treatments) AS Total_treatment_cost;

--3. Bills whose amount or patient does not match the treatment they refer to
SELECT COUNT(*) AS Mismatched_bills
FROM billing b
INNER JOIN treatments t
ON b.treatment_id = t.treatment_id
WHERE b.total_amount <> t.treatment_cost
   OR b.patient_id <> t.patient_id;

--4. Treatments billed more than once
SELECT treatment_id,
       COUNT(*) AS Times_billed
FROM billing
GROUP BY treatment_id
HAVING COUNT(*) > 1
ORDER BY Times_billed DESC;

--5. Treatments that were never billed
SELECT COUNT(*) AS Unbilled_treatments
FROM treatments t
LEFT JOIN billing b
ON t.treatment_id = b.treatment_id
WHERE b.bill_id IS NULL;

--6. Bills paid before the treatment took place
SELECT COUNT(*) AS Paid_before_treatment
FROM billing b
INNER JOIN treatments t
ON b.treatment_id = t.treatment_id
WHERE b.payment_date < t.treatment_date;

--7. Bills marked Pending or Partial that still show the full amount as paid
SELECT payment_status,
       COUNT(*) AS Bills
FROM billing
WHERE payment_status IN ('Pending','Partial')
  AND ABS(total_amount - insurance_covered - patient_paid) <= 1
GROUP BY payment_status;

--8. Actual insurance share of each bill compared with the plan's promised coverage
SELECT i.provider_name,
       i.plan_type,
       i.coverage_percentage AS Promised_coverage,
       ROUND(AVG(b.insurance_covered * 100.0 / b.total_amount),1) AS Actual_coverage
FROM billing b
INNER JOIN patients p
ON b.patient_id = p.patient_id
INNER JOIN insurance i
ON p.insurance_id = i.insurance_id
GROUP BY i.provider_name, i.plan_type, i.coverage_percentage
ORDER BY Promised_coverage DESC;

--9. Doctors whose specialization does not match their department
SELECT d.doctor_name,
       d.specialization,
       dp.department_name
FROM doctors d
INNER JOIN departments dp
ON d.department_id = dp.department_id
WHERE d.specialization <> dp.department_name;

--10. Patients still marked Admitted who already have a discharge date
SELECT COUNT(*) AS Admitted_with_discharge_date
FROM admissions
WHERE admission_status = 'Admitted'
  AND discharge_date IS NOT NULL;
