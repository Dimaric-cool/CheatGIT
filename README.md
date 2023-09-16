# Что такое SSH. Генерируем SSH-ключ

## Что такое SSH
Когда компьютеры обмениваются данными в сети, они следуют сетевым протоколам (англ. network protocols) — правилам обмена данными между компьютерами.

Один из наиболее распространённых сетевых протоколов — SSH (от англ. Secure Shell Protocol). Он обеспечивает безопасный обмен данными в сети. С помощью этого протокола можно получать данные с удалённого компьютера или отправлять их на него. Трафик шифруется, поэтому протокол безопасен.

SSH использует пару ключей для обеспечения безопасности — публичный и приватный: 
**Приватный ключ** (англ. private key) хранится только на вашем компьютере и не должен передаваться кому-либо ещё. Он используется для расшифровки данных.
**Публичный ключ** (англ. public key) доступен всем и используется для шифрования данных. Они могут быть расшифрованы парным приватным ключом.

Только вы можете расшифровать данные с помощью приватного ключа, но любой владелец публичного ключа может их для вас зашифровать. Эти два ключа связаны и образуют SSH-пару. В будущем вы наверняка будете использовать их для взаимодействия с GitHub и другими удалёнными серверами.

## Проверка наличия SSH-ключа

```$ cd ~ # перешли в домашнюю директорию``` 

Обычно SSH-ключи находятся в директории .ssh/. Проверить наличие этой директории и файлов в ней можно с помощью следующей команды.

```$ ls -la .ssh/ # вывели список созданных ключей ```

Если папка пустая или её нет, всё в порядке. 
Если есть файлы с похожими названиями, SSH-ключи уже создавались:
__id_dsa.pub;__
__id_ecdsa.pub;__
__id_ed25519.pub;__
__id_rsa.pub.__
Если вы не создавали эти файлы, удалите их все.

## Инструкция по генерации SSH-ключа
1. Для генерации SSH-пары можно использовать программу ssh-keygen. Откройте терминал и введите следующую команду.

```$ ssh-keygen -t ed25519 -C "электронная почта, к которой привязан ваш аккаунт на GitHub"```

Используйте электронную почту, к которой привязан ваш GitHub-аккаунт.
Если вы видите сообщение об ошибке, то, скорее всего, ваша система не поддерживает алгоритм шифрования ed25519. Ничего страшного: используйте другой алгоритм.

```$ ssh-keygen -t rsa -b 4096 -C "электронная почта, к которой привязан ваш аккаунт на GitHub" ```

После ввода отобразится такое сообщение.

```> Generating public/private rsa key pair. # сгенерированы публичный и приватный ключи ```

2. Укажите место хранения ключей. Простой вариант — сделать домашний каталог пользователя путём по умолчанию. Для этого нажмите Enter.
###   macOS
```> Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter] ```
###   Windows
```> Enter a file in which to save the key (C:\Users\<имя_пользователя>\.ssh\):[Press enter] ```

Теперь в указанной директории появится пара ключей.
3. Программа запросит кодовую фразу (англ. passphrase) для доступа к SSH-ключу. Вы можете оставить поле пустым. Для этого нажмите Enter, а затем ещё раз Enter для подтверждения.
```
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again] 
```
💡 Быть или не быть кодовой фразе — вот в чём вопрос
Как бы странно ни звучало, кодовая фраза — это «пароль от ключа». Представьте, что SSH-ключ лежит в шкатулке. А на самой шкатулке — кодовый замок, который открывается кодовой фразой.
Многие пользователи Git не используют кодовую фразу для защиты своего SSH-ключа. Если такой фразы нет, то её не нужно вводить всякий раз при взаимодействии с удалённым репозиторием.
С другой стороны, применение кодовой фразы усиливает безопасность ключей. Если вы используете эту фразу, ключ будет надёжно защищён в случае несанкционированного доступа к вашему компьютеру.

