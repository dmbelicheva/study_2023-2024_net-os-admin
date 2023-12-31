---
## Front matter
lang: ru-RU
title: Лабораторная работа №13
subtitle: Настройка NFS
author:
  - Беличева Дарья Михайловна
institute:
  - Российский университет дружбы народов, Москва, Россия

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
 - '\makeatletter'
 - '\beamer@ignorenonframefalse'
 - '\makeatother'
---


## Цель работы

Приобрести навыки настройки сервера NFS для удалённого доступа к ресурсам.

## Задание

1. Установить и настроить сервер NFSv4.

2. Подмонтировать удалённый ресурс на клиенте.

3. Подключить каталог с контентом веб-сервера к дереву NFS.

4. Подключить каталог для удалённой работы вашего пользователя к дереву NFS.

5. Написать скрипты для Vagrant, фиксирующие действия по установке и настройке
сервера NFSv4 во внутреннем окружении виртуальных машин server и client. Соответствующим образом внести изменения в Vagrantfile.

# Выполнение лабораторной работы

## Настройка сервера NFSv4

![Установка пакетов](image/1.png){#fig:001 width=70%}

## Настройка сервера NFSv4

На сервере создадим каталог, который предполагается сделать доступным всем пользователям сети (корень дерева NFS):
`mkdir -p /srv/nfs`

## Настройка сервера NFSv4

![Редактирование файла](image/2.png){#fig:001 width=70%}

## Настройка сервера NFSv4

![Настройка межсетевого экрана](image/3.png){#fig:001 width=70%}

## Настройка сервера NFSv4

![Установка пакетов](image/4.png){#fig:001 width=70%}

## Настройка сервера NFSv4

![Просмотр подмонтированных удаленных ресурсов](image/5.png){#fig:001 width=70%}

## Настройка сервера NFSv4

Попробуем на сервере остановить сервис межсетевого экрана:
`systemctl stop firewalld.service`

Затем на клиенте вновь попробуем подключиться к удалённо смонтированному
ресурсу:
`showmount -e server.dmbelicheva.net`

## Настройка сервера NFSv4

![Подключение к удаленно смонтированному ресурсу](image/6.png){#fig:001 width=70%}

## Настройка сервера NFSv4

На сервере запустим сервис межсетевого экрана
`systemctl start firewalld`

## Настройка сервера NFSv4

На сервере посмотрим, какие службы задействованы при удалённом монтировании:

![Задействованные службы при удаленном монтировании по протоколу TCP](image/7.png){#fig:001 width=70%}

## Настройка сервера NFSv4

![Задействованные службы при удаленном монтировании по протоколу UDP](image/8.png){#fig:001 width=70%}

## Настройка сервера NFSv4

![Настройка межсетевого экрана на сервере](image/9.png){#fig:001 width=70%}

## Настройка сервера NFSv4

![Подключение к удаленно смонтированному ресурсу](image/10.png){#fig:001 width=70%}

## Монтирование NFS на клиенте

![Монтирование NFS на клиенте](image/11.png){#fig:001 width=70%}

## Монтирование NFS на клиенте

![Редактирование файла](image/12.png){#fig:001 width=70%}

## Монтирование NFS на клиенте

![Проверка наличия автоматического монтирования удалённых ресурсов](image/13.png){#fig:001 width=70%}

## Монтирование NFS на клиенте

Перезапустим клиент и убедимся, что удалённый ресурс подключается автоматически.

![Проверка](image/14.png){#fig:001 width=70%}

## Подключение каталогов к дереву NFS

На сервере создадим общий каталог, в который затем будет подмонтирован каталог
с контентом веб-сервера:
`mkdir -p /srv/nfs/www`

Подмонтируем каталог web-сервера:
`mount -o bind /var/www/ /srv/nfs/www/`

## Подключение каталогов к дереву NFS

На сервере проверим, что отображается в каталоге /srv/nfs.

![Содержимое каталога](image/15.png){#fig:001 width=70%}

## Подключение каталогов к дереву NFS

На клиенте посмотрим, что отображается в каталоге /mnt/nfs.

![Содержимое каталога](image/16.png){#fig:001 width=70%}

## Подключение каталогов к дереву NFS

![Редактирование файла](image/17.png){#fig:001 width=70%}

## Подключение каталогов к дереву NFS

Экспортируем все каталоги, упомянутые в файле /etc/exports:
`exportfs -r`
Проверим на клиенте каталог /mnt/nfs.

![Содержимое каталога](image/18.png){#fig:001 width=60%}

## Подключение каталогов к дереву NFS

![Редактирование файла](image/19.png){#fig:001 width=70%}

## Подключение каталогов к дереву NFS

Повторно экспортируем каталоги, указанные в файле /etc/exports:
`exportfs -r`

На клиенте проверим каталог /mnt/nfs.

![Содержимое каталога](image/20.png){#fig:001 width=60%}


## Подключение каталогов для работы пользователей

```
mkdir -p -m 700 ~/common
cd ~/common
touch dmbelicheva@server.txt
```

## Подключение каталогов для работы пользователей

![Подключение каталогов для работы пользователей](image/21.png){#fig:001 width=70%}

## Подключение каталогов для работы пользователей

![Редактирование файла](image/22.png){#fig:001 width=70%}

## Подключение каталогов для работы пользователей

Внесем изменения в файл /etc/fstab (вместо user укажите свой логин):
`/home/user/common /srv/nfs/home/user none bind 0 0`

Повторно экспортируем каталоги:
`exportfs -r`

## Подключение каталогов для работы пользователей

На клиенте проверим каталог /mnt/nfs.

![Проверка содержимого каталога](image/23.png){#fig:001 width=70%}

## Подключение каталогов для работы пользователей

![Создание файла](image/28.png){#fig:001 width=70%}


## Внесение изменений в настройки внутреннего окружения виртуальных машин

```
cd /vagrant/provision/server
mkdir -p /vagrant/provision/server/nfs/etc
cp -R /etc/exports /vagrant/provision/server/nfs/etc/
```

## Внесение изменений в настройки внутреннего окружения виртуальных машин

В каталоге /vagrant/provision/server создадим исполняемый файл nfs.sh:

```
cd /vagrant/provision/server
touch nfs.sh
chmod +x nfs.sh
```

## Внесение изменений в настройки внутреннего окружения виртуальных машин

![Редактирование файла](image/29.png){#fig:001 width=70%}

## Внесение изменений в настройки внутреннего окружения виртуальных машин

В каталоге /vagrant/provision/client создадим исполняемый файл nfs.sh:

```
cd /vagrant/provision/client
touch nfs.sh
chmod +x nfs.sh
```

## Внесение изменений в настройки внутреннего окружения виртуальных машин

![Редактирование файла](image/30.png){#fig:001 width=70%}

## Внесение изменений в настройки внутреннего окружения виртуальных машин

```
server.vm.provision "server nfs",
  type: "shell",
  preserve_order: true,
  path: "provision/server/nfs.sh"
```

```
client.vm.provision "client nfs",
  type: "shell",
  preserve_order: true,
  path: "provision/client/nfs.sh"
```

## Выводы

В процессе выполнения данной лабораторной работы я приобрела навыки настройки сервера NFS для удалённого доступа к ресурсам.