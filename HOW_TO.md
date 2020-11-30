# Mockito 

## How to - Mock Service Calls

Den Rückgabewert einer Methode kann man mit Mockito einfach definieren.
Die Syntax ist zuerst das Schlüsselwort `given`, in Klammern kommt dann der 
zu überschreibende Aufruf, dann der Aufruf der `.willReturn()` Methode
mit dem was zurückgegeben werden soll als Parameter.

ein Beispiel:

```java
import static org.mockito.BDDMockito.given;

@MockBean
private WishListService wishListService; 

@Test
public void testWithMockedResult() {
    given(wishListService.getWishes())
        .willReturn(List.of(new Wish("Yogamatte", CHEAP, false)));
}
```

Genauso einfach, wie der Rückgabewert gemockt werden kann, können wir den Service auch 
mit einer Exception antworten lassen: 

```java
import static org.mockito.BDDMockito.given;

@MockBean
private WishListService wishListService; 

@Test
public void testWithMockedResult() {
    given(petService.getPets())
        .willThrow(new RuntimeException());
}
```

# Thymeleaf 

Die [offizielle Thymeleaf Dokumentation](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html) ist ein sehr guter Punkt um Funktionalität nachzuschlagen. 
Die wichtigsten Dinge, die ihr für diese Übung brauchen könntet hab ich euch trotzdem hier zusammengefasst

## How to - Bedingte Anzeige von Elementen - th:if 

Mit `th:if` können Elemente nur dann angezeigt werden, wenn eine bestimmte Bedingung zutrifft.
Es gibt alle logischen Operatoren die wir aus Java kennen - ==, !=, <, >, <=, >=.

```
<span th:if="${wish.priceCategory == 'CHEAP'}">$</span>
<span th:if="${wish.priceCategory == 'OKAY'}">$$</span>
```

Verwendung mit einem Enum, wenn priceCategory im wish ein Enum ist:
```
<span th:if="${wish.priceCategory == T(at.codersbay.wishlist.PriceCategory.CHEAP)}>$</span>
<span th:if="${wish.priceCategory == T(at.codersbay.wishlist.PriceCategory.OKAY)}>$$</span>
```

## How to - Checkbox befüllen

Mittels `th:checked` kann dynamisch definiert werden, ob die Checkbox angehakt sein soll oder nicht. 

```
<input type="checkbox" th:checked="${wish.fulfilled}">
```

## How to - Form Submit Button

Die URL einer Form kann man in Thymeleaf dynamisch mit dem `@` prefix definieren.
Verwendet man diese kann man Parameter praktisch in den runden Klammern in Form von Schlüssel-Wert Paaren mitgeben. 
Mit `th:disabled` kann man definieren ob ein Element disabled angezeigt wird oder nicht. 

```
<form th:action="@{/christmas(fulfilled=${wish.id})}" method="post">
  <input type="submit" value="Mark as Done" th:disabled="${wish.fulfilled}"/>
</form>
```
