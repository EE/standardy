# Instrukcja wyszukiwania i wyświetlania działek katastralnych na mapach Open Street Maps

W tej instrukcji znajdziesz opisane sposoby, dzięki którym możesz pobrać dane działek katastralnych (działka katastralna to najmniejsza jednostka powierzchniowa podziału kraju dla celów ewidencji gruntów i budynków) na terenie Polski i wyświetlić je (na przykład) na mapie [Open Street Map](https://www.openstreetmap.org/#map=6/52.018/19.137) przy użyciu biblioteki [Leaflet](https://leafletjs.com/).

Przykładową implementację znajdziesz w projekcie GUNB.

Głównym założeniem tego dokumentu jest zebranie w jednym miejscu informacji o API użytych do stworzenia tych funkcjonalności.

O ile korzystanie z niżej wymienionych API nie jest zbyt problematyczne (chociaż ich dokumentacja na pierwszy rzut oka wydaje się być mało przyjazna),
tak samo zebranie informacji o ich istnieniu i funkcjach okazało się być pewnym wyzwaniem dla osób nie zajmujących się tą tematyką na co dzień.  
Powodem użycia dwóch API (UUG i ULDK) w celu otrzymania wyczerpujących informacji o działce jest to, że każde z nich osobno nie dostarcza wszystkich informacji o wyszukiwanej działce.

W projekcie zostały uwzlgędnione trzy ścieżki użytkownika:

1. Użytkownik zna adres (miejscowość, ulica, numer budynku) danej działki i chce uzyskać o niej dokładne informacje (identyfikator, obręb ewidencyjny oraz numer działki ewidencyjnej)
2. Użytkownik zna dokładny identyfikator działki lub numer obrębu i numer działki i chce uzyskać resztę informacji na jej temat (miejscowość, ulica, numer budynku)
3. Użytkownik może znaleźć działkę poprzez kliknięcie na mapie.  
   Kolejnym, wspólnym krokiem, jest wyświetlenie ich na mapie.

Realizacja tych ścieżek jest możliwa dzięki użyciu API trzech serwisów obsługiwanych przez [GUGiK](http://www.gugik.gov.pl/).
Są to:

- [Usługa Lokalizacji Działek Katastralnych](https://uldk.gugik.gov.pl/) (dalej ULDK) ([dokumentacja](https://uldk.gugik.gov.pl/opis.html))
- [Uniwersalna Usługa Geokodowania](https://services.gugik.gov.pl/uug) (dalej UUG) ([dokumentacja](https://services.gugik.gov.pl/uug/opis.html))
- [Krajowa Integracja Ewidencji Gruntów](https://integracja.gugik.gov.pl/cgi-bin/KrajowaIntegracjaEwidencjiGruntow) (dalej KIEG) ([dokumentacja](https://integracja.gugik.gov.pl/cgi-bin/KrajowaIntegracjaEwidencjiGruntow))

Poniżej znajdziesz opis dwóch wcześniej wspomnianych ścieżek z dokładnym uwzględnieniem użytych API.

## Ścieżka nr. 1

Chcąc zlokalizować działkę po adresie (miejscowość, ulica, numer budynku) w pierwszym kroku używamy serwisu UUG,
który na podstawie danych adresowych zwraca nam listę dopasowanych działek według dostarczonych przez nas informacji.  
Przykładowe zapytanie:

```
https://services.gugik.gov.pl/uug/?request=GetAddress&location=Warszawa%2C%20Stalowa%201
```

Przykładowa odpowiedź:

```json
{
  "type": "address",
  "max results limit": 10,
  "min accuracy limit": 0.6,
  "only exact numbers": 1,
  "found objects": 10,
  "returned objects": 1,
  "results": {
    "1": {
      "city": "Warszawa",
      "citypart": null,
      "street": "ulica Stalowa",
      "number": "1",
      "teryt": "146501",
      "simc": "0918123",
      "ulic": "20808",
      "code": "03-425",
      "jednostka": "{Polska,mazowieckie,Warszawa,Warszawa}",
      "x": "638901.1258",
      "y": "489996.4866",
      "geometry_wkt": "POINT(638901.1258 489996.4866)",
      "accuracy": "0.75",
      "city_accuracy": "1",
      "street_accuracy": "0.571429",
      "id": 1
    }
  },
  "request time": 0.070067417621613
}
```

Zazwyczaj jest to jedna działka, jednak możliwa jest odpowiedź serwera zwracająca listę działek.
Ze zwróconych z UUG informacji o adresie do użycia API ULDK potrzebujemy tylko współrzędnych X i Y znalezionego obiektu.
W zapytaniu do ULDK, oprócz wspórzędnych X i Y, podajemy listę parametrów, które określają strukturę informacji, jakie chcemy uzyskać w odpowiedzi (czyli np `result=id,numer,powiat,gmina,geom_wkt,`).  
Przykładowe zapytanie, na podstawie poprzedniego zapytania:

```
https://uldk.gugik.gov.pl/?request=GetParcelByXY&xy=638901.1258,489996.4866&result=id,numer,powiat,gmina,geom_wkt,voivodeship,teryt,region,parcel,jednostka_ewidencyjna&srid=4326
```

Odpowiedź:

```
0
146508_8.1304.60|60|Warszawa|Warszawa|SRID=4326;POLYGON((21.0354452630117 52.2584525918487,21.0355334926599 52.2585261742515,21.0355661733376 52.2585503412127,21.0356267871804 52.2585228207044,21.0356319082129 52.258516348045,21.0356225299489 52.2585105987735,21.0356503477517 52.2584978279027,21.0357953834715 52.2585438008329,21.0358040409553 52.2585638406859,21.0358371368578 52.2585578089143,21.035890755559 52.2585739704128,21.0358925097842 52.2585698355567,21.0359706464214 52.2584772388975,21.0355790504327 52.2583526987239,21.0354452630117 52.2584525918487))|Mazowieckie|146508_8.1304.60|4-13-04|60|Praga-Północ
```

W odpowiedzi z ULDK otrzymujemy resztę interesujących nas informacji, takie jak powiat, gmina, województwo, identyfikator działki, obręb ewidencyjny, jednostka ewidecyjna oraz geometrię obiektu, dzięki której możemy wyrenderować odpowiedni kształ na mapie.

## Ścieżka nr. 2

Chcąc zlokalizować działkę po znanym identyfikatorze lub numerze obrębu i numerze działki, używamy na początku serwisu ULDK, któremu przekazujemy tylko identyfikator działki (w tym wypadku to `146508_8.1304.60`),
a w odpowiedzi dostajemy powiat, gmina, województwo, identyfikator działki, obręb ewidencyjny, jednostka ewidecyjna oraz geometrię obiektu, dzięki której możemy wyrenderować odpowiedni kształ na mapie.  
Przykładowe zapytanie:

```
https://uldk.gugik.gov.pl/?request=GetParcelByIdOrNr&id=146508_8.1304.60&result=id,numer,powiat,gmina,geom_wkt,voivodeship,teryt,region,parcel,jednostka_ewidencyjna&srid=4326
```

Przykładowa odpowiedź:

```
1
146508_8.1304.60|60|Warszawa|Warszawa|SRID=4326;POLYGON((21.0354452630117 52.2584525918487,21.0355334926599 52.2585261742515,21.0355661733376 52.2585503412127,21.0356267871804 52.2585228207044,21.0356319082129 52.258516348045,21.0356225299489 52.2585105987735,21.0356503477517 52.2584978279027,21.0357953834715 52.2585438008329,21.0358040409553 52.2585638406859,21.0358371368578 52.2585578089143,21.035890755559 52.2585739704128,21.0358925097842 52.2585698355567,21.0359706464214 52.2584772388975,21.0355790504327 52.2583526987239,21.0354452630117 52.2584525918487))|Mazowieckie|146508_8.1304.60|4-13-04|60|Praga-Północ
```

Następnie kierujemy zapytanie do UUG używając tylko geometrii otrzymanej z poprzedniej odpowiedzi (używając parametru `request=GetAddressReverse`).  
Przykładowe zapytanie:

```
https://services.gugik.gov.pl/uug/?request=GetAddressReverse&location=POLYGON((21.0354452630117%2052.2584525918487,21.0355334926599%2052.2585261742515,21.0355661733376%2052.2585503412127,21.0356267871804%2052.2585228207044,21.0356319082129%2052.258516348045,21.0356225299489%2052.2585105987735,21.0356503477517%2052.2584978279027,21.0357953834715%2052.2585438008329,21.0358040409553%2052.2585638406859,21.0358371368578%2052.2585578089143,21.035890755559%2052.2585739704128,21.0358925097842%2052.2585698355567,21.0359706464214%2052.2584772388975,21.0355790504327%2052.2583526987239,21.0354452630117%2052.2584525918487))&srid=4326
```

Przykładowa odpowiedź:

```json
{
  "type": "address",
  "max results limit": 100,
  "radius": null,
  "max polygon area": 1000000,
  "returned objects": 1,
  "results": {
    "1": {
      "city": "Warszawa",
      "citypart": null,
      "street": "ulica Stalowa",
      "number": "1",
      "teryt": "146501",
      "simc": "0918123",
      "ulic": "20808",
      "code": "03-425",
      "jednostka": "{Polska,mazowieckie,Warszawa,Warszawa}",
      "x": "21.0357645511821",
      "y": "52.2584321119428",
      "geometry_wkt": "POINT(21.0357645511821 52.2584321119428)",
      "id": 1
    }
  },
  "request time": 0.0013017813364665
}
```

Z danego requestu dostajemy informacje o miejscowości, kodzie pocztowym, numerze budynku.
W analogiczny sposób kierujemy zapytanie, jeśli chcemy wyszukać działkę po numerze obrębu (tutaj `4-13-04`) połączonego z numerem działki (tutaj `60`).  
Przykładowe zapytanie:

```
https://uldk.gugik.gov.pl/?request=GetParcelByIdOrNr&id=4-13-04%2060&result=id,numer,powiat,gmina,geom_wkt,voivodeship,teryt,region,parcel,jednostka_ewidencyjna&srid=4326
```

Przykładowa odpowiedź:

```
1
146508_8.1304.60|60|Warszawa|Warszawa|SRID=4326;POLYGON((21.0354452630117 52.2584525918487,21.0355334926599 52.2585261742515,21.0355661733376 52.2585503412127,21.0356267871804 52.2585228207044,21.0356319082129 52.258516348045,21.0356225299489 52.2585105987735,21.0356503477517 52.2584978279027,21.0357953834715 52.2585438008329,21.0358040409553 52.2585638406859,21.0358371368578 52.2585578089143,21.035890755559 52.2585739704128,21.0358925097842 52.2585698355567,21.0359706464214 52.2584772388975,21.0355790504327 52.2583526987239,21.0354452630117 52.2584525918487))|Mazowieckie|146508_8.1304.60|4-13-04|60|Praga-Północ
```

## Ścieżka nr. 3

Działki można też lokalizować poprzez kliknięcie w punkt na mapie.
W tym wypadku najpierw odpytujemy ULDK:

```
https://uldk.gugik.gov.pl/?request=GetParcelByXY&xy=638952.8605770653,490022.82990579586&result=id,numer,powiat,gmina,geom_wkt,voivodeship,teryt,region,parcel,jednostka_ewidencyjna&srid=4326
```

Odpowiedź:

```
0
146508_8.1304.52|52|Warszawa|Warszawa|SRID=4326;POLYGON((21.0364366289483 52.2587570615703,21.0364840946195 52.2587707980298,21.0364810241808 52.2587772700923,21.0364854203202 52.2587799650313,21.0368522600284 52.2588923765819,21.036943564623 52.2587848556609,21.0365263258267 52.2586527770226,21.0364366289483 52.2587570615703))|Mazowieckie|146508_8.1304.52|4-13-04|52|Praga-Północ
```

a następnie na podstawie otrzymanej informacji o geometrii UUG:

```
https://services.gugik.gov.pl/uug/?request=GetAddressReverse&location=POINT(21.036533117294315%2052.2586558021617)&srid=4326
```

Odpowiedź:

```json
{
  "type": "address",
  "max results limit": 1,
  "radius": 100,
  "max polygon area": null,
  "returned objects": 1,
  "results": {
    "1": {
      "city": "Warszawa",
      "citypart": null,
      "street": "ulica Stalowa",
      "number": "5",
      "teryt": "146501",
      "simc": "0918123",
      "ulic": "20808",
      "code": "03-425",
      "jednostka": "{Polska,mazowieckie,Warszawa,Warszawa}",
      "x": "21.0363722492803",
      "y": "52.2586220504341",
      "geometry_wkt": "POINT(21.0363722492803 52.2586220504341)",
      "distance": "11.603237731433",
      "id": 1
    }
  },
  "request time": 0.0010969519615173
}
```

## Generowanie mapy

Po pobraniu wszytkich informacji jesteśmy gotowi do wyświetlenia tych danych na mapie przy pomocy serwisu KIEG.  
Poniżej znajduje się przykładowy kod, który używając biblioteki leaflet wyświetla mapę Polski wraz z nałożoną siatką działek (w elemencie o id `js-interactive-map-container`).

```js
const map = leaflet.map("js-interactive-map-container").setView([52, 19], 6);
leaflet
  .tileLayer(
    "https://mapy.geoportal.gov.pl/wss/ext/OSM/BaseMap/tms/1.0.0/osm_3857/GLOBAL_WEBMERCATOR/{z}/{x}/{y}.png",
    {
      tms: true,
      zoomOffset: -1,
      attribution:
        '&copy; <a href="https://osm.org/copyright">OpenStreetMap</a> contributors',
    }
  )
  .addTo(map);

leaflet.tileLayer
  .wms(
    "https://integracja.gugik.gov.pl/cgi-bin/KrajowaIntegracjaEwidencjiGruntow",
    {
      layers: "geoportal,dzialki,numery_dzialek,budynki",
      format: "image/png",
      transparent: true,
    }
  )
  .addTo(map);

map.zoomControl.setPosition("bottomleft");

map.setMaxBounds([
  [55.2223, 13.717078],
  [48.904122, 24.584035],
]);

map.setMinZoom(7);
```

Wyświetlanie działki na mapie polega na dodaniu do niej polygonu o geometrii, którą dostajemy w zapytaniach do API.

## uwagi końcowe

- Wyżej wymienione API bywają niestabilne (odpowiedzi czasami przychodzą po > 15s).
- Warto zwrócić uwagę, że geometria `POINT(21.036533117294315 52.2586558021617)` ma parametry rozdzielone spacją a nie przecinkiem.
