#**Deconvolução de traços sísmicos em meio atenuante**

A deconvolução é o processo de inverter os resultados de uma convolução em um sistema linear e invariante no tempo(LIT), onde ja é conhecido o resultado final de uma convolução pela qual o sistema alterou a entrada.
O sistema físico que representa o problema da sísmica de reflexão pode ser modelado como um sistema LIT causal que pode ser representado pela equação de convolução

$$
x(t) = r(t) * w(t) = ∫_{-∞}^{∞}r(\tau)w(t-\tau)d\tau
$$

no domínio da frequência, essa equação se torna

$$
X(\omega) = R(\omega) W(\omega)
$$

Onde $x(t)$ é o traço sísmico obtido pelos receptores, $r(t)$ é a refletividade ou resposta ao impulso modelada da superfície, e $w(t)$ é o pulso gerado pela fonte sonora.

Porém em um meio atenuante, existem modelos que transformam esse modelo convolucional em um modelo convolucional não estacionário, em que o operador que representa a wavelet agora é uma convolução não estacionária com entre um sinal que representa a atenuação e a própria wavelet, desse modo temos

O modelo convolutivo com atenuação é dado por uma convolução não estacionária, seguida de uma convolução estacionária

$$
x(t) = w(t) \ast a(t,\tau) \circledast r(t)
$$


analiticamente temos que

$$
x(t) = w(t) \ast \int_{-\infty}^{\infty} a(t-\tau,\tau)r(\tau)d\tau
$$

aplicando a transformada de fourier dos dois lados

$$
X(\omega) = \mathcal{F}\{\  w(t) \ast \int_{-\infty}^{\infty} a(t-\tau,\tau)r(\tau)d\tau  \}\
$$

pla propriedade da convolução na frequência, temos

$$
X(\omega) = W(\omega) \cdot \mathcal{F}\{\  \int_{-\infty}^{\infty} a(t-\tau,\tau)r(\tau)d\tau  \}\
$$

desenvolvendo a transformada de fourier

$$
\mathcal{F}\{\  \int_{-\infty}^{\infty} a(t-\tau,\tau)r(\tau)d\tau  \}\ = \int_{-\infty}^{\infty}  \int_{-\infty}^{\infty} a(t-\tau,\tau)r(\tau)d\tau e^{-i \omega t} dt
$$

podemos agora reverter a ordem da transformada, como segue

$$
\int_{-\infty}^{\infty}  (\int_{-\infty}^{\infty} a(t-\tau,\tau) e^{-i \omega f t} dt ) r(\tau) d\tau
$$

nomeando agora

$$
t- \tau = T  => t = T + \tau => dt = dT
$$

então ficamos com

$$
\int_{-\infty}^{\infty}  (\int_{-\infty}^{\infty} a(T,\tau) e^{-i \omega T} dT ) r(\tau) e^{-i \omega \tau}  d\tau
$$

o termo dentro do parênteses se refere a transformada de fourier do sinal de atenuação, então

$$
\int_{-\infty}^{\infty} a(T,\tau) e^{-i \omega T} dT = A(f,\tau)
$$

assim o espectro do traço sísmico se torna

$$
X(\omega) = W(\omega) \cdot \int_{-\infty}^{\infty} A(\omega,\tau) r(\tau) e^{-i \omega \tau}  d\tau
$$

o termo

$$
\int_{-\infty}^{\infty} A(\omega,\tau) r(\tau) e^{-i \omega \tau}  d\tau
$$

pode ser visto como uma transformada de fourier da refletividade que já leva em conta a atenuação das camadas geológicas, e ainda podemos decompor o termo de atenuação como um número complexo na forma polar, dado por

$$
A(\omega,\tau) = |A(\omega,\tau)| e^{i \phi(\omega,\tau)}
$$

e o espectro de amplitude pode ainda ser representado por outra exponencial, da forma

$$
|A(\omega,\tau)| = e^{\psi(\omega,\tau)}
$$


então o termo de atenuação se torna

$$
A(\omega,\tau) = e^{ \psi(\omega,\tau) + i\phi(\omega,\tau)} = e^{\theta(\omega,\tau)}
$$


assim o modelo convolucional não estacionário no domínio da frequência finalmente se torna


