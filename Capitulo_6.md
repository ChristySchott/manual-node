<h1 align="center"> PROGRAMAÇÃO ASSÍNCRONA </h1>

## Callbacks

### O JavaScript é síncrono e single threaded por padrão. Isso significa que esse código não pode criar novas threads e executar em paralelo. Descubra o que código assíncrono significa e como ele se parece

<h1 align="center"> <img src="https://github.com/ChristySchott/manual-node/blob/master/images/callback.PNG" alt="callbacks" /> </h1>

- Assincronicidade em linguagens de programação
- JavaScript
- Callbacks
- Tratando erros nas callbacks
- O problema com as callbacks
- Alternativa as callbacks

### Assincronicidade em linguagens de programação

Computadores são assíncronos por design.

Assíncrono significa que as coisas podem acontecer independentemente do fluxo do programa principal.

Nos computadores atuais, todos os programas são executados por um intervalo de tempo específico e, em seguida,
interrompem sua execução para permitir que outro programa continue sua execução. Essa coisa roda em um ciclo tão
rápido que é impossível perceber, e achamos que nossos computadores executam muitos programas
simultaneamente, mas isso é uma ilusão (exceto em máquinas com multiprocessadores).

Os programas usam, internamente, as interrupções, que são sinais emitidos ao processador para chamar a atenção
do sistema.

Eu não vou entrar fundo nisso, mas tenha em mente que é normal que os programas sejam
assíncronos e interrompam sua execução até que o compoutador possa lher dar atenção. Enquanto isso, ele executa outras coisas. Quando um programa está aguardando uma resposta da
rede, ele não pode parar o processador até que a solicitação seja concluída.

Normalmente, as linguagens de programação são síncronas e algumas fornecem uma maneira de gerenciar
assincronicidade, seja na linguagem ou através de bibliotecas. C, Java, C #, PHP, Go, Ruby, Swift,
Python, todos eles são síncronos por padrão. Alguns deles lidam com assincronia usando threads,
gerando um novo processo.

### JavaScript


O JavaScript é síncrono por padrão e também single threaded. Isso significa que o código não pode
criar novas threads e executá-las em paralelo.

Linhas de código são executadas em série, uma após a outra, por exemplo:

```
const a = 1
const b = 2
const c = a * b
console.log (c)
doSomething()
```
Mas o JavaScript nasceu dentro do navegador, seu trabalho principal, no início, era responder a
ações do usuário, como `onClick`, `onMouseOver`, `onChange`, `onSubmit` e assim por diante. Como isso pôde ser feito com um modelo de programação síncrona?

A resposta estava em seu ambiente. O navegador fornece uma maneira de fazê-lo, fornecendo um conjunto de
APIs que podem lidar com esse tipo de funcionalidade.

Mais recentemente, o Node.js introduziu um ambiente de E/S sem bloqueio, estendendo esse conceito ao acesso de arquivos, chamadas de rede e assim por diante.

### Callbacks

Você não pode saber quando um usuário clicará em um botão; portanto, o que você faz é definir um manipulador de eventos para o evento do click. Este manipulador de eventos aceita uma função, que será chamada quando
o evento é acionado:

```
document.getElementById('button').addEventListener('click', () => {
  //item clicado
})
```

Este é a chamada callback.

Uma callbackx é uma função simples que é passada como um valor para outra função, e só será
executada quando o evento acontece. Podemos fazer isso porque o JavaScript tem funções de primeira classe, que podem ser atribuídas a variáveis e passadas para outras funções (chamadas
funções de ordem superior).

É comum agrupar todo o código do cliente em um `load` no objeto `window`, que
executa a callback somente quando a página está pronta:

```
window.addEventListener('load', () => {
 // janela carregada
 // faça o que você quiser
})
```

Callbacks são usadas em todos os lugares, não apenas em eventos da DOM.

Um exemplo comum é usando temporizadores:
```
setTimeout(() => {
// executa após dois segundos
}, 2000)
```

### Tratando erros nas Callbacks

Como você lida com erros em callbacks? Uma estratégia muito comum é usar o que o Node.js tem
adotado: o primeiro parâmetro em qualquer callbaxck é o objeto de erro: *error-first callbacks*.

Se não houver erro, o objeto é nulo. Se houver um erro, ele contém alguma descrição do
erro e outras informações.

```
fs.readFile('/file.json', (err, data) => {
  if (err !== null) {
   //tratando o erro
  console.log(err)
  return
  }
  //sem erros, processe os dados
  console.log(data)
})

```

### O problema com as Callbacks

As callbacks são ótimas para casos simples!

No entanto, toda callback adiciona um nível de aninhamento e, quando você tem muitos retornos, o código
começa a ficar complicado muito rapidamente, problema conhecido como *callback hell*.


