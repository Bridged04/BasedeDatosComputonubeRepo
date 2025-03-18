# Agregaciones

1. Para hacer esta práctica vamos a cargar unos datos ficticios de empresas.
2. Tienes un fichero denominado “productos.json”
3. Debes poner el resultado de las consultas en cada apartado

- Cuenta los productos de tipo “medio”, usando un método básico
```json
db.productos.find({ tipo: "medio" }).count()
```
![Contar productos](imgPractica5\Contar-Productos.png)

- Indicar con un distinct, las empresas (fabricantes) que hay en la colección
```json
db.productos.distinct("fabricante")
```
![Empresas-en-Coleccion](imgPractica5\Empresas-coleccion.png)

- Usando aggregate, visualizar los productos que tengan más de 80 unidades
```json
db.productos.aggregate([
  { $match: { unidades: { $gt: 80 } } }])
```
![Producto-80u](imgPractica5\Productos-80U.png)

- Con $project visualizar solo el nombre, unidades y precio de los productos que tengan menos de 10 unidades
```json
db.productos.aggregate([
  { $match: { unidades: { $lt: 10 } } },
  {
    $project: {
      _id: 0,
      nombre: 1,
      unidades: 1,
      precio: 1
    }
  }
])
```
![Productos-con-menos-de-10U](imgPractica5\Productos-10U.png)

- Con $project ponemos el fabricante pero le cambiamos el nombre por “empresa”. Usamos el mismo comando anterior
```json
db.productos.aggregate([
  { $match: { unidades: { $lt: 10 } } },
  {
    $project: {
      _id: 0,
      nombre: 1,
      unidades: 1,
      precio: 1,
      empresa: "$fabricante"
    }
  }
])
```

![Cambiar-Alias](imgPractica5\Cambiar-Alias.png)

- Añadir a la consulta anterior un campo calculado que se llame total y que multiplique precio por unidades.
```json
db.productos.aggregate([
  { $match: { unidades: { $lt: 10 } } },
  {
    $project: {
      _id: 0,
      nombre: 1,
      unidades: 1,
      precio: 1,
      empresa: "$fabricante",
      total: { $multiply: ["$precio", "$unidades"] }
    }
  }
])
```
![Multiply](imgPractica5\Multiply.png)

- Hacer que el nombre salga en mayúsculas con el operador $toUpper.
```json
db.productos.aggregate([
  { $match: { unidades: { $lt: 10 } } },
  {
    $project: {
      _id: 0,
      nombre: { $toUpper: "$nombre" },
      unidades: 1,
      precio: 1,
      empresa: "$fabricante",
      total: { $multiply: ["$precio", "$unidades"] }
    }
  }
])
```
![Mayusculas](imgPractica5\Mayusculas.png)

- Añadir un campo calculado que ponga el nombre del producto y el tipo concatenado con el operador $concat. Le llamamos al campo “completo”.
```json
db.productos.aggregate([
  { $match: { unidades: { $lt: 10 } } },
  {
    $project: {
      _id: 0,
      nombre: { $toUpper: "$nombre" },
      unidades: 1,
      precio: 1,
      empresa: "$fabricante",
      total: { $multiply: ["$precio", "$unidades"] },
      completo: { $concat: ["$nombre", " - ", "$tipo"] }
    }
  }
])
```
![Campo-Calculado-Completo](imgPractica5\Campo-Calculado-completo.png)

- Ordena el resultado por el campo “total”.
```json
db.productos.aggregate([
  { $match: { unidades: { $lt: 10 } } },
  {
    $project: {
      _id: 0,
      nombre: { $toUpper: "$nombre" },
      unidades: 1,
      precio: 1,
      empresa: "$fabricante",
      total: { $multiply: ["$precio", "$unidades"] },
      completo: { $concat: ["$nombre", " - ", "$tipo"] }
    }
  },
  { $sort: { total: 1 } } 
])
```
![Ordenar-por-Total](imgPractica5\Ordenar-Por-Total.png)

- Haciendo una nueva consulta, averiguar el numero de productos por tipo de producto.
```json
db.productos.aggregate([
  {
    $group: {
      _id: "$tipo",
      cantidadProductos: { $sum: 1 }
    }
  }
])
```
![Productos-Por-Tipo](imgPractica5\PRoductos-tipo.png)

- Añadir el valor mayor y el menor.
```json
db.productos.aggregate([
  {
    $group: {
      _id: "$tipo",
      cantidadProductos: { $sum: 1 },
      mayorUnidades: { $max: "$unidades" },
      menorUnidades: { $min: "$unidades" }
    }
  }
])
```
![MaxMin](imgPractica5\MaxMin.png)

- Añade el total de unidades por cada tipo.
```json
db.productos.aggregate([
  {
    $group: {
      _id: "$tipo",
      totalUnidades: { $sum: "$unidades" }
    }
  }
])
```
![Unidades-Tipo](imgPractica5\Unidades-Tipo.png)

- Con el operador $set y el operador “$substr” visualiza todos los datos del producto "Small Metal Tuna" y los primeros 5 caracteres del nombre.
```json
db.productos.aggregate([
  { $match: { nombre: "Small Metal Tuna" } },
  {
    $set: {
      primerosCinco: { $substr: ["$nombre", 0, 5] }
    }
  }
])
```
![5Caracteres](imgPractica5\5caracteres.png)

- Creamos una salida que tenga el nombre del articulo y el total (precio por unidades) y lo guardamos en una colección denominada productos2.
```json
db.productos.aggregate([
  {
    $project: {
      _id: 0,
      nombre: 1,
      total: { $multiply: ["$precio", "$unidades"] }
    }
  },
  { $out: "productos2" }
])
```
![Productos2](imgPractica5\Productos2.png)

- Comprobamos que se ha creado.
```json
show collections
```
![ColeccionCreada](imgPractica5\Coleccion-Creada.png)

- Hacemos un find para comprobar el resultado.
```json
db.productos2.find()
```
![ComprobarResultado](imgPractica5\ComprobarResultado.png)

- Usando $cond y $project vamos a visualizar el nombre del producto, el precio y un campo llamado valoración que ponga “barato” si el precio es menor de 250 y caro si es mayor o igual.
```json
db.productos.aggregate([
  {
    $project: {
      _id: 0,
      nombre: 1,
      precio: 1,
      valoracion: {
        $cond: {
          if: { $lt: ["$precio", 250] },
          then: "barato",
          else: "caro"
        }
      }
    }
  }
])
```
![Caro-Barato](imgPractica5\Caro-Barato.png)
![Caro-Barato2](imgPractica5\Caro-Barato2.png)
![Caro-Barato3](imgPractica5\Caro-Barato3.png)
