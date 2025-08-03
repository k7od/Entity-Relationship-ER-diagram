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



