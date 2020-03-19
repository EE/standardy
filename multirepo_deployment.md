# Deployment projektów składających się z kilku repozytoriów

## Problem
Czasem na jeden projekt składa się kod zawarty w kilku prywatnych repozytoriach. Na przykład projekty Wordpressowe osobno trzymają  kod strony i kod jej skórek, a projekty Sonarowe posiadają osobne repozytorium na kod wspólnego frameworku, niezależne od repozytoriów poszczególnych serwisów.

Powoduje to że na tym samym koncie i serwerze trzeba umieścić kod pochodzący z więcej niż jednego repozytorium. Niestety Github nie pozwala na to żeby wgrać ten sam *deployment key* do więcej niż jednego repozytorium. A więc jeśli wgramy klucz w pierwszym repozytorium, nie będziemy mogli wgrać go w drugim. Rozwiązanie [które proponuje sam GitHub](https://developer.github.com/v3/guides/managing-deploy-keys/#machine-users) jest naszym przypadku jest bez sensu (GitHub proponuje żeby utworzyć specjalnego usera do deploymentów który dostawałby uprawnienia read-only do tych repo... i miałby wgrany jako swój klucz klucz danego serwera - co by dawało przez dany klucz dostęp do obu repo; nie będziemy jednak przecież dla każdego takiego projektu tworzyć nowego usera).

## Rozwiązanie
Rozwiązaniem jest ustawienie serwera tak żeby obsługiwał wiele kluczy i stosował odpowiedni klucz do odpowiedniego repo. Nie jest to trudne ani nie wymaga uprawnień administratora na serwerze - wystarczy zwykły dostęp do folderu .ssh w katalogu domowym użytkownika.

Żeby to osiągnąć należy wygenerować na serwerze osobne pary kluczy dla każdego repozytorium. Klucze publiczne należy dodać jako deploy key w konfiguracji odpowiadających im projektów na GitHubie. Następnie na serwerze w pliku `~/.ssh/config` należy dodać konfigurację ustawiającą który klucz (prywatny) ma być używany z którym repo. Na przykład:

    Host some-repo
      Hostname github.com
      IdentityFile ~/.ssh/some-repo-key

    Host other-repo
      Hostname github.com
      IdentityFile ~/.ssh/other-repo-key

To trochę oszustwo, bo tak naprawdę nie ustawia się tego który klucz związany jest z którym repo, a który klucz używany jest z którą pseudo-domeną (na przykładzie są dwie: `some-repo` i `other-repo`). Jednak ssh config pozwala ustawić arbitralne "domeny", które stają się aliasem hosta ustawionego w `Hostname`, więc to wygodny sposób na routing kluczy do repozytoriów.

Trzeba tylko pamietać żeby klonując projekt danego projektu zamiast domeny github.com używać wersji z odpowiednią "domeną". Na przykład zamiast `git clone git@github.com:EE/some.git` musi to być `git@some-repo:EE/some.git`, a zamiast `git@github.com:EE/other.git` musi być `git@other-repo:EE/other.git`.
