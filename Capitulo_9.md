<h1 align="center"> O módulo fs </h1>

### O módulo `fs` provém funções úteis para interagir com o  sistema do arquivo

Não há necessidade de instalá-lo, Sendo parte do Node, isso pode ser usado simplesmente requisitando isso:

```
const fs = require('fs')
```

Once you do so, you have access to all its methods, which include:
- `fs.access()` : verifca se o arquivo exist e o Node tem permissão para acessá-lo
- `fs.appendFile()` : anexe dados ao arquivo. Se o arquivo não existe, ele será criado
- `fs.chmod()` : muda as permissões de um arquivo especifico passando o nome do arquivo.  Palavras-chave:  `fs.lchmod()` , `fs.fchmod()`
- `fs.chown()` : altere o proprietário e o grupo de um arquivo especificado pelo nome do arquivo passado. Palavras-chave: `fs.fchown()` , `fs.lchown()`
- `fs.close()` : fecha o descritor de um arquivo
- `fs.copyFile()` : copie um arquivocreate a readable file stream
- `fs.createWriteStream()` : crie um arquivo stream de escritura
- `fs.link()` : crie um novo link ao arquivo
- `fs.mkdir()` : cria uma nova pasta
- `fs.mkdtemp()` : cria um diretório temporário
- `fs.open()` : abre o arquivo
- `fs.readdir()` : leia o conteúdo de um diretório
- `fs.readFile()` : leia o conteúdo de um arquivo. Palavras-chave: `fs.read()`
- `fs.readlink()` : leia o valor de um link
- `fs.realpath()` : transform os caminhos relativos ( `.` , `..` ) em caminhos completo
- `fs.rename()` : renomeie uma pasta ou um arquivo
- `fs.rmdir()` : remova uma pasta
- `fs.stat()` : retorna o status de um arquivo especificado pelo nome. Relacionado: `fs.fstat()` , `fs.lstat()`
- `fs.symlink()` : crie um novo link simbólico para o arquivo
- `fs.unlink()` : remove um arquivo ou um link simbólico
- `fs.unwatchFile()` : pare de observar mudanças
- `fs.utimes()` : altere o registro de data e hora do arquivo identificado pelo nome do arquivo passado. Palavras-chave: `fs.futimes()`
- `fs.watchFile()` : comece a observar mudanças no arquivo. Palavras-chave: `fs.watch()`
- `fs.writeFile()` : grave dados em um arquivo. Palavras-chave: `fs.write()`

Uma coisa peculiar sobre o módulo `fs` é que todos os métodos são assíncronos por padrão, mas eles também podem trabalhar de forma síncrona, basta acrescentar o `sync`.

Por exemplo

- `fs.rename()`
- `fs.renameSync()`
- `fs.write()`
- `fs.writeSync()`

Isso faz uma grande diferença no fluxo da sua aplicação.

O Node 10 incluiu um suporte para uma API baseada em promises.

Por example, vamos examinar o método `fs.rename()`. A API assíncrona é usada com uma callback:

```
const fs = require('fs')

fs.rename('before.json', 'after.json', (err) => {
  if (err) {
    return console.error(err)
  }
//terminado
})
```
Uma API assíncrona pode ser usada assim, com um `try/catch` para tratar erros:
```
const fs = require('fs')
  try {
    fs.renameSync('before.json', 'after.json')
   //terminado
  } catch (err) {
      console.error(err)
}
```

A principal diferença aqui é que a execução do seu script será bloqueada no segundo exemplo, até que a operação do arquivo seja bem sucedida.

## path module

O módulo `path` do Node.js fornece funções úteis para interagir com o path (caminho) dos arquivos.

Não há necessidade de instalá-lo. Sendo parte do Node, ele pode ser usado simplesmente digitando isso:

