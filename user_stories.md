## Administrator user stories

**Title:**
_As an Administrator, I want to log in with my username and password, so that I can manage the platform securely._

**Acceptance Criteria:**
1. Given valid admin credentials, when I submit the login form, then I am authenticated and redirected to the admin dashboard.
2. Given invalid credentials, when I submit the login form, then I see an error message and remain unauthenticated.
3. Given I am authenticated, when I refresh or navigate, then my session/token remains valid until logout/expiration.

**Priority:** High
**Story Points:** 3
**Notes:**
- Consider rate-limiting / lockout after repeated failures.

---

**Title:**
_As an Administrator, I want to log out of the portal, so that I can protect access to the system._

**Acceptance Criteria:**
1. Given I am logged in, when I click “Logout”, then my session/token is invalidated.
2. After logout, when I try to access admin pages, then I am redirected to the login page.
3. After logout, when I use the browser back button, then protected pages are not accessible.

**Priority:** High
**Story Points:** 2
**Notes:**
- Ensure cookies/localStorage token is cleared (if used).

---

**Title:**
_As an Administrator, I want to add doctors to the portal, so that patients can book appointments with them._

**Acceptance Criteria:**
1. Given I provide required doctor data (name, email, specialization, contact), when I submit, then a new doctor profile is created.
2. Given the email already exists, when I submit, then I see a validation error and no doctor is created.
3. Given the doctor is created, then the doctor appears in the doctors list and is available for selection.

**Priority:** High
**Story Points:** 5
**Notes:**
- Define required fields and uniqueness constraints (e.g., email).

---

**Title:**
_As an Administrator, I want to delete a doctor profile, so that I can remove inactive or incorrect accounts._

**Acceptance Criteria:**
1. Given I select a doctor, when I confirm deletion, then the doctor profile is removed from the system.
2. Given the doctor has future appointments, when I attempt deletion, then the system prevents deletion or requires a defined policy (cancel/reassign).
3. Given deletion completes, then the doctor no longer appears in public listings.

**Priority:** High
**Story Points:** 5
**Notes:**
- Clarify policy for existing appointments and prescriptions.

---

**Title:**
_As an Administrator, I want to run a MySQL stored procedure via MySQL CLI to get the number of appointments per month, so that I can track usage statistics._

**Acceptance Criteria:**
1. Given the stored procedure exists, when I run it in MySQL CLI, then it returns appointment counts grouped by month.
2. The output includes month and total appointments, and matches the database appointment records.
3. If there are no appointments for a month, then the procedure returns zero or omits the month (as defined).

**Priority:** Medium
**Story Points:** 3
**Notes:**
- Document the exact CLI command and expected output format in the README/report.

## Patient user stories

**Title:**
_As a Patient, I want to view a list of doctors without logging in, so that I can explore options before registering._

**Acceptance Criteria:**
1. Given I am not logged in, when I open the doctors page, then I can see the list of doctors.
2. The list includes key fields (name, specialization, contact/info) needed to choose a doctor.
3. If no doctors exist, then I see an empty-state message.

**Priority:** High
**Story Points:** 3
**Notes:**
- Public endpoint/page should not expose sensitive doctor data.

---

**Title:**
_As a Patient, I want to register using my email and password, so that I can book appointments._

**Acceptance Criteria:**
1. Given I enter a valid email and a compliant password, when I submit registration, then my account is created.
2. Given the email is already registered, when I submit, then I see a clear error message.
3. Given registration succeeds, then I can log in with the created credentials.

**Priority:** High
**Story Points:** 5
**Notes:**
- Define password rules (length, complexity) and validation messages.

---

**Title:**
_As a Patient, I want to log in to the portal, so that I can manage my bookings._

**Acceptance Criteria:**
1. Given valid credentials, when I log in, then I am redirected to my patient dashboard.
2. Given invalid credentials, when I log in, then I see an error and remain logged out.
3. Given I am logged in, then I can access patient-protected pages and actions.

**Priority:** High
**Story Points:** 3
**Notes:**
- Reuse token/session mechanism consistently across UI pages.

---

**Title:**
_As a Patient, I want to log out, so that I can secure my account._

