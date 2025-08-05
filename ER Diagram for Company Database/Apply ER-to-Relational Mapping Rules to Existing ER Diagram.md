# Relational Mapping Schema:

## 1. Employee: 

    Employee(
          SSN PK,
          Fname,
          Lname,
          BirthDate,
          Gender,
          DNUM FK → Department(DNUM),       -- Each employee works in one department
          SuperSSN FK → Employee(SSN)       -- Recursive: supervisor of employee (nullable)
      )



## 2. Department:

      Department(
          DNUM PK,
          DName
      )

      

## 3. DepartmentLocations:


      DepartmentLocations(
          DNUM FK → Department(DNUM),
          Location,
          PRIMARY KEY (DNUM, Location)
      )

      

## 4. Project:



    Project(
        PNumber PK,
        PName,
        Location,
        City,
        DNUM FK → Department(DNUM)
    )
    

5. WorksOn:
   
    
        WorksOn(
            SSN FK → Employee(SSN),
            PNumber FK → Project(PNumber),
            WorkingHours,
            PRIMARY KEY (SSN, PNumber)
        )    


6. Dependent:
   

        Dependent(
            SSN FK → Employee(SSN),
            DependentName,
            Gender,
            BirthDate,
            PRIMARY KEY (SSN, DependentName)
            ON DELETE CASCADE
        )



7. DepartmentManager:
   

        DepartmentManager(
            DNUM FK → Department(DNUM),
            SSN FK → Employee(SSN),
            HireDate,
            PRIMARY KEY (DNUM)
        )

