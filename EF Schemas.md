# Entity FrameWork (ORM)


## What is EF:
* Object Relational Mapper
* Convert from database to code (database-first Approach)
* Convert from code to database (code-first Approach) `most common`

---
## EF Schema
1. TPC  (Table Per Calass)(Default Behaviour)
1. TPH  (Table Per Hierarchy)
1. TPCC (Table Per Concrete Class)

## 1. TPC Schema
if we have this classes
```cs
public class Employee{
    public int Id           {get ; set;};
    public string Name      {get; set;};
    public decimal Salary   {get;set;};
}

public class Product{
    public int Id           {get ; set;};
    public string Name      {get; set;};
    public decimal PricePerUnit   {get;set;};
}

public class Project{
    public int Id           {get ; set;};
    public string Name      {get; set;};
}
```
in database we will find table for every single class Employees , Products and Projects

---

#### `how database will be if there are a Hierarchy Class ?`
```cs
public class Employee{
    public int id {get ; set;};
    public string name{get; set;};
}

public class PartTimeEmployee : Employee{
    public int numofHurs{get; set;};
    public decimal HourRate{get; set;};
}

public class FullTimeEmployee : Employee{
    public DateTime startDate {get; set;};
    public decimal Salary {get; set;};
}
```
in this case the weakness of TPC Schema because of i will get theree tables
and the PK of the base class will be FK in the child Classes
there if i add or update or delete i will do it in two classes (less performance)


---

## 2. TPH Schema
Hierarchy class problem solved in this schema , how?
by merge all atributes in single table and add more colume to detrmind which calss this row in database
but this approach make new problem the number of nulls will increase

## 3. TPCC Schema

The Table per Concrete Class (TPCC) approach is an inheritance mapping strategy used in database design, particularly in Object-Relational Mapping (ORM) frameworks like Entity Framework. In TPCC, each concrete class in an inheritance hierarchy is mapped to its own table in the database, resulting in a one-to-one correspondence between classes and tables.

**Key Points:**

1. **Concrete Classes:** In object-oriented programming, a concrete class is a class that can be instantiated directly. In TPCC, each concrete class in the class hierarchy corresponds directly to a database table.

2. **Table Mapping:** Each concrete class gets its own table in the database. This means that tables in the database mirror the class hierarchy in the application code.

3. **Attributes and Relationships:** Tables created using TPCC contain columns for the attributes defined in the corresponding concrete class. Relationships between classes are represented using foreign key constraints in the database schema.

4. **Simplicity and Clarity:** TPCC offers simplicity and clarity in mapping between classes and tables. It provides a straightforward representation of the class hierarchy in the database schema.

5. **Suitability:** TPCC is suitable for scenarios where entities in the class hierarchy have distinct sets of attributes or require separate tables for other reasons. It offers a clean separation of concerns and facilitates better maintainability of the database schema.

Overall, the Table per Concrete Class (TPCC) approach provides a simple and intuitive way to map object-oriented class hierarchies to relational database schemas, offering clarity and maintainability in database design.

