# MODULARIDADE

---

Se analisarmos nosso [projeto de modulação em anel](proj_01_rm.md) podemos considerar que ele possui três partes claramente delimitadas: algo que nos dá valores numéricos relativos à posição do mouse; um estágio de geração de sinais de áudio; e uma etapa de multiplicação de valores. 

![](./img/proj_modAnel.jpg)

Da mesma maneira também podemos reconhecer no [projeto de sampler](proj_03_sampler.md) (apesar de um pouco mais complexo estruturalmente) componentes individuais: carrega-se arquivos de áudio em vários buffers; lê-se esses buffers de uma maneira específica; sorteiam-se valores que serão utilizados para controle de certos parâmetros da reprodução do áudio.

![](./img/proj_sampler.jpg)

Podemos estender essa análise, reconhecendo não apenas que nossos projetos possuem partes individuais menores que juntas formam um todo, mas que essas partes constituintes _trocam informações entre si_. Os valores de posição do mouse na modulação em anel são "plugados" nos argumentos de frequência dos osciladores. De modo similar, os números sorteados no nosso sampler utilizando ```[].choose``` são enviados para o parâmetro ```rate:``` na função ```PlayBuf.ar()```.

Existe portanto um princípio compartilhado operando, em que módulos menores se conectam para construir a funcionalidade global dos nossos programas.
Quase como que em uma construção de Lego, nesse _princípio de modularidade_ que está em jogo, muitas vezes as componentes individuais cumprem funções pequenas e específicas (sortear um número dentro de uma lista, retornar a posição horizontal do mouse), se recombinando e reconfigurando em algo de maior complexidade.

O reconhecimento desse modo de operação nos permite abordar o desenvolvimento dos nossos programas de um modo mais palatável, principalmente quando estamos tentando desenvolver sistemas que estão um pouco além de um nível básico de construção. 

O fato dos módulos do seu programa compartilharem informações entre si pode ser entendido como um indício de que esses módulos precisam ser programados concomitantemente. Porém, reconhecemos que tentar programar, ao mesmo tempo, diferentes partes de um sistema, muitas vezes não é uma abordagem recomendada. 
No nosso algoritmo de modulação em anel, por mais que soubessemos _de saída_ que pretendíamos usar a posição do mouse como dispositivo de controle para as frequências dos osciladores, esse aspecto da implementação foi deixado por último. Utilizamos osciladores com frequências estáticas para construir toda lógica de geração, modulação e endereçamento dos sinais de áudio, para só então partirmos para a construção do módulo responsável pela leitura das coordenadas do mouse.
De maneira similar, na implementação do nosso sampler, mesmo definido na concepção do projeto que um processo de sorteio de taxas de leitura seria utilizado, toda nossa "máquina tocadora de buffers" foi primeiramente construída com parâmetros fixos, nos certificando de que as implementações das funções ```SynthDef``` e ```PlayBuf.ar()``` estariam devidamente funcionais.

<p align = "center">
<img src = "./img/kiss.jpeg" title = "Do one thing, and do it well" width=60% height=60%>
</p>
<!-- ![](./img/kiss.jpeg "Keep it stupid, simple...") -->

Se trata portanto de uma escolha por um tipo de [arquitetura](https://en.wikipedia.org/wiki/Modular_programming) na construção dos nossos programas, onde vamos reconhecer a especialização de cada etapa do nosso algoritmo ("nesse momento vou me concentrar em implementar a leitura da posição do mouse, e somente isso..."), assim como a modularidade inerente à essa arquitetura ("já que estou controlando a frequência do oscilador com o mouse, talvez eu possa substituir o mouse por um controlador MIDI...").

>Essa abordagem, onde projetamos nosso software através de módulos que ["fazem uma coisa, e fazem essa coisa bem"](https://en.wikipedia.org/wiki/Unix_philosophy#Do_One_Thing_and_Do_It_Well), assim como [outras filosofias de desenvolvimento de software](https://en.wikipedia.org/wiki/KISS_principle), têm origem na década de 70, durante o desenvolvimento do que anos mais tarde viria a se tornar o sistema operacional Linux.

<br>

---

<br>

#### ANOTAÇÕES

Algumas anotações não organizadas sobre possíveis caminhos e conteúdos que farão parte da versão final do texto.

- analogia com rascunho / arte final
  - ninguém começa desenhando os detalhes
  - parte-se do geral em direção ao específico

<br>

- [MVP](https://en.wikipedia.org/wiki/Minimum_viable_product)
  - quer programar um delorean, começa fazendo um carrinho de rolimã, não um capacitor de fluxo...

##### funções como argumentos de outras funções

- SinOsc.ar(freq:MouseX.kr()) || fill(random(255), random(255), random(255)).
- princípio simples que permite resultados complexos. "como controlar a frequência de oscilador usando a distância de um sensor de arduino?
  - SinOsc.ar(FuncaoMagicaQueConectaComArduino.kr()) -> conceitualmente igual à SinOsc.ar(MouseX.kr())
- escalonamento de valores - map(), .range(), [scale 0 127 0. 1.]

##### reaproveitamento de módulos

- projeto de sampler. reaproveitamento do sorteador de números na lista usando [zl.mth]
