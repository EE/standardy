# Zasady rozwijania projektów: Frontend

*Jeśli nie zgadzasz się z czymś w tym dokumencie przeczytaj sekcję ["Dobór i użycie technologii"](project_development.md#dobór-i-użycie-technologii) w "Zasadach Rozwijania Projektów" żeby dowiedzieć się jak go zmienić.*

## JavaScript 
1. Używamy stacku technologicznego opartego na Vue, Nuxt, Vuex i Vue-Router (wyjątkiem mogą być strony treściowe zarządzane przez CMS, dla których dopuszczalne jest użycie [Wagtaila](https://wagtail.io/)).
1. Nie używamy <strike>jQuery</strike>. Nie. Nie ma mowy.
2. Piszemy kod korzystający z funkcjonalności ECMAScript 2015+, który jest konwertowany na kod przyjazny dla przeglądarek przez [Babel](https://babeljs.io/). W szczególności korzystamy z podziału kodu na moduły.
3. Przestrzegamy rekomendacji spisanych w oficjalnym [Style Guidzie Vue](https://vuejs.org/v2/style-guide/). Oznacza to między innymi stosowanie odpowiednich konwencji nazewnictwa:
    * Nazwy komponentów używają *PascalCase*
    * [Nazwy komponentów muszą być co najmniej dwuczłonowe](https://vuejs.org/v2/style-guide/#Multi-word-component-names-essential)
    * [Każdy komponent znajduje się w osobnym pliku](https://vuejs.org/v2/style-guide/#Single-file-component-filename-casing-strongly-recommended), [którego nazwa jest identyczna z nazwą komponentu (czyli też używa *PascalCase*)](https://vuejs.org/v2/style-guide/#Single-file-component-filename-casing-strongly-recommended)
    * [Nazwy komponentów używane jako tagi w templatkach są identyczne do oryginalnej nazwy komponentu (czyli też używają *PascalCase*)](https://vuejs.org/v2/style-guide/#Component-name-casing-in-templates-strongly-recommended)
    * [Komponenty prezentacyjne (takie które przyjmują dane przez `props` i wyświetlają je w określony sposób, bez wchodzenia w logikę biznesową) używają przedrostka `App`](https://vuejs.org/v2/style-guide/#Base-component-names-strongly-recommended)
    * [Komponenty które z samego swojego założenia pojawiają się w aplikacji tylko raz używają przedrosta `The`](https://vuejs.org/v2/style-guide/#Single-instance-component-names-strongly-recommended)
    * [Komponenty które z założenia powinny być wyświetlane jako dziecko konkretnego komponentu używają przedrostka z nazwą tego rodzica](https://vuejs.org/v2/style-guide/#Tightly-coupled-component-names-strongly-recommended)
4. Staramy się jak najbardziej wykorzystywać konwencje i funkcjonalności przychodzące z Nuxt.js (np. [routing oparty na komponentach w katalogu `pages/`](https://nuxtjs.org/guide/routing)).
3. Dane aplikacji przechowujemy w Vuex. Powinny być one logicznie podzielone na wydzielone (`namespaced: true`) moduły, [podzielone na pliki zgodnie z konwencją Nuxt](https://nuxtjs.org/guide/modules). Kod operujący na tych danych (w tym pobierający je z zewnętrznego API) powinien znajdować się w akcjach (*actions*) odpowiedniego modułu.
4. Korzystamy z Vue Single File Components (plików `.vue`). W szczególności wszystkie style specyficzne dla danego komponentu wizualnego powinny znajdować się wewnątrz jego pliku.
5. Pisząc komponenty Vue, preferujemy kompozycję (osadzanie komponentów wewnątrz innych komponentów) ponad dziedziczenie (w tym używanie mixinów).
6. Piszemy testy korzystając z [Jest](https://jestjs.io/). Stopień otestowania zależny jest od projektu, ale w każdym projekcie powinny istnieć testy obejmujące przynajmniej kilka wybranych, najbardziej newralgicznych elementów. Testy powinny być automatycznie wykonywane przez system CI. 
7. Zewnętrzne zależności JavaScript instalujemy przez `npm` i inportujemy w projekcie - nie kopiujemy zewnętrznego kodu do repozytorium projektu.
8. W projektach powinien być skonfigurowany linter [eslint](https://eslint.org/) ([wzorcowa konfiguracja](https://github.com/EE/generator-ee/blob/develop/%7B%7Bcookiecutter.project_slug%7D%7D/.eslintrc.yml)), skonfigurowany z systemem CI.


## Style
1. Nie używamy frameworków CSS.
2. Piszemy własne style w sposób semantyczny, wzorując się na modelu opisanym w dokumencie [MaintainableCSS](https://maintainablecss.com/). 
3. Style piszemy w [SASS](https://sass-lang.com/) i staramy się używać funkcjonalności SASS (np. zmienne, `@include`) żeby uczynić kod łatwiejszym w zarządzeniu.
4. Nie używamy jednak operatora `&` z SASS w sposób który rozdzielałby nazwę klasy (gdzie nazwa zagnieżdżonej klasy zawiera nazwę rodzica).
5. W miejsce jednego dużego frameworku CSS używamy w razie potrzeby małych bibliotek spełniających jedną funkcję:
    * Zapewnienie spójności styli w przeglądarkach: [reset.css](https://www.npmjs.com/package/reset-css)
    * Pozycjonowanie elementów w gridzie / kolumnach: [Neutron](http://neutroncss.com/) (jeśli Flexbox i CSS Grid nie wystarczają)
    * Łatwa i spójna obsługa breakpointów: [@include-media](https://include-media.com/)
    * Gotowe komponenty wizualne: komponenty Material Design z [Vuetify](https://vuetifyjs.com/en/).
6. Trzymamy się zasady *mobile first* - style powinny opisywać najmniejszy obsługiwany breakpoint, z modyfikacjami dla większych breakpointów jako odrębny blok `@media`
7. W projektach powinien być skonfigurowany linter [style-lint](https://stylelint.io/) ([wzorcowa konfiguracja](https://github.com/EE/generator-ee/blob/develop/%7B%7Bcookiecutter.project_slug%7D%7D/.stylelintrc)), skonfigurowany z systemem CI.
8. O ile nie stoi to w sprzeczności z wymaganiami klienta, budujemy strony w oparciu o Material Design.
   
## Na przyszłość
1. Wkrótce przejdziemy na Vue 3, ale chcemy odczekać aż ekosystem Vue się dostosuje do tej wersji.
1. Po przejściu na Vue 3, powinniśmy rozważyć zastąpienie JavaScriptu przez [TypeScript](https://www.typescriptlang.org/).
2. Po wyjściu stabilnej wersji Flutter Web będziemy rozważać budowanie frontendu wybranych projektów w Flutter Web. Takich projektów będą dotyczyć zasady opisane w dokumencie z [zasadami dla projektów mobilnych](project_development_mobile.md).
