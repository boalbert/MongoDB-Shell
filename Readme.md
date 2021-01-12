# Lab 3 - Manipulera data i Mongo Shell

### Kurs: Utveckling mot databas och databasadministration

### Övningsuppgift i Mongo Shell VT2020.

------



AlbertAndersson.txt innehåller mina lösningar.



**Exempel**: Uppgift 14. Visa endast attributen FirstName, LastName och Death för de författare som dog på 1940-talet.

```javascript
db.authors.find(
    { Death : { $gte :  1940, $lte : 1949} },
    { FirstName: 1, LastName: 1, Death: 1, _id: 0 }
 ).pretty()

 >> { "FirstName" : "Selma", "LastName" : "Lagerlöf", "Death" : 1940 }
 { "FirstName" : "Hjalmar", "LastName" : "Söderberg", "Death" : 1941 }
 { "FirstName" : "Karin", "LastName" : "Boye", "Death" : 1941 }
```

