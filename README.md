# # Prova — Computação Gráfica e Processamento de Imagens

## Identificação da atividade

**Disciplina:** Computação Gráfica e Processamento de Imagens
**Professor:** João Francisco Borba
**Modalidade:** Trabalho em dupla

### Integrantes

* **Eduardo dos Santos Prado**
* **Natan Gomes Biazon**

---

## Sobre o projeto

Este repositório apresenta as resoluções práticas das questões da avaliação de **Computação Gráfica e Processamento de Imagens**, desenvolvidas em dupla por **Eduardo dos Santos Prado e Nathan**, sob orientação do professor **João Francisco Borba**.

Os exercícios utilizam principalmente **Python, NumPy e Matplotlib**, demonstrando conceitos relacionados à representação de imagens digitais, dimensões de matrizes, canais RGB/RGBA, manipulação de pixels e composição de regiões coloridas.

## Estrutura do repositório

* `Questao_7` — criação de uma imagem RGBA.
* `Questão 8` — criação e sobreposição de regiões coloridas em uma imagem RGB.
* `Questão_9` — projeto de uma imagem matricial utilizando círculos e máscaras.
* `README.md` — documentação e apresentação das soluções.

---

# Questão 7 — Imagem RGBA

## Código

```python
import numpy as np

imagem = np.zeros((100, 100, 4), dtype=np.uint8)
imagem[:, :, 0] = 255
imagem[:, :, 3] = 255
```

## Explicação

O código cria uma imagem de **100 × 100 pixels** com **4 canais**, utilizando o formato **RGBA**:

* **R** — vermelho (Red)
* **G** — verde (Green)
* **B** — azul (Blue)
* **A** — transparência/opacidade (Alpha)

A estrutura:

```python
(100, 100, 4)
```

representa:

* primeiro `100` → altura da imagem;
* segundo `100` → largura da imagem;
* `4` → quantidade de canais.

Na sequência:

```python
imagem[:, :, 0] = 255
```

define o canal vermelho com intensidade máxima.

Já:

```python
imagem[:, :, 3] = 255
```

define o canal Alpha com intensidade máxima, tornando a imagem **totalmente opaca**.

## Resultado esperado

A imagem possui:

* fundo vermelho;
* canais verde e azul zerados;
* Alpha em 255, indicando opacidade total.

**Conceito principal:** uma imagem RGBA possui três canais de cor mais um canal de transparência.

---

# Questão 8 — Manipulação de canais RGB e regiões da imagem

## Código

```python
import numpy as np
import matplotlib.pyplot as plt

imagem = np.zeros((120, 160, 3), dtype=np.uint8)

imagem[20:100, 30:130] = [255, 0, 0]   # Vermelho
imagem[40:80, 60:100] = [0, 255, 0]    # Verde
imagem[50:70, 70:90] = [0, 0, 255]     # Azul

plt.imshow(imagem)
plt.show()
```

## Explicação

Neste exercício é criada uma imagem RGB com dimensões:

```python
(120, 160, 3)
```

Onde:

* `120` → altura;
* `160` → largura;
* `3` → canais RGB.

As regiões são preenchidas diretamente com valores completos de RGB.

### Região vermelha

```python
imagem[20:100, 30:130] = [255, 0, 0]
```

Seleciona uma região da imagem e atribui:

* R = 255
* G = 0
* B = 0

Resultado: **vermelho**.

### Região verde

```python
imagem[40:80, 60:100] = [0, 255, 0]
```

Resultado: **verde**.

### Região azul

```python
imagem[50:70, 70:90] = [0, 0, 255]
```

Resultado: **azul**.

Como as regiões estão sobrepostas, a região definida por último ocupa os pixels correspondentes. Neste caso, o **azul é aplicado por último**, ficando sobre as regiões anteriores onde houver sobreposição.

## Correção do problema conceitual

A solução utiliza corretamente os três canais de uma imagem RGB:

* índice `0` → R
* índice `1` → G
* índice `2` → B

Além disso, cada região recebe diretamente um vetor completo `[R, G, B]`, evitando o uso incorreto de um índice de canal inexistente, como o índice `3` em uma imagem RGB.

---

# Questão 9 — Projeto de uma Imagem Matricial

## Objetivo

Construir uma imagem **200 × 200 pixels** contendo:

* fundo preto;
* círculo externo branco;
* círculo interno cinza;
* pequeno círculo central vermelho.

A solução utiliza **NumPy** para criar e manipular a matriz de pixels e **Matplotlib** para visualizar o resultado.

## Código

