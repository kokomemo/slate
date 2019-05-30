---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - json: Success
  - shell: Error

toc_footers:
  - <a href='http://kokonutstudio.com/'>Kokonutstudio</a>

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

## Estatus de usuarios

id | Estatus 
---|---------
 1 | Desbloqueado
 2 | Bloqueado
 3 | Eliminado

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
Se muestra listado de los tipos de usuarios con su identificador (id), el cual se utilizará para realizar las diferentes peticiones de los servicios.

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

<aside class="notice">
Para autenticar, usar <code>"token_type"</code> y <code>"access_token"</code>
</aside>

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST |login GENERAL | {{url}}/api/login 

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code> en tu API URL.
</aside>

### Header

No aplica.

### Body

* Type form-data

Key   | Value | Mandatory
---------|-------------|----------
username | email@example.com  |     1
password | 123qwe1 |   1

## Logout
>Response: 

```shell
```

```json
{
    "success": 1,
    "message": "Sesión cerrada correctamente",
    "data": null
}
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET | logout GENERAL | {{url}}/api/logout

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

No requerido.

## Recuperar contraseña
>Response:

```shell
{
    "success": 0,
    "message": "El correo electronico no se encuentra registrado",
    "data": null
}

{
    "success": 0,
    "message": "El correo de recuperación no pudo ser enviado",
    "data": null
}
```

```json
{
    "success": 0,
    "message": "Correo de recuperación enviado exitosamente",
    "data": null
}
```

Se envía correo electronico con un enlace tokenizado, el cual al hacer clic se direccionará a una ruta del front-end, en la cual se solicitará la contraseña nueva y el token de recuperación (éste último no lo ingresará el usuario). Solicitados en el endpoint de Reiniciar Contraseña


HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | restore password GENERAL | {{url}}/api/restore_password 

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
email | Correo electrónico del usuario  |     1

## Reiniciar Contraseña
>Response: 

```shell
{
    "success": 0,
    "message": "El token no es valido",
    "data": null
}

{
    "success": 0,
    "message": "Las contraseñas no coinciden",
    "data": null
}

{
    "success": 0,
    "message": "Hace falta información para atender la petición",
    "data": null
}
```

```json
{
    "success": 1,
    "message": "Contraseña actualizada correctamente",
    "data": null
}
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | reset password GENERAL | {{url}}/api/reset_password

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
password | Correo electrónico del usuario  |     1
password_confirmation | Confirmación de correo electrónico  |     1
token | Token de recuperación |     1

## Registro secundario de usuarios
>Response:


