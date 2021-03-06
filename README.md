# [Как правильно готовить вместе с Chef](http://foodtaster.github.io/dev-highload-2013/)

Поднимите руки те кто является разработчиками! Кто знает, что такое
девопс? Кто когда-либо работал с Chef или любой другой системой
управления конфигурацией.

### N+1 изменений

Поднимите руки те кто за последнюю неделю поднимал приложение на своем новом макбуке или для
нового разработчика!

Те кто вносил правки в инфраструктуру на нескольких машинах!

Хотелось ли вам автоматизировать эту работу?

### Трамвайно-зависимаю инфраструктура

Кому знакома ситуация - когда то что происходит на сервере известно лишь условному Пете
или что еще хуже, на сервер пустили нескольких условных Петь и теперь что там происходит
не изветстно никому?

Кто пыталься заставить админов писать документацию?

### Infrastructure as a Code

Кто слышал о таком слогане?
Кто знает что означает модное слово DevOps?
Кто знаком с Chef или его аналогами (Puppet, Sold etc)?

### Что такое chef?

Это система управления конфигурацией.
Как она работает:

В терминологии chef конфигурируемая машина это нода. На ноде запусается клентский скрипт (chef-client),
который загружает с сервера (или полагает что вы уже загрузили руками в случае chef-solo) конфигурационные параметры
ноды - т.н. аттрибуты (в виде json файла).
После этого chef-client собирает информацию о ноде (кол-во cpu, объем памяти и т.п.) и записывает ее туда же (в аттрибуты ноды).

У ноды есть специальный аттрибут - run list - где перечислены рецепты, по которым нужно ноду приготовить.
Chef-client считывает эти аттрибуты и загружает с сервера соответсвующин кукбуки.

*Кукбуки* это основные единицы дистрибуции конфигурационных скриптов,
содержащие специальные скрипты - называемые рецептами,
которые умеют сделать что-нибудь полезное с машиной, например поставить postgresql или nginx.

Множество кукбуков на разные случаи можно найти на github,
есть также официальный репозиторий chef кукбуков на сайте [http://community.opscode.com/].

Потом эти рецепты выполняются, считывая аттрибуты они определенным образом настраивают машину -
ставят пакеты, конфигурируют и запускают сервисы и т.д. Во время выполнения они могут обратиться на сервер
за датабагами (databag - где может храниться общая конфигурационная информация). Также они могут сделать поиск
по аттрибутам других нод, поскольку после прогона chef-client все аттрибуты ноды сохраняются на сервер.


## Звучит прекрасно, но как начать?

Для того чтобы начать упражняться с chef удобно использовать виртуализацию.
Подняли виртуальную машинку, поигрались - испортили, взяли другую.

Для удобной автоматизированной работы с различными системами виртуализации, существует
утилита [Vagrant](http://www.vagrantup.com/). Она позволяет положить в корень проекта специальный
файл-манифест - [Vagrantfile](http://docs-v1.vagrantup.com/v1/docs/vagrantfile.html),
в котором описано - какие виртуальные машины нужны для этого проекта,
где их скачать (box), как настроить сеть и сколько системных ресурсов выделить.

В vagrant есть понятие box - заготовленный образ виртуальной машины, доступный по определенному url [http://www.vagrantbox.es/].

А еще vagrang поддерживает provisioning виртуальных машин с помощью chef, chef-solo, puppet etc.

Vagrant хорошая точка для того чтобы начать с chef.
