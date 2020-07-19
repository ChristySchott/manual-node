  <h1 align="center"> Descritores de arquivo </h1>
  
### Como interagir com descritores de arquivo usando o Node

Antes de poder interagir com um arquivo que fica no seu sistema de arquivos, você deve obter um a descritor do arquivo.

Um descritor de arquivo é retornado ao abrir o arquivo usando o método `open()` oferecido pelo
módulo `fs`:

```
const fs = require('fs')

fs.open('/Users/flavio/test.txt', 'r', (err, fd) => {
  // fd é o nosso descritor de arquivo
})
```

Observe o `r` que usamos como o segundo parâmetro para a chamada `fs.open()`.

Essa flag significa que abrimos o arquivo para leitura.

Outras flags que você normalmente encontra são

- `r+` abre o arquivo para leitura e gravação
- `w+` abre o arquivo para leitura e gravação, posicionando o fluxo no início do arquivo. Esse arquivo é criado caso não exista ainda.
- `a` abre o arquivo para gravação, posicionando o fluxo no final do arquivo. Esse arquivo é criado caso não exista ainda.
- `a+` abre o arquivo para leitura e gravação, posicionando o fluxo no final do arquivo. Esse arquivo é criado caso não exista ainda.

Você também pode abrir o arquivo usando o método `fs.openSync`, que em vez de fornecer o
objeto de descritor de arquivo em uma callback, retorna:

```
const fs = require('fs')

try {
  const fd = fs.openSync('/Users/flavio/test.txt', 'r')
} catch (err) {
  console.error(err)
}
```

Depois de obter o descritor de arquivo, independente de qual for a maneira usada, você poderá executar todas as
operações que exigem isso, como chamar `fs.open()` e outras operações que interagem com
o sistema de arquivos.

## Estatísticas do arquivo

### Como obter os detalhes de um arquivo usando o Node

Cada arquivo vem com um conjunto de detalhes que podemos inspecionar usando o Node.

Em particular, usando o método `stat()` fornecido pelo módulo `fs`.

Você o chama passando o caminho de um arquivo e, assim que o Node obtém os detalhes do arquivo, ele invoca uma callback que você passa, com 2 parâmetros: uma mensagem de erro e as estatísticas do arquivo:

```
const fs = require('fs')

fs.stat('/Users/flavio/test.txt', (err, stats) => {
  if (err) {
    console.error(err)
  return
  }
})
```

O Node também fornece um método de sincronização, que bloqueia o encadeamento até que as estatísticas do arquivo estejam prontas:

```
const fs = require('fs')
try {
  const stats = fs.stat('/Users/flavio/test.txt')
} catch (err) {
  console.error(err)
}
```

As informações do arquivo estão incluídas na variável de estatísticas. Que tipo de informação podemos extrair
usando o `stat`?

Muitas, por exemplo:

- se o arquivo é um diretório ou um arquivo, usando `stats.isFile()` e `stats.isDirectory()`
- se o arquivo é um link simbólico usando `stats.isSymbolicLink()`
- o tamanho do arquivo em bytes usando `stats.size`.

Existem outros métodos avançados, mas a maior parte do que você usará no seu dia-a-dia
de programador são esses.

```
const fs = require('fs')
 fs.stat('/Users/flavio/test.txt', (err, stats) => {
  if (err) {
   console.error(err)
  return
  }
  stats.isFile() //true
  stats.isDirectory() //false
  stats.isSymbolicLink() //false
  stats.size //1024000 //= 1MB
})
```

## Caminhos de arquivo (file path)

### Como interagir com caminhos de arquivo e manipulá-los no Node

- Obtendo informações fora da path
- Trabalhando com paths

Todo arquivo no sistema tem um caminho.

No Linux e macOS, um caminho pode se parecer com:
```
/users/flavio/file.txt
```

