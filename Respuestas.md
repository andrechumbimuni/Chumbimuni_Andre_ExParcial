# Examen Parcial
Andre Dylan Chumbimuni Ricci    Código: 20230303J
## Pregunta 1:
### a) Identifique con precisión qué especifica el ADT y qué queda abierto a la implementación.
El ADT es la interfaz pública (los métodos insert, erase, moveLeft, moveRight, current, size) y el comportamiento esperado desde la perspectiva del usuario (ej. size te dara el tamaño actual). Lo que queda abierto a la implementación es la estructura de datos (arreglos dinámicos, listas doblemente enlazadas, etc.), la representación interna en memoria (índices o punteros) y el redimensionamiento.
### b) Compare el costo de insert, erase, moveLeft, moveRight y current para A y B. Justifique cada entrada de la tabla.
A:
insert, erase: Requiere desplazar todos los elementos a la derecha o izquierda del cursor. $O(n)$
moveLeft, moveRight, current: Solo sumas o restas 1 a un índice entero o accedes al índice del arreglo (cursor). $O(1)$
B:
insert, erase: Requiere crear o eliminar un nodo y reasignar 4 punteros locales (next y prev), sin importar el tamaño. $O(1)$
moveLeft, moveRight, current: Sigues al puntero prev o next del nodo actual y acceder a su valor. $O(1)$
### c) Indique dos invariantes de representación para cada alternativa.
Para A:
1) El puntero al arreglo subyacente nunca es nulo (siempre apunta a un bloque de memoria válido de tamaño.
2) El tamaño size nunca excede la memoria reservada: $0 \le \text{size} \le \text{capacity}$.

Para B:
1) El puntero del cursor nunca es nulo (apunta a un nodo válido o al centinela).
2) El puntero cursor apunta a un nodo que obligatoriamente pertenece al ciclo de la lista enlazada actual.
d) Si el patrón de uso tiene muchas ediciones locales alrededor del cursor y pocas consultas por índice, elija una representación y defienda la elección.
 Elegimos la Representación B; dado que hay muchísimas ediciones alrededor del cursor, la lista doblemente enlazada lo ejecuta en tiempo constante $O(1)$. Si usáramos el arreglo, costaría $O(n)$ la edición, lo cual degradaría drásticamente el rendimiento para archivos grandes.
### e) Explique un caso borde que pueda romper cada implementación si no se maneja explícitamente.
En A: Si el tamaño es 0 e intentas borrar, la lógica de desplazar elementos hacia la izquierda usando un ciclo for intentará leer o escribir en un índice negativo.
En B: Llamar a erase() cuando la lista está vacía o cuando el cursor está apuntando al nodo centinela. Si no se verifica, el código intentará acceder a los campos next/prev de un puntero inválido.
## Pregunta 2:
### a) Trace gcd(252, 105) mostrando a, b y r.
    Inicio: a = 252, b = 105
    Iteración 1: r = 252 % 105 = 42, a = 105, b = 42.
    Iteración 2: r = 105 % 42 = 21, a = 42, b = 21.
    Iteración 3: r = 42 % 21 = 0, a = 21, b = 0.
    b == 0 (termina), Retorna: 21.