```shell
{
    "success": 0,
    "message": "Hacen falta datos para completar el registro",
    "data": []
}

{
    "success": 0,
    "message": "No se pudo registrar el usuario",
    "data": []
}

{
    "success": 0,
    "message": "Hace falta información de suscripción para completar el registro",
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
Permite registrar admins cliente, admins ind, instaladores, supervisores, choferes, moderadores. Se puede registrar varios usuarios a la vez. Para esto se utiliza un body de tipo raw, mandando un arreglo de objetos.
Al crear un usuario, se le asigna por default un status 1 (Desbloqueado) y la compañía del usuario que hizo el registro. 

Notas:

* En caso de ser un super Admin Soltek el que está registrando usuarios podrá asignar compañías a los usuarios creados.

* Si no es un super Admin Soltek, a todos los usuarios registrados, se les asignará siempre la compañía del Admin que los esté creando.

Éste endpoint se realizó de esta manera debido a que para el flujo de pasar un admin ind a admin cliente, se pueden dar de alta varios usuarios a la vez (administradores, moderadores, supervisores e instaladores)

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin
2  | Admin cliente
3  | Admin ind 
9  | Moderadores

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

La información requerida para realizar un registro secundario, es similar a la de registro general, a diferencia que aquí se puede agregar base_id, company_id y un número de chofer, en caso de dar de alta a un chofer, omitiendo también, correo electrónico y contraseña del registro. A continuación se muestra las dos maneras en las que se puede realizar un registro de usuario, el primer elemento del arreglo pertenece a un registro de chofer:

[{
 "name": "chofer1",

 "last_name": "chofer1",

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

<aside class="notice">
Endpoint utilizado para registrar administradores, moderadores, instaladores, supervisores, choferes 
</aside>

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

Sólo se cambia el estatus del usuario, no se elimina de la base de datos.

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin
2  | Admin cliente
3  | Admin ind 
9  | Moderadores

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
DELETE | disable secondary user STAFF | {{url}}/api/staff/disable/{user_id}

<aside class="notice">
Example user_id: <code>eyJpdiI6IlVrczNRb3Rsdng3W...</code> 
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

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin
2  | Admin cliente
3  | Admin ind 

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

<aside class="warning">
Importante siempre enviar <code>id</code> para completar la actualización del usuario.
</aside>

<aside class="notice">
Example id: <code>eyJpdiI6IlVrczNRb3Rsdng3W...</code> 
</aside>

## Mostrar perfil
>Response:

```shell
{
    "success": 0,
    "message": "No se logró mostrar el usuario",
    "data": null
}
```

```json
{
    "success": 1,
    "message": null,
    "data": [
        {
            "id": 2,
            "name": "Administrador",
            "last_name": "Kokonut",
            "email": "memo@gmail.com",
            "image": "https://soltek-staging.s3.us-west-2.amazonaws.com/profiles/profile_/Xr5H0oBLdHjlby6QoPHJEpg?X-Amz-Content-Sha256=UN",
            "created_at": {
                "date": "2019-05-16 15:48:03.000000",
                "timezone_type": 3,
                "timezone": "UTC"
            }
        }
    ]
}
```
HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET | profile GENERAL | {{url}}/api/profile

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

No requerido.


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

## Mostrar Usuarios por compañía
>Response: 

```shell
```

```json
{
    "success": 1,
    "message": null,
    "data": {
        "soltek": [
            {
                "id_company": "eyJpdiI6IlM3N0owVmQzNG1nUTZjaTNwRHJITnc...",
                "active": 1,
                "users": {
                    "1": [
                        {
                            "id": "eyJpdiI6IjZ5UEhTN3R0bVpIS2JkeFUzMHJ...",
                            "name": "Administrador",
                            "last_name": "Kokonut",
                            "email": "desarrollo@kokonutstudio.com",
                            "user_type": "Super Admin",
                            "user_type_id": 1,
                            "user_status_id": 1,
                            "user_status_description": "Active",
                            "created_at": "2019-05-27 23:17:22",
                            "updated_at": null
                        }
                    ],
                    "3": [
                        {
                            "id": "eyJpdiI6InVwWnZraWk0Y0luOWtVaklQNTVpY...",
                            "name": "Usuario2",
                            "last_name": "Usuario2",
                            "email": null,
                            "user_type": "Instalador",
                            "user_type_id": 7,
                            "user_status_id": 1,
                            "user_status_description": "Active",
                            "created_at": "2019-05-27 23:17:48",
                            "updated_at": null
                        }
                    ]
                },
                "vehicles": {
                    "incidents": 3,
                    "total_vehicles": 16,
                    "vehicles_checked": 10
                }
            }
        ]
    }
}
```

Mustra todos los usuarios, incidencias, vehículos supervisados y total de vehiculos pertenecientes a cada compañía. Servicio utilizado para mostrar listado de Admins cliente y Admins ind en CMS y App

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin
4  | Supervisor Soltek
5  | Instalador Soltek 

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET |  get users by company| {{url}}/api/staff/dashboard-clients

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

No aplica.

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

Las compañías podrán consultar los choferes que tienen registrados. 

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
2  | Admin cliente
3  | Admin ind  

<aside class="notice">
Este servicio se utiliza para CMS y APP de Admin cliente o Admin ind. Para el CMS de Super admin, utilizar el endpoint mencionado en la sección de Choferes de CMS-ENDPOINTS <code>( {{url}}/api/staff/catalog-companies-drivers )</code>
</aside>

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

# CMS-ENDPOINTS

Sólo podrán acceder a CMS los siguientes usuarios:

id | Rol
---|------
 1 | Super Admin
 2 | Admin cliente
 3 | Admin ind
 9 | Moderador 

# Dashboard

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

Se muestran estadisticas sobre los clientes registrados, vehiculos registrados y vehiculos supervisados, los cuales se podrán consultar por periodos. (Mes actual, Mes anterior, Historico). Si se trata de un Super admin soltek, se muestra estadisticas de todas las compañías.

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin
2  | Admin cliente
3  | Admin ind
   | Moderadores 

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | dashboard ADMIN | {{url}}/api/dashboard 

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code> en tu API URL.
</aside>

<aside class="notice">
Si el usuario es un Admin ind o Admin cliente, sólo se mostrará estadisticas acerca de vehiculos registrados y vehiculos supervisados. Todo en relación a la compañía a la que pertenecen.
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

# Compañías

## Limite de usuarios y vehículos
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
    "message": "Actualización de limites correcta",
    "data": null
}
```

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin  

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | update limit subscription | {{url}}/api/staff/update-subscription-limit

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
fk_company | Identificador de la compañía a modificar |     1
users_limit | Limite de usuarios a registrar |     0
vehicles_limit | Limite de vehiculos a registrar |     1

