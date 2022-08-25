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
Para entender melhor o Array < Negociacao >  temos um exemplo simples.
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

Não podemos acessar a variavel por meio de um ponto como fazemos normalmente, ja que ele é privado, então temos que criar um método para ele.
```
adiciona(negociacao: Negociacao){
        this.negociacoes.push(negociacao);
    }
```
Agora precisamos listar o que será retornado em negociações, pois eventualmente teremos que retornar essa lista. Para isso criamos um método com nome lista e passando o tipo array com o parametro de negociacao.
```
lista(): Array<Negociacao>{
        return this.negociacoes;
    }
```

<br></br>

Agora nos deparamos com um erro, a lista de negociacões não pode ser alterada, ela tem que continuar como o usuário informar. Só que na lista que estamos agora ela é facilmente alterável, pode-se enxergar isso quando colocamos um pop (vai remover o ultimo elemento). 
```
this.negociacoes.lista().pop();
```
Vai ser exibido nada no console.
Agora para mudar isso usamos uma propriedade do js que colocar um array dentro de outro.
```
return [...this.negociacoes];
```
Só que o TS tem uma propridade para tornar isso mais fácil. 
```
    lista(): ReadonlyArray<Negociacao>{
        return this.negociacoes;
    }
```
Isso aqui vai fazer com que o Array se torne somente para leitura. Agora se olharmos o negociacao-controller.ts, ele ta dando erro no pop, porque o pop é para alterar um elemento dentro do array, mas agora como o array é só para leitura, não pode ser alterado.

<br></br>

Agora para simplificar um pouco o código e as linhas, vamos alterar algumas coisas. Uma delas é no arquivo "negociacao.ts", nela vemos uma repetição de private e depois uma repetição de atribuição dentro do constructor, e como resolvemos isso? É bem simples, em TS o constructor pode funcionar para atribuir a variavel como private e já dar o tipo dela.
```
    constructor(
        private _data: Date, 
        private _quantidade:number, 
        private _valor:number
    ){}
```
Agora se formos no negociacao.js, vai ter direitinho a variavel criada e o construtor também, da forma que deveria ser.

<br></br>

Se olharmos o negociacoes.ts vemos o Array e dentro do "diamante" existe a definição do parametro, porém isso pode ser simplificado. 
```    
private negociacoes: Negociacao[] = [];
```
Então fomos de Array < Negociacao > para Negociacao[] que é a mesma coisa.
Para o ReadonlyArray é um pouco diferente, temos que colocar o readonly antes e depois o Negociacao[]
```
lista(): readonly Negociacao[]{}
```

<br></br>

Agora queremos colocar as variaveis do negociacao.ts em public, porem se colocarmos só em public ele vai facilmente alterável, para que isso não ocorra temos que definir o readonly, e se definimos o readonly não precisamos definir os getters.
```
        public readonly data: Date, 
        public readonly quantidade:number, 
        public readonly valor:number
```
Porém temos um problema nisso, o TS pode deixar eu fazer algumas alterações nesse caso, como por exemplo se eu colocar um setDate dentro da data. O readonly não permite a atribuição, que seria o (new Date()). Usamos a programação defensiva, basicamente vamos criar uma váriavel data que vai receber o data (para isso precisamos mudar o data para private de novo, tiramos o readonly e criamos um getter).
```
    get data(): Date{
        const data = new Date(this._data.getTime());
        return data;
    }
```
Agora esse getter é imutavel.

<br></br>

<h1>2º Parte do Curso</h1>

<br></br>

Primeiramente precisamos entender uma coisa, o nosso programa atual está retornando as informações informadas pelo usuário no console, porém para que o usuário veja essas informações, ele terá que abrir o console, o que não faz muito sentido, então queremos exibir todas as informações na mesma tela de negociações. E como podemos fazer isso? Usaremos um template que vai buscar as informações, e assim, automaticamente, as informações vão aparecer para o usuário. 
Para isso vamos usar um miniframework no react para criar isso (obs: não vamos programar tudo em react, isso é um curso de typescript).

<br></br>