enquanto os computadores Windows são diferentes e têm uma estrutura assim:
```
C:\users\flavio\file.txt
```
Você deve prestar muita atenção ao usar caminhos em seus aplicativos.

Você inclui este módulo em seus arquivos usando
```
const path = require ('path')
```

e você pode começar a usar seus métodos.

## Obtendo informações fora da path

Dado um path, você pode extrair informações usando esses métodos:

- `dirname`: obtem a pasta pai de um arquivo
- `basename`:  obtem uma parte do nome de um arquivo
- `extname`: obtém a extensão do arquivo

Exemplo: 

```
const notes = '/users/flavio/notes.txt'
path.dirname(notes) // /users/flavio
path.basename(notes) // notes.txt
path.extname(notes) // .txt
```

Você pode obter o nome do arquivo a exntesão especificando um segundo argumento ao `basename`:

```
path.basename(notes, path.extname(notes)) //notes
```

## Trabalhando com caminho (paths)

Você pode unir duas ou mais partes de um caminho usando `path.join()`:

```
const name = 'flavio'
path.join('/', 'users', name, 'notes.txt') //'/users/flavio/notes.txt'
```

Você pode obter o cálculo do caminho absoluto de um caminho relativo usando `path.resolve()`:

```
path.resolve('flavio.txt') //'/Users/flavio/flavio.txt' se eu executar da minha pasta inicial
```
Nesse caso, o Node simplesmente anexará `/flavio.txt` ao diretório de trabalho atual. Se vocês
Para especificar uma segunda pasta de parâmetros, o `resolve` usará o primeiro como base para o segundo:

```
path.resolve('tmp', 'flavio.txt')//'/Users/flavio/tmp/flavio.txt' 
```

Se o primeiro parâmetro começar com uma barra, isso significa que é um caminho absoluto:

```
path.resolve('/etc', 'flavio.txt')
```

`path.normalize()` é outra função útil, que tentará calcular o caminho real quando o comandopath.normalize('/users/flavio/..//test.txt') 
contém especificadores relativos como `.` ou `..`, ou barras duplas:

```
path.normalize('/users/flavio/..//test.txt') 
```

## Lendo arquivos

### Como ler arquivos no Node

A maneira mais simples de ler um arquivo no Node é usar o método `fs.readFile()`, passando para ele o caminho do arquivo e uma callback que será chamada com os dados do arquivo (e o error):

```
const fs = require('fs')

fs.readFile('/Users/flavio/test.txt', (err, data) => {
  if (err) {
    console.error(err)
  return
  }
  
console.log(data)
})
```

Como alternativa, você pode usar a versão síncrona `fs.readFileSync()`:
```
const fs = require('fs')

try {
  const data = fs.readFileSync('/Users/flavio/test.txt', 'utf8')
  console.log(data)
} catch (err) {
  console.error(err)
}
```

A codificação padrão é a utf8, mas você pode especificar uma codificação personalizada usando um segundo
parâmetro.

`Fs.readFile()` e `fs.readFileSync()` leem todo o conteúdo do arquivo na memória antes
de retornar.

Isso significa que arquivos grandes terão um grande impacto no consumo de memória e diminuirão a
velocidade de execução do programa.

Nesse caso, uma opção melhor é ler o conteúdo do arquivo usando streams.

## Escrevendo arquivos

### Como escrever arquivos no Node

A maniera mais fácil de escrever arquivos no Node é usando a API `fs.writeFile()`.

Exemplo:
```
const fs = require('fs')

const content = 'Some content!'

fs.writeFile('/Users/flavio/test.txt', content, (err) => {
  if (err) {
    console.error(err)
  return
  }
})

```

Alternativamente, você pode usar a versão sincrona com o `fs.writeFileSync()`:

```
const fs = require('fs')

 const content = 'Some content!'
  try {
  const data = fs.writeFileSync('/Users/flavio/test.txt', content)
  } catch (err) {
  console.error(err)
  }
```

