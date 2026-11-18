# PISON_HOSPITAL_ANALYSIS
SQL analysis of a 10-table hospital database in PostgreSQL. Covers revenue, collections, appointments, waiting times and doctor workloads, with accounting-style checks that reconcile bills to treatments. Finds ₦722M in bills not marked paid and only 1 in 4 appointments completed. Practice dataset from a SQL bootcamp.
Pison Hospital Operations and Revenue Analysis

Project Overview

Pison Hospital is a multi-branch hospital network. Its database holds two years of records (May 2024 to May 2026) for 500 patients, 40 doctors, 3,000 appointments, 2,000 treatments and 2,000 bills. I wanted to find out how well the hospital was turning appointments into care and bills into cash, and where patients and money were slipping through. A hospital can use those answers to cut waiting times, use its doctors better and collect what it is owed.

Executive Summary

Pison billed ₦1.06 billion, but only a third of its bills are marked fully paid, which leaves ₦722 million to chase. Only one in four appointments ends up completed, and patients wait an hour and a half on average. Better collections and smoother scheduling are the biggest opportunities.

Methodology

Set up the database. I built 10 linked tables covering patients, doctors, departments, appointments, treatments, billing, admissions, insurance, lab results and prescriptions.
Answered the questions management would ask. I used SQL (a language for asking questions of large tables of data) to filter, sort and total the records. That covered revenue by payment status, treatment costs by type, doctor workloads, consultation fees, waiting times and monthly revenue trends.
Connected the tables. I linked patients to their appointments, bills, admissions and lab results, and doctors to their treatments and prescriptions. That made it possible to see the full picture of each patient, and of each doctor’s work.
Checked that the numbers added up. This is where my accounting background came in. I confirmed that every bill’s insurance share plus the patient’s share equalled the bill total. I also tested whether bills matched the treatments they referred to. They didn’t, and the details are listed below.

Findings

Most of the money is still outstanding. Of ₦1.06B billed, only 634 of 2,000 bills (32%) are marked fully paid. A further ₦360M is marked pending and ₦362M partly paid.
Insurers carry most of the bill. Insurance covered ₦688M (65%), and patients paid ₦367M (35%).
Only one in four appointments is completed. Just 712 of 3,000 appointments were completed. 773 were cancelled, 745 rescheduled and 770 are still pending.
Waiting times are long. Patients wait 92 minutes on average. Two in three waited over an hour, one in three waited over two hours, and the longest wait was three hours.
Fees don’t follow experience. Consultation fees range from ₦10,282 to ₦99,353. The lowest fee belongs to a doctor with 13 years’ experience, while a doctor with 10 years charges ₦91,959.
Workloads are uneven. The busiest doctor handled 103 appointments, nearly twice as many as the least busy (52).

Recommendations

Chase the ₦722M not marked as paid. Start with the biggest pending bills and the insurers that owe the most. A monthly report showing how long each bill has been unpaid would stop debts from piling up.
Fix scheduling to cut waits and cancellations. Send appointment reminders and confirmations, and space out bookings so patients aren’t waiting two to three hours.
Review fees and spread the work evenly. Set consultation fees by clear rules, such as specialty and experience. Move appointments from overloaded doctors to those with spare capacity.