Vamos criar um arquivo dentro do views que está dentro da pasta app, esse arquivo vai ter o nome de "negociacoes-view.ts" e lá dentro vamos criar uma classe que vai estar guardando um objeto com nome de template que vai receber uma string e dentro desse template vamos retornar uma tabela em html com o seu cabeçalho.
```
export class NegociacoesView {
    template(): string{
        return `
        <table class="table table-hover table-bordered">
            <thead>
                <tr>
                    <th>DATA</th>
                    <th>QUANTIDADE</th>
                    <th>VALOR</th>
                </tr>
            </thead>
        </table>
        ` 
    }
}
```
Usamos uma template string, para que mais pra frente o template crie um html com os dados que queremos, sem ter que ficar escrevendo.


## View

Criamos uma class na negociacoes view, porém precisamos que apareça na página e para isso precisamos fazer com que o ts identifique onde ele vai aparecer.

Então vamos lá no index.html, vamos criar uma div com o id de "negociacoesView", e vamos criar um constructor no negociacoes-view, passando como parametro um seletor:string. Depois em negociacoes-controller vamos criar um objeto privado passando o id que criamos em html.

```
    private negociacoesView = new NegociacoesView('#negociacoesView');
```
Com isso ele vai chamar o id que criamos no html.

Agora em negociacoes-view, temos que criar um private elemento dizendo que ele vai ser um elemento de html;
```
    private elemento: HTMLElement;
```

No constructor, indicamos que o elemento vai receber o seletor, então ele vai buscar no DOM o seletor que vai receber o elemento.

Agora vamos criar um metodo chamado update, dentro dela vamos passar o elemento porém agora com o innerHTML, passando o template();
```
    update():void{
        this.elemento.innerHTML = this.template();
    }
```
O update vai servir para renderizar o metodo template no elemento que criamos anteriormente.

## Dados passados para o html

Agora temos que passar os dados que o usuário informar para o html, para isso vamos em negociacoesController e vamos passar no metodo update o this.negociacoes que lá vai estar todos os dados, ele não vai compilar porque em negociacoes-view o update não recebe nada.
```
    this.negociacoesView.update(this.negociacoes);
```

No metodo update vamos passar um model que recebe o proprio Negociacoes e o template vai receber esse model.
```
    update(model:Negociacoes):void{
        this.elemento.innerHTML = this.template(model);
    }
```
No metodo template vamos passar o model também, e também vai receber o negociacoes.
```
    template(model:Negociacoes): string{
```

Agora dentro de tbody, vamos colocar os dados que o usuário informar, para isso vamos usar um recurso do js para importar um elemento, que nesse caso vai ser o model que vai passar uma lista que ja criamos em Negociacoes, e vai passar um map recebendo negociacao, e dentro disso vai retornar uma string grande passando um tr e três td's. E dentro desses td's vamos passar os vamos de negociacao.

```
                ${model.lista().map(negociacao =>{
                    return `
                    <tr>
                        <td></td>
                        <td>${negociacao.quantidade}</td>
                        <td>${negociacao.valor}</td>
                    </tr>
                    `
                })}
```
Só que agora tudo isso vai retornar um array e não queremos isso, o array tem uma propriedade que é o join, no join vamos passar uma string vazia.
```
.join('')
```

Depois de criar um constructor para o template, temos que passar o template para o adiciona.
```
    adiciona(): void{
        const negociacao = this.criaNegociacao();
        this.negociacoes.adiciona(negociacao);
        this.negociacoesView.update(this.negociacoes)
        this.limparFormulario();
    }
```

## Data

Agora precisamos resolver sobre a data que será exibida, o proprio navegador tem uma propriedade para informar isso.
```
Intl.DateTimeFormat()
```
E vamos passar isso no td, chamando em js.
```
    <td>${new Intl.DateTimeFormat()}</td>
```
Vamos passar também o formato dele, que vai ser o negociacao.data
```
    <td>${new Intl.DateTimeFormat().format(negociacao.data)}</td>
```

## Mensagem view

Agora temos que pensar em criar uma mensagem de "adicionado com sucesso" após o usuário informar os dados, o processo é mais ou menos parecido com o negociacao-view, mudando apenas o return que o template vai dar. No "negociacao-controller" é a mesma coisa também, vamos criar um private passando um id (que tem que ser criado no html), e depois passando no método adiciona uma string avisando que foi concluída com sucesso.

Porém agora temos um problema aqui, ja que tem muito código sendo repetido entre o mensagem-view e o negociacao-view. Agora temos que usar o conceito de herança para os código repetitivos.

## Herança

