# Consultas


1. Cargar el archivo empleados.json 

-En local:
comando: 
 mongoimport --db curso --collection empleados --file empleados.json\


2. Utilizar la base de datos curso

```json
use curso
```
3. Buscar todos los empleados que trabajen en Google
```json
  db.empleados.find(
{
  empresa:'Google'
}
)

db.empleados.find(
{
    empresa:{$eq:"Google"}
})

```
4. Empleados que vivan en peru
```json
  db.empleados.find(
  {
    pais:'Peru'

  })
```
5. Empleados que ganen mas de 8000 dolares
```json
  db.empleados.find(
  {
    salario:{$gt:8000}
  })
```

6. Empleados con ventas inferiores a 10000 
```json
   db.empleados.find(
  {
    ventas:{$lt:10000}
  })
```

7. realizar la consulta anterio pero devolviendo una sola fila 

```json
   db.empleados.findOne(
  {
    ventas:{$lt:10000}
  }
   )
```

8. empleados que trabajan en google o en yahoo con el operador $in 
```json
   db.empleados.find(
   {
    empresa:{$in:['Yahoo', 'Google']}
   })
```

9. empleados de amazon que ganen mas de 8 mil dolares 
```json
   db.empleados.find(
   {
  $and:[
  {salario:{$gt:8000}},
  {empresa:'Amazon'} 
  ]
 }
)


db.empleados.find(
{
    empresa:'Amazon',
    salario:{$gt:8000}
})

```

10. Empleados que trabajan en google o en yahoo con el operador $or
```json
db.empleados.find(
   {
  $or:[
  {empresa:{$eq:'Google'}},
  {empresa:'Amazon'} 
  ]
 }
)
```

11. Empleados que trabajen en Yahoo que ganen mas de 6 mil o empleados que trabajen en google que tengan ventas inferiores a 20 mil
```json
 db.empleados.find(
  { 
    $or:[
        $and:[{empresa:"Yahoo"},{salario:{$gt:6000}}],
        $and:[{empresa:{$eq:"Google"}},{ventas:{$lt:20000}}]
    ]
  }
 )
```
12. visualizar el nombre apellidos y pais de todo empleado
```json
   db.empleados.find({nombre:0)

```