```
window.addEventListener('load', () => {
  document.getElementById('button').addEventListener('click', () => {
    setTimeout(() => {
      items.forEach(item => {
       //seu código aqui
      })
    }, 2000)
  })
}
```

Este é apenas um código simples de quatro níveis, mas já vi muitos níveis de aninhamento e não é nada divertido.

Como solucionamos isso?

- Promises (ES6)
- Async/Await (ES8)

## Promises

### Promises são uma maneira de lidar com código assíncrono em JavaScript, sem precisar escrever muitas callbacks no seu código.

- Introdução às promises
  - Como as promises funcionam, em resumo
  - Qual API do JavaScript usa promises?
- Criando uma promise
- Consumindo uma promise
- Tratando erros
  - Cascading errors (erros em cascata)
- Orquestando promises
  - Promise.all()
  - Promise.race()
- Erros comuns
  - Uncaught TypeError: undefined is not a promise

### Introdução às promises

Uma promise é geralmente definida como um proxy para um valor que acabará se tornando
acessível.
 
Promises são uma maneira de lidar com código assíncrono, sem escrever muitos callbacks em
seu código.

Embora existam há anos, elas foram padronizadas e introduzidas no ES2015,
e agora já foram substituídos no ES2017 pelas funções assíncronas.

As funções assíncronas usam a API das promises como seu componente básico, portanto, entendê-las é
fundamental, mesmo que no código mais recente você provavelmente usará funções assíncronas em vez de promises.

### Como as promises funcionam, em resumo

Uma vez que uma promise tenha sido chamada, ela começará no estado pendente. Isso significa que a função chamada continua a execução, enquanto aguarda a promise fazer seu próprio processamento e ar algum retorno à função de chamada.

Nesse ponto, a função de chamada espera que ela retorne a promise em um estado resolvido ou
em um estado rejeitado, mas como você sabe que o JavaScript é assíncrono, a função continua sua
execução enquanto a promise trabalha.

### Qual API do JavaScript usa promises?

Além do seu próprio código, e códigos de bibliotecas, as promises são usadas por APIs Web modernas:

- Battery API
- Fetch API
- Service Workers

É improvável que, no JavaScript moderno, você não esteja usando promises, então vamos começar
mergulhando direto nelas.

## Criando uma promise

Na última seção, apresentamos como uma promise é criada.

Agora vamos ver como uma promise pode ser consumida ou usada

```
  const isItDoneYet = new Promise(
    //...
  )

  const checkIfItsDone = () => {
    isItDoneYet
      .then((ok) => {
         console.log(ok)
      })
      .catch((err) => {
         console.error(err)
  }
```

A execução de `checkIfItsDone ()` executará a promessa `isItDoneYet ()` e esperará que ela
esteja resolvida usando a callback `then` e, se houver um erro, ele tratará disso na callback `err`.

## Tratando erros

No exemplo, na seção anterior, tivemos um `catch` que foi anexado à cadeia de
promises.

Quando algo na cadeia de promises falha e gera um erro, ou rejeita a promise, o controle vai para a instrução `catch()` mais próxima.

```
new Promise((resolve, reject) => {
  throw new Error('Error')
}) 
  .catch((err) => { console.error(err) })

// or

new Promise((resolve, reject) => {
  reject('Error')
})
  .catch((err) => { console.error(err) })
```

### Cascading errors (erros em cascata)

Se dentro de um `catch()` você disparar um erro, você pode adicionar um segundo `catch()` para tratá-lo, e assim por diante.

```
new Promise((resolve, reject) => {
  throw new Error('Error')
})
  .catch((err) => { throw new Error('Error') })
  .catch((err) => { console.error(err) })
```

## Orquestrando promises

### Promise.all() 

Se você precisar sincronizar diferentes promises, `Promise.all()`ajuda você a definir uma lista de promises e executar algo quando todas elas estão resolvidas.

Exemplo: 

```
const f1 = fetch('/something.json')
const f2 = fetch('/something2.json')

Promise.all([f1, f2]).then((res) => {
  console.log('Array of results', res)
})
.catch((err) => {
  console.error(err)
})
```

A desestruturação do ES2015 permite que você faça também 

```
Promise.all([f1, f2]).then(([res1, res2]) => {
  console.log('Results', res1, res2)
})
```

### Promise.race()

`Promise.race()` é executado quando a primeira das promises passadas a ele é resolvida, e executa a callback anexada apenas uma vez, com o resultado daquela primeira promise resolvida.

Exemplo:

```
const first = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, 'first')
})

const second = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'second')
})

Promise.race([first, second]).then((result) => {
  console.log(result) // second
})
```

## Erros comuns

### Uncaught TypeError: undefined is not a promise

Se você receber `Uncaught TypeError: undefined is not a promise`no seu console, certifique-se de usar `new Promise()` ao invés de `Promise()`.


## async/await