### b) Justifique la correctitud usando la propiedad gcd(a, b) = gcd(b, a mod b). Debe explicar cómo se conserva el valor buscado.
El algoritmo es correcto porque en cada paso, el problema original gcd(a, b) se transforma en un problema idéntico pero más pequeño gcd(b, a % b). Como se conserva la misma respuesta, al final llegamos al caso base donde retornar el máximo común divisor correcto del inicio.
### c) Justifique terminación.
En cada iteración, el nuevo valor de b es el resto de una división a % b. El resto $r$ siempre cumple que $0 \le r < b$. Por lo tanto, b es decreciente de enteros no negativos. Inevitablemente llegará a $0$, finalizando el bucle while (b != 0) (finito).
### d) Indique precondiciones razonables para evitar ambigüedades con números negativos o ambos argumentos cero.
Ambos argumentos no pueden ser cero simultáneamente: !(a>=0 && b>=0). Ademas (a=0 || b=0).
### e) Compare con un algoritmo que prueba todos los divisores desde mín (a, b) hacia abajo. Analice costo y explique por qué pasar pruebas pequeñas no demuestra eficiencia.
Un algoritmo que prueba divisores hacia abajo costará un tiempo $O(\min(a, b))$. Por su parte, el algoritmo de Euclides reduce el tamaño de los números, costando solo $O(\log(\min(a, b)))$. La ineficiencia de la primera solo se revela al pasar números primos muy grandes (ej. a=1000000 y b=999999).
## Pregunta 3:
### a) Para _size = 5, _capacity = 6 calcule el costo de insert(0,x), insert(3,x) y insert(5,x) sin redimensionamiento. Justifique en términos de desplazamientos.
    insert(0, x): Desplaza los 5 elementos existentes (de la pos 0 al 4 a la 1 al 5). Costo = 5.
    insert(3, x): Desplaza 2 elementos (de la pos 3 y 4 a la 4 y 5). Costo = 2.
    insert(5, x): No desplaza a nadie (inserción al final). Costo = 0.
### b) Repita para _size = 6, _capacity = 6, considerando el costo de copiar al nuevo arreglo y luego desplazar. No olvide la escritura de x.
    insert(0, x): Copia 6 elementos + desplaza los 6 elementos+ 1 escritura.
    insert(3, x): Copia 6 elementos + desplaza 3 elementos + 1 escritura.
    insert(6, x): Copia 6 elementos + 0 desplazamientos + 1 escritura.
### c) Explique por qué pushBack puede ser amortizado $O(1)$, pero insert(0,x) no lo es bajo la misma representación.
pushBack es insertar al final y solo requiere reasignar todo el arreglo de tamaño $n$ una vez cada $O(n)$ operaciones. Al promediarlo el costo es $O(1)$. Sin embargo, insert(0,x) (insertar al inicio) requiere un desplazamiento forzoso de $n$ elementos a la derecha en cada iteración, haya redimensionamiento o no, haciéndolo de tiempo $O(n)$.
### d) Indique un invariante que involucre _size, _capacity y posiciones válidas de _elem.
Siempre se cumple $0 \le \text{\_size} \le \text{\_capacity}$. Además, todos los índices $0 \le i < \text{\_size}$ en _elem contienen objetos lógicos válidos y _elem apunta a un bloque de memoria que aloja exactamente _capacity espacios (continuo).
### e) Proponga una política de expand/shrink y explique como afecta memoria desperdiciada y numero de copias.
La política es hacer un expand duplicando la capacidad (_capacity * 2) cuando se agota los espacios libres; y hacer un shrink dividiéndolo entre 2 (_capacity / 2) solo cuando  _size <= _capacity / 4.
## Pregunta 4:
### a) Dibuje el estado físico inicial.
El arreglo subyacente de 10 celdas tiene la siguiente forma física:
Indices:     0, 1, 2, 3, 4, 5, 6, 7, 8, 9.
          [16] [23] [  ] [  ] [  ] [  ] [  ] [ 4] [ 8] [15]
### b) Aplique add(42), remove(), add(7), add(9), remove() y muestre j, n, contenido lógico y posiciones físicas ocupadas.
    add(42): Entra a índice 2. n=6. Cola: [4, 8, 15, 16, 23, 42].
    remove(): Sale 4 (índice 7). j=8, n=5. Cola: [8, 15, 16, 23, 42].
    add(7): Entra a índice 3. n=6. Cola: [8, 15, 16, 23, 42, 7].
    add(9): Entra a índice 4. n=7. Cola: [8, 15, 16, 23, 42, 7, 9].
    remove(): Sale 8 (índice 8). j=9, n=6.
    j=9, n=6. Contenido lógico: [15, 16, 23, 42, 7, 9]. Posiciones ocupadas: 9, 0, 1, 2, 3, 4.
