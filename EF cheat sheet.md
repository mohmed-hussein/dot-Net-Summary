# Entity Framework Core Cheat Sheet

## Data Annotations

### [Key]
Marks the primary key of the entity.
```csharp
public class Student
{
    [Key]
    public int StudentId { get; set; }
}
```

### [Required]
Indicates that the property is mandatory.
```csharp
public class Student
{
    [Required]
    public string Name { get; set; }
}
```

### [MaxLength], [MinLength]
Sets the maximum or minimum length of a string property.
```csharp
public class Student
{
    [MaxLength(50)]
    public string Name { get; set; }
}
```

### [StringLength]
Sets both maximum and minimum length of a string property.
```csharp
public class Student
{
    [StringLength(50, MinimumLength = 2)]
    public string Name { get; set; }
}
```

### [Column]
Specifies the column name in the database.
```csharp
public class Student
{
    [Column("Student_Name")]
    public string Name { get; set; }
}
```

### [Table]
Specifies the table name in the database.
```csharp
[Table("StudentInfo")]
public class Student
{
    public int StudentId { get; set; }
    public string Name { get; set; }
}
```

### [ForeignKey]
Defines a foreign key relationship.
```csharp
public class Enrollment
{
    [ForeignKey("Student")]
    public int StudentId { get; set; }
    public Student Student { get; set; }
}
```

### [InverseProperty]
Specifies the inverse navigation property.
```csharp
public class Student
{
    public int StudentId { get; set; }
    [InverseProperty("EnrolledStudents")]
    public ICollection<Enrollment> Enrollments { get; set; }
}
```

## Fluent API

### Configuring a Primary Key
Defines the primary key for an entity.
```csharp
modelBuilder.Entity<Student>()
    .HasKey(s => s.StudentId);
```

### Configuring Properties
Sets property configurations like required, max length, etc.
```csharp
modelBuilder.Entity<Student>()
    .Property(s => s.Name)
    .IsRequired()
    .HasMaxLength(50);
```

### Configuring Table Mapping
Maps the entity to a specific table.
```csharp
modelBuilder.Entity<Student>()
    .ToTable("StudentInfo");
```

### Configuring Column Mapping
Maps a property to a specific column.
```csharp
modelBuilder.Entity<Student>()
    .Property(s => s.Name)
    .HasColumnName("Student_Name");
```

### Configuring Relationships

#### One-to-Many Relationship
Defines a one-to-many relationship.
```csharp
modelBuilder.Entity<Student>()
    .HasMany(s => s.Enrollments)
    .WithOne(e => e.Student)
    .HasForeignKey(e => e.StudentId);
```

#### Many-to-Many Relationship
Defines a many-to-many relationship.
```csharp
modelBuilder.Entity<StudentCourse>()
    .HasKey(sc => new { sc.StudentId, sc.CourseId });

modelBuilder.Entity<StudentCourse>()
    .HasOne(sc => sc.Student)
    .WithMany(s => s.StudentCourses)
    .HasForeignKey(sc => sc.StudentId);

modelBuilder.Entity<StudentCourse>()
    .HasOne(sc => sc.Course)
    .WithMany(c => c.StudentCourses)
    .HasForeignKey(sc => sc.CourseId);
```

#### One-to-One Relationship
Defines a one-to-one relationship.
```csharp
modelBuilder.Entity<Student>()
    .HasOne(s => s.Address)
    .WithOne(a => a.Student)
    .HasForeignKey<Address>(a => a.StudentId);
```

### Configuring Indexes
Creates an index on a property.
```csharp
modelBuilder.Entity<Student>()
    .HasIndex(s => s.Name)
    .IsUnique();
```

## Example DbContext Configuration
Combines various configurations in the `OnModelCreating` method.
```csharp
public class SchoolContext : DbContext
{
    public DbSet<Student> Students { get; set; }
    public DbSet<Enrollment> Enrollments { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("YourConnectionStringHere");
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        // Fluent API configurations
        modelBuilder.Entity<Student>()
            .HasKey(s => s.StudentId);

        modelBuilder.Entity<Student>()
            .Property(s => s.Name)
            .IsRequired()
            .HasMaxLength(50);

        modelBuilder.Entity<Student>()
            .ToTable("StudentInfo");

        modelBuilder.Entity<Student>()
            .Property(s => s.Name)
            .HasColumnName("Student_Name");

        modelBuilder.Entity<Student>()
            .HasMany(s => s.Enrollments)
            .WithOne(e => e.Student)
            .HasForeignKey(e => e.StudentId);

        modelBuilder.Entity<Student>()
            .HasIndex(s => s.Name)
            .IsUnique();
    }
}
```

## Tips
- **Data Annotations**: Use these for simple configurations directly in your classes.
- **Fluent API**: Use this for more complex configurations in the `DbContext`.
- You can mix both methods, but Fluent API configurations override Data Annotations.

Feel free to refer to this cheat sheet whenever you need a quick reminder of how to configure Entity Framework Core!