**Acceptance Criteria:**
1. Given I am logged in, when I click logout, then my session/token is invalidated.
2. After logout, when I try to open my dashboard, then I am redirected to login.
3. After logout, sensitive data is not visible on refresh/back navigation.

**Priority:** High
**Story Points:** 2
**Notes:**
- Same behavior as admin logout, but for patient role.

---

**Title:**
_As a Patient, I want to book a one-hour appointment with a doctor after logging in, so that I can consult with them._

**Acceptance Criteria:**
1. Given I am logged in, when I pick a doctor and a date/time, then the appointment duration is exactly 1 hour.
2. Given the doctor is unavailable at that time, when I try to book, then the system prevents booking and shows available slots.
3. Given booking succeeds, then the appointment appears in my upcoming appointments list and in the doctor's calendar.

**Priority:** High
**Story Points:** 8
**Notes:**
- Define time zone and slot boundaries (e.g., start times every 30/60 minutes).

---

**Title:**
_As a Patient, I want to view my upcoming appointments, so that I can prepare accordingly._

**Acceptance Criteria:**
1. Given I am logged in, when I open “Upcoming Appointments”, then I see future appointments sorted by date/time.
2. Each appointment shows doctor name, date/time, and status.
3. If I have no upcoming appointments, then I see an empty-state message.

**Priority:** Medium
**Story Points:** 3
**Notes:**
- Consider pagination if many appointments exist.

## Doctor user stories

**Title:**
_As a Doctor, I want to log in to the portal, so that I can manage my appointments._

**Acceptance Criteria:**
1. Given valid doctor credentials, when I log in, then I am redirected to the doctor dashboard.
2. Given invalid credentials, when I log in, then I see an error and remain logged out.
3. Given I am logged in, then only doctor-authorized pages are accessible.

**Priority:** High
**Story Points:** 3
**Notes:**
- Ensure role-based authorization separates doctor vs admin vs patient.

---

**Title:**
_As a Doctor, I want to log out of the portal, so that I can protect my data._

**Acceptance Criteria:**
1. Given I am logged in, when I click logout, then my session/token is invalidated.
2. After logout, when I try to open the doctor dashboard, then I am redirected to login.
3. After logout, browser back/refresh does not reveal protected content.

**Priority:** High
**Story Points:** 2
**Notes:**
- Same security behavior as other roles.

---

**Title:**
_As a Doctor, I want to view my appointment calendar, so that I can stay organized._

**Acceptance Criteria:**
1. Given I am logged in, when I open my calendar, then I see all upcoming appointments for me.
2. Appointments are displayed with patient name (or patient id per privacy policy), date/time, and duration.
3. The calendar indicates canceled/completed appointments distinctly.

**Priority:** High
**Story Points:** 5
**Notes:**
- Decide whether to show full patient details or minimal info by default.

---

**Title:**
_As a Doctor, I want to mark my unavailability, so that patients only see available times._

**Acceptance Criteria:**
1. Given I select a date/time range, when I mark myself unavailable, then that slot is blocked from booking.
2. Given a patient tries to book during an unavailable slot, then the booking is rejected.
3. Given I remove unavailability, then the slot becomes bookable again.

**Priority:** High
**Story Points:** 8
**Notes:**
- Define if unavailability can overlap existing appointments (and what happens).

---

**Title:**
_As a Doctor, I want to update my profile with specialization and contact info, so that patients have up-to-date information._

**Acceptance Criteria:**
1. Given I edit my profile fields, when I save, then changes persist and display correctly on the doctors list.
2. Required fields are validated and errors are shown before saving.
3. Only the logged-in doctor (or admin) can update the doctor profile.

**Priority:** Medium
**Story Points:** 5
**Notes:**
- Track which fields are editable by doctor vs admin.

---

**Title:**
_As a Doctor, I want to view patient details for upcoming appointments, so that I can be prepared._

**Acceptance Criteria:**
1. Given I have an upcoming appointment, when I open its details, then I can see patient information required for the consultation.
2. I can only access details for patients that have appointments with me.
3. If the appointment is canceled or not found, then access is denied or a clear message is shown.

**Priority:** Medium
**Story Points:** 5
**Notes:**
- Define what “patient details” are allowed (privacy/compliance).