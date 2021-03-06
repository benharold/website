---
id: version-3.8.6-authentification
title: Authentification
original_id: authentification
---

Аутентификација је везана за auth [plugin](plugins.md) који користите. Рестрикције које се односе на пакет се могу подесити (handle) преко [Package Access](packages.md).

Аутентификација клијента се обавља (handle) преко самог `npm` клијента. Након што се пријавите у апликацију:

```bash
npm adduser --registry http://localhost:4873
```

Токен се генерише у фајлу за конфигурацију `npm` који се налази у home фолдеру корисника. Како бисте сазнали више о `.npmrc` прочитајте [official documentation](https://docs.npmjs.com/files/npmrc).

```bash
cat .npmrc
registry=http://localhost:5555/
//localhost:5555/:_authToken="secretVerdaccioToken"
//registry.npmjs.org/:_authToken=secretNpmjsToken
```

#### Анонимно публиковање

`verdaccio` Вам омогућава да пружите могућност анонимног публиковања. Како бисте успели у томе, потребно је да подесите [packages access](packages.md).

Пример:

```yaml
  'my-company-*':
    access: $anonymous
    publish: $anonymous
    proxy: npmjs
```

Као што је описано, [on issue #212](https://github.com/verdaccio/verdaccio/issues/212#issuecomment-308578500) све док `npm@5.3.0` и све верзије не буду усаглашене **неће Вам бити омогућено да публикујете без токена**. Ипак, `yarn` нема таква ограничења.

## Подразумевана htpasswd

Како би се поједноставио setup, `verdaccio` користи plugin базиран на `htpasswd`. Од верзије v3.0.x [екстерни plugin](https://github.com/verdaccio/verdaccio-htpasswd) се користи као подразумеван. Верзија v2.x и даље садржи уграђену верзију овог plugin-а.

```yaml
auth:
  htpasswd:
    file: ./htpasswd
    # Максимални број корисника који могу да се региструју, фабрички је подешено на бесконачно "+inf".
    # Ако хоћете да онемогућите регистрацију, подесите ово на -1.
    #max_users: 1000
```

| Својство  | Тип    | Неопходно | Пример     | Подршка | Опис                                   |
| --------- | ------ | --------- | ---------- | ------- | -------------------------------------- |
| file      | string | Да        | ./htpasswd | all     | фајл који садржи шифроване credentials |
| max_users | number | Не        | 1000       | all     | подешава максимални број корисника     |

Ако се одлучите на то да не дозволите корисницима да се пријаве, можете подесити `max_users: -1`.