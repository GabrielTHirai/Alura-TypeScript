<h1>ANOTAÇÕES</h1>

``` 
npm install
```
Vamos baixar os módulos do node.


A pasta dist vai ser compartilhada com o navegador, exclusivamente ela, pois quando olhamos para o package.json, vemos que o server está alocado na pasta dist.


Vamos utilizar um arquivo js como módulo no index.html.
```
<script src="../dist/js/app.js" type="module"></script>
```

Em um arquivo js vamos começar a programar (negociacao.js), dentro da classe negociacao, vamos ter que criar as variaveis, que nesse caso vão ser privadas, então utilizaremos o #.
```
#data;
```
Nesse caso só vamos conseguir alterar o valor com o constructor, e nada mais.


```
const negociacao = new Negociacao(new Date(), 10, 100);
```
Nesse caso estamos passando os valores para o construtor, então estamos passando a data de hoje, a quantidade(10) e o valor(100).


Caso algum erro em código aconteça, o arquivo JS não vai me retornar o erro em código, e sim em run time, ou seja, quando rodar o site, e isso pode ser modificado utilizando o TS.
Para rodar o TS temos que fazer a instalação dele.
```
npm install typescript@version --save-dev  
```
No caso vamos mudar a parte de "version" e colocar a versão do ts que utilizaremos.


O navegador não vai entender a linguagem ts, então temos que mover os arquivos que antes eram js (agora são ts) para a pasta app, pois tudo o que for em ts vai ser convertido em js. Tudo em ts vai ser compilado para js.


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


Para compilar, vamos utilizar um comando do node.
```
npm run compile
```
obs: caso abra o arquivo js e esteja meio diferente do que você escreveu, não se preocupe, quando compilamos o arquivo ts com o ecmascript, ele vai pegar o que tem de mais moderno em js, então pode ser que mude um pouco.
Então para fazer uma manutenção em um site, sempre olhe o arquivo TS, pois é a base de tudo, o js pode ser criado com o ecmascript, e estar um pouco diferente do normal.


Ficar escrevendo sempre o "npm run compile" é chato e desgastante, então temos um jeito de fazer isso usando o watch do node. Na pasta package.json, em scripts:
```
"watch": "tsc -w"
```
Depois disso abra o terminal e coloque o "npm run watch". Dessa forma, a cada mudança, vai ser compilado automaticamente.


Porém queremos o retorno do nosso site e junto a isso queremos também que ele compile automaticamente, para isso vamos usar um atributo do arquivo package.json.
```
npm run start
```