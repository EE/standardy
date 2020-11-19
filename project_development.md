# Zasady rozwijania projektów

## Aktualne README
Każdy projekt powinien zawerać aktualne README. Powinno być ono na bieżąco aktualizowane, tak aby zawsze zawierał ono aktualne informacje niezbędne do zainstalowania Twojego projektu.

## Styl formatowania kodu
Kod powinien być kontrolowany przy pomocy linterów i formaterów:

| Język           | Linter         | Formatter      | Wzorcowa konfiguracja
| ----------------| ---------------|----------------| -----
| Python          | [flake8]       | -              | [linter][flake8-conf]
| Dart            | [dartanalyzer] | [dartanalyzer] | oparta o pakiet [lint][flutter-lint]
| JavaScript (ES) | [eslint]       | [prettier]     | [linter][eslint-conf], [formatter][prettier-conf]
| TypeScript      | [tslint]       | [prettier]     | [formatter][prettier-conf]
| CSS / SCSS      | [stylelint]    | [prettier]     | [linter][stylelint-conf], [formatter][prettier-conf]
| Json            | -              | [prettier]     | [formatter][prettier-conf]
| Markdown        | -              | [prettier]     | [formatter][prettier-conf]

[flake8]: http://flake8.pycqa.org/
[flake8-conf]: https://github.com/EE/generator-ee/blob/develop/%7B%7Bcookiecutter.project_slug%7D%7D/tox.ini
[eslint]: https://eslint.org/
[eslint-conf]: https://github.com/EE/generator-ee/blob/develop/%7B%7Bcookiecutter.project_slug%7D%7D/.eslintrc.yml
[prettier]: https://prettier.io/
[prettier-conf]: https://github.com/EE/generator-ee/blob/develop/%7B%7Bcookiecutter.project_slug%7D%7D/.prettierrc
[tslint]: https://palantir.github.io/tslint/
[stylelint]: https://stylelint.io/
[stylelint-conf]: https://github.com/EE/generator-ee/blob/develop/%7B%7Bcookiecutter.project_slug%7D%7D/.stylelintrc
[dartanalyzer]: https://dart.dev/tools/dartanalyzer
[flutter-lint]: https://pub.dev/packages/lint

Programiści powinni ustawić w swoich edytorach automatyczne lintowanie i formattowanie przy pomocy narzędzi odpwiadających językom z którymi pracują. Instrukcje takiej konfiguracji znajdują się zazwyczaj w dokumentacji narzędzia i/lub edytora.

## Kontrola wersji
Wszystkie projekty programistyczne w Laboratorium EE powinny posiadać swoje repozytorium kodu na [firmowym Githubie](https://github.com/EE/), zawierające najnowszą wersję kodu projektu. [Zasady pracy z gitem w Laboratorium EE opisane są w osobnym dokumencie](gitflow/command_line_guide.md).

## Definition of Done
Projekty powinny być rozwijane w sposób zgodny z firmowym [Definition of Done](https://eetools.laboratorium.ee/strony/dod/). Opcjonalnie projekty mogą opracowywać własne dokumenty DoD uszczegóławiające zasady ogólnofirmowe.

## Komentarze w kodzie
Wszystkie "hacki", rzeczy które na pierwszy rzut oka mogą wydawać się nieintuicyjne lub dziwne, których powód istnienia nie będzie jasny z samego przeczytania kodu, powinny być opisane odpowiednim komentarzem zamieszczonym bezpośrednio nad danym kodem. Komentarz powinien skupiać się nie na *co*, ale na *dlaczego* - czytający go programista powinien być w stanie zrozumiec motywacją stojącą za zastosowaniem danego nieintuicyjnego rozwiązania. Komentarze powinny być wyczerpujące - jeśli wytłumaczenie sytuacji która doprowadziłą do powstania kodu wymaga bardzo długiego opisu to taki opis powinien się tam znaleźć.

## Dobór i użycie technologii
Staramy się standaryzować to jakich technologii używamy w firmie i w jaki sposób ich używamy. To znaczy że np. decyzja jakiej bazy SQL używamy na backendzie została podjęta na poziomie firmy i dotyczy wszystkich projektów. Chcemy w ten sposób ułatwić wykorzystywanie doświadczeń i narzędzi z projektów w innych projektach oraz ułatwić przechodzenie ludzi między projektami. Poza tym nie ma sensu by każdy projekt każdorazowo od nowa poświęcał czas na dyskutowanie i podejmowanie tych samych decyzji. 

Poniżej znajduje się lista szczegółowych dokumentów opisujących te zasady dla poszczególnych typów oprogramowania. Dopuszczamy odstąpienie przez projekt od tego co w nich zapisano, ale tylko gdy istnieje dobre uzasadnienie wynikajace ze *specyfiki* danego projektu. Jeśli po prostu uważasz, że jakiś zapis jest niesłuszny lub nieaktualny nie dokonuj samowoli w jednym projekcie. Zamiast tego zgłoś pull requesta do tego repozytorium zmieniającego dany zapis. Pamiętaj by zamieścić w opisie pull requesta dobre, merytoryczne uzasadnienie tej zmiany. Jeśli powstanie konsensus za zmianą stanie się ona nową zasadą dotyczącą wszystkich nowych projektów w firmie. W podobny sposób możesz również zgłaszać nowe zapisy.

Szczegółowe zasady:
* [Backend](project_development_backend.md)
* [Frontend](project_development_frontend.md)
* [Mobile](project_development_mobile.md)