```
const path = require('path')
```
Esse módulo fornece o `path.sep`, que disponibiliza o separador de paths ( `\` no Windows, e `/` no Linux e no macOS), e o `path.delimiter`, que disponibiliza o delimitador do path ( `;` no Windows, e `:` no Linux e no macOS).

Esses são os métodos do `
path`:

- `path.basename()`
- `path.dirname()`
- `path.extname()`
- `path.isAbsolute()`
- `path.join()`
- `path.normalize()`
- `path.parse()`
- `path.relative()`
- `path.resolve()`


### `path.basename()`

Retorna a última parte de um caminho. Um segundo parâmetro pode filtrar a extensão do arquivo:

```
require('path').basename('/test/something') //something
require('path').basename('/test/something.txt') //something.txt
require('path').basename('/test/something.txt', '.txt') //something
```

### `path.dirname()`

Retorna a parte do diretório de um caminho:

```
require('path').dirname('/test/something') // /test
require('path').dirname('/test/something/file.txt') // /test/something
```

### `path.extname()`

Retorna a última parte da extensão de um caminho:

```
require('path').extname('/test/something') // ''
require('path').extname('/test/something/file.txt') // '.txt'
```

### `path.extname()`

Retorna a última parte da extensão de um caminho:

```
require('path').extname('/test/something') // ''
require('path').extname('/test/something/file.txt') // '.txt'
```

### `path.isAbsolute()`

Retorna `true` se o caminho for absoluto:

```
require('path').isAbsolute('/test/something') // true
require('path').isAbsolute('./test/something') // false
```

### `path.join()`

Junta duas ou mais partes de um caminho

```
const name = 'flavio'
require('path').join('/', 'users', name, 'notes.txt') //'/users/flavio/notes.txt'
```

### `path.normalize()`

Tenta calcular o caminho atual quando esse contém especificadores relativos como `.` ou `..`, ou barras duplas

```
require('path').normalize('/users/flavio/..//test.txt') ///users/test.txt
```

### `path.parse()`

Analisa um caminho para um objeto com os segmentos que o compõem:

- `root` : a raíz
- `dir` : o caminho do arquivo a partir da raíz
- `base` : o nome do arquivo + extensão
- `name` : o nome do arquivo
- `ext` : a extensão do arquivo

Exemplo:
```
require('path').parse('/users/test.txt')
```

Retorna: 

```
{
  root: '/',
  dir: '/users',
  base: 'test.txt',
  ext: '.txt',
  name: 'test'
}
```
### `path.relative()`

Aceita 2 caminhos como argumentos. Retorna o caminho relativo do primeiro ao segundo caminho,
com base no diretório de trabalho atual.

Exemplo:

```
require('path').relative('/Users/flavio', '/Users/flavio/test.txt') //'test.txt'
require('path').relative('/Users/flavio', '/Users/flavio/something/test.txt') //'something
/test.txt'
```

### `path.resolve()`

Você pode calcular o caminho absoluto de um caminho relativo usando o `path.resolve()`:
```
path.resolve('flavio.txt') //'/Users/flavio/flavio.txt' se eu executar do meu arquivo inicial
```
Ao especificar um segundo pârametro, o `resolve` irá usar o primeiro como base para o segundo:
```
path.resolve('tmp', 'flavio.txt')//'/Users/flavio/tmp/flavio.txt' se eu executar do meu arquivo inicial
```
Se o primeiro pârametro iniciar com uma barra, isso significa que é um caminho absoluto:
```
path.resolve('/etc', 'flavio.txt')//'/etc/flavio.txt'
```
## `os` module

### O módulo `os` fornece algumas funções úteis para interagir com o sistema subjacente

Este módulo fornece muitas funções que você pode usar para recuperar informações do
sistema operacional subjacente e o computador em que o programa é executado, e interagir com elas.
```
const os = require('os')
```
Existem algumas propriedades úteis que nos dizem algumas coisas importantes relacionadas ao manuseio de arquivos:

`os.EOL` fornece a sequência do delimitador de linha. Está \n no Linux e macOS e \r\n no Windows.
There are a few useful properties that tell us some key things related to handling files:
os.EOL gives the line delimiter sequence. It's \n on Linux and macOS, and \r\n on
Windows.

`os.constants.signals` nos mostra todas as constantes relacionadas ao tratamento de sinais no processo, como
SIGHUP, SIGKILL e assim por diante.

`os.constants.errno` define as constantes para o relatório de erros, como EADDRINUSE, EOVERFLOW
e mais.

Você pode ler todos elas em https://nodejs.org/api/os.html#os_signal_constants.

Vamos agora ver os principais métodos que o `os` fornece:

- `os.arch()`
- `os.cpus()`
- `os.endianness()`
- `os.freemem()`
- `os.homedir()`
- `os.hostname()`
- `os.loadavg()`
- `os.networkInterfaces()`
- `os.platform()`
- `os.release()`
- `os.tmpdir()`
- `os.totalmem()`
- `os.type()`
- `os.uptime()`

### `os.arch()`

Retorna a string que identifica a arquitetura subjacente, como `arm`, `x64`, `arm64`.

### `os.cpus()`

Retorna informações sobre as CPUs disponíveis no seu sistema.

Exemplo: 
```
[ { model: 'Intel(R) Core(TM)2 Duo CPU P8600 @ 2.40GHz',
    speed: 2400,
    times:
     { user: 281685380,
       nice: 0,
       sys: 187986530,
       idle: 685833750,
       irq: 0 } },
  { model: 'Intel(R) Core(TM)2 Duo CPU P8600 @ 2.40GHz',
    speed: 2400,
    times:
   { user: 282348700,
     nice: 0,
     sys: 161800480,
     idle: 703509470,
     irq: 0 } } ]
```

### `os.endianness()`

Retura `BE` ou `LE` dependendo se o Node foi compilado com `Big Endian` ou `Little Endian`.

### `os.freemem()`

Retorna o número de bytes que representam a memória livre no sistema.

### `os.homedir()`

Retorna o caminho para o diretório inicial do usuário atual.

Exemplo:
```
'/Users/flavio'
```
### `os.hostname()`

Retorna o nome do host.

### `os.loadavg()`

Retorna o cálculo de média de carga feito pelo sistema operacional.

Ele retorna apenas um valor significativo no Linux e no macOS.

Exemplo:

```
[ 3.68798828125, 4.00244140625, 11.1181640625 ]
```

### `os.networkInterfaces()`

Retorna os detalhes das interfaces de rede disponíveis no seu sistema.

Exemplo:
```
{ lo0:
[ { address: '127.0.0.1',
    netmask: '255.0.0.0',
    family: 'IPv4',
    mac: 'fe:82:00:00:00:00',
    internal: true },
    { address: '::1',
    netmask: 'ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff',
    family: 'IPv6',
    mac: 'fe:82:00:00:00:00',
    scopeid: 0,
    internal: true },
    { address: 'fe80::1',
    netmask: 'ffff:ffff:ffff:ffff::',
    family: 'IPv6',
    mac: 'fe:82:00:00:00:00',
    scopeid: 1,
    internal: true } ],
  en1:
[ { address: 'fe82::9b:8282:d7e6:496e',
    netmask: 'ffff:ffff:ffff:ffff::',
    family: 'IPv6',
    mac: '06:00:00:02:0e:00',
    scopeid: 5,
    internal: false },
    { address: '192.168.1.38',
    netmask: '255.255.255.0',
    family: 'IPv4',
    mac: '06:00:00:02:0e:00',
    The os module
    165
    internal: false } ],
  utun0:
[ { address: 'fe80::2513:72bc:f405:61d0',
    netmask: 'ffff:ffff:ffff:ffff::',
    family: 'IPv6',
    mac: 'fe:80:00:20:00:00',
    scopeid: 8,
    internal: false } ] }
```

### `os.plataform()`

Retorna a plataforma para a qual o Node foi compilado:

- `darwin`
- `freebsd`
- `linux`
- `openbsd`
- `win32`
- ...mais

### `os.release()`

Retorna uma string que identifica o número da versão do sistema operacional

### `os.tmpdir()`

Retorna o caminho para a pasta temporária atribuída.

### `os.totalmem()`

Retorna o número de bytes que representam a memória total disponível no sistema.

### `os.type()`

Identifica o sistema operacional:

- `Linux`
- `Darwin` on macOS
- `Windows_NT` no Windows

### `os.uptime()`

Retorna o número de segundos que o computador está executando desde a última reinicialização.


## events module

### O módulo `events` do Node.js fornece a classe EventEmitter

O módulo `events` do Node.js fornece a classe EventEmitter, que é a chave para trabalhar com eventos no Node.

Eu publiquei um artigo completo sobre isso, então aqui eu irei apenas descrever a API sem examples mais profundos de como usá-la.
```
const EventEmitter = require('events')
const door = new EventEmitter()
```

O ouvinte de evento usa estes eventos:

- `newListener` quando um ouvinte é adicionado
- `removeListener` quando ouvinte é removido

Aqui temos uma descrição detalhada dos métodos mais úteis:

- `emitter.addListener()`
- `emitter.emit()`
- `emitter.eventNames()`
- `emitter.getMaxListeners()`
- `emitter.listenerCount()`
- `emitter.listeners()`
- `emitter.off()`
- `emitter.on()`
- `emitter.once()`
- `emitter.prependListener()`
- `emitter.prependOnceListener()`
- `emitter.removeAllListeners()`
- `emitter.removeListener()`
- `emitter.setMaxListeners()`


### `emitter.addListener()`

Pseudônimo para `emitter.on()`

### `emitter.emit()`

Emite um evento. Chama de forma síncrona todos os ouvintes de eventos na ordem em que foram registrados

### `emitter.eventNames()`

Retorne um array de strings que representam os eventos registrados no EventListener atual:

```
door.eventNames()
```

### `emitter.getMaxListeners()`

Obtenha a quantidade máxima de ouvintes que você pode adicionar a um objeto EventListener. O padrão
é 10, mas pode ser aumentado ou reduzido usando `setMaxListeners()`

```
door.getMaxListeners()
```




### `emitter.listenerCount()`

Obtenha a contagem de ouvintes do evento passados como parâmetro:

```
door.listenerCount('open')
```


### `emitter.listeners()`

Obtém um array de event listeners passados como parâmetro:

```
door.listeners('open')
```

### `emitter.off()`

Pseudônimo para o `emitter.removeListener()`, adicionado no Node 10

### `emitter.on()`

Adiciona uma callback que é chamada quando um evento é acionado.

Use:

```
door.on('open', () => {
  console.log('Door was opened')
})
```

### `emitter.once()`

Adiciona uma callback que é chamada quando um evento é acionado pela primeira vez. Essa callback só será chamada uma vez.

```
const EventEmitter = require('events')
const ee = new EventEmitter()

ee.once('my-event', () => {
  //chama a callback uma vez
})
```
### `emitter.prependListener()`

Quando você adiciona um listener usando `on` ou `addListener`, ele é adicionado por último na fila de listeners e é o último a ser chamado. Usando `prependListener`, ele é adicionado e chamado antes dos outros listeners.

### `emitter.prependOnceListener()`

Quando você adiciona um ouvinte usando o `once`, ele é adicionado por último na fila de ouvintes e é o último a ser chamado.
Usando `prependOnceListener`, ele é adicionado e chamado antes de outros ouvintes.

### `emitter.removeAllListeners()`

Remove todos os listeners de um objeto emissor de eventos que está ouvindo um evento específico:

```
door.removeAllListeners('open')
```

### `emitter.removeListener()`

Remova um listener específico. Você pode fazer isso atribuindo a callback a uma variável, quando
adicionado, para que você possa fazer uma referência posteriormente:

```
const doSomething = () => {}
door.on('open', doSomething)
door.removeListener('open', doSomething)
```

### `emitter.setMaxListeners()`

Define a quantidade máxima de listeners que você pode adicionar a EventListener, cujo padrão é 10, mas pode ser aumentado ou diminuído.

```
door.setMaxListeners(50)
```

## http module

### O módulo http do Node.js fornece funções e classes úteis para criar um Servidor HTTP