## Bloquear/Desbloquear compañías
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
    "message": "Comapañia actualizada correctamente",
    "data": ""
}
```

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | activate/disable company | {{url}}/api/staff/activate-company

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
fk_company | Identificador de la compañía a modificar |     1
active | estatus de compañía |     1

<aside class="notice">
<code>active</code> 1 = Desbloqueada 0 = Bloqueada
</aside>

# Usuarios

## Convertir Admin ind a Admin cliente 
>Response: 

```shell
{
    "success": 0,
    "message": "Tipo de usuario incorrecto para realizar el cambio",
    "data": null
}

{
    "success": 0,
    "message": "Hubo un problema al actualizar la compañia",
    "data": null
}

{
    "success": 0,
    "message": "Hubo un error al consultar informacion",
    "data": null
}
```

```json
{
    "success": 1,
    "message": "Comapañia actualizada correctamente",
    "data": null
}
```

Para realizar el cambio de rol, el cliente debe ser de tipo 3 (Admin Ind). Al llevar a cabo este servicio, el cliente seleccionado cambiará su tipo a Admin cliente (2), después de haber realizado el proceso de alta de cliente.

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin 

<aside class="warning">
Éste servicio puede cambiar, falta definir algunos 
</aside>

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | convert user ADMIN | {{url}}/api/staff/convert-user

### Headers

Key  | Value 
-----|-----
Accept | application/json
lang | es_mx
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key | Value | Mandatory
----|-------|-----------
id_user | id tokenizado | 1
name_company | nombre de la compañía | 1 

<aside class="notice">
Example user_id: <code>eyJpdiI6IlVrczNRb3Rsdng3W...</code>. 
</aside>

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
            "name": "Ernesto",
            "last_name": "Medina",
            "email": "ernesto@gmail.com",
            "user_id": "eyJpdiI6IlFtZ2FXK1NEMGNJQkYy...",
            "user_type": "Supervisor",
            "user_type_id": 6,
            "user_status_description": "Desbloqueado",
            "user_status_id": 1
        }
     ]
  }
```

Devuelve **TODOS** los usuarios registrados. Este servicio se utiliza para los casos que se quiera consultar sólo el listado de usuarios (sin compañía) registrados de todas las compañías 

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin

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

# Vehículos

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

Devuelve vehiculos registrados por todas las compañías. También se puede consultar vehículos por compañía en específico.

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin
2  | Admin cliente
3  | Admin ind

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET | show vehicicle STAFF | {{url}}/api/staff/vehicle/all/all/all

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code>por el API URL.
</aside>

### URL

* Si se desea consultar la información global de todas las compañías utilizar al final: /all/all/all.
* Si se desea consultar información de compañías, vehículos o usuario en especifico, utilizar al final de URL: /{company_id}/{vehicle_id}/{user_id} 

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

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin
2  | Admin cliente
3  | Admin ind
5  | Instalador Soltek
7  | Instalador cliente 

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

El vehiculo no se elimina de la base de datos, sólo se cambia el status y se deja de mostrar en la vista.

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin
2  | Admin cliente
3  | Admin ind

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

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin
2  | Admin cliente
3  | Admin ind
5  | Instalador Soltek
7  | Instalador cliente 

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

## Ficha técnica
>Response:

```shell
{
    "success": 0,
    "message": "Permisos insuficientes",
    "data": []
}
```

```json
{
    "success": 1,
    "message": "Instalación mostrada correctamente",
    "data": [
        {
            "economic_number": null,
            "serial_number": "WmW3NaEFKWaaJXLUdUrFCEuLqQUBbzv3ee7Shd8UFX4GpnMEuVELVT5a4M",
            "security_kit": "Antisifon Tanksafe Standard",
            "group_installation": "Tanque piloto",
            "group_installation_id": 1,
            "revision_point": "Antisifon",
            "revision_point_id": 1,
            "installation_description": "Esto es una prueba",
            "installation_checked": 1,
            "installation_foliate_stamp": "asdas1",
            "created_at": "2019-05-29 14:44:47",
            "updated_at": null,
            "images": [
                {
                    "url": "https://soltek-staging.s3.us-west-2.amazonaws.com/installation/vehicle_1/0HxetssF3wBO9qjqWq5OhMK_install.png?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIYSAD6GNEQTV6FPA%2F20190529%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20190529T202216Z&X-Amz-SignedHeaders=host&X-Amz-Expires=7200&X-Amz-Signature=33fe6e39d5d12634b4bd6e1b1a626a7e46b4ee6f1e8f1d9f7766ddffd1e06435",
                    "created_at": "2019-05-29 14:44:47"
                }
            
            ]
        }
    ]
}
```

