# Pregunta 1: Regularización Adaptiva Híbrida

## a) Análisis Teórico
### Función de penalización

Tenemos $\mathcal{D} = \{(x^{(1)}, y^{(1)}), (x^{(2)}, y^{(2)}), \dots, (x^{(m)}, y^{(m)})\}$, donde $x^{(i)} \in \mathbb{R}^n, y^{(i)} \in \mathbb{R}$. Además, tenemos la matriz de correlación de características $C$, de tamaño $n \times n$ y elementos $c_{i, j} \coloneqq \mathrm{corr}(x_i, x_j)$, y el vector de pesos $w \in \mathbb{R}^n$.

Antes de desarrollar el método, recordemos la forma de la regularización
$$
\underset{w}{\mathrm{min}}\ L_R(w) = L(w) + \lambda\, R (w)
$$
donde $R$ es la medida de "complejidad" de w, la cuál es típicamente la norma, en el caso de *Lasso* ($L_1$) y de *Ridge* ($L_2$):
$$
\begin{align*}
R_{L_2}(w) &= \left\lVert w \right\rVert_2^2 \\
R_{L_2}(w) &= \sum_i^n w_i^2 \\
R_{L_1}(w) &= \left\lVert w \right\rVert_1 \\
R_{L_1}(w) &= \sum_i^n |w_i|
\end{align*}
$$
donde $w_i$ son los componentes del vector. Antes de definir $R$, veamos como se tiene que comportar, supongamos que tenemos una variable $p \in [0, 1]$ la cuál nos indica el grado de correlación de todas las características, no sólo de una. Entonces, si el grado de correlación es alto (digamos $p > 0.5$) $R(C, w) = \left\lVert w \right\rVert_1$, de lo contrario ($p \leq 5$) $R(C, w) = \left\lVert w \right\rVert_2^2$. Si bien esto funciona, nos gustaría que el cambio entre Lasso y Ridge fuera gradual y más como una mezcla de ambos, en vez de usar sólo uno y el otro. Para lograr esto definimos $R$ cómo
$$
\begin{align*}
R(C, w) &= (1-p) R_{L_1}(w) + p R_{L_2}(w) \\
\end{align*}
$$
tal que cuando $p$ es alto $R \approx R_{L_2}$ y cuando $p$ es bajo $R \approx R_{L_1}$. Con esto ya tenemos una idea de que forma sería $R$, pero tenemos dos problemas. No sabemos como calcular $p$, y aún si supieramos, $p$ nos dice el grado de correlación de todas las características, por lo que si una característica es altamente correlacionada con las demás, pero las demás no lo son, $R$ se comportará como $R_{L_2}$, y esto afectará la regularización de **todos** los pesos, incluyendo el peso de la característica altamente correlacionada.

Para evitar esto, tendríamos que tomar en cuenta el grado de correlación de cada característica. Suponiendo que lo tenemos, entonces en vez de $p$, tendríamos $p_i$, el cuál nos índica el grado de correlación de la característica $x_i$, por lo cuál tendríamos
$$
\begin{equation}
R(C, w) = \sum_i^n (1-p_i)R_{L_1} (w_i) + p_iR_{L_2}(w_i)
\tag{1}
\end{equation}
$$
El problema con esto, es que sólo funciona si se cumple $R_{L_1}(w) = \sum_i^n R_{L_1}(w_i)$ y para $R_{L_2}$ respectivamente. Afortunadamente, este si es el caso, ya que
$$
\begin{gathered}
\sum_i^n R_{L_1}(w_i) = \sum_i^n |w_i| = R_{L_1}(w) \\
\sum_i^n R_{L_2}(w_i) = \sum_i^n w_i^2 = R_{L_2}(w) \\
\end{gathered}
$$
pero esto no siempre se cumple, por ejemplo, si hubieramos usado la norma $L_2$ sin elevarla al cuadrado, esto no se cumpliría.

Con lo anterior, tenemos entonces ahora sí la forma de $R(C, w)$, ahora lo que toca ver es como calcularemos $p_i$. Como vimos al principio, los elementos de la matriz de correlación de características son de la forma $c_{i,j} := \mathrm{corr}(x_i, x_j)$, donde $corr$ calcula el coeficiente de correlación de pearson, por lo cuál $c_{i,j} \in [-1,1]$. Para la regularización, tomaremos en cuenta sólo $|c_{i,j}|$, ya que  si bien perdemos el tipo de correlación (positiva o negativa), sólo nos interesa la magnitud de la correlación, no el tipo. Si tomáramos en cuenta el tipo, $p_i$ pudiera tener valores negativos, pero si ese fuera el caso entonces pudieramos tener valores negativos en la sumatoria, y esto causaría que $R$ no funcionara como esperamos.

