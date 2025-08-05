# Company Database ER Diagram Documentation


## Purpose:

This ER diagram models a company’s organizational structure and business rules, capturing relationships between employees, departments, projects, and dependents. It provides a clear schema for database design that reflects real-world management, supervision, and project participation dynamics within the organization.



## Entities & Attributes:


### Employee:

Represents the staff working for the company.

| Attribute  | Type        | Description           |
|------------|-------------|-----------------------|
| SSN        | Primary Key | Unique identifier     |
| Fname      | String      | First name            |
| Lname      | String      | Last name             |
| BirthDate  | Date        | Date of birth         |
| Gender     | String      | Gender                |

- Each employee **must belong** to one **Department**.
- May **supervise** other employees.
- May **have dependents**.
- May **work on multiple projects**, with **WorkingHours** recorded.

---

### Department:

Represents organizational departments.

| Attribute  | Type         | Description                 |
|------------|--------------|-----------------------------|
| DNUM       | Primary Key  | Unique department number    |
| DName      | String       | Department name             |
| Locations  | Multivalued  | One or more department sites|

- Must be **managed by one employee** (includes `HireDate`).
- May include **multiple employees**.
- May have **multiple projects**.

---

### Project:

Represents projects the company is working on.

| Attribute | Type        | Description           |
|-----------|-------------|-----------------------|
| PNumber   | Primary Key | Unique project number |
| PName     | String      | Project name          |
| Location  | String      | Project location      |
| City      | String      | City of the project   |

- Must be assigned to **one department**.
- May have **many employees** working on it (with `WorkingHours`).

---

### Dependent:

Represents dependents of employees.

| Attribute      | Type           | Description                    |
|----------------|----------------|--------------------------------|
| DependentName  | Composite PK   | Unique per employee            |
| Gender         | String         | Gender of dependent            |
| BirthDate      | Date           | Date of birth                  |

- Belongs to **only one employee**.
- Exists **only if the employee is currently employed**.

---

## Relationships:

| Relationship       | Entities Involved              | Description                                                                 |
|--------------------|--------------------------------|-----------------------------------------------------------------------------|
| `Manages`          | Employee → Department          | One employee manages a department, with a `HireDate`.                       |
| `Works_For`        | Employee → Department          | Each employee belongs to exactly one department.                            |
| `Supervises`       | Employee → Employee            | Recursive: one employee supervises others.                                  |
| `Works_On`         | Employee ↔ Project             | Many-to-many with attribute `WorkingHours`.                                 |
| `Assigned_To`      | Project → Department           | Each project is assigned to one department.                                 |
| `Has_Dependent`    | Employee → Dependent           | One-to-many from employee to dependents.                                    |

---

## Key Constraints & Rules:

- **Employee**:
  - Must work in **one department**
  - May supervise other employees
  - May have multiple **dependents**
  - May work on multiple **projects**, each with `WorkingHours`

- **Department**:
  - Has one **manager** (`HireDate` stored)
  - May include **multiple employees**
  - May manage **multiple projects**

- **Project**:
  - Must be part of one **department**
  - May have many **employees** working on it

- **Dependent**:
  - Exists only if associated employee is still employed
  - Tied to **one specific employee**

---

## Cardinalities Summary:

| Relationship           | Cardinality              |
|------------------------|--------------------------|
| Employee–Department    | Many-to-One              |
| Department–Project     | One-to-Many              |
| Employee–Project       | Many-to-Many             |
| Employee–Dependent     | One-to-Many              |
| Employee–Employee      | One-to-Many (Recursive)  |
| Employee–Manages Dept  | One-to-One (with HireDate) |








# Database Implementation Project


## Project Overview:

This project represents the complete implementation of a company database system, transitioning from conceptual design through Entity-Relationship (ER) modeling to actual SQL table creation. The database manages employee information, departmental structure, project assignments, and dependent records.

## Project Objectives:

- Design and implement a comprehensive company database
- Demonstrate understanding of ER modeling and relational schema mapping
- Create functional SQL DDL scripts for database structure
- Establish proper relationships and constraints between entities

## ER Diagram Analysis:

The database design is based on the Entity-Relationship (ER) diagram provided, which illustrates the conceptual structure of a company management system. The diagram shows:

### Visual Representation:
- **Rectangular boxes** represent entities (EMP, DEPT, LOC, PROJECT, WORK, dependent)
- **Diamond shapes** represent relationships between entities
- **Oval shapes** represent attributes of entities
- **Lines with arrows** show the connections and cardinalities
- **Dotted underlines** indicate primary keys
- **Dotted ovals** represent derived or weak entity attributes

### ER Diagram Components Analysis:

The provided ER diagram contains the following key components:

1. **Strong Entities**: EMP, DEPT, PROJECT, LOC
2. **Weak Entity**: dependent (depends on EMP)
3. **Junction Entity**: WORK (represents many-to-many relationship)
4. **Self-Referencing Relationship**: Employee supervision hierarchy
5. **Circular Relationship**: Department-Employee management relationship

## Database Schema:

### Entities and Relationships:

Based on the ER diagram analysis, the database consists of the following main entities:

