# VARIÁVEIS

---

Variáveis estão entre as ferramentas mais basilares do estudo em computação. Se você teve contato, mesmo que inicial, com alguma linguagem de programação, é bastante provável que já tenha esbarrado com o termo.
Apesar de se tratar de um conceito fundamental em computação, coisas interessantes são possíveis de serem criadas sem recorrer ao uso de variáveis.[###exemplo rápido processing/sc/sonic pi]. Muitos dos exemplos do nosso texto sobre [palavras reservadas](txt_palavrasReservadas.md) são construídos dessa maneira. Porém, como veremos a seguir, se trata de uma ferramenta que cumpre algumas funções tão importantes no processo de desenvolvimento de um programa, que seria difícil um estudo mais aprofundado sem abordarmos o tema.

Mas, antes de definirmos precisamente o conceito, ou discutirmos aspectos técnicos, pensemos primeiramente _por que_ variáveis existem. Afinal, se se trata de algo tão  ubíquo, presente em praticamente todas as linguagens de programação<sup>[1]</sup>, variáveis devem suprir alguma necessidade bastante importante dentro do processo de criação de um programa.
Em outras palavas, o que é que nós _não conseguimos fazer_ sem usarmos variáveis?

---

Ouça esse exemplo, feito em SuperCollider, onde três osciladores são acionados um após o outro. Recomendo usar fones de ouvido. Alto-falantes de laptop não costumam gostar de frequências muito graves...

```ruby
{Saw.ar(freq:200, mul:0.1)!2}.play;   //oscilador 1
{Saw.ar(freq:202, mul:0.1)!2}.play;   //oscilador 2
{LFTri.ar(freq:100, mul:0.1)!2}.play; //oscilador 3
```
<center>###Tocador de áudio</center>

O que temos aqui são três geradores de som distintos, onde através do argumento `freq`, seus valores de frequência são especificados: `freq:200`, `freq:202` e `freq:100`.
Perceba que existe uma relação simples entre esses valores. O segundo oscilador tem a frequência do primeiro adicionada em dois [Hertz](https://en.wikipedia.org/wiki/Hertz) (`freq:200` e `freq:202`), e o terceiro oscilador possui a metade da frequência do primeiro (`freq:200 e freq:100`)<sup>[2]<sup>. 

Se modificarmos todas as frequências mas mantivermos essa razão entre os valores, obtemos um som que é mais agudo ou mais grave (dependendo se escolhermos valores maiores ou menores), mas cuja sonoridade geral permanece bastante similar. Ouça o exemplo anterior novamente, e compare com esse:

```ruby
{Saw.ar(freq:300, mul:0.1)!2}.play;   //oscilador 1
{Saw.ar(freq:302, mul:0.1)!2}.play;   //oscilador 2
{LFTri.ar(freq:150, mul:0.1)!2}.play; //oscilador 3
```

<center>###Tocador de áudio</center>

Ainda temos a mesma relação entre o primeiro oscilador e os dois seguintes. Adiciona-se dois Hertz para um, e divide-se a frequência pela metade para o outro: 300:302:150.
Você provavelmente percebeu que o som é mais agudo quando comparado com o primeiro exemplo (`freq:300` > `freq:200`), mas o timbre é bastante similar. A mesma oscilação, o mesmo <a href="https://en.wikipedia.org/wiki/Beat_(acoustics)">batimento</a> resultante da relação entre os dois primeiros osciladores; e a mesma componente grave adicionada pelo terceiro oscilador - grave esse que, para nossos ouvidos ocidentais acostumados com Bach, Beatles e Beyoncé, soa bastante consonante.

---

Você pode gastar bastante tempo experimentando outros valores de frequência para comparar diferentes sonoridades (e recomendo que você faça isso!)<sup>3</sup>. Entretanto, em algum momento pode ser interessante transferir o pesado fardo da escolha desses números de você programador, para o computador. Uma possível  maneira de se fazer isso é solicitando que o programa escolha números aleatórios, e utilize esses números como argumentos de frequência para os osciladores.

Esse seria um ótimo momento para você dizer algo como: "Ah, isso eu sei fazer! Eu vi que existe uma palavra reservada no SuperCollider chamada `rrand()` que faz exatamente isso. Eu só preciso escrever `rrand(1, 10)` por exemplo, que o programa escolhe pra mim um número aleatório entre 1 e 10".
Sabendo do princípio de modularidade que permite que [funções possam ser utilizadas como argumentos de outras funções](txt_modularidade.md), seu primeiro instinto poderia ser o de substituir os valores do argumento `freq` de cada oscilador por uma função `rrand()`. Dessa maneira:

```ruby
{Saw.ar(freq:rrand(200, 300), mul:0.1)!2}.play;
{Saw.ar(freq:rrand(200, 300)+2, mul:0.1)!2}.play;
{LFTri.ar(freq:rrand(200, 300)/2, mul:0.1)!2}.play; 
```

<center>###Tocador de áudio</center>

Com `freq:rrand(200, 300)`, estamos solicitando um valor randômico entre 200 e 300, que está servido de frequência para o primeiro oscilador. Em seguida, tentamos aplicar a mesma lógica dos dois exemplos anteriores para os osciladores restantes: adicionar dois Hertz para o segundo oscilador, e dividir por dois para o terceiro oscilador.
O resultado sonoro no entanto não soa muito parecido com o que ouvimos anteriormente, certo? Eu gosto do som, mas não é exatamente o que estamos procurando nesse momento. 
Se em todos os três exemplos até agora, em tese, aplicamos exatamente a mesma lógica na atribuição de valores aos nossos argumentos `freq`, por que essa diferença sonora? O que está acontecendo?

Nos dois primeiros exemplos (aqueles em que escrevemos manualmente os valores dos argumentos `freq`) as frequências dos osciladores 2 e 3 foram escolhidas _a partir_ do oscilador 1, certo? 
Veja. Ao decidirmos por `freq:300` para o oscilador 1 no segundo exemplo, os próximos passos, as etapas seguintes do algoritmo que fizemos mentalmente, foram "para o oscilador 2, somar 2 ao valor de frequência do oscilador 1": 302 = 300 + 2. Seguido de "para o oscilador 3, dividir por 2 o valor de frequência do oscilador 1": 150 = 300 / 2.

Aí está exatamente a fonte do nosso problema: nosso programa novo, que gera valores randômicos para as frequências, não está escolhendo um valor para o primeiro oscilador e _em função desse valor_ decidindo as frequências dos dois osciladores seguintes, mas sim escolhendo três valores diferentes, um para cada oscilador. Cada ocorrência da função `rrand(200, 300)` _gera um número diferente_, o que quebra nossa lógica de "soma 2, divide por 2".
É por esse motivo que não só esse terceiro exemplo soa diferente dos dois anteriores, como ele vai soar diferente cada vez que você testar!

>Para os mais fãs de matemática, poderiamos dizer que nessa versão do programa nós perdemos a razão n:n+2:n/2 entre as frequências dos três osciladores.

Existe a chance dos três `rrand(200, 300)` gerarem exatamente o mesmo número (preservando assim a nossa lógica, e fazendo o terceiro exemplo soar similar aos dois primeiros)? Eu não testei, mas a matemática me diz que se você tentar um milhão de vezes, vai acontecer.

Nesse momento você, [um ser humano com telencéfalo altamente desenvolvido](https://www.youtube.com/watch?v=e7sD6mdXUyg), provavelmente já entendeu o problema em questão, e está gritando para a tela do seu computador algo como "Hora, mas isso é fácil de resolver! Eu só preciso que meu programa gere **apenas um** número randômico, _e use esse mesmo número_ para definir os valores de `freq` nos três osciladores. 
Em outras palavras, você precisa que seu programa seja capaz de **guardar** uma informação (como um número gerado randomicamente), para que possa ser acessado e utilizado posteriormente. O que você precisa, é que seu programa aprenda a lembrar.

Se você pensou isso, então você já sabe o que são variáveis. O resto são detalhes técnicos e convenções de sintaxe.

<br>

### NOTAS

[1] - Todas? [esoteric languages](https://en.wikipedia.org/wiki/Esoteric_programming_language)

[2] - Comum em síntese. certas relações de valores de frequências - valores muito próximos, dobro, metade. É isso que faz ouvir como um som só.

[3] - Comentário sobre parar o servidor

<br>

---

<br>

#### ANOTAÇÕES

Algumas anotações não organizadas sobre possíveis caminhos e conteúdos que farão parte da versão final do texto.

- exemplo processing. linhas paralelas. mesmo princípio.

```
size(300, 200);
background(255);
line(150, 0, 150, 200);
//line(random(0, 150), 0, random(0, 150), 200);
```

##### definição
- é a ferramenta computacional que nos permite ensinar o computador a guardar dados que são importantes para o nosso programa. pq isso é importante? pq ao guardar, pode acessar novamente / utilizar em mais de um lugar. operar sobre esse valor (freq + 2)
- antes do uso de variáveis, todos os dados são gerados e em seguida descartados... ou seja, é como damos uma memória para nosso programa (nada a ver com downloadmoreram.com)

- "espaço de alocação de memória". 

- analogia da caixa

##### casos de uso
- legibilidade do código
  - código para o computador rodar / para humano ler 
- reutilizar uma informação
  - responde problema synths SC / linhas paralelas processing
- facilitar manutenção / evitar redundâncias
- operação sobre variáveis
  - freq: numRand + 2
  - freq: freq + (freq * 0.1)
	- esse procedimento de usar o valor de uma variável para modificar a própria variável é tão comum que ganha um nome: [iteração](https://www.dropbox.com/s/h4bqoftt1n5q1ce/ileqi3i18vxy.jpg?dl=0). (###explicar aqui, ou merece tópico próprio? talvez linkar para texto sobre estruturas de controle...)

- comentário sobre linguagens de data flow?
  - talvez um tópico inteiro ("sobre variáveis e linguagens de fluxo de dados, ou, pq eu nunca ouvi falar disso em max/pd?")

##### tecnicalidades
- int foo = 10; as 4 partes: 
  - tipo: a caixa comporta vários tipos de dados, e cada tipo precisa de uma caixa com um tamanho diferente
	- sc var vs. arduino int/float vs "nada" sonic pi
	- 1 + 1 = 11
  - nome: a caixa tem uma etiqueta
	- só se recupera da memória aquilo que possui uma representação simbólica (AKA um "nome"). 
	  - {}.fork vs ~batera = {}.fork 
	- convenções de nomenclatura
  - sinal de atribuição: a caixa permite que vc coloque coisas dentro da caixa (mas nem sempre...)
	- x = x + 1 é uma inverdade matemática / x == x + 1 é uma inverdade computacional
  - valor: a caixa tem um conteúdo
	- overflow
- declaração / atribuição - dois estágios (que podem ser...) separados
- escopo
  - bagunça do sc entre global e environment variables...
- better practices (talvez como item extra "observações extras"? ver code complete)
