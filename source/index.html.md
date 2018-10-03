---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

includes:
  - errors

search: true
---

# Introduction

This API service work with Google Sheets to fetch schedule of groups and KPFU site to fetch student academic performance.

# Groups

## Get groups and filters

```shell
curl "https://itis-service.herokuapp.com/groups"
```

> The above command returns JSON structured like this:

```json
"name": "11-701",
"filterParams": [
    {
        "filter": "electiveCourse",
        "name": "Курс по выбору",
        "values": [
            {
                "name": "Ryby--Bazanov-VA",
                "title": "Ryby- Бажанов В.А."
            }
        ]
    }
]
```

This endpoint retrieves all groups and filters with elective courses.

### HTTP Request

`GET https://itis-service.herokuapp.com/groups`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------

# Schedule

## Get schedule of group

```shell
curl "https://itis-service.herokuapp.com/v1/schedule/11-604?electiveCourse=Analiz-dannyh-lekcia-gr2-Novikov-PA&even=true"
```

> The above command returns JSON structured like this:

```json
{
    "even": false,
    "days": [
        {
            "number": 1,
            "lessons": [
                {
                    "dateTimeStart": 1538976600000,
                    "dateTimeEnd": 1538982000000,
                    "name": "Безопасность жизнедеятельности( 4 н.)  в ауд.109 к.2 ( с 5-10 недели практика МисбаховА.А. у гр.11-606 и 11-607 в понд.8.30 в 1309)",
                    "teacher": "Мустаев Р.Ш.",
                    "place": null
                },
            ]
        }
}
```

This endpoint fetch schedule of selected group with elective course and week type

### HTTP Request

`GET https://itis-service.herokuapp.com/v1/schedule/{group}`

### Query Parameters

Parameter  | Description
--------- | ------- 
group | Number of group
electiveCourse | Picked elective course of student
even | Even or odd week

## Get schedule of group (deprecated)

```shell
curl "https://itis-service.herokuapp.com/schedule/11-604?electiveCourse=Analiz-dannyh-lekcia-gr2-Novikov-PA"
```

> The above command returns JSON structured like this:

```json
{
    "timeslots": [
        {
            "dateTimeStart": "2018-10-08T08:30:00+03:00",
            "dateTimeEnd": "2018-10-08T10:00:00+03:00",
            "start": 1538976600000,
            "end": 1538982000000,
            "lessons": [
                {
                    "title": "Безопасность жизнедеятельности( 4 н.) Мустаев Р.Ш. в ауд.109 к.2 ( с 5-10 недели практика МисбаховА.А. у гр.11-606 и 11-607 в понд.8.30 в 1309)",
                    "periodicity": null,
                    "periodicityEnum": null,
                    "room": null,
                    "teacher": null,
                    "lesson": null,
                    "typeEnum": null,
                    "type": null,
                    "parsed": false
                }
            ]
        }
    ]
}
```

This endpoint fetch schedule of selected group with elective course

<aside class="warning">This endpoint use only on old versions applications</aside>

### HTTP Request

`GET https://itis-service.herokuapp.com/schedule/{group}`

### URL Parameters

Parameter  | Description
--------- | ------- 
group | Number of group
electiveCourse | Picked elective couse of student

# Exams

## Authorization

```shell
curl --header "Content-Type: application/json" \
    --request POST \
    --data '{"login":"StudentLogin","password":"StudentPassword"}' \
    https://itis-service.herokuapp.com/exams/login
```

> The above command returns JSON structured like this:

```json
{
    "apiKeyP2": "7988070231179191581700197127199",
    "apiKeyPh": "725E069BEED668A4100EEE10BB76E27A"
}
```

This endpoint autorize on kpfu web site to get access to exams

### HTTP Request

`POST https://itis-service.herokuapp.com/exams/login`

### Query Parameters

Parameter  | Description
--------- | ------- 
login | Login from kpfu.ru
password | Password from kpfu.ru

## Get exams

```shell
curl https://itis-service.herokuapp.com/exams/fetch?apiKeyP2=7988819564634323425101601573026&apiKeyPh=FF51187FDB56CE15EE9EDCBD482A898C
```

> The above command returns JSON structured like this:

```json
[
    {
        "averageRating": 69.14,
        "number": 1,
        "exams": [
            {
                "name": "Алгебра и геометрия",
                "type": "EXAM",
                "summaryMark": 66,
                "practiseMark": 36,
                "examMark": 30
            }
        ]
    }
]
```

This endpoint to fetch results of exams

### HTTP Request

`GET https://itis-service.herokuapp.com/exams/fetch`

### Query Parameters

Parameter  | Description
--------- | ------- 
apiKeyP2 | apiKey from authorization endpoint
apiKeyPh | apiKey from authorization endpoint
