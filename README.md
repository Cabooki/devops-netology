# Домашнее задание к занятию «2.4. Инструменты Git»

##1. Найдите полный хеш и комментарий коммита, хеш которого начинается на aefea.

`git show --pretty=format:"%H **%s**" aefea`

aefead2207ef7e2aa5dc81a34aedf0cad4c32545 **Update CHANGELOG.md**

##2. Какому тегу соответствует коммит 85024d3?

`git show  85024d3`

tag: v0.12.23

##3. Сколько родителей у коммита b8d720? Напишите их хеши.

терминал только в кавычках нормально воспринимает "^", поэтому команды такие:

`git show "b8d720^1"`

56cd7859e05c36c06b56d013b55a252d0bb7e158 **Merge pull request #23857 from hashicorp/cgriggs01-stable**

`git show "b8d720^2"`

9ea88f22fc6269854151c571162c5bcf958bee2b **add/update community provider listings**

**Итого два родителя.**



##4. Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами v0.12.23 и v0.12.24.

`git log --pretty=format:"%H **%s**" "v0.12.23"..."v0.12.24"`

33ff1c03bb960b332be3af2e333462dde88b279e **v0.12.24**

b14b74c4939dcab573326f4e3ee2a62e23e12f89 **[Website] vmc provider links**

3f235065b9347a758efadc92295b540ee0a5e26e **Update CHANGELOG.md**

6ae64e247b332925b872447e9ce869657281c2bf **registry: Fix panic when server is unreachable**

5c619ca1baf2e21a155fcdb4c264cc9e24a2a353 **website: Remove links to the getting started guide's old location**

06275647e2b53d97d4f0a19a0fec11f6d69820b5 **Update CHANGELOG.md**

d5f9411f5108260320064349b757f55c09bc4b80 **command: Fix bug when using terraform login on Windows**

4b6d06cc5dcb78af637bbb19c198faff37a066ed **Update CHANGELOG.md**

dd01a35078f040ca984cdd349f18d0b67e486c35 **Update CHANGELOG.md**

225466bc3e5f35baa5d07197bbc079345b77525e **Cleanup after v0.12.23 release**




##5. Найдите коммит в котором была создана функция func providerSource, ее определение в коде выглядит так func providerSource(...) (вместо троеточего перечислены аргументы).

Сначала найти файл

`git grep "func providerSource("`

*provider_source.go:func providerSource(configs []\*cliconfig.ProviderInstallation, services \*disco.Disco) (getproviders.Source, tfdiags.Diagnostics) {*

Затем найти коммит с объявлением функции

`git log -L :providerSource:provider_source.go`

Последний в списке коммит - 8c928e83589d90a031f811fae52a81be7153e82f

В нем в изменениях добавление указанной функции.

Можно проверить с помощью

`git show 8c928e83589d90a031f811fae52a81be7153e82f`

##6. Найдите все коммиты в которых была изменена функция globalPluginDirs.

Сначала найти файл с объявлением функции

`git grep "func globalPluginDirs"`

Результат - plugins.go

Затем найти коммиты с указанной функцией в изменениях

`git log -L:globalPluginDirs:plugins.go`

В результате список коммитов такой:

78b12205587fe839f10d946ea3fdc06719decb05 **Remove config.go and update things using its aliases**

52dbf94834cb970b510f2fba853a5b49ad9b1a46 **keep .terraform.d/plugins for discovery**

41ab0aef7a0fe030e84018973a64135b11abcd70 **Add missing OS_ARCH dir to global plugin paths**

66ebff90cdfaa6938f26f908c7ebad8d547fea17 **move some more plugin search path logic to command**

8364383c359a6b738a436d1b7745ccdce178df47 **Push plugin discovery down into command package**



##7. Кто автор функции synchronizedWriters?

`git grep "synchronizedWriters"`

Данный запрос не выдает результатов...

Насколько понимаю, такой функции нет в репозитории?

Или я что-то делаю не так?