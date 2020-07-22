<h1 align="center"> INTRODUÇÃO </h1>

## Introdução ao Node

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

## Uma breve história do Node

### Uma retrospectiva da história do Node.js de 2009 até hoje

Acredite ou não, o Node.js tem apenas 11 anos.
Em comparação, o JavaScript tem 23 anos e a Web como a conhecemos (após a introdução do
Mosaic) tem 25 anos.
9 anos é uma quantidade de tempo muito pequena para uma tecnologia, mas o Node.js parece ter existido desde sempre.
Tive o prazer de trabalhar com o Node desde os primeiros dias, quando ele tinha apenas 2 anos e, 
apesar da pouca informação disponível, você já podia sentir que era uma coisa enorme.

Neste post, quero desenhar o panorama geral do Node em sua história, para colocar as coisas em perspectiva.

- Um pouco de história
- 2009
- 2010
- 2011 
- 2012
- 2013
- 2014
- 2015
- 2016
- 2017
- 2018

### Um pouco de história

JavaScript é uma linguagem de programação criada na Netscape como uma ferramenta de script para
manipular páginas da Web dentro de seu navegador, o Netscape Navigator.

Parte do modelo de negócios da Netscape era vender servidores da Web, que incluíam um
ambiente chamado Netscape LiveWire, onde se pode criar páginas dinâmicas usando
JavaScript no lado do servidor. Portanto, a ideia do JavaScript no lado do servidor não foi introduzida pelo Node.js, mas é antiga
assim como o JavaScript - apesar de não ter sido bem-sucedida na época.

Um fator chave que levou ao sucesso do Node.js foi o tempo. JavaScript desde alguns anos foi
começando a ser considerado uma linguagem séria, graças aos aplicativos "Web 2.0" que
mostraram ao mundo como seria uma experiência moderna na web (pense no Google Maps ou
Gmail).

A barra de desempenho dos mecanismos JavaScript aumentou consideravelmente graças a competição dos browser, que continua forte. As equipes de desenvolvimento por trás de cada navegador principal
trabalham duro todos os dias para nos dar um melhor desempenho, o que é uma grande vitória para o JavaScript como uma
plataforma. O V8, mecanismo que o Node.js usa sob o capô, é um deles e, em particular, é
o mecanismo JS do Chrome.

Mas é claro que o Node.js não é popular apenas por pura sorte ou timing. Ele introduziu um pensamento inovador sobre como programar em JavaScript no lado do servidor.

### 2009 

Nasce o Node.js. A primeira forma de `npm` é criada.

### 2010

`Express` nasce. `Socket.io` nasce.

### 2011

npm alcança o 1.0. Grandes companhias começam a adotar o Node: LinkedIn, Uber. Hapi nasce.

### 2012

A adoção continua muito rapidamente.

### 2013

Primeira grande plataforma de blogs usando o Node: Ghost Koa nasce.

### 2014

Grande drama: `IO.js é` o principal fork do Node.js, com o objetivo de introduzir o suporte ao ES6 e avançar mais rapidamente

### 2015

Nasce a Node.js Foundation. O `IO.js` é mesclado novamente no Node.js.

### 2016

O `Yarn` é criado. Node 6.

### 2017

npm se concentra mais na segurança. Node 8. HTTP/2 V8 apresenta o Node em seu conjunto de testes,
tornando o Node oficialmente um alvo para o mecanismo JS, além de 3 bilhões de downloads de npms no Chrome a cada
semana

### 2018 

Node 10 ESModules

## Como instalar o Node

### Como você pode instalar o Node.js no seu sistema: um gerenciador de pacotes, o instalador oficial do site ou nvm

O Node.js pode ser instalado de diferentes maneiras. Este post destaca as mais comuns e
convenientes.

Pacotes oficiais para todas as principais plataformas estão disponíveis em https://nodejs.org/en/download/.

Uma maneira muito conveniente de instalar o Node.js é através de um gerenciador de pacotes. Nesse caso, cada sistema operacional tem o seu próprio.
No macOS, é o Homebrew, e - uma vez instalado - permite instalar o Node.js
com muita facilidade, executando este comando na CLI:
```
brew install node
```

Outros gerenciadores de pacotes para Linux e Windows estão listados em https://nodejs.org/en/download/package-manager/.

O `nvm` é uma maneira popular de executar o Node. Permite alternar facilmente a versão do Node e instalar
novas versões para tentar resolver o problema se algo quebrar, por exemplo.
Também é muito útil testar seu código com versões antigas do Node.

