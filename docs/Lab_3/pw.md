# Задача

Овладеть практическими навыками реализации серверной части (backend) приложений средствами
Django REST framework.

1) получить представление о работе с запросами в Django ORM.
2) получить представление об использовании возможностей работы контроллеров и
   сериализаторов в Django Rest Framework.
3) овладеть навыками написания документации к API.

# Ход работы

## Задача 1

```python
'Вывод даты выдачи самого старшего водительского удостоверения'
>>> from django.db.models import Min, Max
>>> Drivers_license.objects.aggregate(Min('date_issued'))
{'date_issued__min': datetime.date(2011, 11, 11)}
```

```python
'Укажите самую позднюю дату владения машиной, имеющую какую-то из существующих моделей в вашей базе'
def get_last_owned_date(car):
    latest_dt_info = (
        Owns.objects.filter(car_id=car)
        .values('car_id')
        .annotate(latest_dt_from_owned=Max('begin_date'))
        .values('latest_dt_from_owned')
    )
    return latest_dt_info

>>> given_car = Car.objects.filter(model='bolt')
>>> for i in given_car:
    print(get_last_owned_date(i))

< QuerySet[{'latest_dt_from_owned': datetime.date(2024, 11, 11)}]>
< QuerySet[{'latest_dt_from_owned': datetime.date(2021, 11, 11)}]>
```

```python
'Выведите количество машин для каждого водителя'
>> from django.db.models import Min, Max, Count
>> Owns.objects.filter(end_date__isnull=True)
.values('owner_id')
.annotate(Count('car_id'))
< QuerySet[{'owner_id': 1, 'car_id__count': 1}, {'owner_id': 2, 'car_id__count': 2}] >
```

```python
'Подсчитайте количество машин каждой марки'
>> Car.objects.values('brand').annotate(Count('id'))
< QuerySet[{'brand': 'ford', 'id__count': 3}, {'brand': 'tesla', 'id__count': 1}, {
    'brand': 'torvalds', 'id__count': 1}, {'brand': 'valve', 'id__count': 1}] >
```

```python
'Отсортируйте всех автовладельцев по дате выдачи удостоверения'
>> Owner.objects.filter(id__in=Drivers_license.objects.values_list('owner', flat=True)
                          .distinct()
                          ).order_by('drivers_license__date_issued')
< QuerySet[ <Owner: Ownerobject(5)>, <Owner: Owner object(2)>, <Owner: Owner object(1)>] >
```

## Задача 2

Я реализовал весь требуемый функционал при помощи средств django-rest-api. Встроенные 
модули позволили быстро объявить и настроить проект. Я использовал возможности generic 
модулей для инициализации пользовательского взаимодействия с API

## Задача 3

Так как мы уже знакомы с форматированием md файлов и работай с mcdocs это не вызвало 
проблем. Для имплементации swagger я последовал инструкциям из практики и сразу 
получил результат. 

# Выводы

Я научился работать с djangoORM, создавать простые api при помощи django-rest-api и 
документировать их при помощи swagger