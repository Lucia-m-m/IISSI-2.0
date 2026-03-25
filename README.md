# IISSI-2.0
EXAMEN SCHEDULE
- ejercicio 1
<img width="234" height="428" alt="image" src="https://github.com/user-attachments/assets/79e7d8b8-15de-45d5-b3be-beee5367b05f" />

  en create-product
  <img width="203" height="175" alt="image" src="https://github.com/user-attachments/assets/2f397ddf-1351-4a48-b8be-cb5db0a6600a" />

- ejercicio 2
<img width="371" height="304" alt="image" src="https://github.com/user-attachments/assets/40f46239-1437-4316-9d23-97b8da5f32ae" />

  en models/products
<img width="369" height="269" alt="image" src="https://github.com/user-attachments/assets/b2a08c3f-99be-4238-bcf5-416922de5ef0" />

  en models/restaurant
<img width="373" height="79" alt="image" src="https://github.com/user-attachments/assets/b03ce5f3-ec7d-4c16-b531-aafbe71ffff9" />


- ejercicio 3
<img width="227" height="125" alt="image" src="https://github.com/user-attachments/assets/baa4bf3f-c877-4c09-bcb9-6c824370a96a" />
<img width="185" height="161" alt="image" src="https://github.com/user-attachments/assets/f8fadd0f-8bfa-41e8-82dd-3f6d39b5dbe1" />

- ejercicio 4
<img width="293" height="245" alt="image" src="https://github.com/user-attachments/assets/b309b0bb-5008-4f4a-ab01-95a82bd8c8d0" />

- ejercicio 5
<img width="284" height="430" alt="image" src="https://github.com/user-attachments/assets/559d90d4-1bf6-412a-9693-bddee826c56c" />

- ejercicio 6
<img width="327" height="167" alt="image" src="https://github.com/user-attachments/assets/1fa8c3c4-d5ca-4017-bde1-5edddd6fb49a" />

- ejercicio 7
<img width="326" height="165" alt="image" src="https://github.com/user-attachments/assets/dc5ab578-c6f0-452d-a8b8-a3ff5e19e25c" />

- ejercicio 8
<img width="296" height="374" alt="image" src="https://github.com/user-attachments/assets/e5c51fa2-e5cf-48c7-9583-0069320685ea" />





# EXAMEN SHIPPING ADDRESS
- ejercicio 1
<img width="222" height="495" alt="image" src="https://github.com/user-attachments/assets/8462fe00-a36f-49cb-9fdd-f860ba86dfde" />
  en models/shippingAdress
<img width="235" height="491" alt="image" src="https://github.com/user-attachments/assets/0d05fcbd-e9d4-43de-be2c-ff4104674019" />
  en models/user
  <img width="263" height="65" alt="image" src="https://github.com/user-attachments/assets/606ecc16-682a-4041-bb0d-7bd44d89cde9" />

- ejercicio 2
<img width="251" height="289" alt="image" src="https://github.com/user-attachments/assets/240b0308-acb9-46dd-b348-d2de7fd386d4" />

- ejercicio 3
<img width="384" height="197" alt="image" src="https://github.com/user-attachments/assets/97c1ea78-3064-40ce-b3fb-99cb1fed67f1" />

- ejercicio 4
<img width="291" height="431" alt="image" src="https://github.com/user-attachments/assets/59d90ad8-a655-4403-becd-f3aaab10ec3c" />

- ejercicio 5
<img width="383" height="194" alt="image" src="https://github.com/user-attachments/assets/b6250c8c-153e-46cb-aee2-0836d6ed8cd1" />




# APUNTES BACKEND
1. Dónde va una foreign key
Regla general

La FK se pone en la tabla que pertenece a la otra o en la que apunta a la otra.

Relación 1:N

Si:

un Restaurant tiene muchos Products
un Product pertenece a un Restaurant

Entonces la FK va en Products:

restaurantId

Porque:

Restaurant 1 ---- N Product

y el lado muchos guarda la FK.

Relación 1:1

La FK suele ir en la tabla más dependiente.

Ejemplo:

un Profile pertenece a un User

Entonces:

userId // en Profiles
Relación N:N

No se pone la FK en una de las tablas principales.

