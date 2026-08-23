# Project 1 – Simple Clinic

## Database Requirements

### 1. Patients

The database should store information about patients. Each patient should have:

- A unique identifier.
- A name.
- A date of birth.
- Gender.
- Contact information, including a phone number and email address.
- An address.

### 2. Doctors

The database should store information about doctors. Each doctor should have:

- A unique identifier.
- A name.
- A specialization.
- A date of birth.
- Gender.
- Contact information, including a phone number and email address.
- An address.

### 3. Appointments

The database should store information about appointments. Each appointment should have:

- A unique identifier.
- A patient.
- A doctor.
- An appointment date and time.
- An appointment status.

#### Appointment Statuses

- **Pending:** The appointment has been scheduled but has not yet occurred.
- **Confirmed:** The appointment has been confirmed by both the patient and the healthcare provider.
- **Completed:** The appointment has taken place as scheduled.
- **Canceled:** The appointment has been canceled either by the patient or the healthcare provider.
- **Rescheduled:** The appointment has been rescheduled for a different date or time.
- **No Show:** The patient did not show up for the appointment without canceling or rescheduling.

### 4. Medical Records

The database should store medical records for patients. For each attended appointment, there should be a medical record.

Each medical record should have:

- A unique identifier.
- A patient.
- A doctor.
- A description of the visit.
- A diagnosis.
- Prescribed medication.
- Any additional notes.

### 5. Prescription

The database should store information about prescribed medications. For each medical record, there should be at most one prescription.

Each prescription should have:

- A unique identifier.
- A medical record.
- Medication name.
- Dosage.
- Frequency.
- Start date.
- End date.
- Any special instructions.

### 6. Payments

The database should store information about payments. Payment is per appointment.

Each payment should have:

- A unique identifier.
- A patient.
- A payment date.
- A payment method.
- Amount paid.
- Any additional notes.

---

_Source: ProgrammingAdvices.com, © Copyright 2023, Project 1 – Simple Clinic (Requirements)._ 
