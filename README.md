# Lab Assessment System

**Lab Assessment System** is a Windows Forms application built in C# using the .NET Framework, designed to streamline academic lab management. It serves as an academic management tool, providing an admin interface to handle student registrations, attendance, Course Learning Outcomes (CLOs), assessments, and results, with data stored in a SQL Server database. The system features a modular backend for database operations and an intuitive frontend for navigation and data management.

## Table of Contents
- [Features](#features)
- [Backend Overview](#backend-overview)
  - [Database Connectivity](#database-connectivity)
  - [Data Management](#data-management)
- [Frontend Overview](#frontend-overview)
  - [Navigation](#navigation)
  - [Forms](#forms)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Dependencies](#dependencies)
- [Contact](#contact)

## Features
- **Student Management**: Register, update, and delete student records with validation for names, emails, registration numbers, and contact details.
- **Attendance Tracking**: Record class and student attendance with date-based checks to prevent duplicates.
- **CLO Management**: Create, update, and delete Course Learning Outcomes, rubrics, and rubric levels, linked to assessments.
- **Assessment Handling**: Manage assessments and their components, assign rubrics, and evaluate student performance.
- **Result Tracking**: Record and display student results based on rubric levels and assessment components.
- **Soft Deletion**: Mark records as "DELETED" instead of permanent removal for assessments, CLOs, rubrics, and rubric levels.
- **Data Validation**: Ensures input integrity (e.g., email formats, registration number patterns like 2023-CS-73).
- **User Interface**: Intuitive dashboard with navigation to manage students, CLOs, assessments, and results.

## Backend Overview

The backend, primarily under the DB_Lab_Management_System namespace, handles database interactions and business logic using ADO.NET and SQL Server, tailored for academic data management.

### Database Connectivity
- **Configuration Class** (Configuration.cs):
  - Implements a singleton pattern to manage a single SqlConnection instance.
  - Connection string: `Data Source=(local);Initial Catalog=ProjectB;Integrated Security=True.`
  - Provides `getConnection()` for reusable database access.
- **Consistent Connection Strings**: Most forms use a static connection string (`Server=(Local);Database=ProjectB;Trusted_Connection=True;`) for direct SQL operations.

### Data Management
- **Database Schema**:
  - **Student**: Stores student details (ID, FirstName, LastName, Contact, Email, RegistrationNumber, Status).
  - **ClassAttendance**: Records class attendance dates (ID, AttendanceDate).
  - **StudentAttendance**: Tracks individual student attendance (AttendanceID, StudentID, AttendanceStatus).
  - **CLO**: Stores Course Learning Outcomes (ID, Name, DateCreated, DateUpdated).
  - **Rubric**: Defines rubrics linked to CLOs (ID, Details, CLOId).
  - **RubricLevel**: Specifies rubric levels (ID, RubricID, Details, MeasurementLevel).
  - **Assessment**: Manages assessments (ID, Title, DateCreated, TotalMarks, TotalWeightage).
  - **AssessmentComponent**: Links assessments to rubrics (ID, AssessmentId, Name, TotalMarks, RubricId, DateCreated, DateUpdated).
  - **StudentResult**: Records student performance (StudentId, AssessmentComponentId, RubricMeasurementId, EvaluationDate).
  - **Lookup**: Stores status codes (e.g., ATTENDANCE_STATUS, STUDENT_STATUS).
- **CRUD Operations**:
  - **Create**: Inserts records with validation (e.g., Add_Click in StudentRegistration.cs, Assessment.cs).
  - **Read**: Loads data into DataGridView controls (e.g., LoadIntoDataGrid methods).
  - **Update**: Modifies existing records (e.g., Update_Click in Rubrics.cs, Form5.cs).
  - **Delete**: Soft deletes by prefixing "DELETED" to titles/details or updating status (e.g., Delete_Click in RubricLevels.cs, StudentRegistration.cs).
- **Validation**:
  - Registration numbers follow `20xx-xx-xx` format (`RegistrationNumberCheck` in StudentRegistration.cs).
  - Emails validated via regex (`EmailCheck`).
  - Contact numbers require `923xxxxxxxxx` or `03xxxxxxxxx` formats (`IsValidContactNumber`).
  - Names must start with a capital letter and contain only alphabets (`NameInputCheck`).
- **Error Handling**: Try-catch blocks display user-friendly error messages via MessageBox.

## Frontend Overview

The frontend, built with Windows Forms, provides an admin dashboard and specialized forms for managing academic entities.

### Navigation
- **AdminDashboard** (AdminDashboard.cs): Central hub with buttons to access:
  - Manage Students
  - Manage CLOs
  - Manage Assessments
  - Log Out (exits application)
- **Sub-Menus**:
  - **ManageStudents** (ManageStudents.cs): Links to student registration, class attendance, and student attendance.
  - **ManageCLOs** (ManageCLOs.cs): Accesses CLOs, rubrics, and rubric levels.
  - **ManageAssessments** (ManageAssessments.cs): Manages assessments, components, and results.
- **Back/Home Buttons**: Each form includes buttons to return to the previous menu or dashboard.

### Forms
- **Student Management**:
  - StudentRegistration.cs: Add, update, delete students with input validation.
  - ClassAttendance.cs: Record class attendance dates.
  - StudentAttendance.cs: Mark individual student attendance with status (e.g., Present, Absent).
- **CLO Management**:
  - CLO.cs: Manage Course Learning Outcomes.
  - Rubrics.cs: Create, update, delete rubrics linked to CLOs.
  - RubricLevels.cs: Define rubric levels with measurement details.
- **Assessment Management**:
  - Assessment.cs: Add, update, delete assessments with marks and weightage.
  - Form5.cs: Manage assessment components, linking to rubrics.
  - Results.cs: Record and display student evaluation results.
- **Data Display**: Forms use DataGridView for record visualization and selection.

## Project Structure
```bash
    DB_Lab_Management_System/
        ├── Program.cs                      # Application entry point
        ├── Configuration.cs                # Singleton for SQL connection
        ├── AdminDashboard.cs               # Main dashboard form
        
        ├── Attendance/
        │   ├── ManageStudents.cs           # Menu for student-related operations
        │   ├── StudentRegistration.cs      # Student record management
        │   ├── ClassAttendance.cs          # Class attendance management
        │   └── StudentAttendance.cs        # Individual student attendance marking
        
        ├── CLOs/
        │   ├── ManageCLOs.cs               # Menu for CLO-related operations
        │   ├── CLO.cs                      # CLO definitions
        │   ├── Rubrics.cs                  # Rubric creation and linking to CLOs
        │   └── RubricLevels.cs             # Rubric level management
        
        └── Assessments/
            ├── ManageAssessments.cs        # Menu for assessment-related operations
            ├── Assessment.cs               # Assessment creation and management
            ├── Form5.cs                    # Assessment component handling
            └── Results.cs                  # Recording and displaying results
```
## Installation
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/aabr2612/DB-lab-management-system.git
2. **Set Up SQL Server**:
   - Ensure SQL Server is installed and running (e.g., SQL Server Express).
   - Create a database named `ProjectB` using SQL Server Management Studio (SSMS) or a similar tool.
   - Set up the required tables (`Student`, `ClassAttendance`, `StudentAttendance`, `CLO`, `Rubric`, `RubricLevel`, `Assessment`, `AssessmentComponent`, `StudentResult`, `Lookup`). Note: The schema is not included here; refer to your database design to create these tables with appropriate fields and relationships.
   - Update connection strings in `Configuration.cs` and other forms if your SQL Server instance differs (e.g., replace `DESKTOP-L60GA3Q` with your server name or `(local)` if applicable).

3. **Open in Visual Studio**:
   - Launch Visual Studio (2019 or later recommended).
   - Open the `.sln` file from the cloned repository.

4. **Build the Solution**:
   - Go to `Build > Build Solution` or press `Ctrl+Shift+B`.
   - Ensure all dependencies are resolved.

## Usage

1. **Run the Application**:
   - Set `DB_Lab_Management_System` as the startup project in Visual Studio.
   - Press `F5` or select `Debug > Start Debugging` to launch the admin dashboard.

2. **Navigation**:
   - **Admin Dashboard**: Use buttons to access student management, Course Learning Outcomes (CLOs), assessments, or log out.
   - **Manage Students**: Register new students, record class attendance, or mark individual student attendance.
   - **Manage CLOs**: Add or edit Course Learning Outcomes, rubrics, and rubric levels.
   - **Manage Assessments**: Create assessments, define components, or evaluate student results.

3. **Key Operations**:
   - **Student Registration**:
     - Navigate to the student registration section under manage students.
     - Enter details like a registration number in the format `20XX-CS-XX`, a valid email, and a contact number starting with `923` or `03`.
     - Save new records or update/delete existing ones.
   - **Attendance**:
     - Go to class attendance to set a class date.
     - Use student attendance to mark students as Present or Absent for a selected date.
   - **CLOs and Rubrics**:
     - Access the CLO management section to define learning outcomes and link rubrics with evaluation criteria.
     - Specify measurement details for rubric levels, such as a 1-4 scale.
   - **Assessments**:
     - Create assessments with titles, marks, and weightage in the assessments section.
     - Add components and link them to rubrics for evaluation.
   - **Results**:
     - Navigate to the results section to evaluate students by selecting rubrics and components, recording their scores.

## Dependencies
- .NET Framework 4.8 or later
- SQL Server Express or a compatible version
- Visual Studio 2019 or later
- ADO.NET for database connectivity

## Contact

For questions or feedback, feel free to reach out:
- GitHub: [aabr2612](https://github.com/aabr2612)
- Email: aabr2612@gmail.com
