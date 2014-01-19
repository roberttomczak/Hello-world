# Lista lotnisk na swiecie - Mapreduce z [tych danych]("http://openflights.org/data.html")
___

#### Robert Tomczak

Baza zawiera 7663 wpisow dotyczacych lotnisk na swiecie

Przykladowy wpis :

```json
{
        "_id" : ObjectId("52dbfee89a16965ee24b73b1"),
        "Name" : "Goroka",
        "City" : "Goroka",
        "Country" : "Papua New Guinea",
        "IATA/FAA" : "GKA",
        "ICAO" : "AYGA",
        "Latitude" : -6.081689,
        "Longitude" : 145.391881,
        "Altitude" : 5282,
        "Timezone" : 10,
        "DST" : "U"
}
```

### Map Reduce 1 - ilosc lotnisk w każdym kraju

Funkcja mapujaca :

```js
var map1 = function() {
    emit(this.Country, 1);
};
```

Funkcja redukujaca :

```js
var reduce1 = function(key,val) {
    var count = 0;
    for(i = 0; i < val.length; i++) {
        count += val[i];
    }
    return count;
}
```

Funkcja mapReduce

```js
var result1 = db.airports.mapReduce(
    map1,
    reduce1,
    { out : "kraje"}
);
```

Wynik wykolania:

```json
> db.kraje.find()
{ "_id" : "Afghanistan", "value" : 21 }
{ "_id" : "Albania", "value" : 1 }
{ "_id" : "Algeria", "value" : 44 }
{ "_id" : "American Samoa", "value" : 3 }
{ "_id" : "Angola", "value" : 26 }
{ "_id" : "Anguilla", "value" : 1 }
{ "_id" : "Antarctica", "value" : 18 }
{ "_id" : "Antigua and Barbuda", "value" : 2 }
{ "_id" : "Argentina", "value" : 103 }
{ "_id" : "Armenia", "value" : 4 }
{ "_id" : "Aruba", "value" : 1 }
{ "_id" : "Australia", "value" : 256 }
{ "_id" : "Austria", "value" : 28 }
{ "_id" : "Azerbaijan", "value" : 8 }
{ "_id" : "Bahamas", "value" : 34 }
{ "_id" : "Bahrain", "value" : 2 }
{ "_id" : "Bangladesh", "value" : 13 }
{ "_id" : "Barbados", "value" : 1 }
{ "_id" : "Belarus", "value" : 9 }
{ "_id" : "Belgium", "value" : 32 }
...
```