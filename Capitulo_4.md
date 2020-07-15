<h1 align="center"> Expor a funcionalidade de um arquivo Node usando imports </h1>

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
  - Instalando um único pacote
  - Atualizando pacotes
- Versionamento
- Tarefas em execução

## Introdução ao npm

`npm` é o gerenciador de arquivos padrão do Node.

Em janeiro de 2017, mais de 350000 pacotes foram listados em
registro npm, tornando-o o maior repositório de códigos de idioma único da Terra, e você pode ter certeza de que há um pacote para (quase!) tudo.

Ele começou como uma maneira de baixar e gerenciar as dependências dos pacotes Node, mas também se tornou uma ferramenta usada no front-end com JavaScript.

Há muitas coisas que o `npm` pode fazer.

> O [Yarn](https://flaviocopes.com/yarn) é uma alternativa ao npm. Certifique-se de dar uma olhadinha também.

## Downloads 

O npm gerencia os downloads de dependências do seu projeto.

### Instalando todas as dependências

Se um projeto tiver um arquivo `packages.json`, executando

```
npm install
```

ele instalará tudo o que o projeto precisa na pasta `node_modules`. Se essa pasta ainda não existir, ele irá criá-la.

### Instalando um único pacote

Você também pode instalar um pacote específico executando

```
npm install <package-name>

```

Frequentemente, você verá mais flags adicionados a este comando:


- `--save`  instala e adiciona nas `dependencies` do arquivo `package.json`

- `--save-dev` instala e adiciona nas `devDependencies` do arquivo `package.json`

A principal diferença é que as devDependencies geralmente recebem as ferramentas de desenvolvimento, como um biblioteca de teste, enquanto as dependencies são incluídas no aplicativo em produção.

### Atualizando pacotes

A atualização também é fácil, executando

```
npm update
```

o `npm` verificará todos os pacotes em busca de uma versão mais nova e que atenda às suas restrições de versão.

Você também pode especificar um único pacote para atualizar:

```
npm update <nome-do-pacote>
```

## Versionamento

Além dos downloads simples, o `npm` também gerencia o controle de versão, para que você possa especificar
versão específica de um pacote ou requerir uma versão superior ou inferior à que você precisa.

Muitas vezes, você descobrirá que uma biblioteca é compatível apenas com uma versão principal de outra biblioteca.

Ou que um bug na versão mais recente de uma lib, ainda não corrigida, está causando um problema.

Especificar uma versão explícita de uma biblioteca também ajuda a manter todos na mesma 
versão de um pacote, assim toda a equipe pode executar a mesma versão, desde o arquivo `package.json`
esteja atualizado.

Em todos esses casos, o controle de versão ajuda muito. O `npm` segue o controle de versão semântico padrão, o `semver`.

## Tarefas em execução

O arquivo package.json suporta um formato para especificar tarefas da linha de comandos que podem ser executadas
usando

```
npm run <nome-da-task>
```

Por exemplo:

```
{
  "scripts": {
    "start-dev": "node lib/server-development",
    "start": "node lib/server-production"
  },
}

```

É muito comum usar esse recurso para executar o Webpack:

```
{
  "scripts": {
    "watch": "webpack --watch --progress --colors --config webpack.conf.js",
    "dev": "webpack --progress --colors --config webpack.conf.js",
   "prod": "NODE_ENV=production webpack -p --config webpack.conf.js",
  },
}

```

Portanto, em vez de digitar esses comandos longos, fáceis de esquecer ou digitar errado, você pode executar

```
$ npm run watch

$ npm run dev

$ npm run prod
```

## Onde o npm instala os pacotes

### Como descobrir onde o npm instala os pacotes

> Leia o [guia do npm](https://flaviocopes.com/npm/) se você estiver começando com o npm, ele entra em muitos detalhes do npm.

Ao instalar um pacote usando o npm (ou yarn), você pode executar 2 tipos de instalação:

- instalação local
- instalação global

Por padrão, quando você digita um comando npm install, como:

```
npm install lodash
```
o pacote é instalado na árvore de arquivos atual, na subpasta `node_modules`.

Quando isso acontece, o npm também adiciona a entrada lodash nas `dependencies` do
arquivo package.json presente na pasta atual.

Uma instalação global é realizada usando a flag `-g`:

```
npm install -g lodash
```

Quando isso acontece, o npm não instala o pacote na pasta local, mas, em vez disso, ele
faz uma localização global.

Mas onde exatamente?

O comando `npm root -g` informará para onde esse local aponta na sua máquina.

## Como usar ou executar um pacote instalado com o npm

### Como incluir e usar no seu código um pacote instalado no seu arquivo node_modules


Quando você instala um pacote de forma local ou global, como
você usa isso no seu código Node?

Digamos que você instale o lodash usando

```
npm install lodash
```

Isso instalará o pacote na pasta local `node_modules`.

Para usá-lo em seu código, basta importá-lo para o seu programa usando o `require`:

```
const _ = require('lodash)
```

E se o seu pacote for um executável?

Nesse caso, ele colocará o arquivo executável na pasta `node_modules / .bin /`.

Uma maneira fácil de demonstrar isso é usando o `cowsay`.

O pacote `cowsay` fornece um programa de linha de comando que pode ser executado para que uma vaca
diga alguma coisa.

Quando você instala o pacote usando o `npm install cowsay`, ele se instala sozinho e adiciona algumas
dependências na pasta `node_modules`:

<h1 align="center"> <img src="https://github.com/ChristySchott/manual-node/blob/master/images/cowsay1.PNG" alt="Cowsay example image" /> </h1>

Há uma pasta .bin oculta, que contém links simbólicos para os binários do cowsay:

<h1 align="center"> <img src="https://github.com/ChristySchott/manual-node/blob/master/images/cowsay2.PNG" alt="Cowsay example image" /> </h1>

Como você os executa?

É claro que você pode digitar `./node_modules/.bin/cowsay` para executá-lo, e isso funciona, mas o `npx`, incluído nas
as versões recentes do npm (desde a 5.2), é uma opção muito melhor. Você só executou:

```
npx cowsay
```

e o npx vai encontrar a localização do pacote.

<h1 align="center"> <img src="https://github.com/ChristySchott/manual-node/blob/master/images/cowsay3.PNG" alt="Cowsay example image" /> </h1>


## O arquivo package.json

### O arquivo package.json é um elemento-chave nas bases de código dos aplicativos produzidos com o Node.js.





















