# MODULARIDADE

---

Se analisarmos nosso [projeto de modulação em anel](), podemos considerar que ele possui três partes claramente delimitadas: algo que nos dá valores numéricos relativos à posição do mouse; um estágio de geração de sinais de áudio; e uma etapa de multiplicação de valores. 

imagem modAnel

Da mesma maneira, também podemos reconhecer no [projeto de sampler]() (apesar de um pouco mais complexo estruturalmente) componentes individuais: carrega-se arquivos de áudio em vários buffers; lê-se esses buffers de uma maneira espefícia; sorteia-se valores que serão utilizados para controle de certos parâmetros da reprodução do áudio.

imagem sampler

Podemos estender essa análise, reconhecendo não apenas que nossos projetos possuem partes individuais menores que juntas formam um todo, mas que essas partes constituintes _trocam informações entre sí_. Os valores de posição do mouse na modulação em anel são "plugados" nos argumentos de frequência dos osciladores. De modo similar, os números sorteados no nosso sampler utilizando ```[].choose``` são enviados para o parâmetro ```rate:``` na função ```PlayBuf.ar()```.

Existe portanto um princípio compartilhado operando, onde módulos menores se conectam para construir a funcionalidade global dos nossos programas.
Quase como que em uma construção de Lego, nesse _princípio de modularidade_ que está em jogo, muitas vezes os componentes individuais cumprem funções pequenas e específicas (sortear um número dentro de uma lista, retornar a posição horizontal do mouse), se re-combinando e re-configurando em algo de maior complexidade.

O reconhecimento desse modo de operação nos permite abordar o desenvolvimento dos nossos programas de um modo mais palatável, principalmente quando estamos tentando desenvolver sistemas que estão um pouco além de um nível básico de construção. 

O fato dos módulos do seu programa compartilharem informações entre sí pode ser entendido como um indício de que esses módulos precisam ser programados concomitantemente. Porém, reconhecemos que tentar programar, ao mesmo tempo, diferentes partes de um sistema, muitas vezes não é uma abordagem recomendada. 
No nosso algoritmo de modulação em anel, por mais que soubessemos _de saída_ que pretendíamos usar a posição do mouse como dispositivo de controle para as frequências dos osciladores, esse aspecto da implementação foi deixado por último. Utilizamos osciladores com frequências estáticas para construir toda lógica de geração de sinal, modulação e endereçamento das saída de áudios, para só então partirmos para a construção do módulo responsável pela leitura das coordenadas do mouse.
De maneira similar, na implementação do nosso sampler, mesmo definido na concepção do projeto que um processo de sorteio de taxas de leitura seria utilizado, toda nossa "máquina tocadora de buffers" foi primeiramente construída com parâmetros fixos, nos certificando de que as implementações das funções ```SynthDef``` e ```PlayBuf.ar()``` estavam devidamente funcionais.

Se trata portanto de uma escolha por um tipo de [arquitetura](https://en.wikipedia.org/wiki/Modular_programming) na construção dos nossos programas, onde vamos reconhecer a especialização de cada etapa do nosso algoritmo ("nesse momento vou me concentrar em implementar a leitura da posição do mouse, e somente isso..."), assim como a modularidade inerente à essa arquitetura ("já que estou controlando a frequência do oscilador com o mouse, talvez eu possa substituir o mouse por um controlador MIDI...").

>Essa abordagem, onde projetamos nosso software através de módulos que ["fazem uma coisa, e fazem essa coisa bem"](https://en.wikipedia.org/wiki/Unix_philosophy#Do_One_Thing_and_Do_It_Well), assim como [outras filosofias de desenvolvimento de software](https://en.wikipedia.org/wiki/KISS_principle), têm origem na década de 70, durante o desenvolvimento do anos mais tarde viria a se tornar o sistema operacional Linux.

### notas

- analogia com rascunho / arte final
- ninguém começa desenhando os detalhes
- parte-se do geral em direção ao específico

- [MVP](https://en.wikipedia.org/wiki/Minimum_viable_product)
- quer programar um delorean, começa fazendo um carrinho de rolimã, não um capacitor de fluxo...

## funções como argumentos de outras funções

trump china

## reaproveitamento de módulos
- escolha de lista sampler