### Descubra a abordagem moderna para funções assíncronas em JavaScript. O JavaScript evoluiu em um tempo muito curto, de callbacks a Promises, e desde a ES2017, a programação assíncrona em JavaScript se tornou ainda mais simples com o async/await.

- Introdução
- Por que async/await foi introduzido?
- Como funciona
- Um rápido exemplo
- Promise para todas as coisas
- O código é muito mais legível
- Funções assíncronas em série

## Introdução 

As funções assíncronas são uma combinação de promises e generators e são basicamente uma abstração de nível superior sobre as promises. Deixe-me repetir: async/await é construído sobre promises.

## Por que async/await foi introduzido?

Eles reduzem o clichê em torno das promises e a limitação "não quebre a corrente" no encadeamento de promises.

Quando as Promises foram introduzidas no ES2015, elas tinham como objetivo resolver um problema com
código assíncrono, e cosneguiram, mas nos 2 anos que separaram o ES2015 e o ES2017 ficou claro que as promises não poderiam ser a solução final.

Promises foram introduzidas para resolver o famoso problema do callback hell, mas introduziram
uma complexidade por conta própria, além de uma complexidade na sintaxe.

Eles eram boas primitivas para oferecer uma sintaxe mais clara aos desenvolvedore,
então, quando chegou a hora, obtivemos as funções assíncronas.

Elas fazem o código parecer síncrono, mas atrás dos panos é assíncrono e sem bloqueios.

## Como funciona

Uma função assíncrona retorna uma promise, como neste exemplo:

```
const doSomethingAsync = () => {
  return new Promise((resolve) => {
     setTimeout(() => resolve('I did something'), 3000)
  })
}
```


Quando você desejar chamar esta função, acrescente o `await` e o código de chamada será interrompido até
que a promise seja resolvida ou rejeitada.  Uma ressalva: a função deve ser definida com o `async`. Aqui está um exemplo:

```
const doSomething = async () => {
  console.log(await doSomethingAsync())
}
```

## Um rápido exemplo

Este é um exemplo simples do async/await sendo usado para executar uma função de forma assíncrona:

```
const doSomethingAsync = () => {
  return new Promise((resolve) => {
    setTimeout(() => resolve('I did something'), 3000)
  })
}

const doSomething = async () => {
  console.log(await doSomethingAsync())
}

console.log('Before')
doSomething()
console.log('After')
```

O código acima retornará o seguinte:

```
Before
After
I did something //after 3s
```

## Promise para todas as coisas

Anexar a palavra-chave `async` a qualquer função significa que a função retornará uma promise.

Mesmo que não o faça explicitamente, fará com que ele retorne uma promise de forma interna.

É por isso que este código é válido:

```
const aFunction = async () => {
  return 'test'
}

aFunction().then(alert) // isso irá alertar o 'test'
```

e é o mesmo que:

```
const aFunction = async () => {
  return Promise.resolve('test')
}

aFunction().then(alert) // isso irá alertar o 'test'
```

## O código é mais legível

Como você pode ver no exemplo acima, nosso código parece muito simples. Compare-o ao código usando
apenas promises, com encadeamento e callbacks.

E este é um exemplo muito simples, os principais benefícios surgirão quando o código for muito mais
complexo.

Por exemplo, veja como você obteria e analisaria um recurso JSON usando promises:

```
const getFirstUserData = () => {
  return fetch('/users.json') // pega a lista de usuários
   .then(response => response.json()) // parse JSON
   .then(users => users[0]) // pega o primeiro usuários
   .then(user => fetch(`/users/${user.name}`)) // pega dados do usuário
   .then(userResponse => response.json()) // parse JSON
}

getFirstUserData()
```

E aqui está a mesma funcionalidade fornecida usando `async/await`:

```
const getFirstUserData = async () => {
  const response = await fetch('/users.json') //pega a lista de usuários
  const users = await response.json() // parse JSON
  const user = users[0] // pega o primeiro usuário
  const userResponse = await fetch(`/users/${user.name}`) // pega dados do usuário
  const userData = await user.json() // parse JSON
  return userData
}

getFirstUserData()

```

## Funções assíncronas em série

As funções assíncronas podem ser encadeadas com muita facilidade, e a sintaxe é muito mais legível do que com
promises simples:

```
const promiseToDoSomething = () => {
  return new Promise(resolve => {
  setTimeout(() => resolve('Eu fiz algo'), 10000)
})
}

const watchOverSomeoneDoingSomething = async () => {
  const something = await promiseToDoSomething()
  return something + 'e eu assisti'
}

const watchOverSomeoneWatchingSomeoneDoingSomething = async () => {
  const something = await watchOverSomeoneDoingSomething()
  return something + ' e eu assisti também'
}

watchOverSomeoneWatchingSomeoneDoingSomething().then((res) => {
  console.log(res)
})
```

irá retornar: 

```
Eu fiz algo e eu assisti e eu assisti também

```



