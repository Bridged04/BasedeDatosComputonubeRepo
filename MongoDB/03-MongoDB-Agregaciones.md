# Agregaciones en MongoDB(Framework)

## Metodos para realizar agregaciones simples 
- distinct(): Devuelve valores no duplicados 
- countdocument(): Cuenta los documentos en una coleccion
- stimatedDocumentCount(): Cuenta de manera aproximada durante un periodo de tiempo


## Una agregation pipeline consta de una o mas etapas (stage) que procesan documentos

1. Cada etapa realiza una operacion en los documentos de entrada. Por ejemplo una fase pude filtrar documentos, agrupar documentos y calcular valores.

2. Los documentos que se generan en una  fase pasan a la siguiente fase.

3. Puede devolver resultados para grupode de documentos como totales, maximo, minimo, etc.

#### Se utiliza la clausula "aggregate"

- Existen una serie de operadores que se pueden utilizar para realizar operaciones. Se tienen disttintos tipos: etapa, comparacion, booleanos, aritmeticos, de cadena, etc.

## Metodos simples: countDocuments() y distinct()

1. Contar los documentos de la coleccion
```json
db.libros.countDocuments()
```
2. Contar los documentos de la editorial Terra
```json
db.libros.countDocuments({editorial:{$eq:'Terra'}})
```
3. Mostar todos los libros mostrando solamente la editorial
```json
db.libros.find({},{_id:0, editorial:1})
```
4. Mostrar todas las 
```json
db.libros.distinct('editorial')
```

[Documentacion de agregaciones](https://www.mongodb.com/docs/manual/aggregation/)

## $match. Una pipiline basica
## tiene una funcion de etapa
```json
db.libros.aggregate(
    [
        {$match:{editorial:"Terra"}}
    ]
)
```
## $ptoject. Incluir y renombrar campos 
```json
db.libros.aggregate(
    [
        {
            $match:{editorial:'Terra'}
        },
        {
            $project:{
                _id:0,
                titulo:1,
                precio:1,
                NombreEditorial:"$editorial",
                editorial:1
            }
        }
    ]
)

- Generado por Mongo Compass
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        precio: 1,
        cantidad: 1,
        NombreEditorial: "$editorial",
        editorial: 1
      }
  }
]

[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  }
]
```

## $sort. Ordenaciones

```json
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      /**
       * Provide any number of field/order pairs.
       */
      {
        precio: 1
      }
  }
]
```
## $group. Agrupaciones
[Agrupaciones](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/)



-- Cuantos documentos hay por cada una de las editoriales
```json
[
  {
    $group:
        _id: "editorial",
        "Numero de Documentos": {
          $count: {}
        }
      }
  
]
```

--Cuantos documentos hay por cada una de las editoriales por número de
--documentos de manera descendente 



-- Utilizando MongoAtlas Base de datos sample_airnbnb
-- Agrupar por tipo de propiedad mostrando el numero de propiedades y
-- el promedio de sus precios
```json
{
  _id: "$property_type",
  Numero: {
    $count: {}
  },
  Media:
  {
    $avg: "$price"
    
  }
}
```
-- Operadores $set 
```json
[
  
    {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  ,
  {
    $set:
      /**
       * field: The field name
       * expression: The expression.
       */
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  }
]
```

-- Operador $unset
```json
['Media', 'Media_Total']
```
-- Quitando solo un campo
'Media' 


-- Creando nuevas colecciones utilizando el operador $out
-- Nota: debe ser el último en la agregación
```json

[
  ¿
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      },
  {
    $set:
      /**
       * field: The field name
       * expression: The expression.
       */
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  },
  {
    $unset:
      /**
       * Provide the field name to exclude.
       * To exclude multiple fields, pass the field names in an array.
       */
      "Media"
  },
  {
    $out:
      /**
       * Provide the name of the output database and collection.
       */
      {
        db: "sample_airbnb",
        coll: "media_propiedades"
        /*
    timeseries: {
      timeField: 'field',
      bucketMaxSpanSeconds: 'number',
      granularity: 'granularity'
    }
    */
      }
  }
]

```

--Ejemplos con operadores de comparacion y logicos
```json
[
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        price: 1,
        name: 1,
        room_type: 1,
        caro: {
          $gt: ["$price", 300]
        },
        medio: {
          $and: [
            {
              $gt: ["$price", 100]
            },
            {
              $lte: ["$price", 300]
            }
          ]
        },
        baratito: {
          $lte: ["$price", 100]
        }
      }
  }
]
```

--$cond, devuelve valores segun una condicion(es parecido a un operador ternario de un lenguaje de programacion)
```json
[
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        price: 1,
        name: 1,
        room_type: 1,
        caro: {
          $cond: [
            {
              $gt: ["$price", 300]
            },
            "Si",
            "No"
          ]
        },
        medio: {
          $and: [
            {
              $gt: ["$price", 100]
            },
            {
              $lte: ["$price", 300]
            }
          ]
        },
        baratito: {
          $cond: [
            {
              $lte: ["$price", 100]
            },
            "Si",
            "No"
          ]
        }
      }
  }
]
```

-Views
```json
db.createView("ganancias_libros", 
"libros",
  [
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Biblio"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre editorial": "$editorial",
        "Total de ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      /**
       * Provide any number of field/order pairs.
       */
      {
        "Total de ganancias": -1
      }
  }
]
)


db["ganancias_libros"].find(
{"Total gananciias":{$lte:240}},
{titulo:1,  "Total ganancias":1}).
sort({titulo:-1})
```