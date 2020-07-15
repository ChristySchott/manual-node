<h1 align="center> Expor a funcionalidade de um arquivo Node usando imports </h1>

### Como usar a API `module.exports` para expor dados a outros arquivos no seu
aplicativo ou para outros aplicativos também

O Node possui um sistema de módulos embutido.

Um arquivo Node.js pode importar a funcionalidade exposta por outros arquivos Node.js.

Quando você deseja importar algo que você usa

```
const library = require('./library')
```

para importar a funcionalidade exposta no arquivo `library.js` que reside na pasta do arquivo atual.

Nesse arquivo, a funcionalidade deve ser exposta antes de poder ser importada por outros arquivos.

Qualquer outro objeto ou variável definido no arquivo é privado por padrão, não ficando exposto ao
'mundo exterior'.

É isso que a API `module.exports` oferecida pelo sistema do módulo nos permite fazer.

Quando você atribui um objeto ou uma função como uma nova propriedade de exportação, isso é
exposto e, como tal, pode ser importado tanto em outras partes do seu aplicativo local como em outros aplicativos externos.

Você pode fazer isso de duas maneiras.

O primeira é atribuindo um objeto ao `module.exports`, que é um objeto fornecido imediatamente
pelo sistema de módulos, e isso fará com que seu arquivo exporte exatamente esse objeto:

```
const car = {
  brand: 'Ford',
  model: 'Fiesta'
}

module.exports = car
//..em outro arquivo
const car = require('./car')

```

A segunda maneira é adicionar o objeto exportado como uma propriedade de `exports`. Desta forma, você pode exportar vários objetos, funções ou dados:

```
const car = {
  brand: 'Ford',
  model: 'Fiesta'
}

exports.car = car
```

ou diretamente: 

```
exports.car = {
  brand: 'Ford',
  model: 'Fiesta'
}
```

E no outro arquivo, você o utilizará referenciando uma propriedade de sua importação:

```
const items = require('./items')
items.car

```

ou 

```
const car = require('./items').car
```

Qual é a diferença entre `module.exports` e `export`?

O primeiro expõe o objeto para o qual aponta. O último expõe as propriedades do objeto para o qual aponta.

## npm

Um guia rápido para o `npm`, o poderoso gerenciador de pacotes responsável pelo sucesso do Node.js. Em janeiro de 2017, mais de 350000 pacotes foram listados em
registro npm, tornando-o o maior repositório de códigos de idioma único da Terra, e você pode ter certeza de que há um pacote para (quase!) tudo.

- Introdução ao npm
- Downloads 
  - Instalando todas as dependências
  - Instalando um pacote único
  - Atualizando pacotes
- Versionando
- Tarefas em execução
