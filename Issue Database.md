# Issue Database

Serial Number | Created On | Created By | Issue | ClosedOn | Closed By | Notes
------------- | ---------- | ---------- | ----- | -------- | --------- | -----
sghrg3234kn123 | 20201112140000 | Ben | Skewed Parts | 20201113124500 | Ben | Fixed

I do like compact items, but seeing as this database will contain very predictable elements there was no need for arrays. However now that I'm thinking about it I might turn issues into an array and use that to capture just part information, and notes can contain general non part specific information.


Serial Number | Created On | Created By | Issue Array | ClosedOn | Closed By | Notes Array
------------- | ---------- | ---------- | ----- | -------- | --------- | -----
sghrg3234kn123 | 20201112140000 | Ben | {...} | 20201113124500 | Ben | {...}

Issue Array
```json
{
    "R45":["Skewed",1],
    "C13":["Missing",3]
}
```

Notes Array
```json
{
    "Ben":"Super long string here as I always pack in as much information as technically allowed",
    "Randy":"Board just worked"
}
```

Currently have this fleshed out in MariaDB like the second method.
