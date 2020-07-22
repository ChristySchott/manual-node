<h1 align="center"> LINHA DE COMANDO </h1>

## Usando o Node REPL

### REPL significa Read-Evaluate-Print-Loop, e é uma ótima maneira de explorar os recursos do Node de maneira rápida

O comando `node` é único que usamos para rodar nossos scripts:
```
node script.js
```

Se nós omitirmos o nome do arquivos, usamos isso no modo REPL:
```
node
```

Se você tentar isso no seu terminal, é isso que irá acontecer:
```
❯ node
>
```
o comando permanece no modo ocioso e aguarda a entrada de algo.

Para ser mais preciso, REPL está esperando que digitemos algum código JavaScript.

Comece simples e digite
```
> console.log('test')
test
undefined
>
```

O primeiro valor, `test`, é a saída que solicitamos para console imprimir e, em seguida, fica `undefined` o valor de retorno do `console.log()`.

Agora podemos inserir uma nova linha de JavaScript.

### Use o tab para auto-completar

O legal do REPL é que ele é interativo.

Ao escrever seu código, se você pressionar a tecla Tab, o REPL tentará completar automaticamente o que você
escreveu, correspondendo a uma variável que você já definiu ou a alguma predefinida.

### Explorando objetos JavaScript

Tente digitar o nome de uma classe JavaScript, como Number, adicione um ponto e pressione tab.

O REPL imprimirá todas as propriedades e métodos que você pode acessar nessa classe:

<h1 align="center"> <img src="https://github.com/ChristySchott/manual-node/blob/master/images/object.PNG?raw=true" alt=" JavaScript object" /></h1>

### Explorando objetos globais

Você pode inspecionar os globais aos quais tem acesso digitando `global.` e pressionando `tab`:

<h1 align="center"> <img src="https://github.com/ChristySchott/manual-node/blob/master/images/globals.PNG" alt=" JavaScript object" /></h1>

### A variavél especial `_`

Se após algum código você digitar `_`, isso imprimirá o resultado da última operação.

### Comandos com ponto

O REPL possui alguns comandos especiais, todos começando com um ponto `.`. Eles são

- `.help`: mostra o comando help

- `.editor`: da mais liberdade ao editor, permitindo ao usuários escrever código JavaScript multilinha com mais facilidade. Quando você estiver
nesse modo, digite ctrl-D para executar o código que você escreveu.

- `.break`:: ao inserir uma expressão de várias linhas, inserir o comando `.break` abortará
entrada adicional. O mesmo que pressionar ctrl-C.

- `.clear`: redefine o contexto REPL para um objeto vazio e limpa qualquer expressão de várias linhas.

- `.load`: carrega um arquivo JavaScript relativo ao diretório em que você está trabalhando.
- `.save`: salva tudo que você enviou no REPL a um arquivo (especifique o nome do arquivo).

- `.exit`: o mesmo que pressionar Ctrl-C duas vezes

## Passando argumentos da linha de comando

### Como aceitar argumentos transmitidos a partir linha de comando em um programa Node.js

Você pode transmitir quantos argumentos desejar ao invocar uma aplicação Node.js. usando

```
node app.js
```

Os argumentos podem ser autônomos ou ter uma chave e um valor.

Por exemplo:

```
node app.js flavio

```

ou 


```
node app.js name=flavio
```

Isso muda como você recuperará esse valor no seu código.

A maneira como você o recupera é usando o objeto `process` incorporado ao Node.

Ela expõe uma propriedade `argv`, que é uma matriz que contém toda os argumentos invocados na linha de comando.

O primeiro argumento é o caminho completo do comando `node`.

O segundo elemento é o caminho completo do arquivo que está sendo executado.

Todos os argumentos adicionais estão presentes a partir da terceira posição adiante.

Você pode iterar sobre todos os argumentos (incluindo o caminho do node e o caminho do arquivo) usando um loop.

```
process.argv.forEach((val, index) => {
console.log(`${index}: ${val}`)
})
```

Você pode obter apenas os argumentos adicionais criando uma nova matriz que exclua os 2 primeiros
parâmetros:

```
const args = process.argv.slice(2)
```

Se você tiver um argumento sem um nome de índice, assim: 

```
node app.js flavio
```

você pode acessar usando 

```
const args = process.argv.slice(2)
args[0]

```

Nesse caso: 
```
node app.js name=flavio
```

`args[0]` é `name=flavio` e você precisa dar um parse nele. A melhor maneira de fazer isso é usando o
biblioteca minimista, que ajuda a lidar com argumentos:

```
const args = require('minimist')(process.argv.slice(2))
args['name'] //flavio

```

# Saídas para a linha de comando

### Como escrever no console da linha de comando usando o Node, desde o básico, com console.log, até cenários mais complexos

- Saída básica usando o console module
- Limpando o console
- Contando elementos
- Calculando o tempo gasto
- stdout e stderr
- Criando uma barra de progresso

## Saída básica usando o console module

