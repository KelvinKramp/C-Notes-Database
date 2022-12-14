# Csharp notes about databases

## DTO (Data Transfer Objects)
Commong addressed advantages: 
- Hiding data layer complexity.
- Easy for UI/Unit testing by mocking DTOs and injecting as dependency.
- No more direct dealing with faulty data or errors related to network or database.
- Security: Your API should never return or receive domain objects. Receiving domain objects creates security treats.

(Source https://medium.com/@ParthContractor/dto-data-transfer-objects-3848a45a6d6)

#### Martin Fowler (writer of Patterns of Enterprise Application Architecture)
"DTOs are called Data Transfer Objects because their whole purpose is to shift data in expensive remote calls. They are part of implementing a coarse grained interface which a remote interface needs for performance. "

"One case where it is useful to use something like a DTO is when you have a significant mismatch between the model in your presentation layer and the underlying domain model. In this case it makes sense to make presentation specific facade/gateway that maps from the domain model and presents an interface that's convenient for the presentation. It fits in nicely with Presentation Model. I hope to talk about this more in the new volume. This is worth doing, but it's only worth doing for screens that have this mismatch (in this case it isn't extra work, since you'd have to do it in the screen anyway.)"

(Source https://martinfowler.com/bliki/LocalDTO.html)

## EF commands
dotnet restore 
Restores the dependencies and tools of a project.

## Persistence framewqork
A persistence framework is middleware that assists in the storage and retrieval of information between applications and databases, especially relational databases. It acts as a layer of abstraction for persisted data, bridging conceptual and technical differences between storage and utilisation.
(wikipedia)

## Repository
-	Repository pattern mediates between the data and domain layer. It acts as an in-memory collection of domain objects
-	Repository pattern minimizes duplicate query logic.
-	Operations: Add, remove, get, getall, find. 
-	Decouples your application from persistence frameworks. E.g. without repository pattern application -> EF. If new EF is incorporated, you will also need to change things in the application. With repository pattern: application -> repository -> EF. The application code is not affected by database changes! and therefore architecture is independent from frameworks.
-	Use patterns only when you need to. Don???t use them as deodorant to make smelly code look good.	
<img width="444" alt="image" src="https://user-images.githubusercontent.com/76985447/193211766-0875f100-630c-40ba-949f-3cd99e302dce.png">
(Source Repository Pattern with C# and Entity Framework, Done Right | Mosh https://www.youtube.com/watch?v=rtXpYpZdOzM&t=12s) 

## Unit of work pattern: 
The unit of work class coordinates the work of multiple repositories by creating a single database context class shared by all of them.
(Source https://learn.microsoft.com/en-us/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)


#### Unit of work in practie 
In short, the unit of work pattern is used in practice to mantains a list of businesstransactions and coordinates the writing out of changes. Unit of work contains the class DbContext with classes and methods such as DbSets, SaveChanges, etc. These classes and methods are directly related to the database; they perform work by executing database operations dependent on the underlying database used. 

One user has one DbContext per session. The database work is collected in a unit of work. When SaveChanges or similar method is called all tracked changes during that session of this user are implemented in the database:

 <img width="331" alt="image" src="https://user-images.githubusercontent.com/76985447/193211738-5c20e9a1-075a-4cd2-bb70-22c1fc1f7be9.png">
(Source Repository Pattern with C# and Entity Framework, Done Right | Mosh https://www.youtube.com/watch?v=rtXpYpZdOzM&t=12s)

## DbContext
...

## DbSet
Each DbSet has a collection like interface (add, remove, find, where).

# Generic implementations in repository pattern
IBusinessObjectX inherits from IRepostiory. Throught the implementation of generics <T> in the IRepository interface code repetition is prevented. As a consequence the IBusinessObjectXRepository interfaces are mostly empty because they inherit the CRUD operations from IRepository<T>.  

Example in code:
### IBusinessObjectX.cs
 ```
namespace MoneyMakingBusiness.Domain.AggregatesModel
{
    public interface IBusinessObjectXRepository: IRepository<BusinessObjectX>
    {

    }
}
 ```
 
### IRepository.cs
 ```
namespace Shared.Infrastructure.Database
{
    public interface IRepository<T> 
    {
        IUnitOfWork UnitOfWork { get; }
        T Add(T entity);
        T Update(T entity);
        T Get(Guid entityId);
        void Remove(T entity);
    }
 }

```
 
 Example in diagram:  
 <img width="386" alt="image" src="https://user-images.githubusercontent.com/76985447/193221914-a923a0c6-8465-4481-a67c-f1a7992e57dd.png">

Generic implementations reduces code but still requires to create implementation per business object. 
 <img width="372" alt="image" src="https://user-images.githubusercontent.com/76985447/193221941-7fd5dbc0-f3e3-4184-8886-ba9617ed9c78.png">

Solution is to create a baseEntitiy called Entity where every business object inherits from. 
 
Example in code (in this example guid ids are created by the baseentity):  
 ### BusinessObjectX.cs
 ```
namespace MoneyMakingBusiness.Domain.AggregatesModel
{
    public interface BusinessObjectXRepository: Entity
    {
         public BusinessObjectXRepository(string category, int priority)
        {
            Category = category;
            Priority = priority;
        }
        public string Category { get; set; }

        public int Priority { get; set; }
    }
}
 ```
 
 ### Entity.cs
 ```
 namespace Shared.Infrastructure.Database
{
{
    public abstract class Entity
    {
        public Guid Id { get; set; }

        public bool IsTransient()
        {
            return this.Id == default(Guid);
        }
    }
}
 ```
 
Example in diagram:  
<img width="378" alt="image" src="https://user-images.githubusercontent.com/76985447/193221968-ac38c634-5906-4961-8906-1597ab37679c.png">
(Source https://www.youtube.com/watch?v=x6C20zhZHw8)
 



