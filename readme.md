<h1>ANOTAÇÕES</h1>

``` 
npm install
```
Vamos baixar os módulos do node.

<br></br>

A pasta dist vai ser compartilhada com o navegador, exclusivamente ela, pois quando olhamos para o package.json, vemos que o server está alocado na pasta dist.

<br></br>

Vamos utilizar um arquivo js como módulo no index.html.
```
<script src="../dist/js/app.js" type="module"></script>
```
<br></br>

Em um arquivo js vamos começar a programar (negociacao.js), dentro da classe negociacao, vamos ter que criar as variaveis, que nesse caso vão ser privadas, então utilizaremos o #.
```
#data;
```
Nesse caso só vamos conseguir alterar o valor com o constructor, e nada mais.

<br></br>

```
const negociacao = new Negociacao(new Date(), 10, 100);
```
Nesse caso estamos passando os valores para o construtor, então estamos passando a data de hoje, a quantidade(10) e o valor(100).

<br></br>

Caso algum erro em código aconteça, o arquivo JS não vai me retornar o erro em código, e sim em run time, ou seja, quando rodar o site, e isso pode ser modificado utilizando o TS.
Para rodar o TS temos que fazer a instalação dele.
```
npm install typescript@version --save-dev  
```
No caso vamos mudar a parte de "version" e colocar a versão do ts que utilizaremos.

<br></br>

O navegador não vai entender a linguagem ts, então temos que mover os arquivos que antes eram js (agora são ts) para a pasta app, pois tudo o que for em ts vai ser convertido em js. Tudo em ts vai ser compilado para js.

<br></br>

Precisamos configurar o ts, vamos criar um arquivo chamado "tsconfig.json" e passar algumas informações para ele. 
```
"compilerOptions": {
        "outDir" : "dist/js",
        "target": "ES6",
        "noEmitOnError": true
    }
```
Nesse caso estamos passando as opções de compilação. "outDir" vai mostrar onde você quer compilar, "target" vai definir qual versão do EcmaScript você vai utilizar para que converta o ts para js. O "noEmitOnError" vai fazer com que enquanto o arquivo ts estiver com erro, não crie um arquivo js.
Temos que indicar onde está os arquivos .ts
```
"include": ["app/**/*"]
```
E ainda precisamos dizer no arquivo package.json o compilador dentro do script.
```
"compile": "tsc"
```
Nesse caso ele vai procurar em "node_modules" o tsc para que a compilação aconteça.

<br></br>

Para compilar, vamos utilizar um comando do node.
```
npm run compile
```
obs: caso abra o arquivo js e esteja meio diferente do que você escreveu, não se preocupe, quando compilamos o arquivo ts com o ecmascript, ele vai pegar o que tem de mais moderno em js, então pode ser que mude um pouco.
Então para fazer uma manutenção em um site, sempre olhe o arquivo TS, pois é a base de tudo, o js pode ser criado com o ecmascript, e estar um pouco diferente do normal.

<br></br>

Ficar escrevendo sempre o "npm run compile" é chato e desgastante, então temos um jeito de fazer isso usando o watch do node. Na pasta package.json, em scripts:
```
"watch": "tsc -w"
```
Depois disso abra o terminal e coloque o "npm run watch". Dessa forma, a cada mudança, vai ser compilado automaticamente.

<br></br>

Porém queremos o retorno do nosso site e junto a isso queremos também que ele compile automaticamente, para isso vamos usar um atributo do arquivo package.json.
```
npm run start
```

<br></br>

o TypeScript aceita os atributos privados do JS, porém eles recomendam que você use os atributos do TypeScript. Primeiro vamos tirar o "#", após isso vamos usar o "_", se deixarmos assim, vai ser como regredir, ja que é possivel mudar a variavel _data, por exemplo. Então colocamos antes do _ o atributo "private".
```
    private _data;
    private _quantidade;
    private _valor;
```

<br></br>

Precisamos que o programa faça uma instancia de negociação após o usuário informar os dados (data, quantidade e valor), para isso vamos criar um controller, esse controller é uma classe que pega todos os dados informados, e com isso, cria-se um modelo. É basicamente a ponte entre as informações e o modelo.
Então primeiro temos que criar a classe com as variaveis em private. Após isso faremos um constructor informando os inputs. Em cada input do html, existe um id com o respectivo input, então usamos o document.querySelector do js para puxar o id.
```
export class NegociacaoController {
    private inputData;
    private inputQuantidade;
    private inputValor;

    constructor(){
        this.inputData = document.querySelector('#data');
        this.inputQuantidade = document.querySelector('#quantidade');
        this.inputValor = document.querySelector('#valor');
    }
    adiciona(){
        console.log(this.inputData);
        console.log(this.inputQuantidade);
        console.log(this.inputValor);
    }
}
```

<br></br>

