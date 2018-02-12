# Variáveis

---

Variáveis estão entre as ferramentas mais fundamentais no estudo da computação. Se você já teve contato antes, mesmo que inicial, com alguma linguagem de programação, é bastante provável que tenha esbarrado com o termo.
Apesar de de tratar de um conceito fundamental em computação, vimos como é possível chegar em resultados interessantes sem fazermos uso de variáveis. Muitos dos exemplos do nosso texto sobre [palavras reservadas]() são construídos dessa maneira. Porém, como veremos a seguir, variáveis são uma ###ferramenta que cumpre algumas funções tão importantes no processo de desenvolvimento de um programa, que seria difícil um estudo mais aprofundado sem abordarmos o tema.

Mas, antes de definirmos precisamente o conceito, ou discutirmos aspectos técnicos, pensemos primeiramente _por que_ variáveis existem. Afinal, se se trata de algo tão  ubíquo, presente em praticamente todas as linguagens de programação <sup>[1]</sup>, variáveis devem suprir alguma necessidade bastante importante dentro do processo de criação de um programa.
Em outras palavas, o que é que nós _não conseguimos fazer_ sem usarmos variáveis?

---

Ouça esse exemplo, feito em SuperCollider###, onde três osciladores são acionados um após o outro. Recomendo usar fones de ouvido, pois algumas das frequências são muito graves para as caixinhas de som de qualquer laptop##.

```arduino
{Saw.ar(freq:200, mul:0.1)!2}.play;   //oscilador 1
{Saw.ar(freq:202, mul:0.1)!2}.play;   //oscilador 2
{LFTri.ar(freq:100, mul:0.1)!2}.play; //oscilador 3
```
<center>###Tocador de áudio</center>

O que temos aqui são três geradores de som distintos, onde através do [argumento]() `freq`, seus valores de frequência são especificados: `freq:200`, `freq:202` e `freq:100`.
Perceba que existe uma relação simples entre esses valores. O segundo oscilador tem a frequência do primeiro, adicionada em dois [Hertz]() (`freq:200` e `freq:202`), e o terceiro oscilador possui a metade da frequência do primeiro (`freq:200 e freq:100`)<sup>[2]<sup>. 

Se modificarmos todas as frequências mas mantivermos essa razão entre os valores, obtemos um som que é mais agudo ou mais grave (dependendo se escolhermos valores maiores ou menores), mas cuja sonoridade geral permanece bastante similar. Ouça o exemplo anterior novamente, e compare com esse:

```
{Saw.ar(freq:300, mul:0.1)!2}.play;   //oscilador 1
{Saw.ar(freq:302, mul:0.1)!2}.play;   //oscilador 2
{LFTri.ar(freq:150, mul:0.1)!2}.play; //oscilador 3
```

<center>###Tocador de áudio</center>

Ainda temos a mesma relação entre o primeiro oscilador e os dois seguintes: adiciona-se dois Hertz para um, e divide-se a frequência pela metade para o outro. Você provavelmente percebeu que o som é mais agudo quando comparado com o primeiro exemplo (`freq:300` > `freq:200`), mas o timbre é bastante similar. A mesma oscilação###, o mesmo [batimento]() resultante da relação entre os dois primeiros osciladores; e a mesma componente grave adicionada pelo terceiro oscilador - grave esse que, para nossos ouvidos ocidentais acostumados com Bach, Beatles e Beyoncé, soa bastante consonante.

---

Você pode gastar bastante tempo experimentando outros valores de frequência para comparar diferentes sonoridades (e recomendo que você faça isso!)<sup>3</sup>. Entretanto, em algum momento pode ser interessante transferir o pesado fardo da escolha desses números de você programador, para o computador. Uma possível  maneira de se fazer isso é solicitando que o programa escolha números aleatórios, [randômicos](), e utilize esses números como argumentos de frequência para os osciladores.

Esse seria um ótimo momento para você dizer algo como: "Ah, isso eu sei fazer! Eu vi que existe uma palavra reservada no SuperCollider chamada `rrand()` que faz exatamente ###isso. Eu só preciso escrever `rrand(1, 10)` por exemplo, que é gerado um número aleatório entre 1 e 10".
Sabendo do princípio de modularidade que permite que [funções possam ser utilizadas como argumentos de outras funções](), seu primeiro instinto poderia ser o de substituir os valores do primeiro argumento `freq` de cada oscilador por uma função `rrand()`. Dessa maneira:

```
{Saw.ar(freq:rrand(200, 300), mul:0.1)!2}.play;   //oscilador 1
{Saw.ar(freq:rrand(200, 300)+2, mul:0.1)!2}.play; //oscilador 2
{LFTri.ar(freq:rrand(200, 300)/2, mul:0.1)!2}.play; //oscilador 3
```

<center>###Tocador de áudio</center>

Com `freq:rrand(200, 300)`, estamos solicitando um valor randômico entre 200 e 300, que está servido de frequência para o primeiro oscilador. Em seguida, aplicamos a mesma lógica dos dois exemplos anteriores para os osciladores restantes: usar o valor do primeiro oscilador adicionado em dois, e em seguida dividido por dois.
O resultado sonoro no entanto não soa muito parecido com o que ouvimos anteriormente, certo? Eu gosto do som###, mas não é exatamente o que estamos procurando nesse momento. 
Se em todos os exemplos, em tese, aplicamos exatamente a mesma lógica na atribuição de valores aos nossos argumentos `freq`, pq### essa diferença sonora? O ###quê está acontecendo?