Se muestra historial de instalación realizada de un vehículo espcífico. 

También se puede utilizar para listar proceso de instalación, para el rol de instalador y supervisor 

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin
2  | Admin cliente
4  | Supervisor Soltek
5  | Instalador Soltek
6  | Supervisor cliente
7  | Instalador cliente 


HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET | show INSTALL | {{url}}/api/install/show/{vehicle_id}

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

No aplica.

<aside class="notice">
<code>"installation_description"</code> se utilizará en caso de que en una actualización se deseé agregar comentarios acerca de la instalación. <br><br>
<code>{vehicle_id}</code> = id de vehículo sin tokenizar
</aside>

## Detalle ficha técnica
>Response:

```shell
{
    "success": 0,
    "message": "Información ingresada no coincide con el vehículo registrado",
    "data": null
}

{
    "success": 0,
    "message": "No se pudo encontrar el vehículo",
    "data": null
}
```

```json
{
    "success": 1,
    "message": null,
    "data": [
        {
            "economic_number": null,
            "serial_number": "WmW3NaEFKWaaJXLUdUrFCEuLqQUBbzv3ee7Shd8UFX4GpnMEuVELVT5a4M",
            "security_kit": "Antisifon Tanksafe Standard",
            "group_installation": "Tanque piloto",
            "group_installation_id": 1,
            "revision_point": "Antisifon",
            "revision_point_id": 1,
            "installation_description": "Esto es una prueba",
            "installation_checked": 1,
            "installation_foliate_stamp": "asdas1",
            "created_at": "2019-05-29 14:44:47",
            "updated_at": null,
            "images": [
                {
                    "url": "https://soltek-staging.s3.us-west-2.amazonaws.com/installation...",
                    "created_at": "2019-05-29 14:44:47"
                },
                {
                    "url": "https://soltek-staging.s3.us-west-2.amazonaws.com/installation...",
                    "created_at": "2019-05-29 14:44:47"
                }
            ],
            "driver": {
                "name": "Chofer 1",
                "driver_id": "1dXIRZc8FQguY3AjtCB"
            },
            "supervisor": {
                "name": "Supervisor",
                "email": "super@kokonutstudio.com"
            }
        }
    ]
}
```

Este servicio se utiliza para mostrar detalles de un grupo de revisión seleccionado desde la ficha técnica de un vehículo

También se puede utilizar para listar detalle de un grupo de revisión, para el rol de instalador y supervisor 

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin
2  | Admin cliente
4  | Supervisor Soltek
5  | Instalador Soltek
6  | Supervisor cliente
7  | Instalador cliente

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | find vehicle INSTALL | {{url}}/api/install/search

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key | Value | Mandatory  | Notes
----|-------|------------|--------
vehicle_economic | Número económico| 0 | si no se envía, se tiene que envíar vehicle_id
vehicle_id | Número de serie | 0 | si no se envía, se tiene que envíar vehicle_economic
revision_point | id del punto de revisión | 0 |
group_point | id del grupo de revisión | 0 |

<aside class="warning">
Servicio sin funcionar en staging. <code>REVISAR</code>. No se realiza búsqueda.
</aside>

# Catalogo de vehículos

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
                {
                    "id": 1,
                    "description": "W990"
                },
                {
                    "id": 2,
                    "description": "T680"
                },
                {
                    "id": 3,
                    "description": "T880"
                },
                {
                    "id": 4,
                    "description": "K370"
                },
                {
                    "id": 5,
                    "description": "K270"
                }
            ]
         }
      ]
   } 
```
Muestra los modelos correspondientes a cada marca. Este servicio se utiliza para seleccionar marca y modelo en el proceso para dar alta vehículos

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin

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

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin

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

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin

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

# Choferes

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

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin

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

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin
2  | Admin cliente
3  | Admin ind

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

#Bases

## Crear base 
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
    "message": "Base creada correctamente",
    "data": ""
}
```

