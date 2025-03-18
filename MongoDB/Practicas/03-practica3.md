# Practica 3. Updates y Deletes

1. Cambiar el salario del empleado Imogene Nola y se le asgina 8000
```Json
 db.empleados.updateOne({nombre:"Imogene"},{$set:{salario:8000}})
 ```
2. Cambiar "Belgium" por "Belgica" en los empleados 
(debe haber dos)
```Json
db.empleados.updateMany({pais:"Belgium"},{$set:{pais:"Belgica"}})
```
3. Incrementar el salario de todos los empleados de Google en 1000
```Json
 db.empleados.updateMany({empresa:"Google"},{$inc:{salario:1000}})
```
4. Reemplazar el empleado Omar Gentry por el siguiente documento

```Json
{
"nombre": "Omar",
"apellidos": "Gentry",
"correo": "sin correo",
"direccion": "Sin calle",
"region": "Sin region",
"pais": "Sin pais",
"empresa": "Sin empresa",
"ventas": 0,
"salario": 0,
"departamentos": "Este empleado ha sido anulado"
}

 db.empleados.replaceOne(
    { nombre: "Omar", apellidos:"Gentry" },
    {
         nombre: "Omar",
         apellidos: "Gentry",
         correo: "sin correo",
         direccion: "Sin calle",
         region: "Sin region",
         pais: "Sin pais",
         empresa: "Sin empresa",
         ventas: 0,
         salario: 0,
         departamentos: "Este empleado ha sido anulado"
     }
)
```

5. Con un find comprobar que el empleado ha sido modificado
```Json
 db.empleados.find({nombre:"Omar", apellidos:"Gentry"})
```
6. Borrar todos los empleados que ganen mas de 8500,
Nota: deben ser borrados 3 documentos
```Json
db.empleados.deleteMany({salario:{$gt:8500}})
```
7. Visualizar con una expresion regular todos los empleados con apellidos que comiencen con "R"
```Json
 db.empleados.find({apellidos:{$regex:/^R/}})
```
8. Buscar todas las regiones que contengan una "v"
Hacerlo con el operador $regex y que no distinga mayusculas y minusculas. Deben salir 2
 ```json
 db.empleados.find({region:{$regex:/v/i}})
```
9. Visualizar los apellidos de los empleados ordenador por el propio apellido.
```json
 db.empleados.find({},{_id:0,nombre:0,correo:0,direccion:0,region:0,pais:0,empresa:0,ventas:0,salario:0,departamentos:0}).sort({apellidos:-1})
```
10. Indicar el numero de empleados que trabajan en Google
 ```json
 db.empleados.find({empresa:"Google"}).size()
```
11. Borrar la coleccion empleados y la base de datos
```json
 db.empleados.drop()

 db.dropDatabase()
```