### c) Explique por qué el módulo es necesario y qué error aparece si se usa simplemente j+k.
Si sumamos simplemente j + k, al pasar del índice 9 obtendremos 10, 11, etc., lo que causará un desbordamiento de índice en memoria (Segment fault). El operador módulo % garantiza que los indices estén entre 0 y 9.
### d) Compare ArrayQueue con una cola implementada mediante ArrayStack que elimina siempre en posición 0. Analice costos.
Un ArrayStack donde forzosamente eliminas el primero, causa que todos los $n-1$ elementos restantes tengan que ser recorridos hacia la izquierda, tomando tiempo total de $O(n)$. Un ArrayQueue circular evita mover valores al solo mover su j sumando 1, costando $O(1)$.
### e) Explique cómo DualArrayDeque combina dos arreglos y por qué necesita rebalanceo. Indique que propiedad debe mantener el rebalanceo.
Combina dos arreglos; uno que crece hacia "atrás" a modo de Stack para inserciones al frente, y otro convencional para inserciones posteriores. Necesita rebalanceo periódico porque el usuario puede hacer push_front 100 veces y push_back 0 veces, saturando un arreglo y dejando el otro vacío. El rebalanceo distribuye el tamaño actual entre ambos arreglos (la mitad) para mantener el costo de operaciones $O(1)$ en los extremos.
## Pregunta 5:
### a) Escriba pseudocódigo de addBefore(Node w, T x) actualizando todos los enlaces necesarios.
      Node* u = new Node(x);
      u->prev = w->prev;
      u->next = w;
      w->prev->next = u;
      w->prev = u;
