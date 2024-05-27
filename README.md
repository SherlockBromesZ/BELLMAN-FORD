### README.md

# Algoritmo de Bellman-Ford

## Introdução

O algoritmo de Bellman-Ford é utilizado para encontrar as menores distâncias de um vértice de origem para todos os outros vértices em um grafo ponderado, que pode conter arestas com pesos negativos. Além disso, o algoritmo pode detectar ciclos negativos no grafo.

## Funcionamento

O algoritmo relaxa todas as arestas repetidamente, atualizando as distâncias mínimas até que não seja mais possível encontrar distâncias menores. Após \( V-1 \) iterações (onde \( V \) é o número de vértices), as distâncias mínimas são determinadas. Uma iteração adicional é usada para verificar a existência de ciclos negativos.

### Passos do Algoritmo

1. **Inicialização**:
   - Defina a distância do vértice de origem como 0 e todas as outras distâncias como infinito (\( \infty \)).

2. **Relaxação das Arestas**:
   - Para cada aresta \( (u, v) \) com peso \( w \), se a distância para \( v \) através de \( u \) for menor que a distância atual de \( v \), então atualize a distância de \( v \).

3. **Verificação de Ciclos Negativos**:
   - Execute uma iteração adicional para verificar se alguma distância pode ser atualizada. Se puder, então há um ciclo negativo no grafo.

## Exemplo

Dado um grafo com 5 vértices e 8 arestas:

```
Vértices: 5
Arestas: 8
Arestas e pesos:
(0, 1) com peso -1
(0, 2) com peso 4
(1, 2) com peso 3
(1, 3) com peso 2
(1, 4) com peso 2
(3, 2) com peso 5
(3, 1) com peso 1
(4, 3) com peso -3
Origem: 0
```

### Simulação

1. **Inicialização**:
   ```
   Distâncias: [0, ∞, ∞, ∞, ∞]
   ```

2. **Primeira Iteração**:
   ```
   (0, 1): Atualiza dist[1] para -1 -> [0, -1, ∞, ∞, ∞]
   (0, 2): Atualiza dist[2] para 4  -> [0, -1, 4, ∞, ∞]
   (1, 2): Atualiza dist[2] para 2  -> [0, -1, 2, ∞, ∞]
   (1, 3): Atualiza dist[3] para 1  -> [0, -1, 2, 1, ∞]
   (1, 4): Atualiza dist[4] para 1  -> [0, -1, 2, 1, 1]
   (3, 2): Sem atualização
   (3, 1): Sem atualização
   (4, 3): Atualiza dist[3] para -2 -> [0, -1, 2, -2, 1]
   ```

3. **Segunda Iteração**:
   ```
   (0, 1): Sem atualização
   (0, 2): Sem atualização
   (1, 2): Atualiza dist[2] para 1  -> [0, -1, 1, -2, 1]
   (1, 3): Sem atualização
   (1, 4): Sem atualização
   (3, 2): Sem atualização
   (3, 1): Sem atualização
   (4, 3): Atualiza dist[3] para -2 -> [0, -1, 1, -2, 1]
   ```

4. **Terceira Iteração**:
   ```
   (0, 1): Sem atualização
   (0, 2): Sem atualização
   (1, 2): Sem atualização
   (1, 3): Sem atualização
   (1, 4): Sem atualização
   (3, 2): Sem atualização
   (3, 1): Sem atualização
   (4, 3): Sem atualização
   ```

5. **Quarta Iteração**:
   ```
   (0, 1): Sem atualização
   (0, 2): Sem atualização
   (1, 2): Sem atualização
   (1, 3): Sem atualização
   (1, 4): Sem atualização
   (3, 2): Sem atualização
   (3, 1): Sem atualização
   (4, 3): Sem atualização
   ```

### Resultado

Após a execução do algoritmo, as distâncias mínimas do vértice de origem (0) são:

```
0 para: 0
1 para: -1
2 para: 2
3 para: -2
4 para: 1
```

## Código Completo

```cpp
#include <bits/stdc++.h>
using namespace std;

int inf = INT_MAX;

struct Aresta {
    int inicio;
    int fim;
    int peso;
};

vector<int> ford(int source, vector<Aresta> &arestas, int qntd_nos) {
    vector<int> distancia(qntd_nos, inf);
    distancia[source] = 0;
    
    for (int i = 0; i < qntd_nos - 1; i++) { // Correção no loop externo
        for (auto &aresta : arestas) {
            int u = aresta.inicio;
            int v = aresta.fim;
            int p = aresta.peso;
            if (distancia[u] != inf && distancia[v] > distancia[u] + p) {
                distancia[v] = distancia[u] + p;
            }
        }
    }
    
    return distancia;
}

int main() {
    int num_nos, num_arestas;
    cin >> num_nos >> num_arestas;
    
    vector<Aresta> arestas(num_arestas); // Inicializar com num_arestas
    
    for (int i = 0; i < num_arestas; i++) {
        cin >> arestas[i].inicio >> arestas[i].fim >> arestas[i].peso;
    }
    
    int source;
    cin >> source;
    
    vector<int> dist = ford(source, arestas, num_nos);
    
    for (int i = 0; i < num_nos; i++) {
        if (dist[i] == inf) {
            cout << i << " para: INF" << endl;
        } else {
            cout << i << " para: " << dist[i] << endl;
        }
    }
    
    return 0;
}
```

### Como Executar

1. **Compile o código**:
   ```sh
   g++ -o bellman_ford bellman_ford.cpp
   ```

2. **Execute o programa**:
   ```sh
   ./bellman_ford
   ```

3. **Forneça a entrada**:
   ```
   5 8
   0 1 -1
   0 2 4
   1 2 3
   1 3 2
   1 4 2
   3 2 5
   3 1 1
   4 3 -3
   0
   ```

4. **Verifique a saída**:
   ```
   0 para: 0
   1 para: -1
   2 para: 2
   3 para: -2
   4 para: 1
   ```
