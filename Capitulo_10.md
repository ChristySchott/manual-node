<h1 align="center"> DIVERSOS </h1>

- [Streams](#streams)
- [Trabalhando com MySQL](#working-mysql)
- [Diferença entre desenvolvimento e produção](#difference-development-production)

## <a name="streams"></a> Streams

**Saiba para que servem streams, por que eles são tão importantes e como usá-los.**

- [O que são streams](#what-are-streams)
- [Porque streams](#why-streams)
- [Um exemplo de um stream](#example-of-stream)
- [pipe()](#pipe)
- [Streams mantidos por APIs do Node](#powered-node-apis)
- [Diferente tipos de streams](#different-types)
- [Como criar um stream Readable](#readable-stream)
- [Como criar um stream Writable](#writable-stream)
- [Como obter dados de um stream Readable](#get-data-from-readable-stream)
- [Como enviar dados para um stream Writable](#send-data-to-writable-stream)
- [Apontar para um stream Writable que você escreveu](#signaling-writable-stream)
- [Conclusão](#streams-conclusion)

### <a name="what-are-streams"></a> O que são streams

Streams são um dos conceitos fundamentais que impulsionam as aplicações Node.js.

Eles são uma maneira eficiente de lidar com arquivos de leitura/gravação, comunicações de rede ou todo tipo de troca de informações ponta a ponta.

Streams não são um conceito exclusivo do Node.js. Eles foram introduzidos há décadas no sistema operacional Unix, lá os programas podem interagir entre si, passando streams através do operador pipe (`|`).

Por exemplo, na maneira tradicional, quando você diz para o programa ler um arquivo, o arquivo é lido do começo ao fim na memória, para depois ser processado.

Usando streams você o lê peça por peça, processando seu conteúdo sem manter tudo na memória.

O [módulo de `stream`](https://nodejs.org/api/stream.html) do Node.js fornece a base sobre a qual todas as APIs de streaming são construídas.

### <a name="why-streams"></a> Porque streams

Streams fornecem basicamente duas vantagens principais em relação à outros métodos de manipulação de dados:

- **Eficiência de memória**: você não precisa carregar grandes quantidades de dados na memória antes de
poder processá-los

- **Eficiência de tempo**: leva muito menos tempo para começar a processar dados assim que você os tiver, em vez de esperar até que toda a carga útil de dados esteja disponível

### <a name="example-of-stream"></a> Um exemplo de stream

Um exemplo típico é o de ler arquivos de um disco.

Usando o módulo do Node `fs` você pode ler um arquivo e enviá-lo por HTTP quando uma nova conexão for estabelecida no servidor http:

```
const http = require('http')
const fs = require('fs')

const server = http.createServer(function (req, res) {  
  fs.readFile(__dirname + '/data.txt', (err, data) => {    
    res.end(data)  
  })
})
server.listen(3000)
```  
    
`readFile()` lê todo o conteúdo do arquivo e chama a função callback quando estiver pronto.

`res.end(data)` a função callback retornará o conteúdo do arquivo para o cliente HTTP.

Se o arquivo for grande, a operação levará bastante tempo. Aqui está a mesma coisa escrita usando streams:

```
const http = require('http')
const fs = require('fs')

const server = http.createServer((req, res) => {
  const stream = fs.createReadStream(__dirname + '/data.txt')  
  stream.pipe(res)
})
server.listen(3000)
```

Em vez de esperar até que o arquivo seja totalmente lido, começamos a transmiti-lo para o cliente HTTP assim que tivermos uma porção de dados pronta para ser enviada.

### <a name="pipe"></a> pipe()

O exemplo acima usa a linha `stream.pipe(res)`: o método `pipe()` é chamado no fluxo de arquivos.

O que faz este código? Ele pega o código fonte e o envia para um destino.

Você o chama no código do stream. Nesse caso, o stream de arquivos é enviado para a resposta HTTP.

O valor de retorno do método `pipe()` é o stream de destino, que é uma coisa muito conveniente e nos permite encadear várias chamadas `pipe()`, assim:

`src.pipe(dest1).pipe(dest2)`

Essa construção é o mesmo que fazer:

```
src.pipe(dest1)
dest1.pipe(dest2)
```

### <a name="powered-node-apis"></a> Streams mantidos por APIs do Node

Devido às suas vantagens, muitos módulos principais do Node.js fornecem recursos nativos de manipulação de stream, os mais notáveis são:

- `process.stdin` retorna um stream conectado ao stdin
- `process.stdout` retorna um stream conectado ao stdout
- `process.stderr` retorna um stream conectado ao stderr
- `fs.createReadStream()` cria um stream Readable para um arquivo
- `fs.createWriteStream ()` cria um stream Writable em um arquivo
- `net.connect ()` inicia uma conexão baseada em stream
- `http.request ()` retorna uma instância do http.ClasseClientRequest, que é um stream Writable
- `zlib.createGzip ()` compacta dados usando gzip (um algoritmo de compressão) em um stream
- `zlib.createGunzip ()` descomprime um stream gzip.
- `zlib.createDeflate ()` compacta dados usando deflate (um algoritmo de compressão) em um stream
- `zlib.createInflate ()` descomprime um stream de deflate

### <a name="different-types"></a> Diferente tipos de streams

Existem quatro classes de streams:

- Readable: um stream no qual você pode receber, mas não enviar dados. Quando você envia dados para um stream Readable, ele é armazenado em buffer, até que o consumidor comece a ler os dados.
- Writable: um stream no qual você pode enviar, mas não receber dados.
- Duplex: um stream no qual você pode enviar e receber, basicamente uma combinação de um stream Readable e Writable.
- Transform: um stream semelhante ao Duplex, mas a saída é uma transformação de sua entrada.

### <a name="readable-stream"></a> Como criar um stream Readable

Nós obtemos um stream Readable a partir do módulo [`stream`](https://nodejs.org/api/stream.html) e nós o inicializamos:

```
const Stream = require('stream')
const readableStream = new Stream.Readable()
```

Agora que o stream foi inicializado, podemos enviar dados para ele:

```
readableStream.push('hi!')
readableStream.push('ho!')
```

### <a name="writable-stream"></a> Como criar um stream Writable

Para criar um stream Writable, criamos o objeto base `Writable` e implementamos o método `its_write()`.

Primeiro, crie um objeto stream:

```
const Stream = require('stream')
const writableStream = new Stream.Writable()
```

depois implemente `_write`:

```
writableStream._write = (chunk, encoding, next) => {
  console.log(chunk.toString())    
  next()
}
```

Agora você pode iniciar um stream Readable em:

`process.stdin.pipe(writableStream)`

### <a name="get-data-from-readable-stream"></a> Como obter dados de um stream Readable

Como lemos dados de um stream Readable? Usando um stream Writable:

```
const Stream = require('stream')

const readableStream = new Stream.Readable()
const writableStream = new Stream.Writable()

writableStream._write = (chunk, encoding, next) => {
  console.log(chunk.toString())    
  next()
}

readableStream.pipe(writableStream)

readableStream.push('hi!')
readableStream.push('ho!')
```

Você também pode consumir um stream Readable diretamente, usando o evento `readable`:

```
readableStream.on('readable', () => {
  console.log(readableStream.read())
}
```

### <a name="send-data-to-writable-stream"></a> Como enviar dados para um stream Writable

Usando o método stream `write()`:

`writableStream.write('hey!\n')`

### <a name="signaling-writable-stream"></a> Apontar para o stream grávavel que você escreveu

Use o método `end()`:

```
const Stream = require('stream')

const readableStream = new Stream.Readable()
const writableStream = new Stream.Writable()

writableStream._write = (chunk, encoding, next) => {
  console.log(chunk.toString())    
  next()
}

readableStream.pipe(writableStream)
```

### <a name="streams-conclusion"></a> Conclusão

Esta é uma introdução aos streams. Há aspectos muito mais complicados para analisar e espero cobri-los em breve.

## <a name="working-mysql"></a> Trabalhando com MySQL

**O MySQL é um dos bancos de dados relacionais mais populares do mundo. Descubra como fazê-lo funcionar com o Node.js**

É claro que o ecossistema do Node possui vários pacotes diferentes permitindo você interagir com o MySQL para  armazenar dados, recuperar dados e assim por diante.

Usaremos `mysqljs/mysql`, um pacote que existe há anos e tem mais de 12.000 estrelas no GitHub.

### Instalando o pacote mysql no Node

Você instala usando npm

`npm install mysql`

### Inicializando a conexão com o banco de dados

Primeiramente, você inclui o pacote:

`const mysql = require('mysql')`

e cria uma conexão:

```
const options = {  
  user: 'the_mysql_user_name',  
  password: 'the_mysql_user_password',  
  database: 'the_mysql_database_name'
}
const connection = mysql.createConnection(options)
```

Você inicia uma nova conexão chamando:

```
connection.connect(err => {
  if (err) {
    console.error('An error occurred while connecting to the DB')
    throw err  
  }
})
```

### As opções de conexão

No exemplo acima, o objeto options tinha 3 opções:

```
const options = {  
  user: 'the_mysql_user_name',  
  password: 'the_mysql_user_password',  
  database: 'the_mysql_database_name'
}
```

Existem muitas outras que você pode usar, incluindo:

- `host`: o nome padrão do host do banco de dados é `localhost`
- `port`: o número padrão da porta do servidor MySQL é 3306
- `socketPath`: em vez de usar host e porta, socketPath é usado para especificar um socket unix.
- `debug`: por padrão desabilitado, pode ser usado para depuração
- `trace`: por padrão ativado, imprime rastreamentos de pilha quando ocorrem erros
- `ssl`: usado para configurar uma conexão SSL com o servidor (fora do escopo deste tutorial)

### Executar uma consulta SELECT

Agora você está pronto para executar uma consulta SQL no banco de dados. A consulta executada invocará uma função callback que contém um eventual erro, os resultados e os campos.

```
connection.query('SELECT * FROM todos', (error, todos, fields) => {
  if (error) {
    console.error('An error occurred while executing the query')
    throw error  
  }
  console.log(todos)
})
```

Você pode passar valores que serão retirados automaticamente:

```
const id = 223
connection.query('SELECT * FROM todos WHERE id = ?', [id], (error, todos, fields) => {
  if (error) {
    console.error('An error occurred while executing the query')
    throw error  
  }
  console.log(todos)
})
```

Para passar vários valores, basta colocar mais elementos no array que você passa como o segundo parâmetro:

```
const id = 223
const author = 'Flavio'
connection.query('SELECT * FROM todos WHERE id = ? AND author = ?', [id, author], (error, todos, fields) => {
  if (error) {
    console.error('An error occurred while executing the query')throw error  
  }
  console.log(todos)
})
```

### Executar uma consulta INSERT

Você pode passar um objeto:

```
const todo = {  
  thing: 'Buy the milk'  
  author: 'Flavio'
}
connection.query('INSERT INTO todos SET ?', todo, (error, results, fields) => {
  if (error) {
    console.error('An error occurred while executing the query')throw error  
  }
})
```

Se a tabela tiver uma chave primária com `auto_increment`, o valor disso será retornado no valor de `results.insertId`:

```
const todo = {  
  thing: 'Buy the milk'  
  author: 'Flavio'
}
connection.query('INSERT INTO todos SET ?', todo, (error, results, fields) => {
  if (error) {
    console.error('An error occurred while executing the query')
    throw error  
  }}
  const id = results.resultId
  console.log(id)
)
```

### Feche a conexão

Quando você precisar finalizar a conexão com o banco de dados, pode chamar o método `end()`:

`connection.end()`

Isso garante que qualquer consulta pendente seja enviada e a conexão seja encerrada normalmente.

## <a name="difference-development-production"></a> Diferença entre desenvolvimento e produção

**Aprenda a definir diferentes configurações para os ambientes de produção e desenvolvimento**

Você pode ter configurações diferentes para ambientes de produção e desenvolvimento.

O Node assume que está sempre rodando em um ambiente de desenvolvimento. Você pode sinalizar ao Node.js que está executando a produção, definindo a variável de ambiente `NODE_ENV=production`.

Isso geralmente é feito executando o comando:

`export NODE_ENV=production`

no shell, mas é melhor colocá-lo no seu arquivo de configuração do shell (por exemplo, `.bash_profile` com o shell bash) porque, caso o sistema seja reinicializado, a configuração não se manterá.

Você também pode aplicar a variável de ambiente anexando-a ao comando de inicialização do aplicativo:

`NODE_ENV=production node app.js`

Essa variável de ambiente também é uma convenção amplamente usada em bibliotecas externas.

Definir o ambiente como `produção` geralmente garante que:

- o registro é mantido em um nível mínimo e essencial
- mais níveis de cache ocorram, otimizando o desempenho

Por exemplo, Pug, a biblioteca de modelos usada pelo Express, compila no modo de debug se `NODE_ENV` não estiver definido como` produção`. As visualizações do Express são compiladas em cada modo de desenvolvimento de requisição, enquanto na produção elas são armazenadas em cache. 

Há muitos mais exemplos.

O Express fornece hooks de configuração específicos para o ambiente, que são chamados automaticamente com base no valor da variável NODE_ENV:

```
app.configure('development', () => {
  //...
})
app.configure('production', () => {
  //...
})
app.configure('production', 'staging', () => {
  //...
})
```

Por exemplo, você pode usar isso para definir diferentes manipuladores de erro para diferentes modos:

```
app.configure('development', () => {  
  app.use(express.errorHandler({ dumpExceptions: true, showStack: true }));
})
app.configure('production', () => {  
  app.use(express.errorHandler())
})
```

