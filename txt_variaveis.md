# Variáveis

---

- variável é conceito básico e essencial em praticamente qualquer linguagem de programação. talvez vc já tenha ouvido falar.
- é possível criar coisas bastante interessantes sem utilizar variáveis (ver exemplos do tópico sobre argumentos). porém seria difícil continuar um estudo mais profundo em programação sem falar sobre variáveis.

- mas, antes de definir precisamente o conceito ou discutir aspectos técnicos, pensemos primeiramente _pq variáveis existem_. afinal, se variáveis são algo tão ubíquo em praticamente todas linguagens de programação, elas devem servir algum propósito bastante importante.
- em outras palavras, o que é que nós _não conseguimos fazer_ sem utilizar variáveis? 

- ouça esse exemplo, onde 3 osciladores são tocados em sequência (ouça com fones)

```
{Pulse.ar(freq:200, mul:0.1)!2}.play; //oscilador 1
{Pulse.ar(freq:202, mul:0.1)!2}.play; //oscilador 2
{LFTri.ar(freq:100, mul:0.1)!2}.play; //oscilador 3
```
- explicação idéia 3 osciladores, com relação específica de frequência entre eles. 

- vc pode querer trocar para outras frequências, mas mantendo essas relação entre os valores

```
{Pulse.ar(freq:300, mul:0.1)!2}.play; //oscilador 1
{Pulse.ar(freq:302, mul:0.1)!2}.play; //oscilador 2
{LFTri.ar(freq:150, mul:0.1)!2}.play; //oscilador 3
```
- relação entre os valores dos argumentos de frequência continua.

- agora digamos que vc não queira modificar manualmente os valores de frequência. vc quer que o computador selecione números aleatórios, aplique esses números como parâmetros de frequência, mas mantendo essa relação.
"ah, isso eu sei fazer. eu vi que existe uma [palavra reservada](link) no SuperCollider chamada `rrand()` que faz isso"
seu primeiro instinto então seria substituir os valores do primeiro argumento `freq:` de cada oscilador por um `rrand()`. Algo como isso...

```
{Pulse.ar(freq:rrand(200, 300), mul:0.1)!2}.play;
{Pulse.ar(freq:rrand(200, 300)+2, mul:0.1)!2}.play;
{LFTri.ar(freq:rrand(100, 150), mul:0.1)!2}.play;
```
- não muito parecido com o exemplo anterior. eu gosto, mas não é exatamente o que estamos procurando.
- pq? o que está acontecendo?
- a diferença advém do fato de que estamos usando 3 funções diferentes de geração de números randômicos. cada um gera um número diferente. (mastigar bem essa explicação)
- perdemos a relação 1:(1)+2:1/2

- talvez exemplo processing? linhas paralelas...

```
size(300, 200);
background(255);
line(150, 0, 150, 200);
```

- se vc nunca estudou processing, não se preocupe em entender cada linha do código acima. o que importa é...
- mesmo problema do synth. mesmo princípio.

- vc, [um ser humano com encéfalo altamente desenvolvido](ilha das flores) está provavelmente já entendeu o problema e está gritando com o computador, "não seja burro. eu preciso que vc escolha apenas **um** número randômico _e use esse mesmo número_ para a calcular a frequência dos três".
- mas pra isso, o computador precisa de alguma maneira de **guardar** esse número, para usar depois. 
- e é exatamente aí que entram as variáveis
- variáveis são ferramenta que nos permite, entre outras coisas que veremos a seguir, resolver o problema acima.

<!-- - <span style="color:blue">some *blue* text</span>. -->
<!-- - <font color = "blue">adf</font> -->

---

### definição
- é a ferramenta computacional que nos permite ensinar o computador a guardar dados que são importantes para o nosso programa. ao guardar, pode acessar novamente / operar sobre esse valor / utilizar em mais de um lugar.
- antes do uso de variáveis, 

- "espaço de alocação de memória". 

- analogia da caixa

### casos de uso //aqui, exemplos dos videos
- legibilidade do código
  - código para o computador rodar / para humano ler 
- reutilizar uma informação
  - responde problema synths SC / linhas paralelas processing
- facilitar manutenção / evitar redundâncias
- operação sobre variáveis
  - freq: numRand + 2
  - freq: freq + (freq * 0.1) - exemplo marcelo synthAdd
	- esse procedimento de usar o valor de uma variável para modificar a própria variável é tão comum em programação que merece um tópico próprio: [iteração]()

- comentário sobre linguagens de data flow?

### tecnicalidades
- as 3 partes: tipo / nome / valor
- tipologia
- escopo
- nomenclatura
- better practices (talvez como item extra "observações extras"? ver code completes)

## notas
<sub>[1] - explicação de hardcoded

## referências

sonic pi tutorial - 5.6

