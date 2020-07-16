<h1 align="center"> Callbacks </h1>

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

- Introdução as promises
  - Como as promises funcionam, em resumo
  - Qual API do JavaScript usa promises?
- Criando uma promise
- Consumindo uma promise
- Encadeando promises
  - Exemplo de uma cadeia de promises
- Tratando erros
  - Cascading errors (erros em cascata)
- Orquestando promises
  - Promise.all()
  - Promise.race()
- Erros comuns
  - Uncaught TypeError: undefined is not a promise



