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