Por padrão, essa API irá substituir os conteúdos do arquivo se eles já existem.

Você pode modificar o padrão especificando uma flag:
```
fs.writeFile('/Users/flavio/test.txt', content, { flag: 'a+' }, (err) => {})
```

A flags que você deveria conhecer são:

- `r+` abre o arquivo para leitura e gravação
- `w+` abre o arquivo para leitura e gravação, posicionando o fluxo no início do arquivo. Esse arquivo é criado caso não exista ainda.
- `a` abre o arquivo para gravação, posicionando o fluxo no final do arquivo. Esse arquivo é criado caso não exista ainda.
- `a+` abre o arquivo para leitura e gravação, posicionando o fluxo no final do arquivo. Esse arquivo é criado caso não exista ainda.


## Anexando a um arquivo

Um método útil para anexar conteúdo ao final de um arquivo é `fs.appendFile()` (e sua
contraparte` fs.appendFileSync()`):

```
const content = 'Some content!'
  fs.appendFile('file.log', content, (err) => {
  if (err) {
    console.error(err)
  return
  }
})
```

## Usando streams

Todos esses métodos gravam o conteúdo completo no arquivo antes de retornar o controle ao seu
programa (na versão assíncrona, isso significa executar a callback).

Nesse caso, uma opção melhor é escrever o conteúdo do arquivo usando streams.

## Trabalhando com pastas

### Como interagir com pastas usando o Node

O módulo `fs` provém métodos muito úteis que você pode usar para trabalhar com pastas. 

### Checando se uma pasta existe

Use o `fs.access()`para checar se a pasta existe e o Node pode acessá-la.

### Criando uma nova pasta

Use `fs.mkdir()` ou `fs.mkdirSync()` para criar uma nova pasta.

```
const fs = require('fs')

const folderName = '/Users/flavio/test'
 try {
   if (!fs.existsSync(dir)){
    fs.mkdirSync(dir)
 }
 } catch (err) {
   console.error(err)
 }
```
### Lendo o conteúdo de um diretório

Use `fs.readdir()` ou `fs.readdirSync`para ler o conteúdo de um diretório.
Esse pedaço de código lê o conteúdo de uma pasta, tanto os arquivos quanto os subarquivos, e returna o seu caminho relativo:
```
const fs = require('fs')

const path = require('path')

const folderPath = '/Users/flavio'
fs.readdirSync(folderPath)
```
Você pode obter o caminho completo:

```
fs.readdirSync(folderPath).map(fileName => {
  return path.join(folderPath, fileName)
}
```

Você também pode filtrar os resultados para retornar apenas os arquivos e excluir as pastas:

```
const isFile = fileName => {
  return fs.lstatSync(fileName).isFile()
}

fs.readdirSync(folderPath).map(fileName => {
  return path.join(folderPath, fileName)).filter(isFile)
}

```

### Renomeando uma pasta

Use `fs.rmdir()` ou `fs.rmdirSync()` para remover uma pasta.
Remover uma pasta que tenha conteúdo pode ser mais complicado do que você pensa.


Nesse caso eu recomendo instalação do módulo `fs-extra`, que é muito popular e estável, além de ser uma substituição do módulo `fs`, provendo ainda mais recursos que ele.

Nesse caso, o método `remove()` é o que você precisa.

Instale isso usando
```
npm install fs-extra
```
e use isso assim:
```
const fs = require('fs-extra')
const folder = '/Users/flavio'

fs.remove(folder, err => {
  console.error(err)
})
```
Isso também pode ser usado com promises:

```
fs.remove(folder).then(() => {
  //done
}).catch(err => {
  console.error(err)
})
```
ou com async/await:
```
async function removeFolder(folder) {
 try {
  await fs.remove(folder)
  //done
 } catch (err) {
  console.error(err)
  }
}

const folder = '/Users/flavio'
 removeFolder(folder)
```

