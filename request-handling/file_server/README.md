

# Работа с менеджером урлов

## Задание

Необходимо в менеджере урлов `app/urls.py`
реализовать схему отображения для трех ситуаций:

* отображение списка файлов
* отображение списка файлов с фильтрацией по дате `/2018-01-01/`
* отображение отдельных файлов `/file_name.txt`

Для первых двух ситуаций использовать один класс для отображения: `app.views.FileList`
Для второй ситуации реализовать конвертер для преобразования даты
`datetime.date` в указанный в задании вид и наоборот.
Для третьей ситуации использовать функцию для отображения: `app.views.file_content`

В классе `app.views.FileList` и функции `app.views.file_content`
реализовать логику для формирования контекста.
Директорию, файлы которой нужно отображать, берите из настроек `settings.FILES_PATH`

Для начала ваша задача грамотно привязать вьюхи из `app/views.py`
в схему урлов в `app/urls.py`.
В комментариях написаны названия каждой привязки. Их важно сохранить
- по ним будут строиться ссылки в отображаемых документах.

Помните, если вьюха принимает аргумент, то одноименных агрумент должен быть прописан в пути:

```python
def view_func(request, arg1):
    return render(request, 'index.html', context={'arg1': arg1})


urlpatterns = [
    path('', view_func),  # так не срабоатет
    path('<arg1>/', view_func),  # а так сработает
    path('path/<int:arg1>/', view_func),  # и так тоже сработает
]
```

Впрочем для аргумента может быть задано значение по-умолчанию:

```python
def view_func(request, arg1=None):
    return render(request, 'index.html', context={'arg1': arg1})


urlpatterns = [
    path('', view_func),  # В таком случае этот вариант сработает и в arg1 будет None
    path('<arg1>/', view_func),  # а в таком случае в arg1 будет приходить строка указанная в пути
]
```

Для классов принимаемые аргументы прописываются в методе `get_context_data`.
а привязка класса происходит с помощью метода `as_view()`:


```python
class ViewClass(TemplateView):
    template_name = 'index.html'
    def get_context_data(self, arg1=None):
        return {'arg1': arg1}


urlpatterns = [
    path('', ViewClass.as_view())
]
```

![Пример результата](./res/result.gif)

## Вспомогательная информация

Используйте:

* [os.listdir](https://docs.python.org/3/library/os.html#os.listdir) для получения списка файлов
* [os.stat](https://docs.python.org/3/library/os.html#os.stat) для получения данных по файлам

## Документация по проекту

Для запуска проекта необходимо:

Установить зависимости:

```bash
pip install -r requirements.txt
```

Создать файл с локальными настройками `app/settings_local.py`
и задать туда обязательные параметры:

* SECRET_KEY - секретная строка
* DEBUG = True

Например:

```python
SECRET_KEY = 'd+mw&mscg5i&tx+#@bf+6m%e+d5z!u#!n%z-^o9u7y1felv2o&'
DEBUG = True
```

Выполнить команду:

```bash
python manage.py runserver
```