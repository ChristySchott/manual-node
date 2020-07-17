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