Consulte https://github.com/creationix/nvm para obter mais informações sobre esta opção.

Minha sugestão é usar o instalador oficial se você está apenas começando e não usa
linha de comando caso contrário, o Homebrew é a minha solução favorita.
De qualquer forma, quando o Node estiver instalado, você terá acesso ao programa executável `node` na linha de comando.

## Quanto JavaScript você deve saber para usar o Node?

### Se você está apenas começando com JavaScript, o quão fundo você precisa conhecer a linguagem?

Como iniciante, é difícil chegar a um ponto em que você tem confiança suficiente nas suas habilidades de programação.
Ao aprender a codar, você também pode ficar confuso sobre onde termina o JavaScript e onde
o Node.js começa, ou vice-versa.

Eu recomendo que você tenha uma boa noção dos principais conceitos de JavaScript antes de mergulhar
no Node.js:

- Estrutura léxica
- Expressões
- Tipos
- Variáveis
- Funções
- this
- Arrow Functions
- Loops
- Loops e escopo
- Arrays
- Template Literals
- Semicolons
- Strict Mode
- ECMAScript 6, 2016, 2017


Com esses conceitos em mente, você está no caminho certo para se tornar um ótimo desenvolvedor JavaScript, tanto no navegador quanto no Node.js.

Os seguintes conceitos também são fundamentais para entender a programação assíncrona, que é uma parte fundamental do Node.js:

- Programação assíncrona e callbacks
- Temporizadores
- Promises
- Async e Await
- Closures
- Event Loop

Felizmente, escrevi um e-book gratuito que explica todos esses tópicos, o Manual do JavaScript. É o recurso mais compacto que você encontrará para aprender tudo isso.

Você pode encontrar o ebook na parte inferior desta página: https://flaviocopes.com/javascript/.

Eu também fiz a tradução desse livro, o resultado pode ser conferido aqui https://github.com/ChristySchott/Manual-Iniciante-JavaScript.

## Diferenças entre o Node e o navegador

### Como a escrita de uma aplicação JavaScript no Node.js difere da Programação Web dentro do navegador

O navegador e o Node usam JavaScript como sua linguagem de programação, porém, criar aplicativos executados no navegador é algo completamente diferente de criar uma aplicação em Node.js.

Apesar de ambos os casos utilizarem JavaScript, existem algumas diferenças importantes que tornam essa experiência radicalmente diferente. 

Como um desenvolvedor front-end que cria aplicativos para o Node, há uma enorme vantagem - a linguagem ainda é
o mesmo.

Você tem uma grande oportunidade, já sabemos como é difícil aprender uma
linguagem de programação, ao usar a mesma linguagem para executar todo o seu trabalho na web -
tanto no cliente quanto no servidor, você está em posição de vantagem.

O que muda é o ecossistema.

No navegador, na maioria das vezes, o que você está fazendo é interagir com a DOM ou com outra API Web, como os `cookies`. Essas APIs não existem no Node, é claro. Você não tem o `document`, `window` e todos os outros objetos fornecidos pelo navegador.

E no navegador, não temos todas as excelentes APIs que o Node.js fornece por meio de seus módulos,
como a funcionalidade de acesso ao sistema de arquivos.

Outra grande diferença é que no Node.js você controla o ambiente. A menos que você esteja construindo
um aplicativo open souce onde qualquer pessoa, de qualquer lugar, pode densenvolver, você sabe qual versão do
Node em que você executará o aplicativo. Comparado com o ambiente do navegador, onde você não
possui o luxo de escolher qual navegador seus visitantes usarão, isso é muito conveniente.

Isso significa que você pode escrever todo o JavaScript ES6-7-8-9 moderno que sua versão do Node
apoia.

Como o JavaScript se move muito rápido, os navegadores podem ser um pouco lentos e os usuários podem levar tempo para fazer a atualização, então você se vê preso em usar versões mais antigas de JavaScript / ECMAScript.

Você pode usar o Babel para transformar seu código em compatível com ES5 antes de enviá-lo para o
navegador, mas no Node, você não precisará disso.

Outra diferença é que o Node usa o sistema de módulos CommonJS, enquanto no navegador
estão começando a utilizar o padrão ES Modules.

Na prática, isso significa que, por enquanto, você usa `require()` no Node e `import` no
navegador.

**O livro é de 2018, hoje EM a prática mais comum é utilizar o padrão ES Modules. Apessar disso, algumas empresas ainda utilizam o CommonJS**