Agora indo para o app.ts, la vamos apagar tudo o que tinhamos antes e começar a colocar atributos do negociacao-controller, primeiro vamos criar um objeto para o controller, após isso vamos pegar a classe form com o document.querySelector, com isso vamos puxar um evento que a classe form vai fazer, que nesse caso vai ser o submit. Ou seja, após o usuario clicar em submit, um evento será feito.
```
import { NegociacaoController } from "./controllers/negociacao-controller.js";

const controller = new NegociacaoController();
const form = document.querySelector('.form');
form.addEventListener('submit', event => {
    event.preventDefault();
    controller.adiciona();
})
```

<br></br>

Entramos em um problema, ja que o que é retornado no console.log é uma string e se olharmos no tipo de variavel que está atribuido no constructor do negociacao.ts, está como any, ou seja, está aceitando qualquer tipo de variavel.
Para que o TS não defina o tipo "any" para todas as variaveis criadas, precisamos alterar uma configuração do compilador. Então no arquivo "tsconfig.json" tem-se que adicionar um atributo para negar todas as variaveis com o tipo any.
```
"noImplicitAny": true
```
Então agora todas as variaveis com o tipo any vão dar erro, e precisamos alterar a propriedade, o tipo dela, para que dê certo.
```
constructor(data: Date, quantidade:number, valor:number)
```
Diferente de linguagens como C++ e Java (que definem o tipo de uma variavel antes de iniciar o código), aqui em TS pode definido dentro do código, e, também pode ser definido antes, com os private's.
```
    private _data: Date;
    private _quantidade: number;
    private _valor: number;
```
Temos também um tipo de variavel para definir que tal elemento é do html e é um input.
```
private inputData: HTMLInputElement
```
Porém ainda temos um erro para arrumar, o erro em questão é no value do inputData, o value por padrão vai retornar uma string, e o TS ja identifica e mostra que o inputData espera um tipo Date.

<br></br>

Primeiro para arrumar o erro temos que criar uma const com new Date passando ano, mês e dia, só que como vamos ler o ano mes e dia por meio de um value, ele irá vir com um " - ", só que queremos em virgulas. Para arrumar isso temos que fazer uma expressão regular procurando todos os " - ", e queremos que a expressão regular encontre todos os hifens.
```
    const exp = /-/g;
```
Traduzindo: a expressão regular vai buscar todos os hifens.
Agora Temos que passar para o new date o que queremos, então para isso vamos indicar o inputData.value, passando o replace e dentro do replace temos que passar a expressão regular e o que queremos que seja substituido pelo hifen.
```
    const date = new Date(this.inputData.value.replace(exp, ','));
```
Depois precisamos converter a quantidade para inteiro, para isso vamos utilizar o parseInt.
```
    const quantidade = parseInt(this.inputQuantidade.value);
```
Da mesma forma que a quantidade, vamos criar uma const para o valor, passando agora o parseFloat, para que converta para o float.
```
    const valor = parseFloat(this.inputValor.value)
```
Agora podemos mudar no construtor os parametros, passando somente o date, quantidade e valor.
```
    const negociacao = new Negociacao(date, quantidade, valor);
```

<br></br>

Para melhorar a legibilidade do código, nós vamos colocar o que para método retorna.
```
adiciona(): void{
    ...
}
```

<br></br>

Agora queremos que após o usuário digitar no formulário, o que estiver dentro do formulário seja apagado, porém seja mantido no console, para isso vamos criar um método com o nome de "limparFormulario" recebendo void, e para cada input vamos passar uma string vazia, e também vamos colocar um focus no inputData, para que ele volte para a data depois que o formulario for apagado. E tambem precisamos chamar o limparFormulario no método "adiciona" logo após o console.log, porque queremos que apareça no console e depois seja limpado o formulario.
```
    limparFormulario(): void{
        this.inputData.value = '';
        this.inputQuantidade.value = '';
        this.inputValor.value = '';
        this.inputData.focus();
    }
```

<br></br>

Agora temos que arrumar a lista de negociações, dentro da lista, podemos somente adicionar itens e não podemos excluir, então temos que criar um rapper envolta de um array de negociações que eu possa pedir para ele adicionar para mim e listar o que ele tem. Para isso temos que ir na pasta "models", em ts e criar um negociacoes.ts. Primeiro precisamos criar uma classe, já passando o export, e depois criar um private negociacoes passando um array vazio. Porém agora temos um erro, ja que o tipo desse private é any, e ordenamos para o ts que ele desse erro em tipos any, agora temos que informar que o tipo dessa variavel é array, porém temos que passar um parametro, então abrimos <> e colocamos Negociacao.
```
private negociacoes: Array<Negociacao> = [];
```
Para entender melhor o **Array<Negociacao>** temos um exemplo simples.
```
const list = [];
list.push('10');
list.push(11);
```
Nesse caso o const list ta recebendo qualquer tipo de variavel. Só que se passarmos um parametro nele.
```
const list: Array<string> = [];
list.push('10');
list.push(11);
```
Agora o const só vai aceitar o que for uma string, ele vai dar até um erro no ...push(11).

<br></br>