```python
import numpy as np
import matplotlib.pyplot as plt

# Dimensões da imagem
altura = 200
largura = 200

# Cria imagem preta com 3 canais RGB
imagem = np.zeros((altura, largura, 3), dtype=np.uint8)

# Centro automático da imagem
centro_x = (largura - 1) / 2
centro_y = (altura - 1) / 2

# Grade de coordenadas
y, x = np.ogrid[:altura, :largura]

# Distância de cada pixel até o centro
distancia = np.sqrt((x - centro_x) ** 2 + (y - centro_y) ** 2)

# Raios dos círculos
raio_externo = 80
raio_interno = 55
raio_central = 20

# Círculo externo branco
mascara_externa = distancia <= raio_externo
imagem[mascara_externa] = [255, 255, 255]

# Círculo interno cinza
mascara_interna = distancia <= raio_interno
imagem[mascara_interna] = [128, 128, 128]

# Círculo central vermelho
mascara_central = distancia <= raio_central
imagem[mascara_central] = [255, 0, 0]

# Exibe a imagem
plt.imshow(imagem)
plt.axis('off')
plt.show()
```

## Como a matriz representa a imagem?

A estrutura:

```python
imagem = np.zeros((200, 200, 3), dtype=np.uint8)
```

representa uma imagem com:

* **200 linhas**;
* **200 colunas**;
* **3 canais RGB**.

Cada posição da matriz corresponde a um pixel da imagem.

## Como os círculos são calculados?

Para cada pixel, é calculada a distância até o centro da imagem usando a fórmula:

```text
d = sqrt((x - xc)^2 + (y - yc)^2)
```

Quando a distância é menor ou igual ao raio definido, o pixel pertence ao círculo.

Isso permite criar **máscaras booleanas** para selecionar as regiões da imagem.

## Cores utilizadas

| Cor      | RGB               |
| -------- | ----------------- |
| Preto    | `[0, 0, 0]`       |
| Branco   | `[255, 255, 255]` |
| Cinza    | `[128, 128, 128]` |
| Vermelho | `[255, 0, 0]`     |

## Alteração dos tamanhos

Os círculos podem ter seus tamanhos modificados alterando apenas:

```python
raio_externo = 80
raio_interno = 55
raio_central = 20
```

Isso permite reaproveitar a mesma lógica sem reescrever o programa.

## Centralização automática

O centro não é fixado manualmente. Ele é calculado a partir das dimensões da imagem:

```python
centro_x = (largura - 1) / 2
centro_y = (altura - 1) / 2
```

Assim, caso a resolução seja alterada, o centro dos círculos também é atualizado automaticamente.

---

# Conceitos principais demonstrados

Este trabalho reúne conceitos fundamentais de processamento de imagens:

* **Imagem matricial:** representação por pixels organizados em uma matriz.
* **RGB:** três canais de cor — vermelho, verde e azul.
* **RGBA:** RGB acrescido do canal Alpha.
* **Indexação:** acesso a linhas, colunas e canais da imagem.
* **Fatiamento (slicing):** seleção de regiões da matriz.
* **Máscaras booleanas:** seleção de pixels que atendem a uma condição.
* **Coordenadas:** localização dos pixels na imagem.
* **Redimensionamento e centralização:** uso das dimensões da matriz para manter a lógica adaptável.

## Bibliotecas utilizadas

### NumPy

Utilizada para:

* criação das matrizes;
* manipulação de pixels;
* operações matemáticas;
* criação de máscaras e coordenadas.

### Matplotlib

Utilizada para:

* exibição das imagens;
* visualização dos resultados obtidos.

---

# Repositório

Os códigos e arquivos desta atividade estão disponíveis no GitHub:

**https://github.com/Edudsprado/Prova_computacao_grafica_borba**

## Observação

Os códigos foram organizados de forma individual para facilitar a identificação e apresentação de cada questão durante a avaliação.

---

# Conclusão

As atividades demonstram, na prática, como uma imagem digital pode ser tratada como uma estrutura matricial.

Através do **NumPy** é possível controlar dimensões, canais de cor, regiões e pixels individuais, enquanto o **Matplotlib** permite visualizar os resultados.

Os exercícios também reforçam a diferença entre **RGB e RGBA**, a importância da correta **indexação dos canais** e o uso de **coordenadas e máscaras** para construir elementos gráficos a partir de uma matriz de pixels.

---

## Integrantes

**Eduardo dos Santos Prado**

**Natan Gomes Biazon**

**Disciplina:** Computação Gráfica e Processamento de Imagens

**Professor:** João Francisco Borba
