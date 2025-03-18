# Modelo de datos en MongoDB

- Modelos   
    -Embebidos
    -Referenciados

- Modelos Embebidos

**Uno a Uno**
```json
db.departamentos.insertMany(
    [
        {
            _id:1,
        nombre:"Tecnologias de Informacion",
        responsable:{
            nombre:'Monico',
            apellidos:'Martinezz Perez'

        },
        descripcion:'ejemplo de deparamento'
        },
        {
            _id:2,
        nombre:"Contabilidad",
        responsable:{
            nombre:'Raul',
            apellidos:'Lopez Hernandez'

        },
        descripcion:'ejemplo de deparamento'
        }
    ]
)

```

- Modelos Referenciados

**Uno a Uno**
```json
db.localidades.insertMany(
[
    {
        _id:'BA',
        ciudad:'Buenos Aires',
        pais:'Argentina',
        poblacion:'16 millones',
        turismo:["edificios","tango","gastronomia","museos","parques"],
        direccion:"Avenidad Tortolos",
        cod_departamentos:1
    },
    {
        _id:'SA',
        ciudad:'Santiago',
        pais:'Chile',
        poblacion:'20 millones',
        turismo:["iglesias","vino","gastronomia","museos"],
        direccion:"Soy la vaca del corral",
        cod_departamentos:2

    }
])

```
```json
db.departamentos.aggregate(
    [
        {
            $lookup:
            {
                from:"localidades",
                localField:"_id",
                foreignField:"cod_departamentos",
                as: "localidades"
            }
        }
    ]
)
```

```json
db.categoria.insertMany(
[
    {
        _id:1,
        nombre:"lacteos"
    }
    {
        _id:2,
        nombre:""
    }
])

