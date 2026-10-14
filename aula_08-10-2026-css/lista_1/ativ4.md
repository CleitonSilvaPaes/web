# Análise de Especificidade de Seletores CSS

## 1. Demonstração Visual das Cores Aplicadas

Abaixo está a renderização da lista com a cor exata definida por cada regra de especificidade:

<ol>
  <li id="lip0" style="color: red;"><strong>Item 1:</strong> Abacaxi (Regra 1 - red)</li>
  <li class="fruta" style="color: gray;"><strong>Item 2:</strong> Melão (Regra 3 - gray)</li>
  <li id="lip2" class="fruta" style="color: blue;"><strong>Item 3:</strong> Pera (Regra 6 - blue)</li>
  <li id="lip3" class="fruta" style="color: orange;"><strong>Item 4:</strong> Uva (Regra 7 - orange)</li>
  <li id="lip4" class="fruta" style="color: gray;"><strong>Item 5:</strong> Maçã (Regra 3 - gray)</li>
  <li class="churras" style="color: black;"><strong>Item 6:</strong> Carne (Regra 4 - black)</li>
  <li class="churras" style="color: black;"><strong>Item 7:</strong> Linguiça (Regra 4 - black)</li>
  <li id="lip7" class="churras" draggable="true" style="color: magenta;"><strong>Item 8:</strong> Pão de alho (Regra 5 - magenta)</li>
</ol>

---

## 2. Código Original Analisado

```html
<!DOCTYPE html>
<head>   
   <title>Exemplo especificidade de seletores</title>
   <style>
      /*regra 1*/ li{color:red;}
      /*regra 2*/ .churras{color:green;}
      /*regra 3*/ .fruta{color:gray}
      /*regra 4*/ li+li.churras{color: black;}
      /*regra 5*/ .churras[draggable="true"]{color: magenta;}
      /*regra 6*/ #lip1,#lip2,#lip3{color:blue}
      /*regra 7*/ #lip3.fruta{color:orange}
      /*regra 8*/ ol>li.churras#lip5{color:purple}
   </style>
</head>
<body>   
   <ol>
   <!--item 1--> <li id="lip0">Abacaxi</li>
   <!--item 2--> <li           class="fruta">Melão</li>
   <!--item 3--> <li id="lip2" class="fruta">Pera</li>
   <!--item 4--> <li id="lip3" class="fruta">Uva</li>
   <!--item 5--> <li id="lip4" class="fruta">Maçã</li>
   <!--item 6--> <li           class="churras">Carne</li>
   <!--item 7--> <li           class="churras">Linguiça</li>
   <!--item 8--> <li id="lip7" class="churras" draggable="true">Pão de alho</li>
   </ol>   
</body>
</html>
```

| Regra | Seletor | Cálculo de Especificidade | Valor `(ID, Classe/Atributo, Elemento)` | Cor |
| :--- | :--- | :--- | :---: | :--- |
| **Regra 1** | `li` | 1 elemento | `(0, 0, 1)` | `red` |
| **Regra 2** | `.churras` | 1 classe | `(0, 1, 0)` | `green` |
| **Regra 3** | `.fruta` | 1 classe | `(0, 1, 0)` | `gray` |
| **Regra 4** | `li+li.churras` | 1 classe + 2 elementos | `(0, 1, 2)` | `black` |
| **Regra 5** | `.churras[draggable="true"]` | 1 classe + 1 atributo | `(0, 2, 0)` | `magenta` |
| **Regra 6** | `#lip1, #lip2, #lip3` | 1 ID (por seletor avaliado) | `(1, 0, 0)` | `blue` |
| **Regra 7** | `#lip3.fruta` | 1 ID + 1 classe | `(1, 1, 0)` | `orange` |
| **Regra 8** | `ol > li.churras#lip5` | 1 ID + 1 classe + 2 elementos | `(1, 1, 2)` | `purple` |

---

## 3. Análise Detalhada dos Itens (Regras e Disputa)

### Item 1: `<li id="lip0">Abacaxi</li>`
* **Regras que combinam:**
  * Regra 1: `li` $\rightarrow (0, 0, 1)$
* **Disputa:** O seletor `#lip0` não existe nas declarações do CSS. Logo, apenas a Regra 1 se aplica ao elemento.
* **Resultado:** **`red`** (Regra 1).

---

### Item 2: `<li class="fruta">Melão</li>`
* **Regras que combinam:**
  * Regra 1: `li` $\rightarrow (0, 0, 1)$
  * Regra 3: `.fruta` $\rightarrow (0, 1, 0)$
* **Disputa:** Classe supera elemento simples: $(0, 1, 0) > (0, 0, 1)$.
* **Resultado:** **`gray`** (Regra 3).

---

