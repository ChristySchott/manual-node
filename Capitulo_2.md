<h1 align="center"> Rodando scripts do Node da linha de comando </h1>

## Como rodar qualquer comando do Node na CLI

A maneira usual de executar um programa Node é chamar comando global `node` (que fica disponível quando
você instala o Node) e passar o nome do arquivo que deseja executar.

Se o arquivo principal do aplicativo Node estiver em `app.js`, você poderá chamá-lo digitando

```
node app.js
```

## Como sair de um programa em Node

### Aprenda a finalizar um aplicativo Node.js. da melhor maneira possível

Existem várias maneiras de encerrar um aplicativo Node.js.
Ao executar um programa no console, você pode fechá-lo com `ctrl-C`, mas o que eu quero
discutir aqui é sair 'programaticamente'.

Vamos começar com a maneira mais drástica e ver por que é melhor não usá-la.

O módulo principal do `process` é um método útil que permite sair 'programaticamente' de um programa Node.js: `process.exit()`.
Quando o Node.js executa esta linha, o processo é imediatamente forçado a terminar.

Isso significa que qualquer retorno de chamada pendente, qualquer solicitação de rede ainda sendo enviada, qualquer
acesso ao sistema de arquivos ou processos de gravação em `stdout` ou `stderr` - tudo será descartado imediatamente.

Se isso for útil para você, você pode passar um número inteiro que sinalizará ao sistema operacional o código de saída:

```
process.exit(1)
```

Por padrão, o código de saída é 0, o que significa sucesso. Diferentes códigos de saída têm diferentes
significados que você pode querer usar em seu próprio sistema para que o seu programa se comunique
com outros programas.

Você pode ler mais sobre códigos de saída em https://nodejs.org/api/process.html#process_exit_codes

Você também pode definir a propriedade `process.exitCode`:

```
process.exitCode = 1
```

e quando o programa terminar, o Node retornará esse código de saída.

Um programa será encerrado normalmente quando todo o processamento estiver concluído.

Muitas vezes iniciamos servidores com o Node , como este servidor HTTP:

```

const express = require('express')
const app = express()
 
app.get('/', (req, res) => {
  res.send('Hi!')
})

app.listen(3000, () => console.log('Server ready'))

```

Este programa nunca vai acabar. Se você chamar `process.exit()`, qualquer solicitação pendente ou em execução será abortada. Isso não é o ideal.

Nesse caso, você precisa enviar ao comando um sinal SIGTERM e lidar com isso usando
manipulador de sinal do processo:

> Nota: `process` não precisa de um "require", ele está disponível automaticamente.

```

const express = require('express')
const app = express()

app.get('/', (req, res) => {
  res.send('Hi!')
})

const server = app.listen(3000, () => console.log('Server ready'))

process.on('SIGTERM', () => {
  server.close(() => {
    console.log('Process terminated')
  })
})

```

`SIGKILL` são os sinais que informam um processo para terminar imediatamente e, idealmente, agiriam como
`process.exit()`.

`SIGTERM` são os sinais que informam a um processo para terminar normalmente. É o sinal que é enviado
de gerentes de processo, como iniciantes ou supervisores.

Você pode enviar este sinal de dentro do programa, em outra função:

```
process.kill(process.pid, 'SIGTERM')

```

## Como ler variáveis de ambiente

### Aprenda a ler e usar variáveis de ambiente em uma aplicação Node.js

O módulo principal `process` do Node fornece a propriedade `env`, que hospeda todos as
variáveis de ambiente definidas no momento em que o processo foi iniciado.

Aqui está um exemplo que acessa a variável de ambiente NODE_ENV, definida como
`development` por padrão.

```
process.env.NODE_ENV // "development"

```

Configurá-lo para `production` antes da execução do script informará ao Node que esta é uma produção
no seu ambiente.

Da mesma maneira, você pode acessar qualquer variável de ambiente personalizada que definir.

## Opções de hospedagem Node

### Um aplicativo Node.js pode ser hospedado em muitos lugares, dependendo da sua
necessidade. Esta é uma lista de algumas opções que você tem à sua disposição

