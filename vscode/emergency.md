## 📑 Visual Studio Code

### Возможные ошибки

#### The workspace is too large to index (34567 files, 5000 max)

Эта ошибка возможно при использование расширения `Ruby Solargraph` из инстуркции [📑 Visual Studio Code > Перейти к определению для Ruby](./navigate_ruby.md)

для решения проблемы выполните команду, она создаст в корне проекта файл `.solargraph.yml`

```sh
solargraph config
```

увеличьте в нем настройку
```yaml
max_files: 40000
```

выполните в терминале в корне проекта команды
```sh
solargraph scan
```

если будут еще ошибки `Error reading`, `[WARN] <internal ... require': cannot load such file -- version (LoadError)`, `[WARN] .. No such file or directory @ rb_sysopen - (Errno::ENOENT)`

исключите эти файлы, измените в файл конфига `.solargraph.yml` настройку `exclude` добавив в список проблемную директорию

```yaml
exclude:
- spec/**/*
- test/**/*
- vendor/**/*
- ".bundle/**/*"
- ".bundle/**/*"
- notworkdir
```

если через регулярки не получится, исключите всю папку

проверьте ушли ли ошибки, повторите вызов команды
```sh
solargraph scan
```

проверьте сколько теперь файлов
```sh
solargraph list
```

если сталок ок, то откатите настройку вернув исходное значение

```yaml
max_files: 5000
```

в случае проблем используйте очистку
```sh
solargraph clear
```

и повторное сканирование
```sh
solargraph scan
```

перезапустите VS Code, для итоговой корректной работы

если вы не хотите комитеть файл настроек в репозиторий, то можете воспользоваться инструкцией [🐱 Git > Глобальный .gitignore](../git/global_gitignore.md)

подробнее о настройках тут https://solargraph.org/guides/configuration

#### Если не работает просмотр рендора MarkDown

То найдите предыдущую открытую вкладку с рендором MarkDown и закройте ее