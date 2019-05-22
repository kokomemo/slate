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

En caso de ser un super Admin Soltek el que está registrando usuarios y si está dando de alta un chofer, se puede asignar una compañía en específico. 

Éste endpoint se realizó de esta manera debido a que para el flujo de pasar un admin ind a admin cliente, se pueden dar de alta varios usuarios a la vez (administradores, moderadores, supervisores e instaladores)

Roles para permitidos para realizar ésta petición:

Id | Rol
---|----
1  | Super Admin
2  | Admin cliente
3  | Admin ind 

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

# CMS-ENDPOINTS

Sólo podrán acceder a CMS los siguientes usuario:

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

# Usuarios

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

Para realizar el cambio de rol, el cliente debe ser de tipo 3 (Admin Ind). Al llevar a cabo este servicio, el cliente seleccionado cambiará su tipo a Admin cliente (2), después de haber realizado el proceso de alta de cliente.

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

## Mostrar Usuarios por compañía
>Response: 

```shell
```

```json
{
    "success": 1,
    "message": null,
    "data": {
        "KokonutStudio": [
            {
                "users": {
                    "Super Admin": [
                        {
                            "id": "eyJpdiI6IkI4ZlwvTzlJcVpWeGdPelUxemZhTHdnPT0i...",
                            "name": "Administrador",
                            "last_name": "Kokonut",
                            "email": "kokonut@kokonutstudio.com",
                            "created_at": "2019-05-16 15:48:03",
                            "updated_at": "2019-05-21 16:09:11"
                        }
                    ],
                    "Admin Cliente": [
                        {
                            "id": "eyJpdiI6InZcL0twYVc1ekM4WkRERmlzUHlHQ3...",
                            "name": "José",
                            "last_name": "",
                            "email": "jaso111n@efe.com",
                            "created_at": "2019-05-16 15:54:59",
                            "updated_at": "2019-05-20 21:39:42"
                        },
                        {
                            "id": "eyJpdiI6Im12WGhMQTlFSTNaVzJQK1wvZmdCd...",
                            "name": "pepe2",
                            "last_name": "",
                            "email": "jaso1211n@efe.com",
                            "created_at": "2019-05-16 15:55:51",
                            "updated_at": null
                        }
                    ],
                    "Supervisor": [
                        {
                            "id": "eyJpdiI6IlVYdHdKNUdpZEpySVBwQU9tXC9DTjBnPT0...",
                            "name": "Supervisor 1",
                            "last_name": "",
                            "email": "supervisor1@hotmail.com",
                            "created_at": "2019-05-16 16:18:49",
                            "updated_at": null
                        }
                    ],
                    "Instalador": [
                        {
                            "id": "eyJpdiI6ImlkZEpWQnY1Z3d0ZVlMMTVUc1pCYUE...",
                            "name": "Instalador 1",
                            "last_name": "",
                            "email": "instalador1@hotmail.com",
                            "created_at": "2019-05-16 16:18:13",
                            "updated_at": null
                        },
                        {
                            "id": "eyJpdiI6IngzbVZtTytPS09hNXVmM2FXU0t6OUE9PS...",
                            "name": "InstaladorPrueba",
                            "last_name": "",
                            "email": "insta_p@gmaiil.com",
                            "created_at": "2019-05-17 17:02:52",
                            "updated_at": null
                        }
                    ],
                    "Chofer": [
                        {
                            "id": "eyJpdiI6IndQTXBtRVJ5N0dJajI1MjROTi...",
                            "name": "YYY",
                            "last_name": "Perez",
                            "email": null,
                            "created_at": "2019-05-16 22:28:26",
                            "updated_at": null
                        },
                        {
                            "id": "eyJpdiI6IlwvazlDT3E5T3haUXduOXl5NTZiUkJBPT0...",
                            "name": "Soltek",
                            "last_name": "",
                            "email": null,
                            "created_at": "2019-05-17 17:33:48",
                            "updated_at": "2019-05-17 17:34:48"
                        }
                    ]
                },
                "vehicles": {
                    "total_vehicles": 1
                }
            }
        ]
     }
  }
```

Mustra todos los usuarios separados por rol y total de vehiculos pertenecientes a cada compañía. Servicio utilizado para mostrar listado de Admins cliente en CMS

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
GET |  get users by company| {{url}}/api/staff/dashboard-clients

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

No aplica.

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

Devuelve **TODOS** los usuarios registrados. Este servicio alimenta el modulo para enlistar **equipo soltek**, **Instaladores y supervisores**

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

Devuelve todos los vehiculos registrados. Si es un Super Admin Soltek, se mostrarán el total de vehiculos registrados por todos los usuarios. Si es un Admin ind o cliente, sólo se le mostrará los vehiculos registrados pertenecientes a su compañía.

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

El vehiculo no se elimina de la base de datos, sólo se cambia el status y se deja de mostrar en la vista.

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
 9 | Moderador 

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

# Vechículos

## Buscador de vehiculos
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
```

Este servicio se utiliza para filtrar información de un vehículo específico por número de serie o por número económico.

HTTP Request  | Name Endpoint |  Endpoint
--------------|---------------|-----------
POST | find vehicle INSTALL | {{url}}/api/install/search

### Headers

Key  | Value 
-----|-----
Autorization | Bearer eyJ0eXAiOiJKV1Q...

### Body

* Type form-data

Key | Value | Mandatory
----|-------|-----------
vehicle_economic | Número económico| 0
vehicle_id | Número de serie | 0

<aside class="warning">
Servicio sin funcionar. <code>REVISAR</code>. No se realiza búsqueda.
</aside>

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
            "elements": [
                {
                    "description": "Antisifon"
                }
            ]
        },
        {
            "name": "Antisifon Tanksafe Impregnable",
            "elements": [
                {
                    "description": "Antisifon"
                }
            ]
        },
        {
            "name": "Candado Respiradero BreatherSafe",
            "elements": [
                {
                    "description": "Respiradero"
                }
            ]
        },
        {
            "name": "Kit Protección Mangueras",
            "elements": [
                {
                    "description": "Manguera inyeccion"
                },
                {
                    "description": "Manguera retorno"
                },
                {
                    "description": "Flotador"
                },
                {
                    "description": "Drenes"
                },
                {
                    "description": "Filtro"
                },
                {
                    "description": "Mangueras"
                }
            ]
        }
    ]
}
```

Se muestra listado de kits de seguridad con sus grupos de instalación pertenecientes.

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
            "vehicle_db_id": 1,
            "economic_number": "86214",
            "serial_number": "55667",
            "security_kit": "Antisifon Tanksafe Impregnable",
            "group_installation": "Tanque piloto",
            "group_db_id": 1,
            "revision_point": "Antisifón",
            "revision_db_id": 2,
            "installation_description": "",
            "installation_checked": 0,
            "images": [
                {
                    "image": "https://soltek-staging.s3.us-west-2.amazonaws.com/installation/vehicle_1/C1lxI2KzjJtGyr0UM9cdCeH_install.jpg..."
                }
            ],
            "installation_foliate_stamp": "DT456"
        }
    ]
}
```

Se muestra instalación realizada de vehículo. (Para Instalador y Supervisor)

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
<code>"installation_description"</code> se utilizará en caso de que en una actualización se deseé agregar comentarios acerca de la instalación.
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





















