---
title: Юнит-тесты и линтеры
sidebar: applications_guide
guide_code: gitlab_nodejs
permalink: gitlab_nodejs/090_unittesting.html
toc: false
---

{% filesused title="Файлы, упомянутые в главе" %}
- .gitlab-ci.yml
{% endfilesused %}

В этой главе мы настроим в нашем базовом приложении выполнение тестов/линтеров. Запуск тестов и линтеров - это отдельная стадия в пайплайне Gitlab CI, для выполнения которой может требоваться соблюдение определенных условий. Рассмотрим на примере [линтера ESLint](https://eslint.org/) для языка программирования JavaScript (написан на Node.js).

Требуется добавить эту зависимость в `package.json`, создать к нему конфигурационный файл `.eslintrc.json` и прописать выполнение задания отдельной стадией на GitLab Runner командой [werf run]({{ site.docsurl }}/documentation/cli/main/run.html).

{% snippetcut name=".gitlab-ci.yml" url="https://github.com/werf/werf-guides/tree/master/examples/gitlab-nodejs/090-unittesting/.gitlab-ci.yml" %}
{% raw %}
```yaml
Run Tests:
  stage: test
  script:
    - werf run --stages-storage :local node -- npm run pretest
  except:
    - schedules
  tags:
    - werf
  needs: ["Build"]
```
{% endraw %}
{% endsnippetcut %}

Созданную стадию нужно добавить в список стадий:

{% snippetcut name=".gitlab-ci.yml" url="https://github.com/werf/werf-guides/tree/master/examples/gitlab-nodejs/090-unittesting/.gitlab-ci.yml" %}
{% raw %}
```yaml
stages:
  - build
  - test
  - deploy
```
{% endraw %}
{% endsnippetcut %}

Обратите внимание, что процесс будет выполняться на runner'е, внутри собранного контейнера, но без доступа к базе данных и каким-либо ресурсам Kubernetes-кластера.

{% offtopic title="А если нужно больше?" %}
Если нужен доступ к ресурсам кластера или база данных — это уже не линтер: потребуется собрать отдельный образ и прописать сложный сценарий деплоя объектов Kubernetes. Эти случаи выходят за рамки нашего гайда для начинающих.
{% endofftopic %}

<div>
    <a href="110_multipleapps.html" class="nav-btn">Далее: Несколько приложений в одном репозитории</a>
</div>
