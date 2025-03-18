
# Practica 1: Base de daots, colecciones e inserts

1. Conectarnos con mongosh a Mongodb
2. .Crear una base de datos denominda "Curso"
   ```json
   use curso
   ```

3. Verificar que la base datos no existe
   ```json
   show dbs
   ```

4. Crear una coleccion denominada "facturas" con el comando
   ```json
   db.createCollection('facturas')
   ```
   

4. y comprobar la coleccion 
   ```json
   show collections
   ```

5. Insertar un documento con los siguientes datos

| Codigo      | Valor          |
|-------------|----------------|
| Cod_Factura | 10             |
| Ciente      | Frutas Ramirez |
| Total       | 223            |

| Codigo      | Valor          |
|-------------|----------------|
| Cod_Factura | 20             |
| Ciente      | Ferreteria Juan |
| Total       | 140            |


```json
db.facturas.insertOne(
{
 cod_factura: 10,
 cliente: 'Frutas Ramirez',
 total: 223

}
)
```

```json
db.facturas.insertOne(
{
 cod_factura: 20,
 cliente: 'Ferreteria Juan',
 total: 140

}
)
```

6. Crear una nueva coleccion pero usando directamente el insertOne
   Insertar un documento en la coleccion "Productos" con los siguientes datos:

| Codigo       | Valor           |
|--------------|-----------------|
| Cod_Producto | 1               |
| Ciente       | Tornillo x1     |
| Precio       | 2               |
| Unidades     | 1500            |

```json
db.productos.insertOne(
{
 cod_Producto: 1,
 cliente: 'Tornillo x 1',
 precio: 2,
 unidades: 1500
}
)
```

7. Crear un nuevo documento de producto que contenga un array.
   con los siguientes datos:

| Codigo       | Valor           |
|--------------|-----------------|
| Cod_Producto | 1               |
| Nombre       | Martillo        |
| Precio       | 20              |
| Unidades     | 50              |
| Fabricantes     | fab1, fab2, fab3, fab4 |

```json
db.productos.insertOne(
{
 cod_Producto: 2,
 cliente: 'Martillo',
 precio: 20,
 unidades: 50,
 Fabricantes: ['fab1', 'fab2', 'fab3', 'fab4']
}
)
```

8. Borrar la coleccion "facturas" y comprobar que se borro
``json
 db.facturas.drop()
```
``json
show collections
```
9. Inserat un documento en una coleccion denominada **Fabricantes**
   para probar los subdocumentos y la clave_id personalizada

| Codigo       | Valor           |
|--------------|-----------------|
| id           | 1               |
| Nombre       | fab1            |
| Localidad    | ciudad: buenos aires, pais: argentina, calle: 'Calle Pez', cod: 29000          |

```json
db.fabricantes.insertOne(
{
    _id: 1,
    nombre: 'fab1',
    localidad: {
        ciudad:'Buenos Aires',
        pais: 'Argentina',
        calle: 'Calle Pez',
        cod: 29000
    }
}
)
```

10. Realizar una insercion de varios documentos en la coleccion "productos"

| Codigo       | Valor           |
|--------------|-----------------|
| Cod_Producto | 13              |
| Nombre       | Alicates        |
| Precio       | 10              |
| Unidades     | 25              |
| Fabricantes  | fab1, fab2, fab5 |

| Codigo       | Valor           |
|--------------|-----------------|
| Cod_Producto | 4               |
| Nombre       | Arandela        |
| Precio       | 1               |
| Unidades     | 500             |
| Fabricantes  | |

```json
db.productos.insertMany(
[
{
 cod_Producto: 3,
 cliente: 'Alicates',
 precio: 10,
 unidades: 25,
 Fabricantes: ['fab1', 'fab2', 'fab5']
},

{
 cod_Producto: 4,
 cliente: 'Arandela',
 precio: 1,
 unidades: 500,
 Fabricantes: ['fab2', 'fab3', 'fab4']
}
]
)
```

