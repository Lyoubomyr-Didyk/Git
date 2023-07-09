## SSH key
Github рекомендует выбирать не SSH, а HTTPS подключение, обеспечивая, таким образом, универсальный доступ к репозитариям, минуя файрволлы. Настроить соединение по HTTPS не составляет труда - достаточно знать логин и пароль пользователя Github. Плохая новость заключается в том, что, имея HTTPS-подключение к удаленному репозитарию, при каждом выполнении команд git pull, git clone, git push или git fetch Вам потребуется вводить логин и пароль. Конечно, одним из вариантов решения такой проблемы будет кеширование учетных данных на соответствующем сервере или компьютере. Но данные, сохраненные таким образом, будут существовать ограниченный период времени.

Итак, я рекомендую использовать SSH-подключение, так как по моему мнению оно является более быстрым, удобным и безопасным:

- быстрым, потому что не потребуется вводить логин и пароль при каждом обмене данными,
- удобным, потому что, настроив один раз пару ключей SSH, можно длительное время выполнять работу,
- безопасным, потому что в отношении безопасности SSH выглядит более предпочтительным по сравнению с HTTPS. Например, в случае компрометации учетных данных Вам придется менять доступ к Github аккунту и всем связанным с ним репозитариям. Если в открытый доступ поступят Ваши SSH-ключи, достаточно их просто заменить.

Прежде всего есть смысл проверить, не сгенерировали ли вы свой ключ раньше и просто забыли об этом.
```
$ ls -al ~/.ssh
```
Если ключи есть, то мы увидим, как минимум, 4 строки текста
```
drwx------ 2 su su 4096 сеп 24 23:24 .
drwxr-xr-x 23 su su 4096 сеп 24 23:24 ..
-rw------- 1 su su 419 сеп 24 23:24 id_ed25519
-rw-r--r-- 1 su su 104 сеп 24 23:24 id_ed25519.pub
```
Ну, а если нет - то только две верхних. В этом случае, ключи нам придётся сгенерировать самостоятельно.
  
Установка SSH-ключей для git-репозитария не является трудной.
В терминале нужно ввести команду:
```
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
заменив e-mail, приведенный в примере, на актуальный для Вашего проекта.

В результате будет создан новый SSH-ключ с заголовком, соответствующим указанному электронному адресу.

Следующим шагом появится строка “Enter a file in which to save the key”, требующая ввести имя файла для сохранения ключей. Просто нажмите Enter - будут подставлены значения по умолчанию:

Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
Наконец, система попросит Вас ввести ключевое слово. Если Вы не желаете ассоциировать пароль с ключом, просто нажмите Enter еще раз для того, чтобы пропустить этот этап.

```
Your identification has been saved in /home/su/.ssh/id_ed25519
Your public key has been saved in /home/su/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:FOkr66ZIZN5eTZ1omlY77s1GkgZtWa+A3NNDwEKCzcE my_email@example.com
The key's randomart image is:
+--[ED25519 256]--+
| =oo..oo |
| . E.. o.o |
| . *.= . |
| +.Ooo.. |
| o oS=oo |
| + . .B=.o |
| o . =++o |
| . o oo. +. |
| . o+..o.o |
+----[SHA256]-----+
```


Итак, SSH-ключ создан. Теперь нужно запустить SSH-агент
```
eval "$(ssh-agent -s)"
```
и добавить в него ключ:
```
ssh-add ~/.ssh/id_rsa
```
На последнем этапе нужно добавить публичный ключ в свой Github-аккаунт.

Для этого перейдите в настройки своего аккаунта в Github:

  1. кликните по иконке профиля в правом верхнем углу экрана,
  2. выберите пункт Settings,
  3. в меню открывшегося раздела выберите SSH and GPG Keys,
  4. нажмите на зеленую кнопку New SSH key,
  5. введите заголовок SSH ключа,
  6. скопируйте .ssh ключ в соответствующее поле из папки .ssh (или из любой другой папки, в которой Вы сохранили ключ),
  7. нажмите кнопку Add SSH key.

Теперь нужно перейти к работе по SSH: перейдите в соответствующий репозитарий в аккаунте Github, нажмите на зеленую кнопку Clone or download и кликните Use SSH.

https://www.youtube.com/watch?v=OYhPpeufgW8
