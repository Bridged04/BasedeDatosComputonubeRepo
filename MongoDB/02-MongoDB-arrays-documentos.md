# Busqueda en Arrays y Documentos Anidados

## Buscar dentro de un documento anidado
```json
db.cuidades.find({
    alcalde:{
        nombre:"Crystal",
        apellidos:"Long",
        edad:76
    }
})

db.ciudades.find({"alcalde.nombre":"Crystal"})
```
1. Seleccionar las ciudades donde los alcaldes tengan mas de 70 años 
```json
db.ciudades.find({
    "alcalde.edad":{$gt:70}
})
```
2. Realizar la consulta anterior pero utilizanado una proyecccion
```json
db.ciudades.find({
    "alcalde.edad":{$gt:70}}, {_id:0, nombre:1, "alcalde.edad":1})
```
3. Seleccionar las ciudades donde el alcalde tenga 76 años o 41
```json
db.ciudades.find(
   {$or:[
            {"alcalde.edad":76},
            {"alcalde.edad":41}
        ]})
```


## Consultas con Arrays

1. Crear una coleccion cazas 
db.createCollection("cazas")

2. Cargar Documentos de la coleccion cazas, de la data cazas 
```json
db.cazas.insertMany([
   { modelo: "f-35", paises: ["usa", "inglaterra"],dimensiones: [15.60,10.70,4.36]},
   { modelo: "f-16", paises: ["usa", "belgica","egipto","israel","otros"],dimensiones: [15,9.96,4]},
   { modelo: "f-15", paises: ["usa", "israel","japon"],dimensiones: [19,13,6]},
    { modelo: "eurofighter", paises: ["inglaterra", "alemania","españa","italia"],dimensiones: [16,11,4]},
{ modelo: "f-18", paises: ["usa", "canada","españa","otros"],dimensiones: [17.07,11.43,4.66]},
{ modelo: "rafale", paises: ["francia", "egipto"],dimensiones: [15,11,5]},
{ modelo: "su-34", paises: ["rusia"],dimensiones: [23,15,6]}

]);
```

## Ejemplos con consulta con Arrays

1. Buscar los documentos que tengan usa como paises
```json
db.cazas.find({paises:"usa"})
```
2. Buscar los documentos donde cualquier elemento de array dimensiones sea mayor a 5
```json
db.cazas.find({dimensiones:{$gt:5}})
```

3. Buscar los documentos donde las dimensiones del avion por ancho que es la posicion 1, sea superior a 14

```json
db.cazas.find(
    {"dimensiones.1":{$gt:14}},
    {_id:0, modelo:1, dimensiones:1})
```

###   Arrays. Operador $all

1. Buscar los aviones que estan sirviendo en egipto y en israel.
```json
db.cazas.find({paises:{$all:['egipto','israel']}},{_id:0, modelo:1, dimensiones:1}))

db.cazas.find({$and:[{paises:'egipto'},{paises:'israel'}]},{_id:0, modelo:1, dimensiones:1}))
```

2. seleccionar los cazas que sirven en inglaterra y españa
```json
db.cazas.find(  
            {$and:[
                    {paises:'inglaterra'},
                    {paises:'españa'}
                  ]
            })

db.cazas.find({paises:{$all:['inglaterra','españa']}})
```


### Arrays. Operador $elementMatch -> perimite hacer busquedas dentro de arrys, este busca la lista de condiciones que estan dentro del operador.

1. Buscar que aviones tienen uso dentro de inglaterra

```json
db.cazas.find({paises:{$elemMatch:{$eq:'inglaterra'}}})
```

2. Buscar los aviones donde las dimensiones sean mayores a 20 o menores a 15
```json
db.cazas.find({dimensiones:{$elemMatch:{$gt:20 ,$lt:15}}})
```

### Buscar en arrays de documentos anidados 

1. Utilizar la coleccion ciudades
2. Buscar los consejeros de nombre Jeri Flowers de edad 78

```json
db.ciudades.find({consejeros:{identificador:1, nombre:'Jeri Flowers', edad:78}})
```

3. Buscar en consejeros donde tenga una edad mayor a 70
```json
db.ciudades.find({"consejeros.edad":{$gt:70}})
```
4. Buscar en consejeros en la posicion 2 que esten entre 70 y 80 a;os
```json
db.ciudades.find({"consejeros.2.edad":{$gt:70, $lt:80}},{_id:0, "consejeros.edad":1, "consejeros.nombre":1})
```
5. Buscar los consejeros de la posicion 0 que sen mayores a 70
```json
db.ciudades.find({"consejeros.0.edad":{$gt:70}},{_id:0, nombre:1,"consejeros.nombre":1, "consejeros.edad":1})
```
6. Buscar consejeros que sean mayores a 70
```json
db.ciudades.find({"consejeros.edad":{$gt:70}},{_id:0, "alcalde.nombre":1,"alcalde.edad":1})
```