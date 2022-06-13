#Conecta 4 SECOND TRY

##Modelo
Considero que para poder generar usar las reglas del juego y graficarlo es necesario un modelo,

```javascript
   let model = [
            [0,0,0,0,0,0,0],
            [0,0,0,0,0,0,0],
            [0,0,0,0,0,0,0],
            [0,0,0,0,0,0,0],
            [0,0,0,0,0,0,0],
            [0,0,0,0,0,0,0],
        ]
```

##Inicio Grafico
Teniendo como punto de parte el modelo y una estructura simple de html que se un table y dentro de este un tbody con id="game"

```javascript
    let cuerpo = document.getElementById('game)
```


###Graficar/draw
Usando loop/bucle que recorra el modelo, puedo crear document.createElement de cada <tr>(table row) y otro loop/bucle interno crear un <td>(table data cell)

###limpiar/clear
Usando el cuerpo solo hay que usar el textContent = 0

```javascript
    document.getElementById('game').textContent = ''
```



##Reglas de juego
Jugadores : 2
Cada jugador va poniendo una pieza que caera al final de cada columna, el jugador que logre poner de manera continua 4 de estas ganara siendo que puede colocarlas de manera horizontal, vertical, o diagonal.

###Accion click

Al dar click en una columna esta mostrar la pieza que pusiste al final de la columna.
En este sentido, al graficar usando la variable modelo en una tabla agrego atributos siendo que los TR sera las filas (fila=1) y las columnas los TD (columna)

De esta manera al capturar el evento solo tengo que fijarme en el target verficar que sea un td y con esto puedo revisar en target.attribute en que columna estoy, mientras que usando target.parentTarget.attribute puedo verificar la fila.


###Modificar Modelo
Luego de dar click y obtener tanto la fila y la columna toca modificar el modelo, usando asi 
```javascript
    modelo[fila][columna]
```
Dado que el modelo esta lleno de 0 que significan vacio solo se tiene que cambiar por el valor de 1 o 2 dependiendo de que jugador hizo el movimiento

###Verificar Jugador
Usando una variable numerica segun si esta es par o impar me quite ese problema de encima.
La variable se va sumando +1 con cada jugada hecha, si la jugada es ilegal o se ha llegado al final de la columna no se contara.

###Verificar Ganador
Me pase unas 2 o 3 semanas pensando como poder lograr el verficacion de cada tipo de victoria, el metodo que me combencio fue:
```javascript
    // lateral arriba abajo
    [model[f][c],model[f-1][c+1],model[f-2][c+2],model[f-3][c+3]],
    [model[f+1][c-1],model[f][c],model[f-1][c+1],model[f-2][c+2]],
    [model[f+2][c-2],model[f+1][c-1],model[f][c],model[f-1][c+1]],
    [model[f+3][c-3],model[f+2][c-2],model[f+1][c-1],model[f][c]],
    
    // lateral arriba abajo
    [model[f][c],model[f+1][c+1],model[f+2][c+2],model[f+3][c+3]],    
    [model[f-1][c-1],model[f][c],model[f+1][c+1],model[f+2][c+2]],    
    [model[f-2][c-2],model[f-1][c-1],model[f][c],model[f+1][c+1]],    
    [model[f-3][c-3],model[f-2][c-2],model[f-1][c-1],model[f][c]],
                    
    //izquierda derecha
    [model[f][c],model[f][c+1],model[f][c+2],model[f][c+3]],
    [model[f][c-1],model[f][c],model[f][c+1],model[f][c+2]],
    [model[f][c-2],model[f][c-1],model[f][c],model[f][c+1]],
    [model[f][c-3],model[f][c-2],model[f][c-1],model[f][c]],
    
    //abajo
    [model[f][c],model[f+1][c],model[f+2][c]+model[f+3][c]]
```

Cada vez que suceda una jugada se verificara usando el anterior esquema mostrado.
Surge un problema, una lista en javascript si te pasas de valores a la cantidad que este tiene te mandara un error por value undefined, la manera que encontre de solucionarlo fue
```javascript
                try{
                let r = [model[f][c],model[f+1][c],model[f+2][c],model[f+3][c]]
                //console.log('a1',r)
                lista.push(r)
            }catch(e){}
```
Es cutre lo se, pero es la manera mas rapido que encontre que para solucionar el problema, ya que creando una lista, al ejecutarse la funcion de creara la lista y con el problema anterior mandara un error antes de poder hacer cualquier cosa.
En cada verificacion si esta da error solo no se agregara esta a la lista de verificacion

###lista verificacion
Luego de crear esta lista de la verificacion por tipos de victoria, solo se tiene que revisar que estas sean del mismo valor, de no ser el caso que el juego siga.
Si todas son iguales detiene el juego y muestra una cabezera con el valor del Ganador.