O Node fornece um módulo `console`, que oferece maneiras muito úteis de interagir com a linha de comando.

É basicamente o mesmo objeto `console` que você encontra no navegador.

O método mais básico e mais usado é o `console.log()`, que imprime no console a string que lhe é passada.

Se você passar um objeto, ele será renderizado como uma string.
Você pode passar inúmeras variáveis para console.log, por exemplo:

```
const x = 'x'
const y = 'y'
console.log(x, y)
```

e o Node irá printar os dois.

Também podemos formatar frases decorada passando um especificador de formato junto com as variáveis.

Por exemplo:

```
console.log('Meu %s tem %d anos', 'gato', 2)
```

- `%s` formata uma variável como uma string
- `%d` ou %i formata uma variável como um inteiro
- `%f` formata a variável como float
- `%O` usado para printar a representação de um objeto

Example:
```
console.log('%O', Number)
```

## Limpando o console

`console.clear()` limpa o console (o comportamento pode depender do console usado).

## Contando elementos

`console.count()` é um método útil.

Veja esse código: 

```
const x = 1
const y = 2
const z = 3
console.count(
'O valor de x é ' + x + ' e foi verificado.. quantas vezes?'
)
console.count(
'O valor de z é ' + z + ' e foi verificado .. quantas vezes?'
)
console.count(
O valor de y  é ' + y + ' e foi verificado .. quantas vezess?'
)

```

O que acontece é que o `count` contará o número de vezes que uma string é impressa e imprimirá a conta ao lado.

Você pode apenas contar maçãs e laranjas:

```
const laranjas = ['laranja', 'laranja']
const maçãs = ['apenas uma maçã']
maças.forEach(fruta => {
console.count(fruta)
})
maçãs.forEach(fruta => {
console.count(fruta)
})

```

## Calculando o tempo gasto

Você pode calcular facilmente quanto tempo uma função leva para executar, usando time () e timeEnd ()

```
const doSomething = () => console.log('test')
const measureDoingSomething = () => {
console.time('doSomething()')
//faça algo, e calcule o tempo que isso leva
doSomething()
console.timeEnd('doSomething()')
}
measureDoingSomething()

```

## stdout e stderr

Como vimos, o console.log é ótimo para imprimir mensagens no console. É isso que chamamos de saída padrão ou `stdout`.

`console.error` imprime no stderr.

Ele não aparecerá no console, mas aparecerá no log de erros.

## Criando uma barra de progresso

Progress é um pacote incrível para criar uma barra de progresso no console. Instale-o usando o `npm install progress`.

Esse snippet cria uma barra de progresso de 10 etapas e, a cada 100ms, uma etapa é concluída. Quando
a barra é concluída, limpamos o intervalo:

```
const ProgressBar = require('progress')

const bar = new ProgressBar(':bar', { total: 10 })
const timer = setInterval(() => {
  bar.tick()
  if (bar.complete) {
    clearInterval(timer)
  }
}, 100)
```

## Aceitando entradas da linha de comando

Como tornar um programa CLI do Node.js. interativo?

O Node, desde a versão 7, fornece o módulo `readline` para executar exatamente isso: obter a entrada de uma stream legível, como a stream `process.stdin`, que durante a execução de uma aplicação Node é a entrada do terminal, uma linha de cada vez.

```
const readline = require('readline').createInterface({
  input: process.stdin,
  output: process.stdout
})

readline.question(`Qual o seu nome?`, (name) => {
  console.log(`Olá ${name}!`)
  readline.close()
})

```

Esse trecho de código solicita o nome de usuário e, uma vez que o texto é inserido e o usuário pressiona
enter, enviamos uma saudação.

O método `question()` mostra o primeiro parâmetro (uma pergunta) e aguarda a entrada do usuário. Isso
chama a função de callback depois que o enter é pressionado.

Nesta callback, fechamos a interface `readline`.

O `readline` oferece vários outros métodos, e eu vou deixar você conferir no pacote
documentação que eu vinculei acima.

Se você precisar solicitar uma senha, é melhor repeti-la mostrando um `*`.

A maneira mais simples é usar o pacote `readline-sync`, que é muito semelhante em termos de
API e lida com isso imediatamente.

Uma solução mais completa e abstrata é fornecida pelo pacote `Inquirer.js`.

Você pode instalá-lo usando o `npm install inquirer` e, em seguida, pode replicar o código acima, como
isto:

```
const inquirer = require('inquirer')

var questions = [{
  type: 'input',
  name: 'name',
  message: "Qual o seu nome?",
}]

inquirer.prompt(questions).then(answers => {
  console.log(`Oi ${answers['name']}!`)
})

```

O `Inquirer.js` permite fazer várias coisas, como mostrar várias opções, ter radio buttons,
confirmações e muito mais.

Vale a pena conhecer todas as alternativas, especialmente as internas fornecidas pelo Node, mas se você
planeja levar a entrada da CLI para o próximo nível, o `Inquirer.js` é a melhor opção.
