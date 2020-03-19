# Checklista konfiguracji repozytoriów

1. Każde repozytorium powinno mieć przynajmniej dwa branche: *develop* i *master*
2. Jako domyślny branch (karta "Branches" w konfiguracji) powinien być ustawiony branch *develop*.
3. Zarówno *develop* jak i *master* powinny być skonfigurowane jako "protected branch" z następującymi opcjami:
  * *Require pull request reviews before merging*
  * *Dismiss stale pull request approvals when new commits are pushed*
  * *Require status checks to pass before merging*
