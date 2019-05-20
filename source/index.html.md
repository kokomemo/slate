---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - json: Success
  - shell: Error
  - Request
  - raw

toc_footers:
  - <a href='http://kokonutstudio.com/'>Kokonutstudio.</a>

search: true
---

# Soltek Api Reference

Link to source code in GitHub: [Soltek_Backend](https://github.com/KokonutStudioRepository/Soltek_Backend).

### API URL

Variable |  Value
--------------|---------
{{url}} | https://apisoltek.kokonutstudio.com 

<aside class="notice">
Si la respuesta de los servicios son llevadas a cabo satisfactoriamente se devuelve como Success <code>1</code> y si no, se devuelve <code>0</code>.
</aside>

# GENERIC END-POINTS
## Tipo de usuarios
>Response: 

```json
{
    "success": 1,
    "message": null,
    "data": [
        {
            "id": 1,
            "description": "Super Admin"
        },
        {
            "id": 2,
            "description": "Admin Cliente"
        },
        {
            "id": 3,
            "description": "Admin Ind"
        },
        {
            "id": 4,
            "description": "Supervisor Soltek"
        },
        {
            "id": 5,
            "description": "Instalador Soltek"
        },
        {
            "id": 6,
            "description": "Supervisor"
        },
        {
            "id": 7,
            "description": "Instalador"
        },
        {
            "id": 8,
            "description": "Chofer"
        },
        {
            "id": 9,
            "description": "Moderador"
        }
    ]
}
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET | get_type_user STAFF | {{url}}/api/staff/get-type-user 

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body
No requerido

## Login
> Response

```shell
{
    "success": 0,
    "message": "invalid_credentials",
    "data": []
}
```
```json
{
    "success": 1,
    "message": "",
    "data": {
        "token_type": "Bearer",
        "expires_in": 604800,
        "access_token": "xOTFiODEPWeoDy6zilXl_N2o2VGlJCmZFdX1HYaxJB3nGYlcpHNikZ9VzYdHcfgjfV_tQ",
        "refresh_token": "def502007e2aa2ca72848289",
        "user_type": 1
    }
}
```
HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST |login GENERAL | {{url}}/api/login 

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code> en tu API URL.
</aside>

### Body

* Type form-data

Key   | Value | Mandatory
---------|-------------|----------
username | email@example.com  |     1
password | 123qwe1 |   1

## Editar perfil
>Response:

```shell
{
    "success": 0,
    "message": "Hacen falta datos para completar el registro",
    "data": null
}
```

```json
{
    "success": 1,
    "message": "Actualización del usuario correcta",
    "data": null
}
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | update profile | {{url}}/api/staff/update/profile 

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code> en tu API URL.
</aside>

### Headers

Key  | Value 
-----|-----
Accept | application/json
lang | es_mx
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key   | Value | Mandatory
---------|-------------|----------
name | User name  |     0
last_name | User last_name  |     0
email | User email  |     0
password | User password  |     0
password_confirmation | User password_confirmation  |     0
image1 | User image  |     0
type | User type  |     0

## Actualizar Choferes
>Response:

```shell
{
    "success": 0,
    "message": "Error al actualizar el usuario",
    "data": null
}

{
    "success": 0,
    "message": "Hacen falta datos para completar el registro",
    "data": null
}
```

```json
{
    "success": 1,
    "message": "Actualización del usuario correcta",
    "data": null
}
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | update driver | {{url}}/api/staff/update/driver

### Headers

Key  | Value 
-----|-----
Accept | application/json
lang | es_mx
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key   | Value | Mandatory
---------|-------------|----------
name | User name  |     0
last_name | User last_name  |     0
base_id | Base id  |     0
driver_id | Driver number  |     0
fk_company | Company id  |     0

# CMS-ENDPOINTS
## Dashboard
>Response: 

```shell
{
    "success": 0,
    "message": "Error al consultar información",
    "data": null
}
```

```json
{
    "success": 1,
    "message": null,
    "data": {
        "customers": {
            "total": 11,
            "customers_hired": 6,
            "customers_no_hired": 4
        },
        "vehicles": {
            "total": 0,
            "new": 0
        },
        "supervision": {
            "checked": 0,
            "damage": 0
        }
    }
}
```
HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | dashboard ADMIN | {{url}}/api/dashboard 

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code> en tu API URL.
</aside>

### Headers

Key  | Value 
-----|-----
Accept | application/json
lang | es_mx
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key   | Value | Mandatory
---------|-------------|----------
period | 2019-01-01  |     1

## Register Secondary 
>Response:


```shell
{
    "success": 0,
    "message": "Hacen falta datos para completar el registro",
    "data": []
}
```


```shell
{
    "success": 0,
    "message": "No se pudo registrar el usuario",
    "data": []
}
```


```shell
{
    "success": 0,
    "message": "No hace falta información de suscripción para completar el registro",
    "data": []
}
```
```json
{
    "success": 1,
    "message": "Usuario creado correctamente",
    "data": null
}
```
Aquí se registran los usuarios desde CMS. Permite registrar admins cliente, admins ind, instaladores, supervisores, choferes, moderadores. Se puede registrar varios usuarios a la vez.

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST |register secondary user STAFF | {{url}}/api/staff/register

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code>por el API URL.
</aside>

### Headers

Key  | Value 
-----|-----
Accept | application/json
lang | es_mx
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type raw:

[{
 "name": "staff1",

 "last_name": "staff",

 "type": 8,

 "base_id": 1,

 "driver_id":"55634",

 "company_id": 1

}, {
 "name": "staff2",

 "last_name": "staff2",

 "email": "staff@example.com",

 "password": "123123",

 "password_confirmation": "123123",

 "type": 3

}]

## Eliminar Usuarios
>Response:

```shell
{
    "success": 0,
    "message": "Error al eliminar el usuario",
    "data": null
}
```

```json
{
    "success": 1,
    "message": "Usuario eliminado correctamente",
    "data": null
}
``` 
HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
DELETE | disable secondary user STAFF | {{url}}/api/staff/disable/{user_id}

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code>por el API URL.
</aside>

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### URL
Es necesario pasar user_id al final de la URL del endpoint

### Body
No requerido

## Actualizar Usuarios
>Response:

```shell
{
    "success": 0,
    "message": "Error al actualizar el usuario",
    "data": null
}

{
    "success": 0,
    "message": "Hacen falta datos para completar el registro",
    "data": null
}
```

```json
{
    "success": 1,
    "message": "Actualización del usuario correcta",
    "data": null
}
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | update secondary user STAFF | {{url}}/api/update

### Headers

Key  | Value 
-----|-----
Accept | application/json
lang | es_mx
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key   | Value | Mandatory
---------|-------------|----------
name | User name  |     0
last_name | User last_name  |     0
email | User email  |     0
password | User password  |     0
password_confirmation | User password_confirmation  |     0
image1 | User image  |     0
type | User type  |     0
id | User id  |     1

## Mostrar Sólo choferes
>Response:

```shell

```

```json
{
    "success": 1,
    "message": null,
    "data": [
        {
            "name": "Chofer 1",
            "last_name": "Chofer",
            "driver_number": "",
            "status": 1,
            "company": "KokonutStudio",
            "user_id": 8,
            "incidents": 0,
            "status_description": "Active"
        },
        {
            "name": "Chofer 2",
            "last_name": "Chofer",
            "driver_number": "",
            "status": 1,
            "company": "KokonutStudio",
            "user_id": 25,
            "incidents": 0,
            "status_description": "Active"
        }
    ]
}
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET | show drivers STAFF | {{url}}/api/staff/show-drivers

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code>por el API URL.
</aside>

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body
No requerido

## Mostrar usuarios (TODOS)
>Response: 

```json
{
    "success": 1,
    "message": null,
    "data": [
        {
            "name": "Administrador",
            "last_name": "Kokonut",
            "email": "desarrollo@kokonutstudio.com",
            "user_id": "eyJpdiI6IjhQWVBjTUROOGNDaHl...",
            "user_type": "Super Admin",
            "user_type_id": 1,
            "user_status_description": "Desbloqueado",
            "user_status_id": 1
        },
        {
            "name": "Macario",
            "last_name": "Arvizu",
            "email": "memo@gmail.com",
            "user_id": "eyJpdiI6IlFtZ2FXK1NEMGNJQkYy...",
            "user_type": "Super Admin",
            "user_type_id": 1,
            "user_status_description": "Desbloqueado",
            "user_status_id": 1
        }
     ]
  }
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET | show users STAFF | {{url}}/api/staff/show-users

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code>por el API URL.
</aside>

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body
No requerido

## Mostrar vehiculos (TODOS)
>Response:

```json
{
    "success": 1,
    "message": "Consulta de vehículo exitosa",
    "data": [
        {
            "id_vehicle": "eyJpdiI6IjVObDROTTlialdcL3dJSFlWQjFjYk9nPT0iLCJ2YWx1ZSI6ImllUFB3d2YzRWM3R2FZbG1CbTBVcnc9PSIsIm1hYyI6ImJlNDEwZTE1YjRjM2Q4OWJlYjA2MDc2MGJiYjZkNTI4ZDI4YTg1NjE4NDcyNWY2OWVhYmJiNzQ0MzdjOWE4NTYifQ==",
            "serial_number": "645463",
            "economic_number": "86214",
            "brand_vehicle": "INTERNATIONAL",
            "model_vehicle": "SERIE 4000",
            "tanks": 3,
            "created_at": "2019-05-20 18:51:38",
            "updated_at": "2019-05-20 18:51:38"
        }
    ]
}
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET | show vehicicle STAFF | {{url}}/api/staff/vehicle/all

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code>por el API URL.
</aside>

### Headers

Key  | Value 
-----|-----
Accept | application/json
lang | es_mx
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body
No requerido

## Crear vehiculo
>Response: 

```shell
{
    "success": 1,
    "message": "No se pudo crear el vehiculo",
    "data": null
}
```

```json
{
    "success": 1,
    "message": "Vehículo creado correctamente",
    "data": null
}
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | create vehicle STAFF | {{url}}/api/staff/vehicle

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code>por el API URL.
</aside>

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key   | Description | Mandatory
---------|-------------|----------
vehicle_id | Numero de serie  |     1
tanks | Numero de tanques  |     1
vehicle_economic | Numero economico  |     0
fk_vehicle_model | Modelo id  |     1
base_id | Base id  |     1
fk_driver_id | Chofer asignado  |     0

## Eliminiar vehiculo
>Response:

```shell
{
    "success": 1,
    "message": "No se logró elimar vehiculo",
    "data": null
}
```

```json
{
    "success": 1,
    "message": "Vehículo deshabilitado correctamente",
    "data": null
}
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
DELETE | disable vehicle STAFF | {{url}}/api/staff/vehicle/vehicle_id

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code>por el API URL. 
</aside>

<aside class="warning">
Asegurate de reemplazar <code>vehicle_id</code>por el id (encrypt) del vehiculo, correspondiente en la base de datos
</aside>

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### URL
Es necesario pasar vehicle_id (id de vehiculo, NO numero de serie) al final de la URL del endpoint

## Actualizar vehiculo
>Response: 

```shell
{
    "success": 1,
    "message": "Hubo un error al actualizar vehiculo",
    "data": null
}
```

```json
{
    "success": 1,
    "message": "Los datos se actualizaron correctamente",
    "data": null
}
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
UPDATE | update vehicle STAFF | {{url}}/api/staff/vehicle/vehicle_id

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code>por el API URL. 
</aside>

<aside class="warning">
Asegurate de reemplazar <code>vehicle_id</code>por el id (encrypt) del vehiculo, correspondiente en la base de datos
</aside>

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...
Content-Type | application/x-www-form-urlencoded

### URL
Es necesario pasar vehicle_id (id de vehiculo, NO numero de serie) al final de la URL del endpoint

### Body

* Type x-www-form-urlencoded

Key   | Description | Mandatory
---------|-------------|----------
vehicle_id | Numero de serie  |     0
points_revision | Punto de revision  |     0
tanks | Numero de tanques  |     0
vehicle_economic | Numero economico  |     0
fk_vehicle_model | Modelo id  |     0
fk_driver_id | Chofer asignado  |     0

## Mostrar marcas y modelos
>Response:

```json
{
    "success": 1,
    "message": null,
    "data": [
        {
            "brand": "KENWORTH",
            "brand_id": 1,
            "models": [
                "W990",
                "T680",
                "T880",
                "K370",
                "K270",
                "T370",
                "T270",
                "T170",
                "W900",
                "T800",
                "C500",
                "T470",
                "T440"
            ]
        },
        {
            "brand": "FREIGHTLINER",
            "brand_id": 2,
            "models": [
                "COLUMBIA",
                "CASCADIA",
                "M2",
                "FL120"
            ]
        }
      ]
   } 
```
Muestra los modelos correspondientes a cada marca.

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET | show catalog vehicles STAFF | {{url}}/api/staff/catalog-vehicles

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code>por el API URL. 
</aside>

### Headers

Key  | Value 
-----|-----
Accept | application/json
lang | es_mx
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body
No requerido

## Mostrar compañías y choferes
>Response:

```shell
```

```json
{
    "success": 1,
    "message": null,
    "data": [
        {
            "company": "soltek",
            "company_id": 1,
            "drivers": []
        },
        {
            "company": "KokonutStudio",
            "company_id": 2,
            "drivers": [
                {
                    "id": "eyJpdiI6IjVNQjBOcWJzTDEwdmp0bElRS29tV0E9PSIsInZhbHVlIjoidkpSeXBRZm8zNHdRQkRXaTVKdjRTdz09IiwibWFjIjoiN2I2MzQzZWJkZDY5ZWEyYjI5Zjk5YjEwY2E1MDBlNjYxY2EzZTI4YTcwNDA0ZWY0MDkxYWFjZWZhMWVhOTcxNiJ9",
                    "name": "YYY",
                    "last_name": "Perez",
                    "driver_number": "4768",
                    "user_type_id": 8,
                    "status": 1,
                    "company_id": 2,
                    "status_description": "Active",
                    "user_id": 22,
                    "base": "CDMX",
                    "incidents": 0,
                    "grade": 7
                }
            ]
        }
```

Muestra los todos los choferes correspondientes a cada cliente (compañía).

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET | show companies and drivers | {{url}}/api/staff/catalog-companies-drivers

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code>por el API URL. 
</aside>

### Headers

Key  | Value 
-----|-----
Accept | application/json
lang | es_mx
Autorization | Bearer eyJ0eXAiOiJKV1Q...

## Convertir Admin ind a Admin cliente 
>Response: 

```shell
{
    "success": 0,
    "message": "Tipo de usuario incorrecto para realizar el cambio",
    "data": null
}
```

```json
{
    "success": 0,
    "message": "Cambio de rol exitoso",
    "data": null
}
```

Para realizar el cambio de rol, el usuario debe ser de tipo 3 (Admin Ind)

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET | convert user ADMIN | {{url}}/api/staff/convert-user/user_id

<aside class="notice">
Example user_id: <code>eyJpdiI6IlVrczNRb3Rsdng3W...</code> 
</aside>

### Headers

Key  | Value 
-----|-----
Accept | application/json
lang | es_mx
Autorization | Bearer eyJ0eXAiOiJKV1Q...

## Crear Marcas y Modelos
>Response:

```shell
{
    "success": 0,
    "message": "Marca o modelo(s) ya registrados",
    "data": null
}

{
    "success": 0,
    "message": "Hacen falta datos para completar el registro",
    "data": null
}
```

```json
{
    "success": 1,
    "message": "Modelo y/o marca creados correctamnte",
    "data": ""
}
```
HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | create new model vehicle ADMIN | {{url}}/api/staff/create-model-vehicle

### Headers

Key  | Value 
-----|-----
Accept | application/json
lang | es_mx
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key   | Description | Mandatory | Note 
---------|-------------|----------------|------
model | Arreglo de modelos ["M1","M2"]  | 1 |
brand_id | Marca existente a la que se le asignarán modelos nuevos  | 0 | Si quieres agregar modelos nuevos a una marca existente, enviar este campo y no name_brand
name_brand | Nombre de la marca nueva | 0 | Si quieres crear una nueva marca, enviar este campo y no brand_id

## Borrar Marcas y/o modelos
>Response:

```shell
{
    "success": 0,
    "message": "La marca no existe",
    "data": null
}
```

```json
{
    "success": 1,
    "message": "Modelos(s) eliminado(s) correctamente",
    "data": null
}

{
    "success": 1,
    "message": "La marca fue eliminada de manera correcta",
    "data": null
}
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | delete model vehicle admin | {{url}}/api/staff/delete-model-vehicle

### Headers

Key  | Value 
-----|-----
Accept | application/json
lang | es_mx
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key   | Description | Mandatory | Note 
---------|-------------|----------------|------
model | Arreglo de modelos ["M1","M2"] | 0 | Si se manda este campo y brand_id, se borrará modelos
brand_id | Marca a eliminar | 1 | Si sólo se manda este campo, se borra toda la marca

## Crear base 
>Response: 

```shell
```
{
    "success": 0,
    "message": "Hacen falta datos para completar el registro",
    "data": null
}

```json
{
    "success": 1,
    "message": "Modelo y/o marca creados correctamnte",
    "data": ""
}
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | create flotilla admin | {{url}}/api/staff/create-base

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key   | Description | Mandatory | Note
---------|-------------|-------------|-----
base | Nombre de la base  |  1 |
users | Arreglo de usuarios asignados a la base ["1","2"] | 0 | Se puede no enviar, pero si se envia, se tiene que enviar vehicles
vehicles | Arreglo de vehiculos asignados a la base ["3","4"] | 0 | Se puede no enviar, pero si se envia, se tiene que enviar users
fk_company | Compañía asignada a la base  | 1 | 

## Actualizar bases
>Response:

```shell
```

```json
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | uodate flotilla admin | {{url}}/api/staff/update-base

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key   | Description | Mandatory | Note
---------|-------------|-------------|-----
base | Nombre de la base  |  1 |
users | Arreglo de usuarios asignados a la base ["1","2"] | 0 | Se puede no enviar, pero si se envia, se tiene que enviar vehicles
vehicles | Arreglo de vehiculos asignados a la base ["3","4"] | 0 | Se puede no enviar, pero si se envia, se tiene que enviar users
fk_company | Compañía asignada a la base  | 1 | 

