Se crea una tabla intermedia:

Products N ---- N Orders

se convierte en:

OrderProducts
- orderId
- productId
2. En el ejercicio de Schedules, por qué no va productId

Porque el modelo correcto es:

un Restaurant tiene muchos Schedules
un Schedule pertenece a un Restaurant
un Product puede tener un Schedule o ninguno
varios Products pueden compartir un Schedule

Entonces:

Schedules.restaurantId
Products.scheduleId

y no:

Schedules.productId
Por qué

Porque el producto es quien “usa” un horario.

Así que:

Schedule 1 ---- N Product

La FK correcta es:

scheduleId // en Products
3. Cuándo añadir algo en migraciones
Migraciones

Sirven para crear o modificar la estructura real de la BD.

Aquí sí añades:

columnas
claves foráneas
createdAt
updatedAt

Ejemplo:

await queryInterface.addColumn('Products', 'scheduleId', {
  type: Sequelize.INTEGER,
  allowNull: true,
  references: {
    model: 'Schedules',
    key: 'id'
  }
})
Idea clave

La migración no decide la relación.
La relación la decide el modelo lógico, y luego tú la reflejas en la migración.

4. createdAt y updatedAt
En migraciones

Normalmente sí los pones:

createdAt: {
  allowNull: false,
  type: Sequelize.DATE
},
updatedAt: {
  allowNull: false,
  type: Sequelize.DATE
}

Porque estás creando la tabla real.

En models

Normalmente no hace falta ponerlos, porque Sequelize usa por defecto:

timestamps: true

y ya espera esas columnas.

Entonces, si los pongo en el model, está mal?

No.
No está mal si en la tabla existen esas columnas.

Puede ser redundante, pero no incorrecto.

Por qué en unos models aparecen y en otros no

Porque el proyecto puede estar escrito con estilos distintos:

unos modelos los ponen explícitos
otros no

Eso suele ser inconsistencia de estilo, no un error técnico.

Consejo

Hazlo igual que el estilo que ya use el proyecto.

5. isInt() y toInt()
isInt()

Sirve para validar que un valor es entero.

check('scheduleId').isInt()
toInt()

Sirve para convertir el valor a número entero.

check('scheduleId').isInt().toInt()
Regla fácil
isInt() → valida
toInt() → convierte
Lo normal

Usarlos juntos:

check('scheduleId')
  .optional({ nullable: true })
  .isInt().withMessage('scheduleId must be an integer')
  .toInt()
Importante

toInt() no sustituye a isInt().

6. req.body, req.params, req.query
req.body

Son los datos que vienen en el cuerpo de la petición.

Ejemplo:

POST /products
{
  "name": "Pizza",
  "price": 12,
  "scheduleId": 3
}

Entonces:

req.body.name
req.body.price
req.body.scheduleId
Cuándo usarlo

En POST, PUT, PATCH, cuando el enunciado habla de “recibe un objeto JSON en el cuerpo”.

req.params

Son los datos que vienen en la ruta.

Ejemplo:

GET /restaurants/5/schedules/9

Si la ruta es:

/restaurants/:restaurantId/schedules/:scheduleId

entonces:

req.params.restaurantId
req.params.scheduleId
Cuándo usarlo

Cuando la ruta tiene :algo.

req.query

Son los datos que vienen después de ? en la URL.

Ejemplo:

GET /products?page=2&active=true

Entonces:

req.query.page
req.query.active
Cuándo usarlo

Para filtros, paginación, búsqueda, ordenación.

7. check, body, param, query en validations
check('campo')

Busca el campo en varios sitios de la request.

Útil, pero menos preciso.

body('campo')

Valida solo req.body.

body('startTime')
param('campo')

Valida solo req.params.

param('restaurantId')
query('campo')

Valida solo req.query.

query('page')
Consejo para examen

Mejor usar:

body() si viene en JSON
param() si viene en la ruta
query() si viene en la query string

porque queda mucho más claro.

8. Cómo sacar una validation a partir de un enunciado

Hazte estas 5 preguntas:

1. ¿Cuál es la ruta?

De ahí sacas los params.

Ejemplo:

PUT /products/:productId

→ req.params.productId

