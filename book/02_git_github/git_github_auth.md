### Аутентификация на GitHub

> Для того, чтобы начать работать с GitHub, надо на нем [зарегистрироваться](https://github.com/join).

Для безопасной работы с GitHub лучше использовать аутентификацию по ключам SSH.

> Эта же [инструкция на GitHub](https://help.github.com/articles/connecting-to-github-with-ssh/)

Генерация нового SSH ключа (используйте email, который привязан к GitHub):
```
$ ssh-keygen -t rsa -b 4096 -C "github_email@gmail.com"
```

На всех вопросах достаточно нажать enter (более безопасно использовать ключ с passphrase, но можно и без, если нажать enter, при вопросе).

Запуск ssh-agent:
```
$ eval "$(ssh-agent -s)"
```

Добавить ключ в ssh-agent:
```
$ ssh-add ~/.ssh/id_rsa
```

#### Добавление SSH ключа на GitHub

Для добавления ключа надо его скопировать.
Например, таким образом можно отобразить ключ для копирования:
```
$ cat ~/.ssh/id_rsa.pub
```

После копирования надо перейти на GitHub.

Находясь на любой странице GitHub, в правом верхнем углу нажмите на картинку вашего профиля и в выпадающем списке выберите Settings.
В настройках в левой панели надо выбрать поле "SSH and GPG keys".

После надо нажать "New SSH key" и в поле "Title" написать название ключа (например, "Home debian"), а в поле "Key" вставить содержимое, которое было скопировано из файла ~/.ssh/id_rsa.pub

> Если GitHub запросит пароль - введите пароль своего акаунта GitHub.


Чтобы проверить, что всё прошло успешно, попробуйте выполнить команду локально:
```
$ ssh -T git@github.com
```

Вывод будет таким:
```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

Теперь вы готовы работать с Git и GitHub.

