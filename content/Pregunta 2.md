# Pregunta 2: Clustering con Restricciones Prácticas

## a) Diseño Algorítmico

### Repaso K-Means

Veamos primero el algoritmo de K-Means, el cuál hace lo siguiente:
![[Pasted image 20251107193434.png]]

donde la entrada es $X = \{x_1, x_2, \dots, x_N | x_i \in \mathbb{R}^n\}$ y la salida son los centroides $C = \{\mu_1, \mu_2, \dots, \mu_k | \mu_i \in \mathbb{R}^n\}$ y los clusters $S = \{S_1, S_2, \dots, S_k\}$ donde $S_j = \{x_i | c_i = j\}$

Para atacar el problema, es gradualmente modificar el algoritmo para que cumpla las restricciones

#### Restricción Must-Link

Las restricciones son pares ordenados $ML = \{(x_i, x_j)\}$ donde $ML \subseteq X \times X$ y $(x_1, x_2) \in ML \implies x_1, x_2 \in S_j$ . Si bien $ML$ nos dice únicamente cuando dos elementos tienen que estar juntos, podemos observar que $(x_1, x_2) \in ML \wedge (x_2, x_3) \in ML \implies x_1, x_2, x_3 \in S_j$, es decir, hay una pseudo-transitividad en $ML$. Decimos pseudo-transitividad ya que si bien se cumple lo anterior, no es cierto que $(x_1, x_2) \in ML \wedge (x_2, x_3) \in ML \nRightarrow (x_1, x_3) \in ML$, ya que esta relación puede no ser parte del conjunto, siendo sólo implícita.

En base a lo anterior, podemos calcular subconjuntos $C = \{x_1, x_2, \dots\}\, |\, x_i, x_j \in C \implies x_i, x_j \in S_j$, donde se cumpla la pseudo-transitividad para todos los elementos de $C$. Esto nos daría subconjuntos de puntos que **tienen** que estar juntos, pero, suponiendo que tenemos $C_1, C_2, \dots, C_m$ subconjuntos de éstos, únicamente cubren a los puntos $x$ que están sujetos a un restricción $ML$, y no a todos los puntos $x \in X$. Por lo tanto, haremos que si $x \in X$ pero $x$ no está sujeto a una restricción $ML$, entonces $x \in C_i \wedge |C_i| = 1$, es decir, lo agregamos a un $C$ donde sea el único elemento.

De esta forma, podemos ver a $C$ cómo unidades mínimas, ya no asignamos puntos $x_i$ a un cluster, si no unidades $C_i$ a un cluster. Así, siempre respetaremos las restricciones de Must-Link. Definimos un conjunto de unidades $\mathcal{C} = \{C_1, C_2, \dots, C_M\}$, que contiene a todos los puntos ($\cap_{C_i \in \mathcal{C}}\, C_i = X$), y cada unidad tiene mínimo un punto ($M \leq N$) . Así que ahora sólo ajustamos
$$
\begin{gathered}
c^{(i)} := \underset{j}{\text{arg min}}\,\
\sum_{x \in C_i} \lVert x - \mu_j \rVert_2^2 \\
\text{tal que}\; S_j = \{C_i\, |\, c^{(i)} = 1\} \\
\mu_j = \frac{1}{|S_j|} \sum_{C_i \in S_j} \mu^{(i)}_j \\
\text{donde} \quad \mu^{(i)}_j := \frac{1}{|C_i|} \sum_{x \in C_i} x \\
\end{gathered}
$$

El único detalle, es que ahora los clusters no son conjuntos de puntos, si no conjuntos de unidades $C_i$, pero siempre podemos obtener los puntos de un cluster calculando $\cap_{C_i \in S_j} C_i$.
#### Balance

