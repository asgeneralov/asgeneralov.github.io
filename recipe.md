---
layout: default 
title: "Рецепты"
---
# {{page.title}}
## Сгенерировать SSH ключ для доступа в репозиторий


```
cd ~/.ssh
ssh-keygen
```
На запрос имени лучше ввести id_rsa, его git будет использовать по умолчанию.
Не забыть зарегистрировать ключ на сайте куда требуется доступ. Например на github.com
Profile -> Settings -> Access -> SSH and GPG keys -> New SSH key 

## Jekyll начало работы
Инструкция по установке Ruby и Gems 
https://jekyllrb.com/docs/installation/

```
#Установить менеджер пакетов bundler 
gem install jekyll bundler
#В папке с проектом есть Gemfile
bundle install or bundle init
#Создать новый проект
bundle init
#pf
bundle exec jekyll serve
```

## Как настроить переключение раскладки клавиатуры в Windows кнопкой Capc Lock
Утилита Switchy
https://github.com/erryox/Switchy?tab=readme-ov-file