$$
X(\omega) = W(\omega) \cdot \int_{-\infty}^{\infty} r(\tau) e^{-i \omega \tau + \theta(\omega,\tau)}  d\tau
$$

que pode ser colocado numa forma compacta como

$$
X(\omega) = W(\omega) R(\mathbf{Q},\omega)
$$


**Refletividade com Atenuação**
Normalmente a atenuação é jogada para a wavelet, porém nesse artigo ela é colocada para a refletividade e seu j-ésimo coeficiente de sua transformada de fourier atenuada é dada por

$$
R(\omega_j) = \sum_{l = 1}^{K} r_l e^{i \omega_j l \Delta \tau + \theta_l (\omega_j)}
$$


Assim o traço sísmico atenuado é escrito como

$$
X(\omega_j) = W(\omega_j) \cdot \sum_{l = 1}^{K} r_l e^{i \omega_j \Delta \tau + \theta_l (\omega_j)}
$$

que pode ser visto ainda como

$$
X(\omega_j) = \sum_{l = 1}^{K}  r_l  W(\omega_j) e^{i \omega_j l \Delta \tau + \theta_l (\omega_j)} = \sum_{l = 1}^{K}  M_{j,l} r_l
$$

que equivale a uma multiplicação de matrizes da forma

$$
X(\omega_j) = \sum_{l = 1}^{K}  r_l  W(\omega_j) e^{i \omega_j l \Delta \tau + \theta_l (\omega_j)}
$$

termo $\theta_m(\omega)$ é dado pelo modelo de atenuação, como por exemplo o modelo de Kjartansson, dado por

$$
\theta_m(\omega) = \omega \Delta \tau [ i \cdot ln(\frac{\omega_0}{\omega}) - \frac{1}{2} ] \sum_{n = 1}^{m} \frac{1}{Q_n}
$$


então o espectro do dado é modelado matricialmente por

$$
\mathbf{X = Mr}
$$


##**Estimativa por média móvel**
Uma técnica muito conhecida para a estimação da wavelet em um ambiente de alta SNR e fase mínima, é a suavização do espectro de magnitude do traço sísmico, essa suavização é feita com um filtro de média móvel.

Seja o espectro de magnitude do traço sísmico dado por

$$
A(f) = |X(f)|
$$

e o filtro de média móvel, na frequência

$$
M_a(f) = \frac{1}{M} rect_M(f) =  \frac{1}{M}, 0 \leq n \leq M
$$

assim, temos que o espectro da wavelet suavizada discreta, será dada por

$$
W(k) = M_a(k) \ast |X(k)| = \frac{1}{M} \sum_{p = 0}^{M-1} |X(k - p)|
$$



##**Deconvolução por mínima norma $l_1$ em meio atenuante**
A deconvolução por norma $l_2$ pode ser feita utilizando otimização, e minimizando a seguinte função objetivo

$$
J\mathbf{(r)} = ||\mathbf{X} - \mathbf{Mr}||_1 + \mu ||\mathbf{r}||_1
$$


**Suavização da norma $l_1$**

A norma l1 é uma função que não é suave e pode ter variações bruscas, a mesma coisa pra sua derivada, que por partes é a função sinal, então é proposto uma suavização da mesma, partindo da igualdade

$$
||\vec{v}||_1 = \lim_{\epsilon \to 0} \sum_{i = 1}^{N} \sqrt{v_i^2 + \epsilon^2} = \sum_{i = 1}^{N} \lim_{\epsilon \to 0} \sqrt{v_i^2 + \epsilon^2} = \sum_{i = 1}^{N} |v_i|
$$

então assim temos que

$$
||\vec{v}||_1  \approx  \sum_{i = 1}^{N} \sqrt{v_i^2 + \epsilon^2}
$$

para valores de $ϵ$ pequenos.


Agora nomeando a diferença dentro da primeira norma $l_1$ como o erro de predição, temos

$$
\mathbf{X} - \mathbf{Mr} = \mathbf{E}
$$

a função objetivo fica

$$
J\mathbf{(r)} = ||\mathbf{E(\mathbf{r})}||_1 + \mu ||\mathbf{r}||_1
$$

substituindo a norma l1 por sua aproximação, temos

$$
J\mathbf{(r)} = \sum_{i}^{} \sqrt{|E_i|^2 + \epsilon^2} + \mu  \sum_{i}^{} \sqrt{r_i^2 + \epsilon^2}
$$