Aqui está uma lista das opções que você pode explorar quando deseja implantar seu aplicativo
e torná-lo acessível ao público.

Vou listar opções tanto simples e restritas quanto complexas e poderosas.

- A opção mais simples: local tunnel
- Deploys sem configuração 
  - Glitch
  - Codepen
- Serverless
- PAAS 
  - Zeit Now
  - Nanobox
  - Heroku
  - Microsoft Azure
  - Google Cloud Plataform
- Servidor Virtual Privado

## A opção mais simples: local tunnel

Mesmo se você possuir um IP dinâmico ou estiver sob um NAT, poderá implantar sua aplicação e servir
solicitações diretamente do seu computador usando um túnel local.

Essa opção é adequada para testes rápidos, demonstrações de produtos ou compartilhamento de uma aplicação com um grupo pequeno de pessoas.

Uma ferramenta muito boa para isso, disponível em todas as plataformas, é o `ngrok`.

Usando-o, você pode digitar `ngrok PORT` e o PORT desejado é exposto à Internet. Você
obterá um domínio `ngrok.io`, mas com uma assinatura paga poderá obter um URL personalizado e
mais opções de segurança (lembre-se de que você está expondo sua máquina na Internet).

Outro serviço que você pode usar é https://github.com/localtunnel/localtunnel.

## Deploys sem configuração

### Glitch

O Glitch é um playground e uma maneira de criar seus aplicativos mais rapidamente do que nunca, além de vê-los ao vivo
no próprio subdomínio glitch.com. No momento, você não pode ter um domínio personalizado e existem outras
restrições, mas é uma plataforma realmente ótima para prototipações. Você obtém todo o poder do Node.js, uma CDN, armazenamento seguro para
credenciais, importação/exportação do GitHub e muito mais.

Fornecido pela empresa por trás do FogBugz e Trello (e co-criadores do Stack Overflow).

Eu o uso muito para fins de demonstração.

### Codepen

Codepen é uma incrível plataforma e comunidade. Você pode criar um projeto com vários arquivos,
e implantá-lo com um domínio personalizado.


## Serverless

O Serverless é uma maneira de publicar seus aplicativos sem um servidor para gerenciá-los. É um
paradigma em que você publica seus aplicativos como funções e eles respondem em um endpoint de rede
(também chamado FAAS - Functions As a Service*).  *Funções como um serviço.


Algumas opções populares são:

- Serverless Framework
- Standard Library

## PAAS

PAAS signfica Platform As A Service (Plataforma como um serviço). Essas plataformas poupam o seu trabalho em muitas coisas que você deveria se preocupar, permitindo que foque no seu aplicativo.

### Zeit Now

Zeit é uma opção interessante. Você apenas digita `now` no seu terminal, e ele cuida da implantação
sua aplicação. Existe uma versão gratuita com limitações, e a versão paga, que é ainda mais poderosa.
Você simplesmente esquece que existe um servidor, apenas foca no aplicativo.

### Nanobox

[Nanobox](https://nanobox.io/)

### Heroku

Heroku é uma plataforma incrível.
Este é um ótimo artigo sobre como startar o [Node.js no Heroku](https://devcenter.heroku.com/articles/getting-started-with-nodejs).

### Microsoft Azure

Azure é uma oferta da Microsoft Cloud.

Confira como criar uma aplicação web com [Node.js. no Azure](https://docs.microsoft.com/en-us/microsoftteams/platform/get-started/get-started-nodejs).

### Google Cloud Platform

O Google Cloud é uma estrutura incrível para seus apps.

Eles tem uma ótima sessão de [documentação do Node](https://cloud.google.com/node/).

## Servidor Virtual Privado 

- Digital Ocean
- Linode
- Amazon Web Services, em particular, menciono o Amazon Elastic Beanstalk, que consegue diminuir um pouco a complexidade da AWS.

Como eles fornecem uma máquina Linux vazia na qual você pode trabalhar, não há tutorial específico para estes.

Existem muito mais opções nessa categoria, essas são apenas as que eu usei e recomendo.