Nos dois primeiros exemplos (aqueles em que escrevemos manualmente os valores dos argumentos `freq`) as frequências dos osciladores 2 e 3 foram escolhidas _a partir_ do oscilador 1, certo? 
Após definir um valor para o argumento `freq` do oscilador 1 (`freq:200` e `freq:300` nos nosso exemplos), os próximos passos, as etapas seguintes do nosso [algoritmo]() eram "para o oscilador 2, somar 2 ao valor de frequência do oscilador 1" e "para o oscilador 3, dividir por 2 o valor de frequência do oscilador 1". Só assim então obtemos os valores 100, 202, 150, etc.

Aí está exatamente a [fonte do problema](): nosso programa novo, que gera valores randômicos para as frequências, não está escolhendo um valor para o primeiro oscilador e _em função desse valor_ decidindo as frequências dos dois osciladores seguintes, mas sim escolhendo _três valores diferentes_, um para cada oscilador. Cada instância da função `rrand(200, 300)` gera um número diferente, o que quebra nossa lógica de "soma 2, divide por 2".

É por esse motivo que não só esse terceiro exemplo soa diferente dos dois anteriores, como ele vai soar diferente cada vez que você testar!
O áudio aqui postado é apenas uma possível combinação de valores randômicos gerados pela utilização de três funções `rrand(200, 300)` diferentes. Na realidade, o que você está ouvindo no terceiro exemplo é, na prática, o mesmo que:<sup>[4]</sup>


```
{Saw.ar(freq:x, mul:0.1)!2}.play;   //oscilador 1
{Saw.ar(freq:y, mul:0.1)!2}.play; //oscilador 2
{LFTri.ar(freq:z, mul:0.1)!2}.play; //oscilador 3
```

Para os mais ###fãs de matemática, poderiamos dizer que nessa versão do programa nós perdemos a razão ###n:n+2:n/2 entre as frequências dos três osciladores.
Existe a chance dos três `rrand(200, 300)` gerarem exatamente o mesmo número (fazendo o terceiro exemplo soar similar aos dois primeiros)? Eu não testei, mas a matemática me diz que se você tentar um milhão de vezes, vai acontecer.

---

exemplo processing?

---

Nesse momento você, [um ser humano com encéfalo altamente desenvolvido](), provavelmente já entendeu o problema em questão, e está gritando para a tela do seu computador algo como "Hora###, mas isso é fácil de resolver! Eu só preciso que meu programa gere **apenas um** número randômico, _e use esse mesmo número_ para definir os valores de `freq` nos três osciladores / definir as coordenadas _x_ do início e fim da função `line()`. E outras palavras, você precisa que seu programa seja capaz de **guardar** uma informação (como um número gerado randomicamente), para que possa ser acessado e utilizado posteriormente.
Se você está fazendo isso, meus parabéns, você já sabe o que são variáveis. O resto são detalhes técnicos e convenções de sintaxe.









## NOTAS

[1] - [todas? esoteric languages]()
[2] - Comum em síntese. certas relações de valores de frequências - valores muito próximos, dobro, metade. É isso que faz ouvir como um som só.
[3] - comentário sobre parar o servidor
[4] - mencionar .poll(1)

## topicos

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

- vc pode querer trocar para outras frequências, mas mantendo essas relações entre os valores

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
- não muito parecido com os dois exemplos anteriores. eu gosto, mas não é exatamente o que estamos procurando.
- pq? o que está acontecendo?
- a diferença advém do fato de que 1.nos exemplos anteriores a freq dos osc 2 e 3 são _em função da freq do osc 1_ 2.como estamos usando 3 funções diferentes de geração de números randômicos. cada uma gera um número diferente. (mastigar bem essa explicação)
- perdemos a razão n:n+2:n/2
- repetir literalmente exemplo do rrand, mas com os valores hardcoded
  - nota de rodapé. .poll
- talvez exemplo processing? linhas paralelas...

```
size(300, 200);
background(255);
line(150, 0, 150, 200);
```

- se vc nunca estudou processing, não se preocupe em entender cada linha do código acima. o que importa é...
- mesmo problema do synth. mesmo princípio.

- nesse momento, vc, [um ser humano com encéfalo altamente desenvolvido]() está provavelmente já entendeu o problema e está gritando para a tela do seu computador dizendo, "isso é fácil de resolver. eu só preciso que vc escolha apenas **um** número randômico _e use esse mesmo número_ para a calcular a frequência dos três osc / posição da linha".
- se você está fazendo isso, meus parabéns, vc já sabe o que são variáveis. o resto agora são detalhes técnicos.
- variáveis são ferramentas que nos permites, entre outras coisas que veremos a seguir, resolver o problema acima.
- variáveis permitem que ao seu programa **guardar** uma informação (como um número que representa uma frequência), para usar depois. 

<!-- - <span style="color:blue">some *blue* text</span>. -->
<!-- - <font color = "blue">adf</font> -->

---

### definição
- é a ferramenta computacional que nos permite ensinar o computador a guardar dados que são importantes para o nosso programa. pq isso é importante? pq ao guardar, pode acessar novamente / utilizar em mais de um lugar. operar sobre esse valor (freq + 2)
- antes do uso de variáveis... ou seja, é como damos uma memória para nosso programa (downloadmoreram.com)

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
  - talvez um tópico inteiro ("sobre variáveis e linguagens de fluxo de dados)

### tecnicalidades
- as 3 partes: tipo / nome / valor
- declaração / atribuição
- tipologia
- escopo
- nomenclatura
- better practices (talvez como item extra "observações extras"? ver code complete)

## referências

sonic pi tutorial - 5.6

