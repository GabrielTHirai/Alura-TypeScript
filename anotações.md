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