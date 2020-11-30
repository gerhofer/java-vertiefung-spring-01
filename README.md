FYI im `HOW_TO.md` sind einige Tipps für Dinge, die in dieser Übung das erste mal vorkommen würden.

# WishList for Santa 

Entwickle eine Webseite auf der man eine Wunschliste fürs Christkind verwalten kann.
Ein Wunsch auf der Wunschliste kann erfüllt oder unerfüllt sein, 
hat eine bestimmte Preiskategorie (CHEAP, OKAY, EXPENSIVE), eine Beschreibung und eine ID. 

Persistieren müssen wir unsere Daten noch nirgends, wir speichern sie, wie beim PetWalker example in einer Klasse
`WishListService` am besten in einer privaten Map von ID auf Wish ab (`Map<Long, Wish>`).

## Grafischer Teil mit Thymeleaf 

Unter `/christmas` sollte in einem grafischen UI die Liste dargestellt werden (Stichwort `@Controller`). 
Unter `/christmas?fulfilled=12` sollte der Wunsch mit der ID 12 als erlegdigt markiert werden und das UI neu angezeigt.

### minimal Anforderungen 
* Aufzählung aller Elemente mit:
    - einer Checkbox die aussagt ob der Wunsch bereits erfüllt wurde
    - Anzeige der Preiskategorie (vielleicht mit 1 bis 3 Dollar Symbolen, $ für cheap, $$ für okay, ...)
    - Anzeige der Bezeichnung
    - Anzeige eines Buttons zum Als erledigt markieren (nur klickbar wenn noch nicht erledigt)

## REST Schnittstelle 

Zusätzlich sollten wir eine REST-Schnittstelle bieten (Stichwort `@RestController`). 

Die Rest Schnittstelle sollte: 
* unter GET `/api/wishes` eine Liste der Wünsche als JSON zurück geben.
* unter POST `/api/wish` einen neuen Wunsch entgegen nehmen und dieses im WishListService speichern.
* unter POST `/api/wish/{id}/fulfill` soll der Status der Wunsches mit ID {id} auf erfüllt gesetzt werden.
    - Gibt es den Wunsch mit dieser ID nicht wird eine `WishNotFoundException` geworfen.
    
### minimale Unit Test Fälle für den RESTController

* Ein GET Aufruf auf `/wishes` gibt eine leere Liste zurück, wenn der WishListService keine Wünsche findet
* Ein POST Aufruf auf `/wishes` liefert eine 405 - Method Not Allowed zurück.
* Ein POST Aufruf auf `/wish` ohne RequestBody liefert einen 4xxx Status zurück. Verifiziere außerdem dass keine Methode `add` auf dem WishListService aufgerufen wurde.
* Ein POST Aufruf auf `/wish` mit gültigem RequestBody liefer einen 2xxx Status (Ok) zurück und ruft die `add` Methode auf dem WishListService auf.
* Ein POST Aufruf auf `/wish/12/fulfill` liefert einen 5xxx Status zurück falls der `WishListService` bei Aufruf der `fulfill` Methode eine Exception wirft. 
* Ein POST Aufruf auf `/wish/12/fulfill` liefert einen 2xxx Status zurück falls der Aufruf der `fulfill` Methode auf dem `WishListService` erfolgreich ist. 

### minimale Unit Test Fälle für den WishListService 

* Wird `getWishes()` aufgerufen und der WishListService ist leer, dann wird eine leere Liste zurückgegeben. 
* Wird `add(Wish wish)` aufgerufen und danach `getWishes()` bekommt man das eben angefügte Elemente in der Liste zurück.
* Wird `fulfill(Long wishId)` aufgerufen und es existiert kein Wunsch mit der übergebenen ID, dann fliegt eine Exception.
* Wird `fulfill(Long wishId)` aufgerufen und der Wunsch existiert, dann wird nach dem `fulfill` Aufruf `getWishes()` aufgerufen und der Status des Wunsches verifiziert.