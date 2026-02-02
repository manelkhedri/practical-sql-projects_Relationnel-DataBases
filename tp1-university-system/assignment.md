# TP1: University Management System

## 🎯 Objectives
- Design and create a relational database
- Implement tables with constraints
- Insert test data
- Write SQL queries to analyze data

---

## 📊 Database Description

You need to create a **University Management System** to track:
- Departments
- Professors
- Students
- Courses
- Enrollments
- Grades

---

## 🗂️ Tables to Create

### 1. Table: `departments`
Stores information about university departments.

**Columns:**
- `department_id` (INT, PRIMARY KEY, AUTO_INCREMENT)
- `department_name` (VARCHAR(100), NOT NULL)
- `building` (VARCHAR(50))
- `budget` (DECIMAL(12, 2))
- `department_head` (VARCHAR(100))
- `creation_date` (DATE)

---

### 2. Table: `professors`
Stores professor information.

**Columns:**
- `professor_id` (INT, PRIMARY KEY, AUTO_INCREMENT)
- `last_name` (VARCHAR(50), NOT NULL)
- `first_name` (VARCHAR(50), NOT NULL)
- `email` (VARCHAR(100), UNIQUE, NOT NULL)
- `phone` (VARCHAR(20))
- `department_id` (INT, FOREIGN KEY → departments)
- `hire_date` (DATE)
- `salary` (DECIMAL(10, 2))
- `specialization` (VARCHAR(100))

**Constraints:**
- Foreign key: `department_id` references `departments(department_id)`
- ON DELETE SET NULL
- ON UPDATE CASCADE

---

### 3. Table: `students`
Stores student information.

**Columns:**
- `student_id` (INT, PRIMARY KEY, AUTO_INCREMENT)
- `student_number` (VARCHAR(20), UNIQUE, NOT NULL)
- `last_name` (VARCHAR(50), NOT NULL)
- `first_name` (VARCHAR(50), NOT NULL)
- `date_of_birth` (DATE)
- `email` (VARCHAR(100), UNIQUE, NOT NULL)
- `phone` (VARCHAR(20))
- `address` (TEXT)
- `department_id` (INT, FOREIGN KEY → departments)
- `level` (VARCHAR(20), CHECK: must be 'L1', 'L2', 'L3', 'M1', or 'M2')
- `enrollment_date` (DATE, DEFAULT CURRENT_DATE)

**Constraints:**
- Foreign key: `department_id` references `departments(department_id)`
- ON DELETE SET NULL
- ON UPDATE CASCADE

---

### 4. Table: `courses`
Stores course information.

**Columns:**
- `course_id` (INT, PRIMARY KEY, AUTO_INCREMENT)
- `course_code` (VARCHAR(10), UNIQUE, NOT NULL)
- `course_name` (VARCHAR(150), NOT NULL)
- `description` (TEXT)
- `credits` (INT, NOT NULL, CHECK: must be > 0)
- `semester` (INT, CHECK: must be between 1 and 2)
- `department_id` (INT, FOREIGN KEY → departments)
- `professor_id` (INT, FOREIGN KEY → professors)
- `max_capacity` (INT, DEFAULT 30)

**Constraints:**
- Foreign key: `department_id` references `departments(department_id)`
  - ON DELETE CASCADE, ON UPDATE CASCADE
- Foreign key: `professor_id` references `professors(professor_id)`
  - ON DELETE SET NULL, ON UPDATE CASCADE

---

### 5. Table: `enrollments`
Links students to courses.

**Columns:**
- `enrollment_id` (INT, PRIMARY KEY, AUTO_INCREMENT)
- `student_id` (INT, NOT NULL, FOREIGN KEY → students)
- `course_id` (INT, NOT NULL, FOREIGN KEY → courses)
- `enrollment_date` (DATE, DEFAULT CURRENT_DATE)
- `academic_year` (VARCHAR(9), NOT NULL) - Format: '2024-2025'
- `status` (VARCHAR(20), DEFAULT 'In Progress', CHECK: 'In Progress', 'Passed', 'Failed', 'Dropped')

**Constraints:**
- Foreign keys with CASCADE delete/update
- UNIQUE constraint on (student_id, course_id, academic_year)

---

### 6. Table: `grades`
Stores student grades.

**Columns:**
- `grade_id` (INT, PRIMARY KEY, AUTO_INCREMENT)
- `enrollment_id` (INT, NOT NULL, FOREIGN KEY → enrollments)
- `evaluation_type` (VARCHAR(30), CHECK: 'Assignment', 'Lab', 'Exam', 'Project')
- `grade` (DECIMAL(5, 2), CHECK: between 0 and 20)
- `coefficient` (DECIMAL(3, 2), DEFAULT 1.00)
- `evaluation_date` (DATE)
- `comments` (TEXT)

**Constraints:**
- Foreign key: `enrollment_id` references `enrollments(enrollment_id)`
- ON DELETE CASCADE, ON UPDATE CASCADE

---

## 📌 Required Indexes

Create the following indexes for performance:
- `idx_student_department` on `students(department_id)`
- `idx_course_professor` on `courses(professor_id)`
- `idx_enrollment_student` on `enrollments(student_id)`
- `idx_enrollment_course` on `enrollments(course_id)`
- `idx_grades_enrollment` on `grades(enrollment_id)`

---

## 📝 Test Data to Insert

After creating the tables, insert the following test data:

### Departments (4 departments)
1. Computer Science - Building A - Budget: 500,000
2. Mathematics - Building B - Budget: 350,000
3. Physics - Building C - Budget: 400,000
4. Civil Engineering - Building D - Budget: 600,000

### Professors (6 professors)
- At least 3 in Computer Science
- At least 1 in each other department

### Students (8 students minimum)
- Mix of levels (L2, L3, M1)
- Distributed across departments

### Courses (7 courses minimum)
- Various credits (5-6 credits)
- Different semesters
- Assigned to professors

### Enrollments (15 enrollments minimum)
- Students enrolled in multiple courses
- Mix of current year and previous year
- Various statuses

### Grades (12 grades minimum)
- Different evaluation types
- Various coefficients
- Range of grades (10-18)

---

##  Submission Requirements

Your submission file `tp1_solutions.sql` must contain:

1. **Database creation:** `CREATE DATABASE university_db;`
2. **All 6 tables** with correct structure and constraints
3. **All indexes**
4. **INSERT statements** for test data
5. **30 SQL queries** (see queries.sql file)

---

## 📅 Deadline
**February 17, 2026**

Good luck! 
