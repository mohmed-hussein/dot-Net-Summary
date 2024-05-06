## Four Mapping Ways in Entity Framework

### 1. By Convention (Default Behavior)
By Convention, also known as Default Behavior, is the approach where Entity Framework automatically maps entity classes to database tables based on a set of conventions. These conventions infer mapping rules from the class and property names, such as naming conventions and key properties, without the need for explicit configuration.

```csharp
public class Product
{
    public int ProductId { get; set; } // primary key
    public string Name { get; set; }   //nvarchar(max) 
    public decimal Price { get; set; }  //decimal(18 , 2)
}

```

### 2. Data Annotations
Data Annotations is an attribute-based approach where developers use attributes provided by the `System.ComponentModel.DataAnnotations` namespace to apply mapping configurations directly to entity classes and properties. Annotations such as `[Table]`, `[Column]`, and `[Key]` are used to specify table names, column names, primary keys, and other mapping details.

```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

[Table("Products")]
public class Product
{
    [Key]
    public int Id { get; set; }

    [Column("ProductName")]
    public string Name { get; set; }

    public decimal Price { get; set; }
}

```

### 3. Fluent API
Fluent API is a programmatic configuration approach where developers use a fluent interface provided by Entity Framework to specify mapping configurations in the `OnModelCreating` method of a DbContext class. With Fluent API, developers have more control over mapping details and can define complex mappings that cannot be achieved using conventions or data annotations alone.

```csharp
public class MyDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>()
            .ToTable("Products")
            .HasKey(p => p.Id)
            .Property(p => p.Name)
            .HasColumnName("ProductName");
    }
}

```
In this example, we use Fluent API to specify that the `Product` entity should map to a table named `Products`, customize the primary key to be `Id`, and customize the column name for the `Name` property to be `ProductName`.



### 4. Mixed Approach
In a mixed approach, developers combine the use of conventions, data annotations, and Fluent API to configure mappings. This approach provides flexibility by allowing developers to choose the most appropriate method for each mapping scenario. For example, developers may rely on conventions for basic mappings, use data annotations for simple overrides, and utilize Fluent API for more complex mappings or to centralize configuration in one place.

These mapping approaches offer varying levels of control and flexibility in how entity classes are mapped to database tables in Entity Framework, catering to different preferences and requirements of developers.


```csharp
[Table("Products")]
public class Product
{
    [Key]
    public int Id { get; set; }

    [Column("ProductName")]
    public string Name { get; set; }

    public decimal Price { get; set; }
}

```

In this example, we use data annotations to specify the table name and column name, while relying on conventions for the primary key mapping.

These examples demonstrate how you can use different mapping approaches in Entity Framework to customize how entity classes are mapped to database tables.