La restricción del balance la podemos tomar de dos formas, como una restricción suave o una restricción fuerte, aquí lo tomaremos como una restricción suave, es decir, intentamos maximizar el balance, pero no lo obligamos a que este dentro de un límite. Así que sólo agregamos la restricción a la asignación de la forma
$$
c^{(i)} := \underset{j}{\text{arg min}}\,
\sum_{x \in C_i} \lVert x - \mu_j \rVert_2^2
+ \alpha |S_j|
$$

donde $\alpha$ es un parámetro que nos índica que tanto peso le queremos dar al balance.

#### Capacidad

Si $K$ es el tamaño máximo de elementos que puede contener un clúster, hay que restringir la asignación 
$$
\begin{gather}
c^{(i)} := \underset{j}{\text{arg min}}\,
\sum_{x \in C_i} \lVert x - \mu_j \rVert_2^2
+ \alpha |S_j| \\
\text{sujeto a} \hspace{0.2cm} \left(\sum_{C_p \in S_j} |C_p|\right) \leq K
\end{gather}
$$

Esta restricción puede causar que no exista una asignación válida, por ejemplo en el caso de que exista un $C_i$ tal que $|C_i| \gt K$, o si se quiere asignar un $C_i$ pero $|C_i| + |S_j| \gt K$ para todo $j$, es decir, ya no haya ningún cluster con espacio. Para el primer caso podemos simplemente calcular si se cumple $|C_i| \gt K$ después de calcular $\mathcal{C}$, y para el segundo terminar la iteración, randomizar los centroides y volver a calcular. Además, agregaríamos un límite tal que si más de $T$ iteraciones se interrumpen por el segundo caso, terminar la ejecución del algoritmo.

#### Cannot-Link

Las restricciones Cannot-Link son muy similares a las de Must-Link, nuevamente son pares ordenados $CL = \{(x_i, x_j)\, |\, x_i, x_j \in X\}$ tales que si $(x_1, x_2) \in CL$ entonces $x_1, x_2$ deberán estar en distintos clusters. Nótese que no se cumple la pseudo-transitividad para $CL$, ya que si $(x_1, x_2) \in CL$ y $(x_2, x_3) \in CL$,  $x_1$ y $x_3$ si pueden estar en el mismo cluster.

Lo primero que deberíamos checar es que se cumpla $ML \cap CL = \emptyset$, de lo contrario no existe una asignación valida. Lo segundo es checar que $c \in C_i \times C_i \implies c \notin CL$ para todo $C_i \in \mathcal{C}$. Ahora, solo queda modificar nuevamente la asignación, tal que
$$
\begin{gather}
c^{(i)} := \underset{j}{\text{arg min}}\,
\sum_{x \in C_i} \lVert x - \mu_j \rVert_2^2
+ \alpha |S_j| \\
\text{sujeto a} \hspace{0.2cm} \left(\sum_{C_p \in S_j} |C_p|\right) \leq K, \\
\text{y} \quad (x, x') \notin CL,\; x \in C_i,\, x' \in \cap_{C_p \in S_j} C_p
\end{gather}
$$




```pseudocode
FUNCTION kmeans(k, points):
    // Initialize centroids
    centroids ← list of k starting centroids
    converged ← false

    WHILE converged == false DO
        // Create empty clusters
        clusters ← list of k empty lists

        // Assign each point to the nearest centroid
        FOR i ← 0 TO length(points) - 1 DO
            point ← points[i]
            closestIndex ← 0
            minDistance ← distance(point, centroids[0])
            FOR j ← 1 TO k - 1 DO
                d ← DISTANCE(point, centroids[j])
                IF d < minDistance THEN
                    minDistance ← d
                    closestIndex ← j
            clusters[closestIndex].append(point)

        // Recalculate centroids as the mean of each cluster
        newCentroids ← empty list
        FOR i ← 0 TO k - 1 DO
            newCentroid ← calculateCentroid(clusters[i])
            newCentroids.append(newCentroid)

        // Check for convergence
        IF newCentroids == centroids THEN
            converged ← true
        ELSE
            centroids ← newCentroids

    RETURN clusters
```