2. ¿Qué recibe en el body?

De ahí sacas los body.

Ejemplo:

startTime
endTime
scheduleId
3. ¿Qué campos son obligatorios?

Entonces usas:

.exists()
.notEmpty()
4. ¿Qué formato deben tener?

Entonces usas:

.isInt()
.isFloat()
.isBoolean()
.isEmail()
.custom(...)
5. ¿Hay reglas contra la BD?

Entonces necesitas:

.custom(async (value, { req }) => { ... })
9. Estructura mental para una validation
Paso 1

Identifica de dónde viene cada dato:

ruta → param
body → body
query → query
Paso 2

Mira si es obligatorio u opcional:

obligatorio → exists().notEmpty()
opcional → optional()
Paso 3

Mira el tipo:

entero → isInt()
float → isFloat()
hora → custom(validateTimeFormat)
Paso 4

Mira si depende de otro campo:

Ejemplo:

endTime > startTime

Entonces haces un custom() que use req.body.startTime.

Paso 5

Mira si depende de la BD:

Ejemplo:

“el scheduleId debe pertenecer al restaurante del producto”

Entonces necesitas buscar en BD dentro de una validación custom.

10. Cuándo usar una validación custom

Cuando no basta con comprobar el tipo.

Casos típicos
comparar dos campos
validar una hora
comprobar que algo existe en BD
comprobar que una relación entre tablas es correcta

Ejemplo:

check('scheduleId')
  .optional({ nullable: true })
  .isInt()
  .custom(async (value, { req }) => {
    const schedule = await Schedule.findByPk(value)
    if (!schedule) {
      throw new Error('Schedule does not exist')
    }
    return true
  })
11. En el ejercicio de Schedules, qué te pide el enunciado
Schedule
crear tabla Schedules
relación con Restaurant
startTime y endTime
crear, editar, borrar horarios
validar formato hora y que endTime > startTime
Product
añadir scheduleId opcional
si se especifica, debe pertenecer al restaurante correcto
Restaurant
crear showWithActiveProducts
solo productos con horario activo
si scheduleId = null, no está activo


- ¿qué viene en la URL? --> params
- ¿qué me mandan en JSON? --> body
- ¿qué tengo que comprobar? --> validation



# ROUTES 
- get = obtener --> .index=todos .show=solo q
- post = crear --> .create = crear
- put = actualizar --> .update = actualizar
- delete = borrar --> .destroy/borrar

# ESTRUCTURAS
1. Estructura de una migration

Sirve para crear o modificar tablas en la base de datos.

Estructura general
'use strict'

module.exports = {
  async up (queryInterface, Sequelize) {
    // cambios que se aplican
  },

  async down (queryInterface, Sequelize) {
    // cambios para deshacer
  }
}
Ejemplo creando tabla
'use strict'

module.exports = {
  async up (queryInterface, Sequelize) {
    await queryInterface.createTable('Schedules', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      startTime: {
        type: Sequelize.TIME,
        allowNull: false
      },
      endTime: {
        type: Sequelize.TIME,
        allowNull: false
      },
      restaurantId: {
        type: Sequelize.INTEGER,
        allowNull: false,
        references: {
          model: 'Restaurants',
          key: 'id'
        },
        onUpdate: 'CASCADE',
        onDelete: 'CASCADE'
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE
      }
    })
  },

  async down (queryInterface, Sequelize) {
    await queryInterface.dropTable('Schedules')
  }
}
Ejemplo añadiendo columna
'use strict'

module.exports = {
  async up (queryInterface, Sequelize) {
    await queryInterface.addColumn('Products', 'scheduleId', {
      type: Sequelize.INTEGER,
      allowNull: true,
      references: {
        model: 'Schedules',
        key: 'id'
      },
      onUpdate: 'CASCADE',
      onDelete: 'SET NULL'
    })
  },

  async down (queryInterface, Sequelize) {
    await queryInterface.removeColumn('Products', 'scheduleId')
  }
}
Idea clave
up = lo que haces
down = cómo lo deshaces
2. Estructura de un model

Sirve para definir la entidad en Sequelize.

Estructura general
'use strict'