derivando temos ainda


$$
\frac{ \partial}{\partial r_n} J\mathbf{(r)} = \frac{ \partial}{\partial r_n}  \sum_{i}^{} \sqrt{|E_i|^2 + \epsilon^2} + \mu  \sum_{i}^{} \sqrt{r_i^2 + \epsilon^2}
$$

a derivada da norma l1 suavizada é dada por

$$
\frac{ \partial}{\partial r_n} \sum_{i}^{} \sqrt{r_i^2 + \epsilon^2} = \sum_{i}^{} \frac{2 r_n}{2 \sqrt{r_i^2 + \epsilon^2}}
$$


a derivada da norma l1 do erro é feita utilizando a regra da cadeia, então temos

$$
\frac{ \partial}{\partial r_n}  \sum_{i}^{} \sqrt{|E_i|^2 + \epsilon^2}  =   \sum_{i}^{} \frac{\frac{ \partial}{\partial r_n} |E_i|^2 }{2\sqrt{|E_i|^2 + \epsilon^2} }
$$

agora utilizando que o módulo de um número complexo pode ser calculado como

$$
|z|^2 = z \cdot \bar{z}
$$

temos que

$$
|E_i|^2 = E_i \cdot \bar{E_i}
$$

agora a derivada fica

$$
\frac{ \partial}{\partial r_n} E_i \cdot \bar{E_i}
$$

utilizando ainda a regra do produto

$$
E_i \frac{ \partial}{\partial r_n} \bar{E_i} +  \bar{E_i} \frac{ \partial}{\partial r_n} E_i
$$


e o i-ésimo termo do vetor $\mathbf{E}$ é dado por

$$
 E_i = (\mathbf{X} - \mathbf{Mr})_i =  (\mathbf{X}_i - \sum_{l} \mathbf{M_{i,l} r_l})
$$

Então sua derivada se torna

$$
\frac{ \partial}{\partial r_n}  E_i = \frac{ \partial}{\partial r_n}  (\mathbf{X}_i - \sum_{l} \mathbf{M_{i,l} r_l}) = - \frac{ \partial}{\partial r_n} \sum_{l} \mathbf{M_{i,l} r_l} = -\mathbf{M_{i,n}}
$$

e temos que tanto a operação de derivada tanto a operação de conjugado é linear, então podemos inverter suas ordens


$$
\frac{d}{d z}  \overline{\alpha z}  = \overline{\frac{d}{d z}  \alpha z  } = \overline{\alpha}
$$

e a derivada do conjugado fica

$$
\frac{ \partial}{\partial r_n} \bar{E_i} = \frac{ \partial}{\partial r_n} \overline{ \mathbf{X}_i - \sum_{l} \mathbf{ M_{i,l} r_l} } = -\frac{ \partial}{\partial r_n} \sum_{l} \overline { \mathbf{ M_{i,l} r_l} }  = - \overline{\mathbf{ M_{i,n}}}
$$

então temos que

$$
\frac{ \partial}{\partial r_n} E_i \cdot \bar{E_i}  = E_i \cdot - \overline{\mathbf{ M_{i,n}}} + \bar{E_i} \cdot - \mathbf{ M_{i,n}}
$$

agora podemos simplificar utilizando as seguintes propriedades dos conjugados

$$
\bar{z_1} z_2 = \overline{ z_1 \bar{z_2} }
$$

$$
z + \bar{z} = 2Re\{\ z \}\
$$

então podemos simplificar como

$$
- (E_i \cdot \overline{\mathbf{ M_{i,n}}} + \bar{E_i} \cdot \mathbf{ M_{i,n}}) =-(E_i \cdot \overline{\mathbf{ M_{i,n}}} + \overline{E_i \cdot \overline{\mathbf{ M_{i,n}}}} ) = -2Re\{\ E_i \overline{\mathbf{M_{i,n}}} \}\
$$

e ainda substituindo o vetor de erro

$$
-2Re\{\ E_i \overline{\mathbf{M_{i,n}}} \}\ = -2Re\{\ ( \mathbf{X}_i - \sum_{l} \mathbf{ M_{i,l} r_l} ) \overline{\mathbf{M_{i,n}}} \}\ = -2Re \{\  \mathbf{X}_i \overline{\mathbf{M_{i,n}}} \}\ + 2Re \{\ \sum_{l} \mathbf{ M_{i,l} r_l} \overline{\mathbf{M_{i,n}}} \}\
$$

