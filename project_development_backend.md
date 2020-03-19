# Zasady rozwijania projektów: Backend

*Jeśli nie zgadzasz się z czymś w tym dokumencie przeczytaj sekcję ["Dobór i użycie technologii"](project_development.md#dobór-i-użycie-technologii) w "Zasadach Rozwijania Projektów" żeby dowiedzieć się jak go zmienić.*

## Ogólne zasady
1. Backend tworzymy korzystając z najnowszej wersji "Long Term Support" frameworku Django i z Pythona 3
2. Nowe projekty powinny zostać wygenerowane poprzez [generator projektów][generator-ee]
3. Projekty powinny stosować się do [Coding Style projektu Django](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/). Oznacza to jednocześnie stosowanie się do zasad [PEP-8](https://www.python.org/dev/peps/pep-0008/) (wyjątkiem jest zgoda na linie do 119 znaków)
4. Projekty powinny mieć skonfigurowane Continuus Integration (zazwyczaj CI od Heroku) skonfigurowane z linterem i odpalane automatycznie dla każdego pull requesta

## Standardowe komponenty
Lista standardowych komponentów których używamy w projektach przy budowaniu backendu

| Typ komponentu                              | Wybrane oprogramowanie                                  | Uwagi
| --------------------------------            | ------------------------------------------              | -----
| Manager paczek pythonowych                  | [pipenv](https://github.com/pypa/pipenv)                |
| Framework backend                           | [Django][django]                                        | wygenerowany z [generatora projektów][generator-ee]
| Baza SQL                                    | [PostgreSQL][postgres]                                  |       
| Linter Python                               | [flake8][flake8]                                        | `max-line-length = 119`
| Zaawansowany silnik wyszukiwania            | [Elastcsearch][elastic]                                 |                                           |
| Klient HTTP                                 | [requests][requests]                                    |
| CMS dla stron treściowych                   | [Wagtail][wagtail]                                      |
| Prosty silnik wyszukiwania                  | [django.contrib.postgres.search][contrib]               | projekt wbudowane w Django
| Kompilacja i minifikacja plików statycznych | [django-compressor][compressor]                         |
| *Context processor* dla ustawień            | [django-settings-context-processor][settings-con-procs] |
| Tłumaczenia treści                          | [django-modeltranslation][django-modeltranslation]      |
| Edytor WYSIWYG                              | [django-ckeditor][django-ckeditor]                      |
| Generowanie REST API                        | [Django Rest Framework][django-rest-framework]          |
| Szeregowanie wpisów w adminie               | [django-admin-sortable2][django-admin-sortable2]        |
| Zarządzanie ustawieniami w adminie          | [django-constance][django-constance]                    |
| Skład plików media w AWS S3 (i innych CDNach) | [django-storages](https://django-storages.readthedocs.io) |
| Przetwarzanie obrazów                       | [django-imagekit][django-imagekit]

[generator-ee]: https://github.com/EE/generator-ee
[django]: https://www.djangoproject.com/
[postgres]: https://www.postgresql.org/
[django-imagekit]: https://github.com/matthewwithanm/django-imagekit
[flake8]: http://flake8.pycqa.org/en/latest/
[contrib]: https://docs.djangoproject.com/en/dev/ref/contrib/postgres/search/
[elastic]: https://www.elastic.co/products/elasticsearch
[compressor]: https://github.com/django-compressor/django-compressor
[settings-con-procs]: https://pypi.python.org/pypi/django-settings-context-processor/0.2
[django-admin-sortable2]: https://github.com/jrief/django-admin-sortable2
[django-modeltranslation]: https://github.com/deschler/django-modeltranslation
[django-ckeditor]: https://github.com/django-ckeditor/django-ckeditor/
[django-rest-framework]: http://www.django-rest-framework.org/
[requests]: http://docs.python-requests.org/en/master/
[django-constance]: https://github.com/jazzband/django-constance
[wagtail]: https://wagtail.io/
