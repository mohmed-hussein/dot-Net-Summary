### Migration in Entity Framework

Migration in Entity Framework refers to the process of applying changes to the database schema to reflect changes made in the application's data model or domain model.

#### How It Works:

1. **Change Detection**: Entity Framework detects changes made in the application's data model.

2. **Creating Migrations**: It generates a migration script based on the detected changes. This script contains instructions on how to update the database schema.

3. **Applying Migrations**: The migration script is executed against the target database, making necessary modifications to bring the schema in sync with the data model.

4. **Updating Metadata**: Entity Framework updates its internal metadata to keep track of applied migrations, ensuring correct order and preventing redundant or conflicting changes.

5. **Database Schema Changes**: The database schema is updated accordingly, including creating new tables, modifying existing ones, adding or removing columns, etc.

#### Example Code:

```csharp
// Define a model class
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

// Define a DbContext class
public class MyDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("your_connection_string");
    }
}

// Enable Migrations (in Package Manager Console)
// Enable-Migrations -ContextTypeName MyDbContext

// Add Initial Migration (in Package Manager Console)
// Add-Migration InitialCreate

// Apply Migration (in Package Manager Console)
// Update-Database
```
## Entity Framework Migration Commands

### 1. Enable Migrations
Enabling migrations is the first step to manage database changes in Entity Framework. It initializes the migrations feature for your project.

```powershell
Enable-Migrations -ContextTypeName MyDbContext
```
Replace `MyDbContext` with the name of your `DbContext` class.

### 2. Add Migration
Adding a migration creates a snapshot of the current state of your data model. It generates a migration file containing the code necessary to update the database schema to match the current state of your model.

```powershell
Add-Migration InitialCreate
```
Replace `InitialCreate` with a descriptive name for your migration.


### 3. Update Database
Updating the database applies any pending migrations to the target database, making the necessary changes to the schema.

```powershell
Update-Database
```


### 4. Rollback Migration
Rolling back a migration removes the last applied migration from the database, reverting the database schema to its previous state.

```powershell
Update-Database -TargetMigration LastAppliedMigrationName
```
Replace `LastAppliedMigrationName` with the name of the migration you want to roll back.


### 5. Revert All Migrations
Reverting all migrations removes all applied migrations and rolls back the database to its initial state.
```powersheel
Update-Database -TargetMigration $InitialDatabase
```
This command is typically used to start fresh or when you need to reset your database to its original state.