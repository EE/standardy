# Potrzeby programistów względem makiet / projektów graficznych

*Ten dokument opisuje jak według naszego zespołu programistów frontendowych jakie cechy powinien mieć projekt graficzny lub makieta żeby mógł być szybko i bezproblemowo wdrożony w życie. Dokument nie jest zbiorem twardych wymogów, a raczej podstawą do ustaleń w danym projekcie - warto się na nim oprzeć, ale dopuszczalne jest ustalenie z góry że w danym projekcie wybrane (konkretne) cechy nie zostaną spełnione (na przykład ze względu na koszt lub specyfikę projektu).*


1. Projekt zawierający przynajmniej widok desktopowy (1360 px) i mobilny (375 px), a także przewidzenie jak zachowuje się strona dla szerszych ekranów (np. określenie że w linii mieści się odpowiednio więcej elementów lub że elementy stają się proporcjonalnie szersze lub że elementy zostają bez zmian z marginesami po bokach). Mile widziany dodatkowo projekt na tablety.
2. Preferujemy projekty z Figma lub Zeplin, wyeksportowane z włączoną opcją deweloperską (nie tylko podglądem) i dołączonymi assetami (np. ikonami czy zdjęciami).
3. Mile widziany design system zawierający:
    * paletę kolorów użytych w projekcie razem z kontekstem użycia
    * fonty użyte w projekcie  razem z kontekstem użycia
    * powtarzalne elementy interfejsu (przyciski, pola itd)
4. Używanie w projekcie nie więcej niż dwóch-trzech fontów aby ograniczyć ilość danych koniecznych do załadowania strony. Fonty te muszą być dostępne na Google Fonts, lub musimy posiadać wykupione do nich prawa do dystrybucji przez www (oraz odpowiednio przygotowane do tego pliki).
5. Przewidzenie możliwych w danej sytuacji alternatywnych stanów aplikacji (bezpośrednio na makiecie lub w komentarzu obok jeśli stan nie wymaga dokładnego rozrysowania) takich jak:
    * pusty stan (np. lista bez dodanych żadnych elementów)
    * stan wystąpienia błędu (np. błąd walidacji lub błąd połączenia przy wysyłaniu danych)
    * stan oczekiwania na załadowanie / wysyłanie danych
    * stan nieaktywnego elementu (np. gdy funkcja jest niedostępna)
    * stany związane z interakcją użytkownika z interfejsem (np. co się dzieje po po najechaniu myszką na aktywny element czy po rozwinięciu listy)
6. Uwzględnienie projektów stron 404 (“Strony nie znaleziono”) i 500 (“Wystąpił błąd serwera”).
7. Zachowanie konsekwencji przy stosowaniu rozmiarów, zarówno na pojedyńczej stronie jak i między stronami (np. stała konwencja dotycząca rozmiaru czcionki akapitu czy odległości między tekstem a poprzedzającym go tytułem).
8. Przy projektowaniu tekstów do tytułów i elementów o ograniczonej powierzchni (np. kafelki) przewidzenie maksymalnej długości tekstu które może się tam znaleźć a także zachowania gdy znajdujące się obok siebie elementy będą miały różne długości tekstu (co mogłoby powodować np. różną wysokość kafelków lub niekonsekwentne położenie elementów nawigacyjnych wewnątrz nich).
9. W miejscach gdzie lista elementów może się zmieniać (np. konfigurowalne menu czy lista elementów tworzonych przez użytkownika) - przewidzenie co stanie się gdy elementy przestają się mieścić w przewidzianej przestrzeni.
