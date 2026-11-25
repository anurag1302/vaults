- Installed PostgreSQL 19 on macOS via home brew, created few tables with sample data and got a hand of it
- Connect it with a .NET 10 API via EF Core 10
	- Code First Approach to be more fluent in it
	- No repository pattern or any such added abstractions for now
	- Use plain old EF `DbContext` 
		- https://baldbeardedbuilder.com/data/repository-pattern-vs-dbcontext-in-entity-framework-core/
	- ``` csharp
		dotnet add package Microsoft.EntityFrameworkCore --version 10.0.0
		dotnet add package Microsoft.EntityFrameworkCore.Design --version 10.0.0
		dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL --version 10.0.0
		```
- Create Entities to start with, like Customer, Order, OrderItems, Product etc
	- These are POCOs (plain old class objects)
- Create an `AppDbContext` class that inherits from EF's `DbContext` class and holds the `DbSet` of all the entities.
	- Override the `OnModelCreating` method and use `modelBuilder.ApplyConfigurationsFromAssembly` in it.
- Create Configurations for individual entities so that upon migrations, the table gets created in the database.
	- `EF Core` is smart enough to create DB tables directly after looking at the `Entities` once migration is applied and database is updated. This follows standard EF conventions.


> [!info] IMP
> Configurations classes are needed if we want to have more control over the objects or want to customize the model. For example - add max length to the column, change the name of the column etc etc.


- ```csharp
  public class CustomerConfiguration:IEntityTypeConfiguration<Customer>  
	{  
	    public void Configure(EntityTypeBuilder<Customer> builder)  
	    {        
		    builder.ToTable("customers");  
            
            builder.HasKey(c=>c.Id);  
            builder.Property(c => c.Id)  
            .HasColumnName("id")  
            .UseIdentityAlwaysColumn();  
                
            builder.Property(c => c.FirstName)  
            .HasColumnName("first_name")  
            .HasMaxLength(100)  
            .IsRequired();  
                
            builder.Property(c => c.LastName)  
            .HasColumnName("last_name")  
            .HasMaxLength(100)  
            .IsRequired();  
                
            builder.Property(c => c.Email)  
            .HasColumnName("email")  
            .HasMaxLength(255)  
            .IsRequired();  
  
	        builder.Property(c => c.CreatedAt)  
            .HasColumnName("created_at")  
            .HasColumnType("datetime")  
            .HasDefaultValueSql("NOW()");
	    }
	}
  ```

> [!info] EF Migration Commands
	> - `dotnet ef migrations add InitialCreate/OrderTableAdded`
	> - `dotnet ef database update`
> 
> To undo the recent migration, below command can be used
	> - `dotnet ef migrations remove`