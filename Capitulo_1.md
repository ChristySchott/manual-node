<h1 align="center"> Introdução ao Node </h1>

### Este post é um guia de introdução ao Node.js, o ambiente de execução do JavaScript no lado do servidor. O Node.js é construído sobre a engine Google Chrome V8, e é usado principalmente para criar Web Servers - mas não é limitado a isso

- Visão geral
- Os melhores recursos do Node
  - Rápido
  - Simples
  - JavaScript
  - V8
  - Plataforma assíncrona
  - Um enorme número de bibliotecas
- Um exemplo de aplicação Node
- Frameworks e ferramentas do Node

## Visão geral

O Node.js é um ambiente de tempo de execução* para JavaScript que é executado no servidor.

* Runtime, ou em tempo de execução, significa que o programa é gerenciado enquanto é executado.

O Node.js é de open source, multiplataforma e, desde sua introdução em 2009, ficou muito popular, desempenhando um papel significativo no desenvolvimento web. Se as estrelas do GitHub são um
fator de indicação de popularidade, ter mais de 46000 delas significa ser muito popular.

## Os melhores recursos do Node.js

### Fast 

Um dos principais pontos de venda do Node.js é a velocidade. Devido ao seus paradigma não-bloqueador, o código JavaScript em execução no Node.js (dependendo do benchmark) pode ser duas vezes mais rápido que as linguagens compiladas, como C ou Java, e ainda mais rápido se comparado a linguagens interpretadas, como Python ou Ruby.

### Simples

O Node.js é simples. Extremamente simples, na verdade.

### JavaScript

O Node executa código JavaScript. Isso significa que milhões de desenvolvedores front-end que já usam JavaScript no navegador podem ser capazes de executar o código do servidor e o código do frontend usando a mesma linguagem de programação, sem a necessidade de aprender uma ferramenta completamente diferente.

Os paradigmas são todos iguais, e no Node.js os novos padrões ECMAScript podem ser usados primeiro, já que você não precisa esperar que todos os usuários atualizem seus navegadores - você decide qual versão do ECMAScript deseja usar, alterando a versão do Node.js.

### V8

Em execução na Google V8 JavaScript engine, de código aberto, o Node.js pode alavancar o trabalho de milhares de engenheiros que fazem (e continuarão a fazer) o Tempo de execução do Chrome JavaScript em alta velocidade.

### Plataforma assíncrona

Nas linguagens de programação tradicionais (C, Java, Python, PHP), todas as instruções são bloqueadas por
padrão, a menos que você "aceite" explicitamente a execução de operações assíncronas. Se você executar uma
solicitação de rede para ler algum JSON, a execução desse encadeamento específico é bloqueada até que o
resposta está pronta.

O JavaScript permite criar código assíncrono e sem bloqueio de uma maneira muito simples,
usando um único encadeamento (single thread), callbacks e programação orientada a eventos. Toda vez
que uma operação cara (expensive operation) ocorrer, passamos uma função de callback, que será chamada assim que pudermos
continuar com o processamento. Não estamos mais esperando que isso termine antes de continuar com o resto
do programa.

Esse mecanismo deriva do navegador. Mal podemos esperar até que algo carregue de um AJAX
antes de poder interceptar eventos de clique na página. Para foornecer uma boa experiência ao usuário, tudo deve acontecer na hora.

> Se você já criou um manipulador `onclick` para uma página da web, você já usou técnicas de programação assíncrona com event listeners (algo como ouvintes de eventos).

Isso permite que o Node.js manipule milhares de conexões simultâneas com um único servidor,
sem introduzir o ônus de gerenciar a simultaneidade de threads, o que seria uma grande
fonte de erros.
O Node fornece E/S primitivas sem bloqueio e, geralmente, as bibliotecas no Node.js são gravadas usando
paradigmas não-bloqueadores, tornando o comportamento de bloqueio uma exceção.

Quando o Node.js precisa executar uma operação de E/S, como a leitura da rede, acessar um
banco de dados ou um sistema de arquivos, em vez de bloquear o encadeamento, ele simplesmente retomará a
operação quando a resposta voltar, evitando desperdiçar ciclos de CPU em espera.

### Um enorme número de bibliotecas

O `npm`, com sua estrutura simples, ajudou o ecossistema do Node.js a proliferar. Atualmente o npm hospeda quase 500.000 pacotes open source, os quais você pode usar livremente.

## Um exemplo de aplicação Node

O exemplo mais comum do Hello World em Node.js é um web server:

```

const http = require('http')

const hostname = '127.0.0.1'
const port = 3000

const server = http.createServer((req, res) => {
res.statusCode = 200
res.setHeader('Content-Type', 'text/plain')
res.end('Hello World\n')
})

server.listen(port, hostname, () => {
console.log(`Server running at http://${hostname}:${port}/`)
})

```

Para executar esse snippet, salve-o como um arquivo `server.js` e execute `node server.js` no seu terminal.
Esse código primeiro inclui o módulo `http`.

O Node.js possui uma biblioteca padrão incrível, incluindo um suporte de primeira classe para redes.

O método `createServer()` do `http` cria um novo servidor HTTP e o retorna.
O servidor está configurado para escutar na porta e no nome do host especificados. Quando o servidor estiver pronto, a callback é chamada, neste caso, informando que o servidor está em execução.
Sempre que uma nova solicitação é recebida, o evento `request` é chamado, fornecendo dois objetos:
`request` (um objeto http.IncomingMessage) e `send` (um objeto http.ServerResponse).
Esses 2 objetos são essenciais para lidar com a chamada HTTP.
O primeiro fornece os detalhes da solicitação. Neste exemplo simples, isso não é usado, mas você pode
acessar os cabeçalhos da solicitação e solicitar dados.

O segundo é usado para retornar dados a quem o invocar.
Neste caso com

```
res.statusCode = 200
```

definimos a propriedade `statusCode` como 200, para indicar uma resposta bem-sucedida.
Definimos o cabeçalho Content-Type:

```
res.setHeader('Content-Type', 'text/plain')
```

e encerramos a resposta, adicionando o conteúdo como argumento ao `end()`:

```
res.end('Hello World\n')
```

## Frameworks e ferramentas do Node

O Node.js é uma plataforma de baixo nível, e para tornar as coisas mais fáceis e interessantes para os desenvolvedores
milhares de bibliotecas foram criadas no Node.js.
Muitos das quais foram estabelecidas ao longo do tempo como opções populares. Aqui está uma lista para
as que considero muito relevantes e que valem a pena aprender:

- `Express`, uma das maneiras mais simples e poderosas de criar um web server. Possui uma abordagem, sem opinião, focada nos principais recursos de um servidor, é a chave para seu sucesso.

- `Meteor`, uma estrutura full-stack incrivelmente poderosa, fornecendo uma isomórfica
abordagem para criar aplicativos com JavaScript, compartilhando código no cliente e no servidor.
Antes era uma ferramenta pronta para uso que fornecia tudo, agora ela se integra às bibliotecas de front-end
React, Vue e Angular. Também pode ser usada para criar aplicativos móveis.

- `koa`, criado pela mesma equipe por trás do Express, pretende ser ainda mais simples e menor,
construindo sobre anos de conhecimento. O novo projeto nasceu da necessidade de criar
alterações incompatíveis sem interromper a comunidade existente.

- `Next.js`, um framework para renderizar aplicativos React renderizados no servidor.

- `Micro`, um servidor muito leve para criar microsserviços HTTP assíncronos.

- `Socket.io`, um mecanismo de comunicação em tempo real para criar aplicativos de rede.

