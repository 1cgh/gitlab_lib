# gitlab_lib
Примеры пайплайнов GitLab для разработки на 1С и заметки по работе с GitLab

## FAQ

### При регистрации GitLab Runner в Windows некорректно указывается путь к исполняемому файлу PowerShell. Как исправить?

В Windows при регистрации раннера в конфигурационном файле **config.toml** в поле **shell** указывается по умолчанию **pwsh**. В большинстве случаев в системе установлена старая версия PowerShell (5.1 или ниже), для которой основным исполняемым файлом является powershell.exe. В результате путь к pwsh в системе не обнаруживается, что приводит к ошибке.

Способы решения:
* Установить актуальную версию PowerShell. После установки в PATH должен быть указан путь к основному исполняемому файлу pwsh.exe.
* В конфигурационном файле **config.toml** в поле **shell** указать **powershell**. Также необходимо убедиться, что путь к powershell.exe есть в PATH.
* Если вы привыкли писать скрипты автоматизации для CMD, то можно в конфигурационном файле **config.toml** в поле **shell** указать **cmd**, в этом случае скрипты Gitlab CI будут выполняться в CMD. Данный способ **не рекомендуется**, так как поддержка CMD в Gitlab CI признана устаревшей.

### Как обеспечить параллельное выполнение заданий в Gitlab Runner?

В конфигурационном файле **config.toml** нужно задать настройку **concurrent**. В качестве значения указывается число заданий, которые могут одновременно выполняться. Настройка может быть задана как для всего файла, так и в каждой отдельной секции \[\[runners\]\]. [Подробности](https://docs.gitlab.com/runner/configuration/advanced-configuration.html#the-global-section).

Пример:
concurrent = 4

### При завершении задания по таймауту не срабатывает after_script

after_script не выполняется для отмененных заданий и заданий с истекшим таймаутом. Есть [незакрытый issue на эту тему](https://gitlab.com/gitlab-org/gitlab/-/issues/15603).
