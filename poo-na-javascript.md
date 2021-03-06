
# Programação orientada a objetos na JavaScript

## O que é um objeto?

Antes de entendermos o conceito de programação orientada a objetos, é válido compreender o que é um objeto. 

Na JavaScript, o objeto pode ser interpretado como um dicionário, pois possui um conjunto de `key/value`, isto é, um conjunto de chaves e valores.

Por exemplo, se você deseja saber o que significa a palavra "inspiração", procura por ela dentro de um dicionário para saber qual é o valor correspondente a ela. Na JS, o que ocorre é o mesmo. Se deseja saber qual é o valor de determinada chave, ou propriedade, de um objeto, faz referência a ela.

### Sintaxe

O objeto na JS é representado por chaves (`{}`). Seguindo o exemplo anterior, vamos supor que deseje criar uma variável cujo valor é um pequeno dicionário de palavras. Veja como poderia fazer:

```js
var dicionario = {
  programar: 'Significado de programar',
  café: 'Significado de café',
  inspiração: 'Significado de inspiração',
  codificar: 'Signifcado de codificar'
};
```
Certo, temos nosso dicionário. Mas como procuramos o significado de "inspiração" dentro dele?
```js
var significado = dicionario.inspiração;
```
Se imprimirmos o valor da variável `significado` na tela ou no console, o valor obtido será "Significado de inspiração". Bem mais simples do que folhear um dicionário, não acha?

Agora vamos para um outro exemplo. Vamos supor que deseje desenvolver um jogo de corrida. Para isto, cada carro possuirá as informações nome, velocidade atual e velocidade máxima e algumas ações, como acelerar ou frear. Como podemos fazer isso?
```js
var carro1 = {
  nome: 'Carro 1',
  velocidadeAtual: 0,
  velocidadeMaxima: 220,
  acelerar: function(valor) {
    if (this.velocidadeAtual + valor <= this.velocidadeMaxima) {
      this.velocidadeAtual += valor;
    }
  },
  frear: function(valor) {
    if (valor && this.velocidadeAtual - valor >= 0) {
      this.velocidadeAtual -= valor;
    } else {
      this.velocidadeAtual = 0;
    }
  }
};
```

Certo, agora estamos com um objeto um pouco diferente: ele possui propriedades e métodos. O que difere um método de uma propriedade é que seu valor é uma função. Ou seja, métodos realizam ações, podendo modificar as propriedades do objeto ao qual fazem parte. No nosso exemplo, o método `acelerar` acessa as propriedades `velocidadeAtual` e `velocidadeMaxima`, verifica se a velocidade do carro pode ser aumentada e, se sim, altera o valor da `velocidadeAtual`, acrescentando o valor que passamos como argumento.

Na prática, poderíamos utilizar o código acima da seguinte forma para testar os comandos:
```js
carro1.acelerar(20); // Altera a velocidade para 20
carro1.acelerar(25); // Altera a velocidade para 45
carro1.frear(10); // Altera a velocidade para 35
```

Agora, preste atenção a um detalhe:
```js
if (this.velocidadeAtual + valor <= this.velocidadeMaxima) {
  this.velocidadeAtual += valor;
}
```
Por que utilizamos a palavra-chave `this` antes do nome da propriedade que desejamos ler ou alterar? Porque propriedades e métodos são dependentes do objeto ao qual fazem parte. Portanto, se escrevêssemos apenas `velocidadeAtual`, o nosso interpretador buscaria por uma variável com este nome ao invés da propriedade dentro do objeto. Isso quer dizer que sempre devemos utilizar a seguinte sintaxe para ler propriedades de dentro do objeto:

`this.nomeDaPropriedade;`

Mas, para ler uma propriedade do objeto do lado de fora deste, devemos utilizar a sintaxe:

`nomeDoObjeto.nomeDaPropriedade;`


## Quais as vantagens de um código orientado a objetos?

A vantagem já começa pelo fato de que tudo na JS é um objeto, o que inclui o escopo global onde escrevemos o nosso código. Quer ver?
```js
console.log('Algum texto aqui');
this.console.log('Outro texto aqui');
```
Se rodar este código e abrir o console, as duas linhas irão aparecer, incluindo aquela que possui a palavra-chave `this` antes da referência ao `console`.

Até o fim desta leitura, irá perceber dois fatores essenciais para uma programação mais confortável: legibilidade e automatização.

Mas as vantagens se tornam perceptíveis ao passo que o nosso código aumenta. No exemplo do jogo de carros, vimos que controlar as propriedades do carro de dentro do objeto é realmente muito eficaz. Mas e se você tivesse que criar 10 carros? Aí o código ficaria bem extenso, sem considerar ainda que seria fácil cometer erros, como escrever errado o nome de alguma propriedade ou método.

Como podemos contornar essa situação?

## Classes

As classes permitem que criemos um modelo de objeto que representará uma entidade específica no nosso código.

