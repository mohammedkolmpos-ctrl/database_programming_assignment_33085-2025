# database_programming_assignment_33085-2025_mohamed ali 
# Individual Assignment I: CTEs & SQL Window Functions 


---

## [cite_start]1. Business Scenario & Database Schema 
[cite_start]This project implements a **Hospital Management System** to optimize clinical operations, track financial performance, and analyze doctor-patient metrics. The database consists of 6 interrelated tables:
* [cite_start]**Patients**: Stores detailed demographic data for hospital patients.
* [cite_start]**Doctors**: Contains physician profiles, specialties, salaries, and managerial hierarchies.
* [cite_start]**Appointments**: Links patients and doctors with specific clinical visit dates and statuses.
* [cite_start]**Medications**: Maintains the inventory and retail pricing of pharmaceutical products.
* [cite_start]**Prescriptions**: Tracks the specific medications and quantities prescribed during appointments.
* [cite_start]**Billings**: Records individual appointment financial transactions and payment status.

## [cite_start]2. Database Creation & Data Insertion (SQL Scripts)

 Create Doctors Table with a self-referencing relationship for management
CREATE TABLE Doctors (
    DoctorID INT PRIMARY KEY IDENTITY(1,1),
    DoctorName VARCHAR(100) NOT NULL,
    Specialty VARCHAR(50) NOT NULL,
    Salary DECIMAL(10,2),
    HireDate DATE,
    ManagerID INT NULL FOREIGN KEY REFERENCES Doctors(DoctorID));

Create Patients Table
CREATE TABLE Patients (
    PatientID INT PRIMARY KEY IDENTITY(1,1),
    PatientName VARCHAR(100) NOT NULL,
    Gender VARCHAR(10),
    DateOfBirth DATE,
    City VARCHAR(50));

 Create Appointments Table linking Patients and Doctors
CREATE TABLE Appointments (
    AppointmentID INT PRIMARY KEY IDENTITY(1,1),
    PatientID INT FOREIGN KEY REFERENCES Patients(PatientID),
    DoctorID INT FOREIGN KEY REFERENCES Doctors(DoctorID),
    AppointmentDate DATE,
    Status VARCHAR(20) -- Completed, Cancelled, Pending);

Create Medications Table
CREATE TABLE Medications (
    MedicationID INT PRIMARY KEY IDENTITY(1,1),
    MedicationName VARCHAR(100) NOT NULL,
    Price DECIMAL(10,2) NOT NULL);

 Create Prescriptions Table linking Appointments and Medications
CREATE TABLE Prescriptions (
    PrescriptionID INT PRIMARY KEY IDENTITY(1,1),
    AppointmentID INT FOREIGN KEY REFERENCES Appointments(AppointmentID),
    MedicationID INT FOREIGN KEY REFERENCES Medications(MedicationID),
    Quantity INT NOT NULL);

Create Billings Table linked directly to Appointments
CREATE TABLE Billings (
    BillingID INT PRIMARY KEY IDENTITY(1,1),
    AppointmentID INT FOREIGN KEY REFERENCES Appointments(AppointmentID),
    Amount DECIMAL(10,2) NOT NULL,
    PaymentStatus VARCHAR(20) -- Paid, Unpaid);


Insert Doctors (Including Chief Medical Officer and Department Heads)
INSERT INTO Doctors (DoctorName, Specialty, Salary, HireDate, ManagerID) VALUES
('Dr. Ahmed', 'Cardiology', 12000, '2020-01-15', NULL),   -- Chief Medical Officer
('Dr. Haneen', 'Pediatrics', 9500, '2022-03-10', 1),     -- Reports to Dr. Ahmed
('Dr. Ferdous', 'Cardiology', 11000, '2021-06-20', 1),   -- Reports to Dr. Ahmed
('Dr. Khalid', 'Neurology', 10500, '2023-01-05', 1);

 Insert Patients
INSERT INTO Patients (PatientName, Gender, DateOfBirth, City) VALUES
('John Doe', 'Male', '1985-05-12', 'Kigali'),
('Jane Smith', 'Female', '1992-08-24', 'Gisenyi'),
('Ali Mustafa', 'Male', '1978-11-03', 'Kigali'),
('Grace Mutoni', 'Female', '2000-01-15', 'Butare');

 Insert Appointments
INSERT INTO Appointments (PatientID, DoctorID, AppointmentDate, Status) VALUES
(1, 1, '2026-06-01', 'Completed'),
(2, 2, '2026-06-02', 'Completed'),
(3, 1, '2026-06-15', 'Completed'),
(4, 3, '2026-06-16', 'Completed'),
(1, 4, '2026-06-20', 'Completed'),
(2, 1, '2026-06-25', 'Completed');

 Insert Medications
INSERT INTO Medications (MedicationName, Price) VALUES
('Aspirin', 5.00),
('Amoxicillin', 15.50),
('Paracetamol', 3.00),
('Lipitor', 45.00);

 Insert Prescriptions
INSERT INTO Prescriptions (AppointmentID, MedicationID, Quantity) VALUES
(1, 1, 2),
(1, 4, 1),
(2, 2, 1),
(3, 1, 3),
(4, 3, 2);

 Insert Billings
INSERT INTO Billings (AppointmentID, Amount, PaymentStatus) VALUES
(1, 250.00, 'Paid'),
(2, 150.00, 'Paid'),
(3, 300.00, 'Unpaid'),
(4, 180.00, 'Paid'),
(5, 400.00, 'Paid'),
(6, 220.00, 'Unpaid');