4. Готово! Теперь осталось проверить, что ключи действительно сгенерировались. Для этого вызовите эту команду.

```ls -a ~/.ssh ```

На экране должны появиться два файла — один с расширением .pub, другой — без. Файл в .pub — публичный, им можно делиться с веб-сайтами или коллегами. Файл без расширения .pub — приватный. Ни в коем случае не передавайте его никому! 

# Привязываем SSH-ключ к GitHub
В прошлом уроке вы сгенерировали SSH-ключ, но он пока не привязан к аккаунту на GitHub. Исправим это.
## Инструкция по связыванию SSH-ключа и GitHub-аккаунта
1. После выполнения команды __ssh-keygen__ из предыдущего урока в директории __~/.ssh__ будет создано два файла — __id_ed25519__ и __id_ed25519.pub__ (или __id_rsa__ и __id_rsa.pub__ — в зависимости от того, какой алгоритм вы использовали):

**id_ed25519/id_rsa** — приватный ключ (файл без .pub в конце). Ни в коем случае не копируйте его и не делитесь им.

**id_ed25519.pub/id_rsa.pub** — публичный ключ (на это указывает расширение .pub).
Скопируйте содержимое файла с публичным ключом в буфер обмена.
### macOS
```
# скопировать содержимое ключа в буфер обмена:
$ pbcopy < ~/.ssh/id_rsa.pub
# для ed25519:
$ pbcopy < ~/.ssh/id_ed25519.pub 
```
Здесь используется команда __pbcopy__ — она копирует поток данных в буфер обмена. Запись __pbcopy < ~/.ssh/id_rsa.pub__ означает: «Скопируй в буфер обмена всё содержимое файла __~/.ssh/id_rsa.pub__».
В качестве альтернативы вы можете распечатать файл на экран с помощью __cat ~/.ssh/id_rsa.pub__ и скопировать его вручную.
### Windows
```
# скопировать содержимое ключа в буфер обмена:
$ clip < ~/.ssh/id_rsa.pub
# для ed25519:
$ clip < ~/.ssh/id_ed25519.pub 
```
Если __clip__ не сработает, выведите содержимое файла с помощью __cat ~/.ssh/id_rsa.pub__ или __cat ~/.ssh/id_ed25519.pub__ и скопируйте вывод в буфер обмена из консоли.

2. Перейдите на GitHub и выберите пункт Settings (англ. «настройки») в меню аккаунта.


3. В меню слева нажмите на пункт SSH and GPG keys.

4. В открывшейся вкладке выберите New SSH key (англ. «новый SSH-ключ»).

5. В поле **Title** (англ. «заголовок») напишите название ключа. Например, **Personal key** (англ. «личный ключ»).

6. В поле **Key type** (англ. «тип ключа») должно быть **Authentication Key** (англ. «ключ аутентификации»).

7. В поле **Key** скопируйте ваш ключ из буфера обмена.

8. Нажмите на кнопку **Add SSH key** (англ. «добавить SSH-ключ»).

9. Проверьте правильность ключа с помощью следующей команды.
```
$ ssh -T git@github.com 
```
Если это первый раз, когда вы используете Git, чтобы поделиться проектом на GitHub, появится похожее предупреждение.

    The authenticity of host 'github.com (140.82.121.4)' can't be established. ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU. This key is not known by any other names. Are you sure you want to continue connecting (yes/no/[fingerprint])? 

Это предупреждение сообщает, что вы никогда не соединялись с сервером GitHub. Поэтому Git не может гарантировать, что сервер является тем, за кого он себя выдаёт.

Для подтверждения подлинности сервер генерирует и публикует ключи SHA256. Вы можете проверить ключи GitHub по [этой ссылке](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints). Если ключ в предупреждении совпадает с тем, что вы видите на сайте, значит, сервер является действительным. Введите yes, чтобы продолжить. Вы увидите приветствие на экране.

    Hi %ВАШ_АККАУНТ%! You've successfully authenticated, but GitHub does not provide shell access. 