#### 1. **EMP (Employee)**
- **Primary Key**: SSN (Social Security Number)
- **Attributes**: 
  - Fname (First Name)
  - Lname (Last Name)
  - gender (M/F)
  - BD (Birth Date)
  - super_id (Supervisor SSN - Self-referencing)
  - DUNM (Department Number - Foreign Key)

#### 2. **DEPT (Department)**
- **Primary Key**: DUNM (Department Number)
- **Attributes**:
  - DN (Department Name)
  - SSN (Manager SSN - Foreign Key to EMP)
  - HIRDATE (Manager Hire Date)

#### 3. **LOC (Location)**
- **Primary Key**: DUNM (Department Number - Foreign Key)
- **Attributes**:
  - LOC (Location Name)

#### 4. **PROJECT**
- **Primary Key**: PNUM (Project Number)
- **Attributes**:
  - PN (Project Name)
  - LOC (Location)
  - CITY (City)
  - DUNM (Controlling Department - Foreign Key)

#### 5. **WORK (Employee-Project Assignment)**
- **Composite Primary Key**: (SSN, PNUM)
- **Attributes**:
  - SSN (Employee SSN - Foreign Key)
  - PNUM (Project Number - Foreign Key)
  - hours (Hours worked)

#### 6. **dependent (Employee Dependents)**
- **Composite Primary Key**: (dnum, ssn)
- **Attributes**:
  - dnum (Dependent Number)
  - BD (Birth Date)
  - GENDER (M/F)
  - ssn (Employee SSN - Foreign Key)

## Key Relationships:

1. **Employee-Department**: Many-to-One (Each employee belongs to one department)
2. **Employee-Supervisor**: One-to-Many Self-Referencing (Employees can supervise other employees)
3. **Department-Manager**: One-to-One (Each department has one manager who is an employee)
4. **Department-Location**: One-to-One (Each department has one location)
5. **Employee-Project**: Many-to-Many (Employees can work on multiple projects, projects can have multiple employees)
6. **Employee-Dependent**: One-to-Many (Employees can have multiple dependents)



## Design Process:

### Phase 1: ER Diagram Analysis:

The project began with analyzing the provided ER diagram which showed:
- Entity types and their attributes
- Relationship types and cardinalities
- Primary key identification (underlined attributes)
- Weak entity dependencies (dependent entity)
- Complex relationships (self-referencing, circular)

### Phase 2: Relational Schema Mapping:

The ER diagram was mapped to relational schema following standard rules:
- Each entity type → relation (table)
- Each relationship type → foreign key or junction table
- Weak entities → composite primary keys
- Many-to-many relationships → separate junction tables

### Phase 3: SQL Implementation:

The mapped schema was implemented using SQL DDL commands with:
- Proper data types for each attribute
- Primary and foreign key constraints
- Check constraints for data validation
- Referential integrity maintenance

  

## Implementation Details:

### Database Creation Script:

The `database_implementation.sql` file contains:

- **CREATE TABLE** statements for all entities
- **Primary Key** definitions
- **Foreign Key** constraints with referential integrity
- **CHECK** constraints for data validation
- **ALTER TABLE** statements for circular relationships

### Key Features:

1. **Referential Integrity**: All foreign keys properly reference parent tables
2. **Data Validation**: CHECK constraints ensure valid gender values (M/F)
3. **Composite Keys**: Proper implementation for weak entities and junction tables
4. **Self-Referencing**: Employee supervisor relationship handled correctly
5. **Circular Relationships**: Department-Employee manager relationship implemented safely

## Getting Started:

### Prerequisites:

- SQL Database Management System (MySQL, PostgreSQL, SQL Server, etc.)
- Basic understanding of SQL DDL commands


## Database Constraints:

- **Primary Keys**: Ensure unique identification for each record
- **Foreign Keys**: Maintain referential integrity between related tables
- **CHECK Constraints**: Validate gender values (M/F only)
- **NOT NULL**: Essential fields cannot be empty

## Future Enhancements:

- [ ] Add indexes for performance optimization
- [ ] Implement triggers for business logic
- [ ] Create views for common queries
- [ ] Add stored procedures for complex operations
- [ ] Implement data validation functions
- [ ] Create a web interface for database interaction
- [ ] Add data visualization for the ER diagram relationships
      

## 📋 ER Diagram to SQL Mapping Summary:

| ER Component | SQL Implementation | Notes |
|--------------|-------------------|-------|
| Strong Entity (EMP) | CREATE TABLE EMP | Primary key: SSN |
| Strong Entity (DEPT) | CREATE TABLE DEPT | Primary key: DUNM |
| Strong Entity (PROJECT) | CREATE TABLE PROJECT | Primary key: PNUM |
| Strong Entity (LOC) | CREATE TABLE LOC | Primary key: DUNM (FK) |
| Weak Entity (dependent) | CREATE TABLE dependent | Composite PK: (dnum, ssn) |
| M:N Relationship (WORK) | CREATE TABLE WORK | Junction table with composite PK |
| 1:1 Relationship (DEPT-LOC) | Foreign key in LOC | DUNM references DEPT |
| Self-Referencing (EMP supervision) | super_id in EMP table | References EMP(SSN) |
| Circular Relationship (DEPT-EMP) | ALTER TABLE after creation | Handles manager relationship |