Por lo tanto, definiremos
$$
p_i := \frac{1}{n-1} \sum_1^n |c_{i,j}|, \hspace{0.2cm} \text{para}\; i \neq j
$$
hacemos la restricción de $i \neq j$ ya que $c_{i, i} = 1$, y dividmos la suma entre $n-1$ para que $p_i \in [0,1]$ usando la fórmula
$$
\mathrm{valor}_\mathrm{escalado} = \frac{\mathrm{valor} - \mathrm{min}}{\mathrm{max} - \mathrm{min}}
$$
Ahora si, con este valor de $p_i$, sustituyendo en la ecuación (1) obtenemos
$$
\begin{equation}
R(C, w) = \sum_i^n (1-p_i)R_{L_1} (w_i) + p_iR_{L_2}(w_i) \hspace{0.2cm} \text{para}\; i \neq j
\tag{2}
\end{equation}
$$
Para esta función, $R$ se comporta cómo $R_{L_2}$ puro si $p_i = 1$ para todas las características, y cómo $R_{L_1}$ puro si $p_i = 0$ para todas las características

### Derivación del gradiente

Como $R$ depende de $\mathbf{C}$ y $\mathbf{w}$ normalmente calcularíamos el gradiente para ambos, pero $\mathbf{C}$ es constante, ya que las correlaciones dependen de $X$, el cuál no varía, y por lo tanto $p_i$ también es constante. Debido a esto, solo deviraremos con respecto a $\mathbf{w}$. La función a minimizar es 
$$
\begin{gather}
\underset{w}{\min}\ E_{\mathrm{aug}}(\mathbf{w}) = E_{\mathrm{in}}(\mathbf{w}) + R(\mathbf{C}, \mathbf{w}) \\
\nabla_w E_{\mathrm{aug}}(\mathbf{w}) = \nabla_w E_{\mathrm{in}}(\mathbf{w}) + \nabla_w R(\mathbf{C}, \mathbf{w})
\end{gather}
$$
donde
$$
\begin{align*}
\nabla_w E_{\mathrm{aug}} &= \begin{bmatrix}
\frac{\partial E_{\mathrm{aug}}}{\partial w_1} \\
\dots \\
\frac{\partial E_{\mathrm{aug}}}{\partial w_k} \\
\dots \\
\frac{\partial E_{\mathrm{aug}}}{\partial w_n} \\
\end{bmatrix}
\end{align*}
$$
calculemos
$$
\begin{align*}
\frac{\partial E_{\mathrm{aug}}}{\partial w_k} &= \frac{\partial E_{\mathrm{in}}}{\partial w_k} + \frac{\partial R}{\partial w_k}\\
\frac{\partial R}{\partial w_k} &= \frac{\partial}{w_k} \sum_i^n (1-p_i)R_{L_1} (w_i) + p_iR_{L_2}(w_i) \hspace{0.2cm} \text{para}\; i \neq j\\
\frac{\partial R}{\partial w_k} &= \sum_{i \neq j}^n \frac{\partial}{w_k} \lbrack (1-p_i)R_{L_1} (w_i) + p_iR_{L_2}(w_i)\rbrack \hspace{0.2cm} \\
\end{align*}
$$
nótese que aunque estamos derivando con respecto a $w_k$, las funciones de penalización $R_{L_1}(w_i)$, $R_{L_1}(w_i)$ dependen de $w_i$, por lo que $i \neq k \implies \frac{\partial}{w_k} \lbrack \dots \rbrack = 0$, así que nos enfocaremos en el caso $i = k$.
$$
\begin{align*}
\frac{\partial R}{\partial w_k} &= \frac{\partial}{w_k} \lbrack (1-p_k) R_{L_1} (w_k) + p_k R_{L_2}(w_k)\rbrack \hspace{0.2cm} \\
\frac{\partial R}{\partial w_k} &= (1 - p_k) \cdot \frac{d R_{L_1}(w_k)}{d w_k} + p_k \cdot \frac{d R_{L_2}(w_k)}{d w_k} \\
\frac{\partial R}{\partial w_k} &= (1 - p_k)\, \mathrm{sign}(w_k) + p_k w_k \quad \therefore \\
\nabla_{\mathbf{w}} R &= \begin{bmatrix}
(1 - p_1)\, \mathrm{sign}(w_1) + p_1 w_1 \\
(1 - p_2)\, \mathrm{sign}(w_2) + p_2 w_2 \\
\vdots \\
(1 - p_n)\, \mathrm{sign}(w_n) + p_n w_n \\
\end{bmatrix}
\end{align*}
$$
Y las actualizaciones del gradiente serían de la forma
$$
\mathbf{w}_{t+1} \leftarrow \mathbf{w}_t - \eta \left( \nabla_{\mathbf{w}_t} E_{\mathrm{in}} + \lambda \nabla_{\mathbf{w}}R \right)
$$

## b) Implementación y Validación Empírica