Se puede dar de alta sólo bases o se puede crear bases con usuarios y vehículos.

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin

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

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | update flotilla admin | {{url}}/api/staff/update-base

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

## Mostrar bases registradas 
>Response:

```shell
```

```json
{
    "success": 1,
    "message": null,
    "data": [
        {
            "base_id": 1,
            "base": "CDMX",
            "vehicles": [
                {
                    "vehicle_id_bd": 2,
                    "vehicle_id": "6577",
                    "vehicle_economic": "",
                    "fk_vehicle_model": 18
                }
            ],
            "users": [
                {
                    "user_id": 22,
                    "user_name": "Chofer",
                    "user_email": null
                }
            ]
        }
    ]
}
```

Muestra listado de bases registradas, con sus vehiculos y choferes pertenecientes

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET | get base admin | {{url}}/api/staff/get-base

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

No requerido.

## Asignar Choferes a vehículos
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
    "message": "Chofer asignado correctamente",
    "data": null
}
```
Sirve para asignarle un vehículo y una base a un chofer

Roles permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin
2  | Admin cliente
3  | Admin ind

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | assign-vehicle | {{url}}/api/staff/assign-vehicle

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key | Value | Mandatory
----|-------|-----------
vehicle_id | Identificador de vehículo (id) | 1
fk_driver | Identificador del chofer (id) | 1
base_id | Identificador de la base (id) | 1

# APP-ENPOINTS

Sólo podrán acceder a la APP los siguientes usuario: 

id | Rol
---|------
 1 | Super Admin
 2 | Admin cliente
 3 | Admin ind
 4 | Supervisor Soltek
 5 | Instalador Soltek
 6 | Supervisor cliente
 7 | Instalador cliente 

# Usuarios

## Registro de usuarios
>Response:

```shell
{
    "success": 0,
    "message": "Hacen falta datos para completar el registro",
    "data": []
}

{
    "success": 0,
    "message": "No se pudo registrar el usuario",
    "data": []
}

