# Práctica 1 · Búsqueda (ramificación y acotación)

↑ [[FSI]]

> [!info] Datos
> - **Enunciado:** PDFs en https://cayetanoguerra.github.io/ia/
> - **Código:** `3º Curso/FSI - .../Código/practica1-FSI/` → `search.py` (algoritmos), `run.py` (pruebas), `utils.py`
> - **Defensa:** hasta diciembre. Se pueden defender las dos prácticas juntas o por separado. **El trabajo tiene que estar entregado 48 h antes de la defensa.**
> - Notas tomadas en clase: [[Práctica 1 - Notas de clase]]

## Tareas (primera parte)
- [ ] Programar **ramificación y acotación** (*branch and bound*) a partir del código base, usando el grafo de ciudades de Rumanía.
- [ ] Comparar los **nodos expandidos** por este método frente a **anchura** (BFS) y **profundidad** (DFS).
- [ ] (Opcional) Hacer a mano la traza de una búsqueda.
- [x] Revisar en la web si hay segunda parte → sí, ver abajo.

## Tareas (segunda parte) · Ramificación y acotación con subestimación
Es búsqueda **informada**: la lista abierta se ordena por $f(n) = g(n) + h(n)$.
- $g(n)$ = coste acumulado desde el inicio (`node.path_cost`)
- $h(n)$ = heurística: **distancia en línea recta** hasta el objetivo (`GPSProblem.h(node)`)

> [!important] Heurística consistente → camino óptimo
> Para cada nodo $n$ y cada hijo $n'$ al que se llega con la acción $a$: $h(n) \le c(n, a, n') + h(n')$

- [ ] Programar **ramificación y acotación con subestimación** a partir del código base, con el grafo de Rumanía y la distancia en línea recta como heurística.
- [ ] Comparar los **nodos expandidos** con los de ramificación y acotación sin subestimación (la primera parte).
- [ ] (Opcional) Demostrar con un ejemplo hecho a mano que, si la heurística **no es consistente**, la búsqueda deja de ser óptima.

![[FSI P1 - grafo Rumanía.png]]

## Qué hay en el código base (`search.py`)
| Elemento | Qué hace |
|---|---|
| `Problem` | clase abstracta: estado inicial, objetivo, `successor`, `goal_test`, `path_cost` |
| `Node` | nodo del árbol de búsqueda: estado, padre, acción, `path_cost`; `path()` y `expand()` |
| `graph_search(problem, fringe)` | búsqueda genérica en grafo con lista de cerrados; el tipo de `fringe` decide la estrategia |
| `breadth_first_graph_search` | `fringe` = cola FIFO |
| `depth_first_graph_search` | `fringe` = pila |
| `Graph`, `UndirectedGraph` | grafo con distancias; `romania` es el mapa |
| `GPSProblem` | problema de ir de una ciudad a otra; tiene `h()` (distancia en línea recta) |

> [!tip] Idea para ramificación y acotación
> Es `graph_search` con un `fringe` que saque siempre el nodo de **menor `path_cost`** (cola ordenada por coste acumulado). Si piden subestimación: ordenar por `path_cost + h(nodo)` (`GPSProblem.h` ya da la distancia en línea recta).

## Resultados (A → B)
| Método | Camino | Coste | Nodos expandidos |
|---|---|---|---|
| Anchura | B ← P ← R ← S ← A | 418 | |
| Profundidad | B ← F ← S ← A | 450 | |
| Ramificación y acotación | | | |
| R. y A. con subestimación | | | |

## Qué contar para la métrica
Según la clase: **nodos generados**, **nodos visitados**, nodos origen de una expansión y nodos resultado de una expansión. Decidir cuál se reporta y contarlo igual en todos los métodos.
