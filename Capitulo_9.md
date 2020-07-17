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