```js
class Carro {
  constructor(nome) {
    this.nome = nome;
    this.velocidadeMaxima = 220;
    this.velocidadeAtual = 0;
  }

  acelerar(valor) {
    if (this.velocidadeAtual + valor <= this.velocidadeMaxima) {
      this.velocidadeAtual += valor;
    }
  }

  frear(valor) {
    if (valor && this.velocidadeAtual - valor >= 0) {
      this.velocidadeAtual -= valor;
    } else {
      this.velocidadeAtual = 0;
    }
  }
}
```

Vamos analisar o código acima. Ao invés de criar as propriedades diretamente, definimos elas dentro de uma função construtora. O `constructor` é disparado assim que a instância é criada. Ou seja, quando criarmos um novo carro utilizando esta classe, o código dentro de `constructor` será rodado. Isto quer dizer que é lá que devemos definir as propriedades que farão parte do nosso objeto e seus respectivos valores.

Também é possível definir parâmetros no `constructor`:
```js
  ...
  constructor(nome) {
    this.nome = nome;
  ...
```
Agora, se quisermos criar um novo carro, basta invocar a nossa classe utilizando a palavra-chave `new`:
```js
var carro1 = new Carro('Carro 1');
```
Podemos alterar os valores do objeto ou utilizar seus métodos da seguinte maneira:
```js
carro1.nome = 'Impala';
carro1.acelerar(100);
```
Assim, é possível criar quantas instâncias da classe `Carro` forem necessárias utilizando apenas uma linha para cada carro:
```js
var carro2 = new Carro('Fusca');
var carro3 = new Carro('Opala');
```

Lembra daquele código grande que escrevemos no princípio para criar um carro? Agora ele foi diminuído para apenas uma linha!

Podemos modificar outras propriedades no nosso `constructor`. Todo jogo de corrida legal possui carros com velocidades diferentes. Vamos deixar o nosso assim também?

Podemos deixar que a velocidade máxima seja personalizada e garantir um valor padrão, para quando nenhum outro for passado. Veja:
```js
  ...
  constructor(nome, velocidadeMaxima) {
    this.nome = nome;
    this.velocidadeMaxima = velocidadeMaxima || 220;
    ...
```

Nossos carros já podem ser criados desta forma:
```js
var carro1 = new Carro('Carro lento', 60);
var carro2 = new Carro('Carro com velocidade padrão');
var carro3 = new Carro('Carro veloz', 300);
```
As velocidades máximas destes carros serão, respectivamente: 60, 220 e 300.

### O que significa ser um carro?

As classes devem sempre representar uma entidade. No nosso caso, a entidade é o carro. Ao criar uma classe baseando-se em uma entidade - neste caso o carro -, pare e pense:
- O que significa ser um carro?
- O que um carro tem?
- O que um carro faz?

As propriedades e métodos da classe serão instanciadas em todos os objetos por ela criados. Portanto, só coloque na classe `Carro` aquilo que todos os carros do jogo irão possuir.

Em alguns casos, porém, você terá carros com as mesmas propriedades e métodos da classe `Carro`, mas com algumas características a mais.

Por exemplo, vamos supor que alguns carros tenham a opção turbo, que faz com que o carro vá direto à sua velocidade máxima. Estes carros continuarão tendo as mesmas propriedades e métodos que os outros, entretanto terão também o método `turbo`. E agora? Como resolvemos isso?

### Extends 

Para criar carros com a opção turbo, podemos criar uma nova classe que irá estender a classe `Carro`:
```js
class Turbo extends Carro {
  turbo() {
    this.velocidadeAtual = this.velocidadeMaxima;
  }
}
```

Que tal criarmos um novo carro com a opção turbo?

```js
var carro1 = new Turbo('Carro com turbo', 320);
carro1.turbo();
carro1.frear(20);
```
A velocidade atual de `carro1` é 300.

>Tanto os carros criados diretamente com o comando `new Carro()` como os criados com `new Turbo()` são instâncias da classe `Carro`. Isto acontece porque  `Carro` é a classe mãe de `Turbo`. Ou seja, independente de como as instâncias forem criadas, todos os objetos representarão carros.


## Conclusão

Escrever um código orientado a objetos pode trazer mais praticidade ao desenvolvedor:

 - **O código fica mais organizado**, pois declaramos propriedades e métodos dentro dos escopos onde eles serão utilizados, isto é, suas classes. Assim, fica muito mais fácil encontrar a parte do código que precisa ser editada.
 - A **redução do número de linhas de código** é claramente perceptível, uma vez que podemos instanciar classes com apenas uma linha de código.
 - Ao invés de criar diversas funções e passar nossos objetos como parâmetros para elas, fazemos com que as ações modificadoras de conteúdo do objeto ocorram dentro dele mesmo. Deste modo, **são os objetos que orquestram o código**, sem necessitar de chamadas de funções externas.

>Vale lembrar que classes, na sintaxe que vimos aqui, são um recurso novo da JS, o que quer dizer que muitos navegadores ainda não as suportam. Para contornar esta situação, você pode instalar pacotes do Node que adaptam o seu código para os navegadores. Um deles é o [Browserify](http://browserify.org/).
