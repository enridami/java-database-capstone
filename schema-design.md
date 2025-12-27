
## MySQL Database Design
### Patients
- id: INT, Primary Key, Auto Increment
- first_name: VARCHAR(100), Not Null
- last_name: VARCHAR(100), Not Null
- email: VARCHAR(150), Not Null, Unique
- password_hash: VARCHAR(255), Not Null
- phone: VARCHAR(30), Null
- date_of_birth: DATE, Null
- created_at: DATETIME, Not Null (default current timestamp)
- updated_at: DATETIME, Null

**Notes:**
- Email must be unique to support login/registration.
- Store passwords as hashes only (never plain text).
- If a patient is deleted, consider whether appointments should be retained (audit/history) vs deleted (privacy). A common policy is SOFT DELETE.

### Doctors
- id: INT, Primary Key, Auto Increment
- first_name: VARCHAR(100), Not Null
- last_name: VARCHAR(100), Not Null
- email: VARCHAR(150), Not Null, Unique
- specialization: VARCHAR(120), Not Null
- phone: VARCHAR(30), Null
- office_location: VARCHAR(150), Null
- bio: TEXT, Null
- active: TINYINT(1), Not Null (default 1)
- created_at: DATETIME, Not Null (default current timestamp)
- updated_at: DATETIME, Null

**Notes:**
- Email unique: prevents duplicate doctor profiles.
- `active` allows disabling profiles without deleting and breaking FK references.

### Appointments
- id: INT, Primary Key, Auto Increment
- doctor_id: INT, Not Null, Foreign Key → doctors(id)
- patient_id: INT, Not Null, Foreign Key → patients(id)
- start_time: DATETIME, Not Null
- end_time: DATETIME, Not Null
- status: INT, Not Null (0 = Scheduled, 1 = Completed, 2 = Cancelled)
- reason: VARCHAR(255), Null
- created_at: DATETIME, Not Null (default current timestamp)

**Constraints / Rules (business-level, enforce in code and optionally DB):**
- Prevent overlapping appointments for the same doctor:
  - Enforced via service logic; optionally add a unique constraint approach is limited for overlap, so validate in code.
- For this capstone: duration should be 1 hour:
  - Enforce `end_time = start_time + 1 hour` in service layer.

**Notes:**
- For deletes:
  - If doctor is deleted, prefer restricting deletion or soft-delete (because appointments reference it).
  - If patient is deleted, decide whether to anonymize appointment records.


### Admin
- id: INT, Primary Key, Auto Increment
- username: VARCHAR(80), Not Null, Unique
- password_hash: VARCHAR(255), Not Null
- email: VARCHAR(150), Null, Unique
- created_at: DATETIME, Not Null (default current timestamp)
- last_login_at: DATETIME, Null

**Notes:**
- Admin is separated from patients/doctors for clear role boundaries.

### Doctor_unavailability
- id: INT, Primary Key, Auto Increment
- doctor_id: INT, Not Null, Foreign Key → doctors(id)
- start_time: DATETIME, Not Null
- end_time: DATETIME, Not Null
- reason: VARCHAR(255), Null
- created_at: DATETIME, Not Null (default current timestamp)

**Notes:**
- Supports the user story “doctor marks unavailability”.
- Booking service should reject appointments that overlap these ranges.


## MongoDB Collection Design

### Collection: prescriptions
```json
{
  "_id": "ObjectId('64abc123456')",
  "patientName": "John Smith",
  "appointmentId": 51,
  "medication": "Paracetamol",
  "dosage": "500mg",
  "doctorNotes": "Tome 1 tableta cada 6 horas.",
  "refillCount": 2,
  "pharmacy": {
    "name": "Walgreens SF",
    "location": "Market Street"
  }
}