module.exports = (sequelize, DataTypes) => {
  const NombreModelo = sequelize.define('NombreModelo', {
    // atributos
  }, {
    // opciones
  })

  NombreModelo.associate = function (models) {
    // asociaciones
  }

  return NombreModelo
}
Ejemplo
'use strict'

module.exports = (sequelize, DataTypes) => {
  const Schedule = sequelize.define('Schedule', {
    startTime: {
      type: DataTypes.TIME,
      allowNull: false
    },
    endTime: {
      type: DataTypes.TIME,
      allowNull: false
    },
    restaurantId: {
      type: DataTypes.INTEGER,
      allowNull: false
    }
  }, {})

  Schedule.associate = function (models) {
    Schedule.belongsTo(models.Restaurant, {
      foreignKey: 'restaurantId',
      as: 'restaurant'
    })

    Schedule.hasMany(models.Product, {
      foreignKey: 'scheduleId',
      as: 'products'
    })
  }

  return Schedule
}
Partes
sequelize.define(...) → define el modelo
atributos → campos
opciones → {} o cosas como timestamps: false
associate → relaciones
3. Asociaciones típicas en models
belongsTo

La tabla tiene la FK.

Product.belongsTo(models.Restaurant, {
  foreignKey: 'restaurantId',
  as: 'restaurant'
})
hasMany

La otra tabla tiene muchas filas relacionadas.

Restaurant.hasMany(models.Product, {
  foreignKey: 'restaurantId',
  as: 'products'
})
hasOne

Relación 1:1.

User.hasOne(models.Profile, {
  foreignKey: 'userId',
  as: 'profile'
})
belongsToMany

Relación N:N con tabla intermedia.

Product.belongsToMany(models.Order, {
  through: 'OrderProducts',
  foreignKey: 'productId',
  otherKey: 'orderId',
  as: 'orders'
})
4. Estructura de un controller

Sirve para manejar la lógica de las rutas.

Estructura general
import { Modelo } from '../models'

const nombreFuncion = async function (req, res) {
  try {
    // lógica
    return res.status(200).json(...)
  } catch (error) {
    return res.status(500).send(error.message)
  }
}

export { nombreFuncion }
Ejemplo real
import { Schedule } from '../models'

const index = async function (req, res) {
  try {
    const schedules = await Schedule.findAll({
      where: { restaurantId: req.params.restaurantId }
    })
    return res.json(schedules)
  } catch (error) {
    return res.status(500).send(error.message)
  }
}

const create = async function (req, res) {
  try {
    const schedule = await Schedule.create({
      startTime: req.body.startTime,
      endTime: req.body.endTime,
      restaurantId: req.params.restaurantId
    })

    return res.status(201).json(schedule)
  } catch (error) {
    return res.status(500).send(error.message)
  }
}

export { index, create }
Estructura típica de cada función
try
buscar/crear/editar/borrar
devolver res
catch con error 500
5. Estructura de un route file

Sirve para enlazar rutas con controladores y middlewares.

Estructura general
import express from 'express'
import * as NombreController from '../controllers/NombreController'
import { middleware1, middleware2 } from '../middlewares/Algo'

const router = express.Router()

router.route('/ruta')
  .get(NombreController.index)
  .post(middleware1, middleware2, NombreController.create)

export default router
Ejemplo
import express from 'express'
import * as ScheduleController from '../controllers/ScheduleController'
import { isLoggedIn, hasRole } from '../middlewares/AuthMiddleware'

const router = express.Router()

router.route('/restaurants/:restaurantId/schedules')
  .get(ScheduleController.index)
  .post(isLoggedIn, hasRole('owner'), ScheduleController.create)

router.route('/restaurants/:restaurantId/schedules/:scheduleId')
  .put(isLoggedIn, hasRole('owner'), ScheduleController.update)
  .delete(isLoggedIn, hasRole('owner'), ScheduleController.destroy)

export default router
6. Estructura de una validation

Sirve para validar lo que llega en la request.

Estructura general
import { body, param, query, check } from 'express-validator'

const create = [
  // validaciones
]

const update = [
  // validaciones
]

export { create, update }
Ejemplo sencillo
import { body, param } from 'express-validator'

