# C-Notes-Database

## DTO (Data layer objects)
Commong addressed advantages: 
- Hiding data layer complexity.
- Easy for UI/Unit testing by mocking DTOs and injecting as dependency.
- No more direct dealing with faulty data or errors related to network or database.
- Security: Your API should never return or receive domain objects. Receiving domain objects creates security treats. ![image](https://user-images.githubusercontent.com/76985447/193202963-f6e4bf55-957f-45a1-8b47-75be23820929.png)

(Source https://medium.com/@ParthContractor/dto-data-transfer-objects-3848a45a6d6)

## EF commands
dotnet restore 
Restores the dependencies and tools of a project.

## Repository
-	Repository pattern mediates between the data and domain layer. It acts as an in-memory collection of domain objects
-	Repository pattern minimizes duplicate query logic.
-	Operations: Add, remove, get, getall, find. 
-	Decouples your application from persistence frameworks. E.g. without repository pattern application -> EF. If new EF is incorporated, you will also need to change things in the application. With repository pattern: application -> repository -> EF. The application code is not affected by database changes! and therefore architecture is independent from frameworks.
-	Use patterns only when you need to. Don’t use them as deodorant to make smelly code look good.	
<img width="444" alt="image" src="https://user-images.githubusercontent.com/76985447/193211766-0875f100-630c-40ba-949f-3cd99e302dce.png">
(Source Repository Pattern with C# and Entity Framework, Done Right | Mosh https://www.youtube.com/watch?v=rtXpYpZdOzM&t=12s) 

## Unit of work pattern: 
The unit of work class coordinates the work of multiple repositories by creating a single database context class shared by all of them.
(Source https://learn.microsoft.com/en-us/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)

### Martin Fowler (writer of Patterns of Enterprise Application Architecture)
"DTOs are called Data Transfer Objects because their whole purpose is to shift data in expensive remote calls. They are part of implementing a coarse grained interface which a remote interface needs for performance. "
"One case where it is useful to use something like a DTO is when you have a significant mismatch between the model in your presentation layer and the underlying domain model. In this case it makes sense to make presentation specific facade/gateway that maps from the domain model and presents an interface that's convenient for the presentation. It fits in nicely with Presentation Model. I hope to talk about this more in the new volume. This is worth doing, but it's only worth doing for screens that have this mismatch (in this case it isn't extra work, since you'd have to do it in the screen anyway.)"
(Source https://martinfowler.com/bliki/LocalDTO.html)

### Unit of work pattern is used in practice to:
-	Used to save objets within the collection to the database. 
-	Mantains a list of businesstransactions and coordinates the writing out of changes.
-	DbContext contains DbSets, SaveChanges, etc and acts as the unit of work. It keeps track of Db objects and coordinates the writing out of changes.
 <img width="331" alt="image" src="https://user-images.githubusercontent.com/76985447/193211738-5c20e9a1-075a-4cd2-bb70-22c1fc1f7be9.png">
(Source Repository Pattern with C# and Entity Framework, Done Right | Mosh https://www.youtube.com/watch?v=rtXpYpZdOzM&t=12s)

## DbSet
Each DbSet has a collection like interface (add, remove, find, where).
![image](https://user-images.githubusercontent.com/76985447/193212722-8c463892-abb1-4f03-87a1-2e75304c41a5.png)

## Persistence framewqork
A persistence framework is middleware that assists in the storage and retrieval of information between applications and databases, especially relational databases. It acts as a layer of abstraction for persisted data, bridging conceptual and technical differences between storage and utilisation.
(wikipedia)
![image](https://user-images.githubusercontent.com/76985447/193212823-5525d20e-692c-4a61-80dc-03a36305194d.png)