### Item 3: `<li id="lip2" class="fruta">Pera</li>`
* **Regras que combinam:**
  * Regra 1: `li` $\rightarrow (0, 0, 1)$
  * Regra 3: `.fruta` $\rightarrow (0, 1, 0)$
  * Regra 6: `#lip2` $\rightarrow (1, 0, 0)$
* **Disputa:** O seletor de ID supera qualquer combinação exclusiva de classes e elementos: $(1, 0, 0) > (0, 1, 0) > (0, 0, 1)$.
* **Resultado:** **`blue`** (Regra 6).

---

### Item 4: `<li id="lip3" class="fruta">Uva</li>`
* **Regras que combinam:**
  * Regra 1: `li` $\rightarrow (0, 0, 1)$
  * Regra 3: `.fruta` $\rightarrow (0, 1, 0)$
  * Regra 6: `#lip3` $\rightarrow (1, 0, 0)$
  * Regra 7: `#lip3.fruta` $\rightarrow (1, 1, 0)$
* **Disputa:** Ambas as regras 6 e 7 possuem um ID, porém a Regra 7 possui também uma classe, vencendo por especificidade: $(1, 1, 0) > (1, 0, 0)$.
* **Resultado:** **`orange`** (Regra 7).

---

### Item 5: `<li id="lip4" class="fruta">Maçã</li>`
* **Regras que combinam:**
  * Regra 1: `li` $\rightarrow (0, 0, 1)$
  * Regra 3: `.fruta` $\rightarrow (0, 1, 0)$
* **Disputa:** O identificador `#lip4` não foi estilizado no CSS. Entre a tag e a classe, a classe vence: $(0, 1, 0) > (0, 0, 1)$.
* **Resultado:** **`gray`** (Regra 3).

---

### Item 6: `<li class="churras">Carne</li>`
* **Regras que combinam:**
  * Regra 1: `li` $\rightarrow (0, 0, 1)$
  * Regra 2: `.churras` $\rightarrow (0, 1, 0)$
  * Regra 4: `li+li.churras` $\rightarrow (0, 1, 2)$
* **Disputa:** Este item é um `li.churras` imediatamente precedido por outro `li` (o item "Maçã"). Por isso, ele ativa o combinador adjacente da Regra 4. Por ter mais elementos associados à classe, a Regra 4 vence: $(0, 1, 2) > (0, 1, 0) > (0, 0, 1)$.
* **Resultado:** **`black`** (Regra 4).

---

### Item 7: `<li class="churras">Linguiça</li>`
* **Regras que combinam:**
  * Regra 1: `li` $\rightarrow (0, 0, 1)$
  * Regra 2: `.churras` $\rightarrow (0, 1, 0)$
  * Regra 4: `li+li.churras` $\rightarrow (0, 1, 2)$
* **Disputa:** O item é um `li.churras` precedido diretamente por outro `li` ("Carne"). A Regra 4 prevalece sobre a Regra 2: $(0, 1, 2) > (0, 1, 0)$.
* **Resultado:** **`black`** (Regra 4).

---

### Item 8: `<li id="lip7" class="churras" draggable="true">Pão de alho</li>`
* **Regras que combinam:**
  * Regra 1: `li` $\rightarrow (0, 0, 1)$
  * Regra 2: `.churras` $\rightarrow (0, 1, 0)$
  * Regra 4: `li+li.churras` $\rightarrow (0, 1, 2)$
  * Regra 5: `.churras[draggable="true"]` $\rightarrow (0, 2, 0)$
* **Disputa:**
  * A Regra 8 não se aplica porque mira o ID `#lip5` e não `#lip7`.
  * Na comparação entre Regra 4 `(0, 1, 2)` e Regra 5 `(0, 2, 0)`, a coluna de classes/atributos tem precedência sobre a de elementos. Como $2 > 1$ nessa coluna, a Regra 5 vence.
* **Resultado:** **`magenta`** (Regra 5).

---

## 4. Tabela Resumo Final

| Posição | Item | Cor Final | Regra Vencedora | Especificidade |
| :---: | :--- | :--- | :--- | :---: |
| 1 | Abacaxi | **Vermelho** (`red`) | Regra 1 | `(0, 0, 1)` |
| 2 | Melão | **Cinza** (`gray`) | Regra 3 | `(0, 1, 0)` |
| 3 | Pera | **Azul** (`blue`) | Regra 6 | `(1, 0, 0)` |
| 4 | Uva | **Laranja** (`orange`) | Regra 7 | `(1, 1, 0)` |
| 5 | Maçã | **Cinza** (`gray`) | Regra 3 | `(0, 1, 0)` |
| 6 | Carne | **Preto** (`black`) | Regra 4 | `(0, 1, 2)` |
| 7 | Linguiça | **Preto** (`black`) | Regra 4 | `(0, 1, 2)` |
| 8 | Pão de alho | **Magenta** (`magenta`) | Regra 5 | `(0, 2, 0)` |