### b) Explique por qué el nodo centinela elimina casos especiales al insertar al inicio o al final.
Al haber un nodo fantasma inicial y otro final en una lista doblemente enlazada circular, se garantiza que todo nodo insertable, incluso el primero y el último, siempre tendrán un nodo->prev y un nodo->next válido. Esto hace innecesario escribir verificaciones como assert(head == nullptr).
### c) Justifique por qué getNode(i) puede implementarse en O(1 + min(i, n-i)).
Porque sabiendo el tamaño $n$, si pedimos el índice $i < n/2$, podemos recorrer usando head->next $i$ veces; y si se pide $i > n/2$, es más corto empezar desde tail->prev y retroceder $n-i$ veces.
### d) Diseñe rotate(r) que rota la lista r posiciones a la derecha sin mover los datos elemento por elemento. Puede describir los cambios de enlaces.
En lugar de reescribir elementos, se mueve el dummy o los apuntadores de la cabeza. Se unen el extremo final y inicial temporalmente (haciéndola circular head.prev = tail). Luego, desde el inicio se avanza $(n-r)$ pasos para hallar la nueva cola. Se rompe el enlace ahí y se asigna el nodo que le sigue como nuevo head. Solo se reasignan $\sim 4$ punteros y se recorre parte de la lista.
### e) Proponga dos invariantes estructurales que permitan detectar errores de punteros en una prueba tipo checkSize() o recorrido doble.
1. Para cualquier nodo: v->next->prev == v o v->prev->next==v.
2. Si comenzamos un recorrido iterativo con un contador empezando de la cabeza hasta el centinela, debe darnos exactamente _size pasos.
## Pregunta 6:
### a) Proponga casos de prueba con salida esperada. Deben incluir cadena  vacía, anidamiento correcto, cruce incorrecto, cierre sin apertura y apertura sin cierre.
Cadena vacía: "" $\to$ true
Anidamiento correcto: "({[]})" $\to$ true
Cruce incorrecto: "([)]" $\to$ false.
Cierre sin apertura: "}" o "()}" $\to$ false.
Apertura sin cierre: "{" o "[{}](" $\to$ false.
### b) Explique qué error específico detecta cada grupo de pruebas.
Cadena vacía: Detecta si el código maneja mal el límite inicial.
Cruce incorrecto: Valida que sea  LIFO (útimo en abrir primero en cerrar).
Cierre sin apertura(viceversa): Evita hacer pop o push en una estructura vacía .
### c) Indique el ADT adecuado para resolver el problema y justifique por qué.
El ADT adecuado es la Pila (Stack). Su naturaleza LIFO (Last In, First Out) es el mejor para verificar balanceos en strings anidados: el último paréntesis/llave de apertura guardado en el tope de la Pila debe coincidir con el primer carácter de cierre que aparezca.
### d) Analice complejidad temporal y espacial.
Temporal: $O(n)$, donde $n$ es la longitud de la cadena de texto, dado que el proceso hace $1$ push o $1$ pop una única vez por cada iteración.
Espacial: $O(n)$ en el peor caso posible, que pasaría si un string posee solo corchetes de apertura "[[[[[[" y termina guardando todos en la pila sin vaciarse.
## Pregunta 7:
### a) Defina claramente la entrada, la salida y las precondiciones sobre k.
Entrada: Un flujo de datos o arreglo de números (arr) y un tamaño entero k.
Salida: En cada avance del flujo a partir del índice $i=k-1$, el elemento con valor numérico menor dentro del subarreglo de rango $[i-k+1, i]$.
Precondiciones: La ventana k debe ser $\ge 1$, y el arreglo debe tener un tamaño $\ge k$.
### b) Proponga una representación basada en una cola/deque auxiliar que mantenga candidatos a mínimo.
Se crea una estructura Deque auxiliar que almacenará los índices de los elementos de entrada. La Deque se asegurará de mantener en la parte delantera siempre el índice al menor candidato actual para la ventana. Además, los valores se mantendrán puramente crecientes (eliminando en orden todos los elementos de atrás de la Deque si un valor menor que ellos acaba de entrar).
### c) Dé los invariantes de la estructura auxiliar.
1. Los índices guardados siempre estarán estrictamente dentro del margen actual $(curr\_idx - k, curr\_idx]$.
2. Los valores del arreglo vinculados a los índices de la deque (arr[deque[j]]) se mantendrán estrictamente ordenados de menor a mayor.
### d) Trace su algoritmo para la secuencia [5, 2, 4, 1, 3, 0, 6] con k=3.
    5: Entra pos 0. Deque [0].
    2: Entra pos 1. 2<5 $\to$ saca 0. Deque [1].
    4: Entra pos 2. 4>2 $\to$ Deque [1, 2]. (Resultado: arr[1]=2)
    1: Entra pos 3. 1<4 y 1<2 $\to$ saca 2 y 1. Deque [3]. (Resultado: arr[3]=1)
    3: Entra pos 4. 3>1 $\to$ Deque [3, 4]. (Resultado: arr[3]=1)
    0: Entra pos 5. 0<3 y 0<1 $\to$ saca 4 y 3. Deque [5]. (Resultado: arr[5]=0)
    6: Entra pos 6. 6>0 $\to$ Deque [5, 6]. (Resultado: arr[5]=0)
    Resultados: 2, 1, 1, 0, 0.
### e) Justifique complejidad total y costo amortizado por elemento.
 Dado que a lo largo de todo el ciclo de iteraciones cada número de entrada se le hace un único push_back en la deque y un solo pop_back/pop_front respectivamente, nunca hay $O(n)$ iteraciones anidadas. Las operaciones acumuladas se limitan a máximo $2n$. Esto brinda una complejidad total en tiempo de $\approx O(n)$ y, en consecuencia, el costo medio amortizado por elemento es constante $O(1)$.
### f) Compare con recalcular el mínimo recorriendo la ventana en cada posición.
El enfoque que itera internamente de 0 a $k$ en cada uno de los $n$ elementos, nos condena a un rendimiento de $O(n \cdot k)$. En el caso de listas de datos muy voluminosos con un $k$ alto, el uso de la Deque en tiempo $O(n)$ demuestra una eficiencia mayor contra el costo del recalculo de $O(n \cdot k)$.n

