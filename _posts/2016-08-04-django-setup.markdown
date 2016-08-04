---
layout: post
title: Установка Django на Debian-based дистрибутив
date: 2016-08-04 21:45 +0300
categories: it django howto
excerpt_separator: <!--more-->
---

Небольшой how-to для себя по поводу базовой установки Django. Пояснений особых нет, за всеми подробностями welcome в официальную  документацию. Тут просто последовательные шаги, чтобы каждый раз их не вспоминать.

Первым делом проверить наличие третьей версии Python. Пробуем найти его установки:
 
{% highlight shell linenos %}
~ $ which python3
/usr/bin/python3
{% endhighlight %}

Если не нашли, то ставим:

{% highlight shell linenos %}
 ~ $ sudo apt-get install python3
{% endhighlight %}

Для более удобной работы с пакетами для Python ставим pip:

{% highlight shell linenos %}
 ~ $ sudo apt-get install python-pip
{% endhighlight %}

Для работы с виртуальным окружением ставим virtualenv при помощи pip:

{% highlight shell linenos %}
 ~ $ sudo pip install virtualenv
{% endhighlight %}

Теперь для каждого своего проекта можно поддерживать отдельный набор зависимостей. 

Создадим папку для нашего проекта:

{% highlight shell linenos %}
~ $ mkdir django_test
~ $ cd ./django_test/
{% endhighlight %}

Создадим среду c Python третьей версии:

{% highlight shell linenos %}
 ~/django_test $ virtualenv -p python3 test
{% endhighlight %}

Появилась папка test, где и хранится виртуальное окружение. Теперь запустим его:

{% highlight shell linenos %}
~/django_test $ source ./test/bin/activate
{% endhighlight %}

Устанавливаем новейшую версию Django:

{% highlight shell linenos %}
~/django_test $ pip install django
{% endhighlight %}

Проверим, как прошла установка. Запустим консоль Python и там проверим версию Django:

{% highlight shell linenos %}
 ~/django_test $ python3
Python 3.4.3 (default, Oct 14 2015, 20:28:29) 
[GCC 4.8.4] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import django
>>> print(django.get_version())
1.10
>>> 
{% endhighlight %}

Переходим непосредственно к работе с фреимворком. Создаём новый проект:

{% highlight shell linenos %}
~/django_test $ django-admin startproject mysite
{% endhighlight %}

Получим базовую для Django структуру файлов:
{% highlight shell %}
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
{% endhighlight %}

Django имеет встроенный сервер для разработки, который позволяет проверить результат нашей работы. Зайдём в папку mysite и запустим его:

{% highlight shell linenos %}
~/django_test/mysite $ python manage.py runserver
Performing system checks...

System check identified no issues (0 silenced).

You have 13 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.

August 04, 2016 - 19:26:57
Django version 1.10, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
{% endhighlight %}

Собственно, вбив адрес <http://127.0.0.1:8000/> в браузер, увидим приветственную страницу:

![Приветственная страница](/images/django/django_start.png "Приветственная страница") 
*Приветственная страница*

При запуске сервер сообщил нам, что в приложении имеются изменения, не применённые к базе данных (по умолчанию в Django используется компактный sqlite). Применим их при помощи команды migrate:

{% highlight shell linenos %}
~/django_test/mysite $ python manage.py migrate
{% endhighlight %}

База, которую изначально требует создать django, используется для служебных целей, главным образом для того, чтобы можно было пользоваться встроенной админкой. Так же, чтобы воспользоваться ей, нужно создать суперпользователя:

{% highlight shell linenos %}
~/django_test/mysite $ python manage.py createsuperuser
{% endhighlight %}

Задаём нужные логин/пароль, перезапускаем сервер, и по адресу <http://127.0.0.1:8000/admin/> находим окно логина в админскую часть. Выполняем вход и видим следующее:

![Администратор](/images/django/django_admin.png "Администратор") 
*Администратор*

Это наша панель для администрирования. Можем добавить/удалить/настроить пользователей. Таким образом, мы произвели установку Django и создали базовый костяк проекта, с которым дальше можно работать по своему усмотрению.