const create = [
  param('restaurantId')
    .isInt().withMessage('restaurantId must be an integer')
    .toInt(),

  body('startTime')
    .exists().withMessage('startTime is required')
    .bail()
    .notEmpty().withMessage('startTime cannot be empty'),

  body('endTime')
    .exists().withMessage('endTime is required')
    .bail()
    .notEmpty().withMessage('endTime cannot be empty')
]

export { create }
Estructura de una validación típica
exists() → existe el campo
notEmpty() → no está vacío
isInt() / isFloat() / etc → tipo
optional() → opcional
custom() → validación propia
toInt() → convierte
7. Estructura de una validación custom

Se usa cuando necesitas una regla especial o mirar la BD.

Ejemplo simple
const validateSomething = (value) => {
  if (value < 0) {
    throw new Error('Value cannot be negative')
  }
  return true
}
Ejemplo con req
const validateEndTimeAfterStartTime = (value, { req }) => {
  if (value <= req.body.startTime) {
    throw new Error('endTime must be greater than startTime')
  }
  return true
}
Ejemplo async con base de datos
import { Schedule } from '../../models'

const checkScheduleExists = async (value) => {
  const schedule = await Schedule.findByPk(value)
  if (!schedule) {
    throw new Error('Schedule does not exist')
  }
  return true
}
Uso
body('scheduleId')
  .optional({ nullable: true })
  .isInt()
  .bail()
  .custom(checkScheduleExists)
8. Estructura de un middleware

Un middleware se ejecuta antes del controller.

Estructura general
const nombreMiddleware = (req, res, next) => {
  // hacer algo
  next()
}

export { nombreMiddleware }
Ejemplo comprobando login
const isLoggedIn = (req, res, next) => {
  if (!req.user) {
    return res.status(401).send('Unauthorized')
  }
  next()
}

export { isLoggedIn }
Ejemplo con parámetro
const hasRole = (role) => {
  return (req, res, next) => {
    if (!req.user || req.user.role !== role) {
      return res.status(403).send('Forbidden')
    }
    next()
  }
}

export { hasRole }
Idea clave
si todo va bien → next()
si no → return res.status(...).send(...)
9. Estructura de helpers de validation

Se usan para reutilizar funciones.

Estructura general
const helperName = (value, { req }) => {
  // lógica
  return true
}

export { helperName }
Ejemplo
const validateTimeFormat = (value) => {
  const regex = /^([0-1]\d|2[0-3]):([0-5]\d):([0-5]\d)$/
  if (!regex.test(value)) {
    throw new Error('Invalid time format')
  }
  return true
}

export { validateTimeFormat }
10. Estructura para usar validation en controller

A veces el controller ejecuta las validaciones manualmente.

Ejemplo
import { validationResult } from 'express-validator'
import * as ScheduleValidation from './validation/ScheduleValidation'

const handleValidation = async (req, res, validations) => {
  for (const validation of validations) {
    await validation.run(req)
  }

  const errors = validationResult(req)
  if (!errors.isEmpty()) {
    return res.status(422).json(errors.array())
  }
  return null
}

Y luego:

const create = async function (req, res) {
  try {
    const validationError = await handleValidation(req, res, ScheduleValidation.create)
    if (validationError) return validationError

    // resto
  } catch (error) {
    return res.status(500).send(error.message)
  }
}
11. Estructura básica de acceso a datos en controller
Buscar uno
const product = await Product.findByPk(req.params.productId)
Buscar muchos
const products = await Product.findAll({
  where: { restaurantId: req.params.restaurantId }
})
Buscar uno con condición
const schedule = await Schedule.findOne({
  where: {
    id: req.params.scheduleId,
    restaurantId: req.params.restaurantId
  }
})
Crear
const product = await Product.create({
  name: req.body.name,
  price: req.body.price
})
Actualizar
product.name = req.body.name
await product.save()
Borrar
await product.destroy()
12. Estructura de respuestas HTTP en controller
OK
return res.status(200).json(data)

o simplemente:

return res.json(data)
Created
return res.status(201).json(data)
Bad request / validation
return res.status(422).json(errors.array())
Unauthorized
return res.status(401).send('Unauthorized')
Forbidden
return res.status(403).send('Forbidden')
Not found
return res.status(404).send('Not found')
Server error
return res.status(500).send(error.message)
13. Estructura mental de cada archivo
Migration