então a derivada total pode finalmente ser escrita como

$$
\frac{ \partial}{\partial r_n} J\mathbf{(r)} = \sum_{i}^{} \frac{ -2Re \{\  \mathbf{X}_i \overline{\mathbf{M_{i,n}}} \}\ + 2Re \{\ \sum_{l} \mathbf{ M_{i,l} r_l} \overline{\mathbf{M_{i,n}}} \}\ }{2\sqrt{|E_i|^2 + \epsilon^2} } + \mu \sum_{i}^{} \frac{2 r_n}{2 \sqrt{r_i^2 + \epsilon^2}}
$$

Agora separando cada termo, temos ainda

$$
\frac{ \partial}{\partial r_n} J\mathbf{(r)} = \sum_{i}^{} \frac{ 2Re \{\ \sum_{l} \mathbf{ M_{i,l} r_l} \overline{\mathbf{M_{i,n}}} \}\ }{2\sqrt{|E_i|^2 + \epsilon^2} } + \mu \sum_{i}^{} \frac{2 r_n}{2 \sqrt{r_i^2 + \epsilon^2}} -     \sum_{i}^{} \frac{ 2Re \{\  \mathbf{X}_i \overline{\mathbf{M_{i,n}}} \}\  \}\ }{2\sqrt{|E_i|^2 + \epsilon^2} }
$$

que pode ser escrita ainda como um produto de matrizes da forma

$$
\nabla J(\mathbf{r} ) = Re\{\ \mathbf{M}^H \mathbf{A} \mathbf{Mr} \}\ + \mu \mathbf{Br} -  Re\{\ \mathbf{M}^H \mathbf{AX} \}\ = \mathbf{0}
$$

Colocando a refletividade em evidência, a equação final do ponto de mínimo se torna

$$
 Re \{\ \mathbf{M}^H \mathbf{A} \mathbf{M} + \mu \mathbf{B} \}\  \mathbf{r} =  Re\{\ \mathbf{M}^H \mathbf{AX} \}\
$$

Que pode ser visto como um sistema de equações não lineares, uma vez que as matrizes dependem da refletividade não linearmente.

As matrizes A e B são matrizes diagonais, da forma

$$
A_{i,i} = \frac{1}{ \sqrt{|E_i|^2 + \epsilon^2}}
$$

e

$$
B_{i,i} = \frac{1}{ \sqrt{r_i^2 + \epsilon^2}}
$$

Esse sistema de equações não lineares pode ser resolvido iterativamente, gastando menos custo computacional do que um algorítmo de otimização, por exemplo.

**Resolução do sistema de equações**

O sistema de equações para a refletividade pode ser resolvido pela própria equação, primeiro nomeamos os termos

$$
\mathbf{P}(\mathbf{r}) =  Re \{\ \mathbf{M}^H \mathbf{A}(\mathbf{r}) \mathbf{M} + \mu \mathbf{B}(\mathbf{r}) \}\
$$

e

$$
\mathbf{z}(\mathbf{r}) = Re\{\ \mathbf{M}^H \mathbf{AX} \}\
$$

então o sistema de equações assume a forma compacta

$$
\mathbf{P}(\mathbf{r})  \mathbf{r} = \mathbf{z}(\mathbf{r})
$$

que pode ser resolvido iterativamente pela equação:

$$
 \mathbf{r}^{k+1} = [\mathbf{P}(\mathbf{r^k})]^{-1} \mathbf{z}(\mathbf{r^k})
$$



##**Deconvolução por mínimo erro médio quadrático em meio atenuante**





**Referências**

* OLIVEIRA, S. A. M.; LUPINACCI, W. M. L1 norm   
  inversion method for deconvolution in attenuating media.  North Fluminense State University.  Petroleum Exploration and Production Laboratory Macae.  RJ, Brazil and Inversion Geophysics.  https://doi.org/10.1111/1365-2478.12002


* Ergun, Erhan (2019). Non-stationary Iterative
  Time-Domain Deconvolution for Enhancing the Resolution of Shallow Seismic Data. Purdue University Graduate School. Thesis. https://doi.org/10.25394/PGS.8126687.v1



