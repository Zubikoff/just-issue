# Инструкция к использованию коллекции Postman
## Описание коллекции
Данная коллекция предназначена для работы с GitHub Issues.

Коллекция имеет общую для всех запросов авторизацию типа Bearer Token. По-умолчанию API-ключ авторизации содержится в переменной коллекции.

Коллекция включает в себя следующие переменные:
- `Token` - API-ключ для авторизации. Без него доступ к репозиторию будет **закрыт**
- `owner` - владелец репозитория. Значение по-умолчанию - "Zubikoff"
- `repo` - название репозитория. Значение по-умолчанию - "just-issue"
- `issueNum` - содержит в себе номер issue для последующей работы с ней. По-умолчанию, перезаписывается каждый раз, когда исполняется запрос на создание новой issue

В заголовках запросов не содержится обязательных полей.
Коллекция включает в себя 4 запроса:
- Add new issue - POST-запрос на добавление новой issue в репозиторий.  
  URS запроса: https://api.github.com/repos/OWNER/REPO/issues.
  По-умолчанию OWNER и REPO уже заняты переменными `owner` и `repo` соответственно.
    
  Запрос содержит следующие элементы в Body:  
  `title` - название создаваемой issue. **Это обязательное поле**  
  `body` - содержимое новой issue.  
  `assignee` - пользователи, подписанные на новую issue. По-умолчанию соджержит переменную `owner`  
  `labels` - метки на новой issue.
    
  Запрос также содержит следующие автотесты:  
  Проверка статус-кода ответа - по умолчанию ожидается код 201.  
  Проверка содержимого тела ответа - по-умолчанию ищется текст "Issue 1".

  Также в данном запросе поле "number" из тела ответа записывается в переменную `issueNum`
- Get issue list - GET-запрос на получение списка issue репозитория
  URL запроса: https://api.github.com/repos/OWNER/REPO/issues.
  По-умолчанию OWNER и REPO уже заняты переменными `owner` и `repo` соответственно.

  Запрос также содержит следующие автотесты:  
  Проверка статус-кода ответа - по умолчанию ожидается код 200.  
  Проверка содержимого тела ответа #1 - по-умолчанию ищется наличие в ответе массива объектов JSON.  
  Проверка содержимого тела ответа #2 - по-умолчанию ищется наличие в ответе значения, совпадающего со значением переменной `issueNum`
- Rename issue - PATCH-запрос на изменение уже существующей issue.  
  URL запроса: https://api.github.com/repos/OWNER/REPO/issues/ISSUE_NUMBER.
  По-умолчанию OWNER и REPO уже заняты переменными `owner` и `repo` соответственно, а ISSUE_NUMBER - переменной `issueNum`.

  Запрос содержит следующие элементы в Body:  
  `title` - новое название issue.

  Запрос также содержит следующие автотесты:  
  Проверка статус-кода ответа - по умолчанию ожидается код 200.  
  Проверка содержимого тела ответа - по-умолчанию ищется текст "Issue 2".
- Lock issue - PUT-запрос на закрытие issue для комментирования.  
  URL запроса: https://api.github.com/repos/OWNER/REPO/issues/ISSUE_NUMBER/lock.
  По-умолчанию OWNER и REPO уже заняты переменными `owner` и `repo` соответственно, а ISSUE_NUMBER - переменной `issueNum`.

  Запрос содержит следующие элементы в Body:  
  `lock_reason` - причина закрытия issue для комментирования. По-умолчанию содержит "resolved" **Это обязательное поле**

  Запрос также содержит следующие автотесты:  
  Проверка статус-кода ответа - по умолчанию ожидается код 204.

## Работа с коллекцией
Для работы с коллекцией необходимо заполнить переменные коллекции `Token`, `owner` и `repo`.  
Для заполнения переменной `owner` необходимо создать свой аккаунт на GitHub. Инструкцию, как это сделать, можно получить здесь: [Signing up for a new GitHub account](https://docs.github.com/en/get-started/signing-up-for-github/signing-up-for-a-new-github-account)

Далее необходимо создать свой репозиторий и поместить его название в переменную `repo`. Как создать репозиторий: [Create a repository](https://docs.github.com/en/get-started/quickstart/create-a-repo#create-a-repository)

И последнее - создать Personal Access Token и заполнить им переменную `Token`. Как создать Personal Access Token: [Creating a fine-grained personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-fine-grained-personal-access-token)  
Альтернативный способ создания токена: [Creating a personal access token (classic)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic)

После этого можно запускать запросы.  
**ВАЖНО:** для корректной автоматической работы запросов *Rename issue* и *Lock issue* необходимо сначала как минимум один раз запустить запрос *Add new issue*. Альтернатива - самостоятельно заполните переменную `issueNum` номером существующей Issue в вашем репозитории.
