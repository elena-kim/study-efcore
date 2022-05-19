## Docs

- [Ignoring a class property in Entity Framework 4.1 Code First](https://stackoverflow.com/questions/10385248/ignoring-a-class-property-in-entity-framework-4-1-code-first)
  #### `NotMapped` Attribute
  ```csharp
  public class Customer
  {
      public int CustomerID { set; get; }
      public string FirstName { set; get; } 
      public string LastName { set; get; } 
      [NotMapped]
      public int Age { set; get; }
  }
  ```
  
  #### `OnModelCreating` function
  ```csharp
  public class SchoolContext : DbContext
  {
      public SchoolContext(DbContextOptions<SchoolContext> options) : base(options)
      {
      }
      
      protected override void OnModelCreating(ModelBuilder modelBuilder)
      {
          modelBuilder.Entity<Customer>().Ignore(t => t.FullName);
          base.OnModelCreating(modelBuilder);
      }
      
      public DbSet<Customer> Customers { get; set; }
  }
  ```

<br>

- [Entity Framework Extensions](https://entityframework-classic.net/update-from-query)
  #### `UpdateFromQuery`
  ```csharp
  // UPDATE all customers that are inactive for more than two years
  var date = DateTime.Now.AddYears(-2);
  context.Customers
      .Where(x => x.IsActive && x.LastLogin < date)
      .UpdateFromQuery(x => new Customer {IsActive = false});

  // UPDATE customers by id
  context.Customers.Where(x => x.ID == userId).UpdateFromQuery(x => new Customer {IsActive = false});
  ```

  #### `DeleteFromQuery`
  ```csharp
  // DELETE all customers that are inactive
  context.Customers.Where(x => !x.IsActive).DeleteFromQuery();

  // DELETE customers by id
  context.Customers.Where(x => x.ID == userId).DeleteFromQuery();
  ```

  #### `Performance Comparisons`
  |Operations|1,000 Entities|2,000 Entities|5,000 Entities|
  |:------|:--------:|:----------:|:---------:|
  |SaveChanges|1,000 ms	|2,000 ms	|5,000 ms	|
  |UpdateFromQuery|1 ms	|1 ms	|1 ms	|
  |DeleteFromQuery|1 ms	|1 ms	|1 ms	|

<br>

- [EF-Core ORM](https://medium.com/uniquegood/%EB%A6%AC%EC%96%BC%EC%9B%94%EB%93%9C%EB%A5%BC-%EC%A7%80%ED%83%B1%ED%95%98%EB%8A%94-%EA%B8%B0%EC%88%A0-1-entity-framework-core-orm-%EC%86%8C%EA%B0%9C%ED%8E%B8-9af53bbbe9b1)