{
    "success": 0,
    "message": "Hace falta información de suscripción para completar el registro",
    "data": []
}
```

```json
{
    "success": 1,
    "message": "",
    "data": {
        "token_type": "Bearer",
        "expires_in": 604797,
        "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI...",
        "user_type": 3
    }
}
```
Registro utilizado por usuarios que recién descargaron la app. En este registro siempre se asignará por default un tipo de usuario 3 (Admin ind)

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | register user GENERAL | {{url}}/api/register

### Headers

Key  | Value 
-----|-----
Accept | application/json
lang | es_mx

### Body

Type form-data

Key | Value | Mandatory
----|-------|---------- 
name | Nombre de usuario | 1
last_name | Apellido del usuairo | 0
email | Correo electrónico | 1
password | Contraseña del usuario | 1
password_confirmation | Confirmación de contraseña | 1
image | Foto del usuario | 0
company | Nombre de la compañía | 1

Nota: Si se envía passsword, obligatoriamente se debe enviar password_confirmation.
Si no se manda el "company", se crea una compañía llamada igual que el correo electrónico ingresado.

# Vehículos

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

El vehiculo no se elimina de la base de datos, sólo se cambia el status y se deja de mostrar en la vista.

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
DELETE | disable vehicle STAFF | {{url}}/api/staff/vehicle/vehicle_id

<aside class="notice">
Asegurate de reemplazar <code>{{url}}}</code>por el API URL. 
</aside>

<aside class="notice">
Example id: <code>eyJpdiI6IlVrczNRb3Rsdng3W...</code> 
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

<aside class="notice">
Example vehicle_id: <code>eyJpdiI6IlVrczNRb3Rsdng3W...</code> 
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

# Instalación 

## Mostrar Kits de Seguridad 
>Response:

```json
{
    "success": 1,
    "message": "Consulta de kits exitosa",
    "data": [
        {
            "name": "Antisifon Tanksafe Standard",
            "id": 1,
            "elements": [
                {
                    "id": 1,
                    "description": "Antisifon"
                }
            ]
        },
        {
            "name": "Antisifon Tanksafe Impregnable",
            "id": 2,
            "elements": [
                {
                    "id": 1,
                    "description": "Antisifon"
                }
            ]
        },
        {
            "name": "Candado Respiradero BreatherSafe",
            "id": 3,
            "elements": [
                {
                    "id": 2,
                    "description": "Respiradero"
                }
            ]
        },
        {
            "name": "Kit Protección Mangueras",
            "id": 4,
            "elements": [
                {
                    "id": 3,
                    "description": "Manguera inyeccion"
                },
                {
                    "id": 4,
                    "description": "Manguera retorno"
                },
                {
                    "id": 5,
                    "description": "Flotador"
                },
                {
                    "id": 6,
                    "description": "Drenes"
                },
                {
                    "id": 7,
                    "description": "Filtro"
                },
                {
                    "id": 8,
                    "description": "Mangueras"
                }
            ]
        }
    ]
}
```

Se muestra listado de kits de seguridad con sus grupos de revisión pertenecientes.

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET | list security kits INSTALL | {{url}}/api/install/security-kit-list

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

No aplica.

##Instalar kit de seguridad
>Response:

```shell
{
    "success": 0,
    "message": "Permisos insuficientes",
    "data": []
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
    "message": "Instalación completada",
    "data": null
}
```
Roles para realizar instalación de kit de seguridad:

Id | Rol
---|----
2  | Admin cliente
3  | Admin ind
5  | Instalador Soltek
7  | Instalador 

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | install kit INSTALL | {{url}}/api/install/install?=

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key | Value | Mandatory
----|-------|-----------
image | Foto de cada punto instalado | 1
foliate_stamp | Sello foliado de cada pieza instalada | 1
vehicle_id | Identificador de vehiculo (id) al cual se le realizará la inatalación | 1
security_kit_id | Identificador de kit a instalar (id) | 1
group_installation_id | Identificador de grupo de instalación (id) | 1 
revision_point_id | Identificador de punto de revisión (id) | 1

## Mostrar instalación 
>Response:

```shell
{
    "success": 0,
    "message": "Permisos insuficientes",
    "data": []
}
```

```json
{
    "success": 1,
    "message": "Instalación mostrada correctamente",
    "data": [
        {
            "economic_number": null,
            "serial_number": "WmW3NaEFKWaaJXLUdUrFCEuLqQUBbzv3ee7Shd8UFX4GpnMEuVELVT5a4M",
            "security_kit": "Antisifon Tanksafe Standard",
            "group_installation": "Tanque piloto",
            "group_installation_id": 1,
            "revision_point": "Antisifon",
            "revision_point_id": 1,
            "installation_description": "Esto es una prueba",
            "installation_checked": 1,
            "installation_foliate_stamp": "asdas1",
            "created_at": "2019-05-29 14:44:47",
            "updated_at": null,
            "images": [
                {
                    "url": "https://soltek-staging.s3.us-west-2.amazonaws.com/installation/vehicle_1/0HxetssF3wBO9qjqWq5OhMK_install.png?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIYSAD6GNEQTV6FPA%2F20190529%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20190529T202216Z&X-Amz-SignedHeaders=host&X-Amz-Expires=7200&X-Amz-Signature=33fe6e39d5d12634b4bd6e1b1a626a7e46b4ee6f1e8f1d9f7766ddffd1e06435",
                    "created_at": "2019-05-29 14:44:47"
                }
            
            ]
        }
    ]
}
```

Se muestra instalación realizada de un vehículo espcífico. (Para Instalador y Supervisor)

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET | show INSTALL | {{url}}/api/install/show/{vehicle_id}

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

No aplica.

<aside class="notice">
<code>"installation_description"</code> se utilizará en caso de que en una actualización se deseé agregar comentarios acerca de la instalación. <br><br>
<code>{vehicle_id}</code> = id de vehículo sin tokenizar
</aside>

# Supervisión
## Supervisar vechículo
>Response:

```shell
{
    "success": 0,
    "message": "Tu rol no tiene permisos para ver este recurso",
    "data": []
}

{
    "success": 0,
    "message": "Hace falta datos para atender la peticion",
    "data": null
}
```

```json
{
    "success": 1,
    "message": "Supervisión completada correctamente",
    "data": ""
}
```

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | revision REVISION | {{url}}/api/install/revision

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key | Value | Mandatory
----|-------|-----------
vehicle_db_id | Identificador de vehiculo a supervisar (id) | 1
group_db_id | Identificador de grupo de instalación a supervisar (id) | 1
comments | Si se desea agregar comentarios acerca de la supervisión | 0
failed | Enviar "1" si hay alguna falla | 0

<aside class="notice">
<code>"coments"</code> se utilizará en caso de que en una actualización se deseé agregar comentarios acerca de la supervisión.
</aside>

# Choferes

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

Las compañías podrán consultar los choferes que tienen registrados.

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





















