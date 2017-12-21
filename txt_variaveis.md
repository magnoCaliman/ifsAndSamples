# Variáveis

---

- variável é conceito básico e essencial em praticamente qualquer linguagem de programação
- é possível criar coisas bastante interessantes sem utilizar variáveis (ver exemplos do tópico sobre argumentos). porém seria difícil continuar um estudo mais profundo em programação sem falar de variáveis.

- mas, antes de definir precisamente o conceito ou discutir aspectos técnicos, pensemos primeiramente *pq variáveis existem*. afinal, se variáveis são algo tão ubíquo em praticamente todas linguagens de programação, elas devem servir algum propósito bastante importante.
- em outras palavras, o que é que nós *não conseguimos fazer* sem utilizar variáveis? 

---

- desafio: programe a seguinte animação
[exemplo código de barra]

- como definir abstratamente essa animação? como descrever os passos pelos quais percorre o código que gera essa animação? qual o seu *algoritmo*?

- partindo da descrição mais abstrata e afunilando para a mais específica:
<br>
- esse programa:
	- desenha linhas

- esse programa:
	- desenha linhas que individualmente tem seus pontos de início e fim sempre no topo e fundo da janela.
	- desenha linhas com uma relação específica de posição entre elas.
  
- esse programa:
	- desenha linhas que, randomicamente, começam no topo e terminam no fundo da tela
	- desenha linhas sempre paralelas entre si
	- desenha linhas sempre perpendiculares à tela

- desenhar linhas que começam no topo e terminam no fundo da tela não é desafio:

```processing
//exemplo 1

size(300, 300);
line(30, 0, 200, 300);
```

- randomizar os pontos de início e fim, também não

```processing
// exemplo 2

void setup()

{
	size(400, 200);
	frameRate(5);
}

void draw()
{
	line(random(400), 0, random(400), 400);
}

```

- se desenhar linhas randômicas *e ao mesmo tempo paralelas* é o problema, divide o problema grande em problemas menores

- desenhar uma linha paralela estática:

```processing
// exemplo 3

void setup()
{
	size(300, 300);
}

void draw()
{
	line(30, 0, 30, height);
}
```

- desenhar várias linhas paralelas estáticas:


```processing 
//  exemplo 4

void setup()
{
	size(300, 300);
}

void draw()
{
	line(30, 0, 30, height);
	line(40, 0, 40, height);
	line(200, 0, 200, height);
}
```

- onde está o conflito entre ex.2 e ex.4?
- em outras palavras: pq o random não funciona como esperado / de onde vem a diferença de resultado?

___

- já que uma linha é [uma conexão entre dois pontos](https://processing.org/reference/line_.html), para desenhar uma linha perperdicular à tela, precisamos que os valores das coordenadas X do primeiro e do segundo ponto (ou seja, do início e do fim da linha) sejam iguais. em outras palavras: o primeiro e terceiro argumentos tem que possuir o mesmo valor, como mostra o ex.4

- porém, ao tentar substituir os valores *hardcoded*<sup>[1]</sup> nesses argumentos por um gerador de números randômicos, como no exemplo 2, esbarramos no problema de que dois números *diferentes* são gerados*, um para cada argumento, fazendo com que nossa linha não seja desenhada de forma perpendicular à janela.

```processing
line(random(400), 0, random(400), 400) // duas funções random() diferentes
	                                   // geram valores diferentes
```

- o que precisamos é que apenas uma função `random()` seja utilizada, e que o valor por ela gerado possa ser aplicado aos dois parâmetros.

- concluimos então que precisamos de algo que nos permita que uma mesma informação (no nosso exemplo, o número gerado por uma única função `random()`), seja acessada e utilizada por duas partes diferentes do código - os primeiro e teceiros argumentos da função `line()`.
- ou seja, o computador precisa ser capaz de *lembrar* de uma certa informação, *para utilizar mais de uma vez*


- variáveis são a maneira que o computador ganha memória (nada a ver com isso (downloadmoreram.com))
- definição técnica de "espaço de alocação de memória"

___

introVar_2.pde
  - conceito
  - height / width
	
- variaVar_3.pde
  - tipo
  - operacao ("variáveis variam...")
	- mouseX / mouseY: aula04/shieldsUp.pde

- circulosRandom_4.pde
  - declaração (tipo + nome) + atribuição (inicialização)
  
- iteracaoVar_5.pde
  - iteração = animação!
  
- exemploShiffman_6.pde
  - futucar...
  
- paintSemPmouse_7.pde
  - desafio, entender a lógica...

## rascunho

## notas
<sub>[1] - explicação de hardcoded
## referências #

