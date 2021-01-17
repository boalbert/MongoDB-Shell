# MongoDB Shell Manipulera data

## Utveckling mot databas och databasadministration



Denna repository innehåller mina lösningar för Lab 3 i kursen "Utveckling mot databas och databasadministration".

1. Använd en databas med namn myDB.

```java
use myDB
switched to db myDB
```

  

2. Skapa ett dokument med innehåll FirstName: ”Selma”, LastName: Lagerlöf och sätt in det i en kollektion med namn ”authors”.

```java
db.authors.insertOne(
	{
	FirstName: "Selma", 
	LastName: "Lagerlöf"
})

{
        "acknowledged" : true,
        "insertedId" : ObjectId("5ffe248d2ca128348840f583")
}
```



3. Lägg till ytterligare ett dokument i ”authors” med FirstName: ”August”, LastName: ”Bondeson”, Birth: 1854

```java
db.authors.insertOne(
{
	FirstName: "August", 
    LastName: "Bondesson",
    Birth: 1854
})

{
        "acknowledged" : true,
        "insertedId" : ObjectId("5ffe24972ca128348840f584")
}
```



4. Uppdatera dokumentet för August Bondeson och lägg till Death: 1906

```java
db.authors.updateOne(
          {FirstName: "August", LastName: "Bondesson"},
            {$set: 
               {
                     Death: 1906
               }
})
    
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
```



5. Lägg till ytterligare författare i ”authors” genom att köra kommandot load(”addAuthors.js”) från mongo Shell. 

```java
load("addAuthors.js")

true
```



6. Räkna hur många dokument det finns totalt i ”authors”.

```java
db.authors.count()

6
```



7. Räkna hur många författare som heter August i förnamn.

```java
db.authors.find({FirstName: "August" }).count()

2
```



8. Lägg till Birth: 1858 och Death: 1940 för Selma Lagerlöf

```java
db.authors.update(
          { FirstName: "Selma", LastName: "Lagerlöf"  },
          { $set:
             {
               Birth: 1858,
               Death: 1940
             }
})

WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```



9. Lägg till en array ”Books” för Selma Lagerlöf med följande böcker: ”Gösta Berlings saga”, ”En herrgårdsägen”, ”Nils Holgerssons underbara resa genom Sverige”

```java
db.authors.update(
    { FirstName: "Selma", LastName: "Lagerlöf" },
      { $set:
         { Books: ["Gösta Berlings sage", "En herrgårdssägen", "Nils 						Holerssons underbara resa genom Sverige"] }
      }
   )

WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```



10. Lägg till boken ”Vi på Saltkråkan” bland Astrid Lindgrens böcker.

```java
db.authors.update(
    { FirstName: "Astrid", LastName: "Lindgren" },
    { $push: { Books: "Vi på Saltkråkan" } 
})

WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```



11. Ta bort boken ”Bröderna Lejonhjärta” från Astrid Lindgrens böcker.

```java
db.authors.update(
    { FirstName: "Astrid", LastName: "Lindgren" },
    { $pull: { Books: "Bröderna Lejonhjärta" } }
)

WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```



12. Visa dokument för de författare som dog år 2000 eller senare.

```java
db.authors.find( { Death: { $gte: 2000 } } ).pretty()

{
    "_id" : ObjectId("5ffe265d2ca128348840f585"),
    "FirstName" : "Astrid",
    "LastName" : "Lindgren",
    "Birth" : 1907,
    "Death" : 2002,
    "Books" : [
            "Här kommer Pippi Långstrump",
            "Mio min Mio",
            "Vi på Saltkråkan"
    ]
}
```



13. Räkna hur många författare som dog på 1940-talet.

```java
db.authors.find({ Death : { $gte :  1940, $lte : 1949}}).count()

3
```



14. Visa endast attributen FirstName, LastName och Death för de författare som dog på 1940-talet.

```java
db.authors.find(
    { Death : { $gte :  1940, $lte : 1949} },
    { FirstName: 1, LastName: 1, Death: 1, _id: 0 }
 ).pretty()

 { "FirstName" : "Selma", "LastName" : "Lagerlöf", "Death" : 1940 }
 { "FirstName" : "Hjalmar", "LastName" : "Söderberg", "Death" : 1941 }
 { "FirstName" : "Karin", "LastName" : "Boye", "Death" : 1941 }
```



15. Lägg till Gender: ”Male” i dokument med FirstName = ”Hjalmar”.

```java
db.authors.update(
    { FirstName: "Hjalmar" },
    { $set:
       { Gender: "Male" }
    }
)

WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```



16. Lägg till Gender: ”Female” i dokument där FirstName är Astrid, Karin eller Selma.

```java
db.authors.updateMany( { 
    FirstName: { $in: [ "Karin", "Selma", "Astrid" ] }},
       { $set: { Gender: "Female" } }
)

{ "acknowledged" : true, "matchedCount" : 3, "modifiedCount" : 3 }
```



17. Ta bort arrayen Books från dokumentet med August Strindberg.

```java
db.authors.update(
    { FirstName: "August", LastName: "Strindberg"  },
        { $unset: { Books: "" }
    }
)

WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```



18. Ta bort dokumenten där FirstName = ”August”.

```java
db.authors.deleteMany( { "FirstName" : "August" } );

{ "acknowledged" : true, "deletedCount" : 2 }
```