Temos que criar um novo arquivo, nesse caso vai ter um nome de "view" (obviamente que é dentro da pasta views), e vamos copiar o private e o constructor para esse arquivo.
```
export class View{
    private elemento: HTMLElement;

    constructor(seletor:string){
        this.elemento = document.querySelector(seletor)
    }
}
```
Nesse caso só definimos, agora temos que mostrar para o mensagem-view e negociacao-view que ele tem que estender até o view.
```
export class MensagemView extends View
```
Agora temos um problema de compilação, a class não ta conseguindo acessar o "elemento". Se olharmos bem, o "elemento" está definido como private, e ele não consegue acessar se não for do mesmo arquivo, então precisamos mudar para "protected".
```
protected elemento: HTMLElement;
```
Para entender isso, vamos pensar que o view.ts é o pai de mensagem-view e negociacao-view, e ele precisa mandar uma herança para seus filhos, porém os filhos não podem acessar (pegar) algo que é privado, porém se o pai proteger a herança, seus filhos vão poder acessar, porém quem está de fora não consegue acessar.

Ai você se pergunta:"Mas não era mais fácil colocar só um public". A resposta é que não, se colocarmos como public, todo mundo vai poder acessar, então você vai poder acessar os dados de qualquer arquivo, e não é isso que queremos, queremos que somente os filhos acessem o "elemento".

Temos mais coisas repetitivas (o update), então vamos fazer com que a mensagem-view e o negociacoes-view herdem o update.

Para isso vamos passar tudo de update para dentro de view, só que também temos que passar o template, porque senão o update não roda, só que se deixarmos passando somente o template, vai dar um erro no negociacoes-view, porque ele não vai receber os mesmo parametros, então adicionamos um throw Error para que se o método template não for executado apareça um erro na tela.
```
    template(model:string):string{
        throw Error('Classe filha precisa implementar o método template');
    }
```
Caso a filha sobreescreva o comportamento do método, ele não vai dar a mensagem de erro.

Analisando o negociacoes-view, temos um erro de compilação, ja que o model está recebendo uma string em view, porém em negociacoes-view, o model vai receber o Negociacoes, e temos que arrumar isso, porque o mesmo tipo de model que está em view tem que estar em negociacoes e em mensagem.

## Generics

Vimos o generics anteriormente, mas para ressaltar, ele cria um tipo vazio, que quando for chamado, tenha que ser informado o tipo que será colocado.

Para que isso aconteça, temos que substituir todos os tipos que informamos no update e no template, pelo o que informamos dentro do "diamante" <>.

```
View<T>
```
É assim que criamos um generic.

```
    update(model:T):void{
        const template = this.template(model);
        this.elemento.innerHTML = template;
    }

    template(model:T):string{
        throw Error('Classe filha precisa implementar o método template');
    }
```
E assim que implementamos ela.

Agora quando a class View for estendida, terá que ir junto com ela o tipo que será utilizado.
```
export class MensagemView extends View<string>
```

## Classes abstratas

Uma classe abstrata não pode ser instanciada diretamente, você só pode instanciar caso o filho herda a classe e você faz uma instancia do filho.

```
export abstract class View<T>{

}
```

### Método Abstrato

Com a classe abstrata, não precisamos fazer o throw error em template para sobreescrever, apenas precisamos dizer ao template que ele é abstrato.

```
    abstract template(model:T):string;
```

Agora caso o template não esteja definido, ele vai dar erro em tempo de desenvolvimento, e não em run time. Com isso gerando menos erros.

## Template visivel

Em negociacao controller precisamos ver somente o método update, porém se olharmos bem estamos vendo também o método template, e isso precisa ser limitado.

Os métodos, por padrão, adotam o public, porém queremos que ele seja escondido no controller, então colocamos protected, como vimos anteriormente. O protected vai permitir que as classes filhas acessem o método da classe pai, porém se não forem do mesmo "parentesco" ele não vai aparecer.

```
    protected abstract template(model:T):string;
```

Porém mesmo assim o método template aparece em controller. Como os métodos por padrão ja vem como public, temos que informar em mensagem view e em negociacoes view que é protected, se não o método template se torna public novamente.

```
export class MensagemView extends View<string>{

    protected template(model:string):string{
        return`
            <p class="alert alert-info"> ${model}</p>
        `
    }

}
```

## Data de negociacoes-view

Se analisarmos bem, o código em de data está bem grande e não ta definido com clareza o que ele vai fazer, para resolver isso vamos criar um método *privado* para colocar essa data.

```
    private formatar(data:Date){
        return new Intl.DateTimeFormat().format(data);
    }
```

