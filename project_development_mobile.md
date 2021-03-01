# Zasady rozwijania projektów: Mobile

*Jeśli nie zgadzasz się z czymś w tym dokumencie przeczytaj sekcję ["Dobór i użycie technologii"](project_development.md#dobór-i-użycie-technologii) w "Zasadach Rozwijania Projektów" żeby dowiedzieć się jak go zmienić.*

## Ogólne zasady
1. Projekty mobilne tworzymy korzystając z najnowszej wersji frameworku Flutter z kanału *stable* wraz z odpowiadającą jej wersją języka Dart
2. Projekty powinny powstawać w IDE z mocnym wsparciem narzędzi flutterowych: Android Studio ([instrukcja konfiguracji](https://flutter.dev/docs/development/tools/android-studio)) lub VS Code ([instrukcja konfiguracji](https://flutter.dev/docs/development/tools/vs-code))
3. Projekty powinny mieć skonfigurowany linter/autoformatter `dartanalyzer` z włączonym pakietem [`very_good_analysis`](https://github.com/VeryGoodOpenSource/very_good_analysis)
4. Projekty powinny mieć skonfigurowane Continuus Integration skonfigurowane z linterem i odpalane automatycznie dla każdego pull requesta

## Standardowe komponenty
Lista standardowych komponentów których używamy w projektach flutterowych. Ta lista nie jest kompletna.
Przy wyborze zależności warto również sugerować się [listą pakietów z oficjalnym statusem *Flutter Favorite*](https://pub.dev/flutter/favorites).

| Typ komponentu                              | Wybrane oprogramowanie                                  | Uwagi
| --------------------------------            | ------------------------------------------              | -----
| Zarządzanie stanem                          | [bloc](https://pub.dev/packages/bloc)                   | wolimy używać cubitów niż bloców
| Webview                                     | [webview_flutter](https://pub.dev/packages/webview_flutter)
| Serializacja jsona                          | [json_serializable](https://pub.dev/packages/json_serializable) |
| Nierelacyjna baza danych                    | [Hive](https://pub.dev/packages/hive)                   |
| Ustawianie ikonki aplikacji                 | [flutter_launcher_icons](https://pub.dev/packages/flutter_launcher_icons)
