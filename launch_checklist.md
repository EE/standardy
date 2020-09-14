# Checklista przy uruchamianiu projektów Django na produkcji

## Bezpieczeństwo
1. Strona posiada skonfigurowany certyfikat SSL.
2. Strona wymusza użycie SSL. W szczególności nastęujące ustawienia Django są ustawione prawidłowo:
    * `SECURE_SSL_REDIRECT = True` (przekierowywanie na wersję SSL)
    * `SECURE_HSTS_SECONDS = 31536000`, `SECURE_HSTS_PRELOAD = True` (zapamiętywanie wyboru wersji SSL w przeglądarce)
    * `CSRF_COOKIE_SECURE = True` (przesyłąnie ciasteczek csrf wyłącznie przez połączenia SSL)
    * `SESSION_COOKIE_SECURE = True` (przesyłanie ciasteczek sesji wyłącznie przez połączenia SSL)
3. Serwis **nie** działa w trybie `DEBUG`. Wiąże się to z ustawieniem `DEBUG = FALSE`, ale należy dodatkowo zweryfikować tryb poprzez wyświetlenie ekranu błędu 404 (który wygląda różnie zależnie od wartości `DEBUG`).
4. W ustawieniach Django włączone są middleware związane z bezpieczeństwem: `XFrameOptionsMiddleware`, `SecurityMiddleware`.
5. Serwis używa [bazy danych o odpowiednich parametrach](https://www.heroku.com/pricing#postgres-pricing) (w przypadku Heroku oznacza to zwykle płatną bazę danych, poza sytuacjami bardzo małych, niemal statycznych stron).
6. Baza danych ma [skonfigurowane automatyczne backupy](https://devcenter.heroku.com/articles/heroku-postgres-backups#scheduling-backups).
7. W bazie danych nie ma użytkowników Django z ustawienionymi "testowymi" hasłami. W szczególności użytkownicy o uprawnieniach administratora mają mocne hasła.
8. Serwis jest skonfigurowany z narzędziem Sentry do którego trafiają wyjątki występujące podczas jego działania. Jest wyznaczona osoba odpowiedzialna za czytanie tych zgłoszeń.
9. Wykonano audyt bezpieczeństwa Django [Pony Checkup](https://www.ponycheckup.com/).

## Prezentacja
1. W nagłówku strony ustawione są odpowiednie metadane, w szczególności tag `title`, oraz właściwości meta: `description`, `viewport`, `og:image`, `og:site_name`. Działanie metadanych zostało przetestowane m.in. przez wstawienie linka do strony na Slacku i Facebooku i obejrzenie wygenerowanego poglądu strony.
2. Działanie i wygląd strony zostało sprawdzone zarówno na przeglądarkach mobilnych jak i przeglądarkach desktopowych.
3. Zostały zdefiniowane strony błędów 404 i 500 spójne z wyglądem serwisu.
4. Zwracając się do użytkowników/użytkowniczek oraz pisząc o nich aplikacja używa w komunikatach końcówek neutralnych płciowo (np. zamiast "czy chciałbyś..." można napisać "czy chcesz...").
5. Grafiki wykorzystywane na stronie są wektorowe (najlepiej) lub w rozdzielczości wystarczającej dla ekranów retina. Do sprawdzenia tego można użyć [automatycznych narzędzi](http://www.retinaextension.com/).
6. Domena z `www.` przkierowuje do wersji bez www (jeśli ograniczenia techniczne serwera DNS narzucają obecność serwisu pod subdomeną, dopuszczalne jest odwrotne przekierowanie).
7. Wykonano audyty w kategoriach *Best practises*, *Accessibility*, *SEO*, *Performance* w narzędziu Lighhouse wbudowanych w Google Chrome (*View / Developer / Developer Tools / Audits*). Nie jest wymagane spełnienie każdego zalecenia z raportu, ale jest wymagane dokładne przejrzenie wszystkich i podjęcie świadomych decyzji dotyczących tego które zostaną zaimplementowane.

## Wydajność
1. Pliki statyczne są zminimalizowane. W przypadku plików CSS i JS oznacza to skonfigurowanie automatycznego narzędzia do minimalizacji. W przypadku plików graifcznych oznacza to użycie odpowiedniego formatu (jpeg dla zdjęć, svg lub png dla rysunków i grafik), kompresji i wymiarów. W odpowiedniej kompresji opornych plików graficznych przydają się narzędzia takie jak [ImageOptim](https://imageoptim.com/) (lub [TinyPNG](https://tinypng.com/)) oraz [pngquant](https://pngquant.org/). 
2. Pliki statyczne (w tym pliki wgrywane przez użytkowników) są serwowane z nagłówkami pozwalającym na cachowanie ich w przeglądarce.
3. Wykonano audyt [Google PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/).
