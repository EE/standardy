# Zasady rozwijania projektów: Frontend

*Jeśli nie zgadzasz się z czymś w tym dokumencie przeczytaj sekcję ["Dobór i użycie technologii"](project_development.md#dobór-i-użycie-technologii) w "Zasadach Rozwijania Projektów" żeby dowiedzieć się jak go zmienić.*

## Style
1. Nie używamy frameworków CSS.
2. Piszemy własne style w sposób semantyczny, wzorując się na modelu opisanym w dokumencie [MaintainableCSS](https://maintainablecss.com/). 
3. Style piszemy w [SASS](https://sass-lang.com/) i staramy się używać funkcjonalności SASS (np. zmienne, `@include`) żeby uczynić kod łatwiejszym w zarządzeniu.
4. W miejsce jednego dużego frameworku CSS używamy w razie potrzeby małych bibliotek spełniających jedną funkcję:
   * Zapewnienie spójności styli w przeglądarkach: [normalize.css](https://github.com/necolas/normalize.css)
   * Pozycjonowanie elementów w gridzie / kolumnach: [Neutron](http://neutroncss.com/) (jeśli Flexbox i CSS Grid nie wystarczają)
   * Łatwa i spójna obsługa breakpointów: [@include-media](https://include-media.com/)
   * Gotowe komponenty wizualne: instalowane pojedyńczo [komponenty Material Design](https://material.io/develop/web/). W projekcie korzystającym z Vue alternatywnie można użyć komponentów Material Design z [Vuetify](https://vuetifyjs.com/en/).
5. W projektach powinien być skonfigurowany linter [sass-lint](https://github.com/sasstools/sass-lint) ([wzorcowa konfiguracja](https://github.com/EE/generator-ee/blob/develop/%7B%7Bcookiecutter.project_slug%7D%7D/.sass-lint.yml)), skonfigurowany z systemem CI.
6. Zewnętrzne zależności CSS/SASS instalujemy przez `npm` i inportujemy w projekcie - nie kompiujemy zewnętrznego kodu do repozytorium projektu.
 
## JavaScript 
1. Nie używamy <strike>jQuery</strike>. Nie. Nie ma mowy.
2. Piszemy kod korzystający z funkcjonalności ECMAScript 2015+, który jest konwertowany na kod przyjazny dla przeglądarek przez [Babel](https://babeljs.io/). W szczególności korzystamy z podziału kodu na moduły.
3. Zewnętrzne zależności JavaScript instalujemy przez `npm` i inportujemy w projekcie - nie kompiujemy zewnętrznego kodu do repozytorium projektu.
4. W projektach powinien być skonfigurowany linter [eslint](https://eslint.org/) ([wzorcowa konfiguracja](https://github.com/EE/generator-ee/blob/develop/%7B%7Bcookiecutter.project_slug%7D%7D/.eslintrc.yml)), skonfigurowany z systemem CI.
5. W przypadku potrzeby zastosowania frameworku JavaScript używamy [Vue](https://vuejs.org/). Zakres użycia Vue powinien być dostosowany do potrzeb projektu (np. możliwe jest użycie Vue tylko na jednej podstronie).
   
## Na przyszłość
1. Po wyjściu wersji 3 Vue powinniśmy rozważyć zastąpienie JavaScriptu przez [TypeScript](https://www.typescriptlang.org/).
2. Po wyjściu stabilnej wersji Flutter Web będziemy rozważać budowanie frontendu wybranych projektów w Flutter Web. Takich projektów będą dotyczyć zasady opisane w dokumencie z [zasadami dla projektów mobilnych](project_development_mobile.md).
3. W przypadku budowania dużej aplikacji z gotowymi komponentami i frontendem budowanym w całości po stronie klienta (czyli backendem udostępniającym wyłącznie API) warto rozważyć użycie [frameworku Quasar](https://quasar.dev/), który integruje Vue i Material Design w *opinionated* sposób, dając dużo struktury i dodatkowych narzędzi. 