Qué cambia en la BD

up / down
Model

Qué es la entidad

atributos + asociaciones
Route

Qué URL llama a qué controller

router.route(...).get/post/put/delete(...)

- Validation

Qué se comprueba de la request

body / param / query / custom

- Middleware

Qué se hace antes de llegar al controller

(req, res, next)
Controller

Qué lógica ocurre cuando se llama a la ruta

buscar / crear / editar / borrar / responder
14. Plantilla ultra rápida de examen
Migration
'use strict'
module.exports = {
  async up (queryInterface, Sequelize) {},
  async down (queryInterface, Sequelize) {}
}

- Model
module.exports = (sequelize, DataTypes) => {
  const Model = sequelize.define('Model', {}, {})
  Model.associate = function (models) {}
  return Model
}

- Route
import express from 'express'
const router = express.Router()
router.route('/ruta').get(controller.index)
export default router
Validation
import { body, param } from 'express-validator'
const create = []
export { create }
Middleware
const middleware = (req, res, next) => {
  next()
}

- Controller
const action = async function (req, res) {
  try {
    return res.json(...)
  } catch (error) {
    return res.status(500).send(error.message)
  }
}



# DATOS IMPORTANTES 

1. findAll()

Sirve para buscar varios registros.

Devuelve un array.

Ejemplo
const products = await Product.findAll()

Esto trae todos los productos.

Resultado típico
[
  { id: 1, name: 'Pizza' },
  { id: 2, name: 'Hamburguesa' }
]
Con where
const products = await Product.findAll({
  where: { restaurantId: 3 }
})

Esto trae todos los productos cuyo restaurantId sea 3.

Idea mental

findAll = “dame todos los que cumplan esta condición”.

2. findByPk()

Pk = primary key.

Sirve para buscar un registro por su clave primaria, normalmente el id.

Ejemplo
const product = await Product.findByPk(5)

Busca el producto con id = 5.

Resultado
si existe → te devuelve el objeto
si no existe → devuelve null
Ejemplo real
const restaurant = await Restaurant.findByPk(req.params.restaurantId)

Si req.params.restaurantId es 7, busca el restaurante con id = 7.

Idea mental

findByPk = “búscame este elemento por su id”.

3. findOne()

Sirve para buscar un solo registro que cumpla una condición.

Devuelve:

un objeto
o null si no existe
Ejemplo
const schedule = await Schedule.findOne({
  where: {
    id: req.params.scheduleId,
    restaurantId: req.params.restaurantId
  }
})

Busca un horario que cumpla ambas cosas:

id = scheduleId
restaurantId = restaurantId
Cuándo usarlo

Cuando quieres buscar un único elemento, pero no solo por PK.

Idea mental

findOne = “dame uno que cumpla esta condición”.

4. Diferencia rápida
findAll

Devuelve muchos → array

const products = await Product.findAll()
findByPk

Devuelve uno por id → objeto o null

const product = await Product.findByPk(3)
findOne

Devuelve uno por condición → objeto o null

const product = await Product.findOne({
  where: { name: 'Pizza' }
})
5. Cuándo usar cada uno
Usa findAll() cuando:

quieres una lista

Ejemplos:

todos los productos de un restaurante
todos los schedules de un restaurante
todos los pedidos de un usuario
const schedules = await Schedule.findAll({
  where: { restaurantId: req.params.restaurantId }
})
Usa findByPk() cuando:

quieres buscar por id

Ejemplos:

restaurante con id 5
producto con id 8
schedule con id 2
const product = await Product.findByPk(req.params.productId)
Usa findOne() cuando:

quieres buscar un único elemento con una condición más concreta

Ejemplos:

el schedule con cierto id que además pertenece a cierto restaurante
el pedido activo de un usuario
el producto con cierto nombre en cierto restaurante
const schedule = await Schedule.findOne({
  where: {
    id: req.params.scheduleId,
    restaurantId: req.params.restaurantId
  }
})
6. Por qué no usar siempre findByPk()

Porque findByPk() solo busca por la clave primaria.

Por ejemplo:

Schedule.findByPk(req.params.scheduleId)

esto solo comprueba el id.

Pero a veces tú necesitas comprobar también que ese horario pertenece a ese restaurante.

Entonces mejor:

Schedule.findOne({
  where: {
    id: req.params.scheduleId,
    restaurantId: req.params.restaurantId
  }
})

Así te aseguras de ambas cosas.

7. where

where es la condición de búsqueda.

Ejemplo
where: { restaurantId: 3 }

significa algo como:

WHERE restaurantId = 3
Varias condiciones
where: {
  id: 5,
  restaurantId: 3
}

significa:

WHERE id = 5 AND restaurantId = 3
8. Qué devuelve cada uno
findAll()

Siempre devuelve array.

si encuentra datos → array con elementos
si no encuentra → []
const schedules = await Schedule.findAll(...)
console.log(schedules) // [] o varios objetos
findByPk()

Devuelve objeto o null.

const schedule = await Schedule.findByPk(4)
findOne()

Devuelve objeto o null.

const schedule = await Schedule.findOne(...)
9. Comprobaciones típicas
Con findByPk() o findOne()
const product = await Product.findByPk(req.params.productId)

if (!product) {
  return res.status(404).send('Product not found')
}

Porque puede devolver null.

Con findAll()
const products = await Product.findAll({
  where: { restaurantId: req.params.restaurantId }
})

Aquí no suele hacer falta:

if (!products)

porque findAll() devuelve array, no null.

Si no hay resultados, será:

[]
10. Otros muy usados
create()

Crea un registro nuevo.

const product = await Product.create({
  name: 'Pizza',
  price: 10
})
save()

Guarda cambios en un objeto que ya has cargado.

const product = await Product.findByPk(1)
product.name = 'Pizza BBQ'
await product.save()
destroy()

Borra un registro.

const product = await Product.findByPk(1)
await product.destroy()
11. Ejemplos típicos de controller
Listar
const index = async function (req, res) {
  try {
    const products = await Product.findAll({
      where: { restaurantId: req.params.restaurantId }
    })
    return res.json(products)
  } catch (error) {
    return res.status(500).send(error.message)
  }
}
Mostrar uno
const show = async function (req, res) {
  try {
    const product = await Product.findByPk(req.params.productId)

    if (!product) {
      return res.status(404).send('Product not found')
    }

    return res.json(product)
  } catch (error) {
    return res.status(500).send(error.message)
  }
}
Buscar uno con condiciones
const showSchedule = async function (req, res) {
  try {
    const schedule = await Schedule.findOne({
      where: {
        id: req.params.scheduleId,
        restaurantId: req.params.restaurantId
      }
    })

    if (!schedule) {
      return res.status(404).send('Schedule not found')
    }

    return res.json(schedule)
  } catch (error) {
    return res.status(500).send(error.message)
  }
}
12. Chuleta rápida
findAll()

Busca muchos.

const items = await Model.findAll({ where: {...} })

Devuelve:

[]

o varios objetos.

findByPk(id)

Busca uno por id.

const item = await Model.findByPk(id)

Devuelve:

objeto o null
findOne({ where: ... })

Busca uno por condición.

const item = await Model.findOne({
  where: {...}
})

Devuelve:

objeto o null
13. Truco para examen

Piensa así:

quiero una lista → findAll
quiero buscar por id → findByPk
quiero buscar uno con condiciones → findOne
14. Ejemplo con tu ejercicio
Todos los horarios de un restaurante
const schedules = await Schedule.findAll({
  where: { restaurantId: req.params.restaurantId }
})
Un restaurante por id
const restaurant = await Restaurant.findByPk(req.params.restaurantId)
Un horario concreto de ese restaurante
const schedule = await Schedule.findOne({
  where: {
    id: req.params.scheduleId,
    restaurantId: req.params.restaurantId
  }
})
15. Diferencia importante entre findByPk y findOne

Esto:

const schedule = await Schedule.findByPk(req.params.scheduleId)

solo mira el id.

Esto:

const schedule = await Schedule.findOne({
  where: {
    id: req.params.scheduleId,
    restaurantId: req.params.restaurantId
  }
})

mira el id y además que pertenezca a ese restaurante.
