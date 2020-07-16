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


Se você já trabalhou em um projeto de Frontend, ou apenas utilizou JavaScript ou Node.js, você certamente encontrou o arquivo `package.json`.

Para que ele serve? O que você deve saber sobre ele e quais coisas você pode
fazer com ele?

O arquivo `package.json` é uma espécie de manifesto para o seu projeto. Ele pode fazer muitas coisas, algumas delas nem um pouco relacionadas. Pode servir como um repositório central de configuração de ferramentas, por exemplo. É também onde o `npm`
e o `yarn` armazenam os nomes e as versões dos pacotes instalados.

- A estrutura do arquivo
- Divisão de propriedades
  - name
  - author
  - contributors
  - bugs
  - homepage
  - version
  - license
  - keywords
  - description
  - repository
  - main
  - private
  - scripts
  - dependencies
  - devDependencies
  - engines
  - browserList
- Versões dos pacotes

## A estrutura do arquivo

Aqui está um arquivo package.json de exemplo:

```
{

}
```

Está vazio! Não há requisitos fixos do que deve estar em um arquivo package.json, para um
aplicação. O único requisito é que respeite o formato JSON, caso contrário não poderá ser
lido por programas que tentam acessar suas propriedades de forma programática.

Se você estiver criando um pacote Node.js. que deseja distribuir por `npm`, as coisas mudam
radicalmente, e você deve ter um conjunto de propriedades que ajudem outras pessoas a usá-lo. Veremos
mais sobre isso mais adiante.

Este é outro package.json:

```
{
"name": "test-project"
}

```

Ele define uma propriedade `name`, que informa o nome do aplicativo ou pacote que está contido pasta em que esse arquivo está localizado.
Aqui está um exemplo muito mais complexo, que eu extraí de uma aplicação em Vue.js:

```
{
  "name": "test-project",
  "version": "1.0.0",
  "description": "A Vue.js project",
  "main": "src/main.js",
  "private": true,
  "scripts": {
  "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
  "start": "npm run dev",
  "unit": "jest --config test/unit/jest.conf.js --coverage",
  "test": "npm run unit",
  "lint": "eslint --ext .js,.vue src test/unit",
  "build": "node build/build.js"
},
  "dependencies": {
    "vue": "^2.5.2"
},
  "devDependencies": {
  "autoprefixer": "^7.1.2",
  "babel-core": "^6.22.1",
  "babel-eslint": "^8.2.1",
  "babel-helper-vue-jsx-merge-props": "^2.0.3",
  "babel-jest": "^21.0.2",
  "babel-loader": "^7.1.1",
  "babel-plugin-dynamic-import-node": "^1.2.0",
  "babel-plugin-syntax-jsx": "^6.18.0",
  "babel-plugin-transform-es2015-modules-commonjs": "^6.26.0",
  "babel-plugin-transform-runtime": "^6.22.0",
  "babel-plugin-transform-vue-jsx": "^3.5.0",
  "babel-preset-env": "^1.3.2",
  "babel-preset-stage-2": "^6.22.0",
  "chalk": "^2.0.1",
  "copy-webpack-plugin": "^4.0.1",
  "css-loader": "^0.28.0",
  "eslint": "^4.15.0",
  "eslint-config-airbnb-base": "^11.3.0",
  "eslint-friendly-formatter": "^3.0.0",
  "eslint-import-resolver-webpack": "^0.8.3",
  "eslint-loader": "^1.7.1",
  "eslint-plugin-import": "^2.7.0",
  "eslint-plugin-vue": "^4.0.0",
  "extract-text-webpack-plugin": "^3.0.0",
  "file-loader": "^1.1.4",
  "friendly-errors-webpack-plugin": "^1.6.1",
  "html-webpack-plugin": "^2.30.1",
  "jest": "^22.0.4",
  "jest-serializer-vue": "^0.3.0",
  "node-notifier": "^5.1.2",
  "optimize-css-assets-webpack-plugin": "^3.2.0",
  "ora": "^1.2.0",
  "portfinder": "^1.0.13",
  "postcss-import": "^11.0.0",
  "postcss-loader": "^2.0.8",
  "postcss-url": "^7.2.1",
  "rimraf": "^2.6.0",
  "semver": "^5.3.0",
  "shelljs": "^0.7.6",
  "uglifyjs-webpack-plugin": "^1.1.1",
  "url-loader": "^0.5.8",
  "vue-jest": "^1.0.2",
  "vue-loader": "^13.3.0",
  "vue-style-loader": "^3.0.1",
  "vue-template-compiler": "^2.5.2",
  "webpack": "^3.6.0",
  "webpack-bundle-analyzer": "^2.9.0",
  "webpack-dev-server": "^2.9.1",
  "webpack-merge": "^4.1.0"
},
  "engines": {
    "node": ">= 6.0.0",
    "npm": ">= 3.0.0"
},
  "browserslist": [
    "last 2 versions",
    "not ie <= 8"
]
}

```

há muita coisa acontecendo aqui:

- `name` define o nome do pacote/aplicativo
- `version` indica a versão atual
- `description` é uma curta explicação do pacote/aplicativo
- `main` define o ponto de entrada da aplicação
- `private` se definido como `true`, previne que o pacote/aplicativo seja acidentalmente publicado no `npm`
- `scripts` define um conjuto de scripts que você pode rodar
- `dependencies` define uma lista de pacotes `npm` instalados como dependências
- `devDependencies` define uma lista de pacotes `npm` instalados como dependências de desenvolvimento
- `engines` define em quais versões do Node este pacote/aplicativo funciona
- `browserslist` é usado para dizer a quais navegadores (e suas versões) você deseja oferecer suporte

Todas essas propriedades são utilizadas tanto pelo `npm` quanto por qualquer outra ferramenta que possamos escolher.

## Versões dos pacotes

Você viu na descrição acima números de versão como estes: `~3.0.0` e `^0.13.0`.

O que eles significam e quais outros especificadores de versão você pode usar?

Esse símbolo define quais atualizações daquela dependência o seu pacote aceita.

Dado que usando `semver`(controle de versão semântico) todas as versões têm 3 dígitos, sendo o primeiro a
versão principal, a segunda a versão menor e a terceira a versão do patch, você tem esses
regras:

- `~ `: se você escrever `~0.13.0` , você deseja atualizar apenas a versão do patch: `0.13.1` está ok, mas `0.14.0` não.
- `^ `: se você escrever `^0.13.0` , você deseja atualizar versão do patch e a versão menor: `0.13.1`, `0.14.0` e assim por diante.
- `* `: se você escrever `*`, isso significa que você aceita todas as atualizações, incluindo as principais atualizações de versão.
- `> `: você aceita qualquer versão superior à especificada
- `>= `: você aceita qualquer versão igual ou superior à especificada
- `<= `: você aceita qualquer versão igual ou inferior à especificada
- `< `: você aceita qualquer versão inferior à especificada

Existem outras regras também:

- nenhum símbolo: você aceita apenas a versão especificada
- `latest`: você deseja usar a versão mais recente disponível

## O arquivo package-lock.json

### Como descobrir qual a versão de um pacote que você instalou no seu aplicativo

Para ver a versão mais recente de todo o pacote npm instalado, incluindo suas dependências:

```
npm list
```

Exemplo: 
```
❯ npm list
/Users/flavio/dev/node/cowsay
└─┬ cowsay@1.3.1
├── get-stdin@5.0.1
├─┬ optimist@0.6.1
│ ├── minimist@0.0.10
│ └── wordwrap@0.0.3
├─┬ string-width@2.1.1
│ ├── is-fullwidth-code-point@2.0.0
│ └─┬ strip-ansi@4.0.0
│ └── ansi-regex@3.0.0
└── strip-eof@1.0.0
```

Você também pode simplesmente abrir o arquivo `package-lock.json`, mas isso envolve alguma verificação visual.

`npm list -g` é o mesmo, mas para pacotes instalados globalmente.

Para obter apenas os pacotes de nível superior (basicamente, aqueles que você disse ao npm para instalar e que listou
no `package.json`), execute `npm list --depth = 0`:

```
❯ npm list --depth=0
/Users/flavio/dev/node/cowsay
└── cowsay@1.3.1

```

Você pode obter a versão de um pacote especificando o seu nome:

```
❯ npm list cowsay
/Users/flavio/dev/node/cowsay
└── cowsay@1.3.1

```

Se você quiser ver qual é a versão mais recente do seu pacote disponível no repositório npm, execute
`npm view [package_name] version`:

```
❯ npm view cowsay version

1.3.1
```

## Como instalar uma versão mais antiga de um pacote `npm`

### Aprenda a instalar uma versão mais antiga de um pacote npm, algo que pode ser útil para resolver um problema de compatibilidade

Você pode instalar uma versão antiga de um pacote npm usando a sintaxe `@`:

```
npm install <package>@<version>
```
Exemplo:
```
npm install cowsay
```
instala a versão 1.3.1 (no momento da digitação).
Instale a versão 1.2.0 com:

```
npm install cowsay@1.2.0
```

O mesmo pode ser feito com pacotes globais:

```
npm install -g webpack@4.16.4
```

Você também pode estar interessado em listar todas aa versôes anteriores de um pacote. Você pode fazer isso com
`npm view <package> versions`:

```
❯ npm view cowsay versions
  [ '1.0.0',
    '1.0.1',
    '1.0.2',
    '1.0.3',
    '1.1.0',
    '1.1.1',
    '1.1.2',
    '1.1.3',
    '1.1.4',
    '1.1.5',
    '1.1.6',
    '1.1.7',
    '1.1.8',
    '1.1.9',
    '1.2.0',
    '1.2.1',
    '1.3.0',
    '1.3.1' ]
```

## Como atualizar todas as dependências do Node para a última versão

### Como você atualiza todas as npm dependenciesx no arquivo package.json, para a versão mais recente disponível?

Quando você instala um pacote usando o `npm install <packagename>`, a versão mais recente disponível do
o pacote é baixado e colocado na pasta `node_modules`, e uma entrada correspondente é
adicionado aos arquivos `package.json` e `package-lock.json` presentes na sua pasta atual.

O `npm` calcula as dependências e instala também a versão mais recente disponível.

Digamos que você instale o `cowsay`, uma ferramenta legal de linha de comando que permite fazer uma vaca dizer coisas.

Quando você npm instala o `cowsay`, essa entrada é adicionada ao arquivo `package.json`:

```
{
  "dependencies": {
    "cowsay": "^1.3.1"
  }
}
```

e este é um extrato do package-lock.json, onde removi as dependências aninhadas para ter mais
clareza:

```
{
  "requires": true,
  "lockfileVersion": 1,
  "dependencies": {
    "cowsay": {
      "version": "1.3.1",
      "resolved": "https://registry.npmjs.org/cowsay/-/cowsay-1.3.1.tgz",
      "integrity": "sha512-3PVFe6FePVtPj1HTeLin9v8WyLl+VmM1l1H/5P+BTTDkMAjufp+0F9eLjzRnOHz
      VAYeIYFF5po5NjRrgefnRMQ==",
      "requires": {
      "get-stdin": "^5.0.1",
      "optimist": "~0.6.1",
      "string-width": "~2.1.1",
      "strip-eof": "^1.0.0"
  }
}
}
}
```

Agora, esses 2 arquivos nos dizem que instalamos a versão `1.3.1` do cowsay, e nossa regra para atualizações é
`^1.3.1`.

Se houver uma nova versão secundária ou de patch e digitarmos `npm update`, a versão instalada será
atualizado e o arquivo `package-lock.json` preenchido de acordo com a nova versão.

O `package.json` permanece inalterado.

Para descobrir novos lançamentos dos pacotes, execute npm `outdated`.

Aqui está a lista de alguns pacotes desatualizados em um repositório que não atualizei por um bom tempo:

<h1 align="center"> <img src="https://github.com/ChristySchott/manual-node/blob/master/images/outdated.PNG" alt="outdate example" /> </h1>

Algumas dessas atualizações são major releases (versões principais). Rodar `npm update` não atualizará essas versões. As major releases nunca são atualizadas dessa maneira porque (por definição) introduzem alterações que podem quebrar a aplicação, e o npm quer evitar esses problemas.

Para atualizar todos os pacotes para uma nova versão principal, instale o pacote `npm-check-updates`
globalmente:

```
npm install -g npm-check-updates
```

depois execute isso: 

```
ncu -u
```

isso atualizará todas as dicas de versão no arquivo package.json, tanto nas `dependencies` quanto nas `devDependencies`, assim o npm poder instalar a nova versão principal.

Agora você está pronto para executar a atualização:

```
npm update
```
Se você acabou de baixar o projeto sem as dependências do `node_modules` e deseja instalar as novas versões primeiro, basta executar

```
npm install
```

## Desinstalando pacotes npm

### Como desinstalar um pacote npm no Node, localmente ou globalmente

To uninstall a package you have previously installed locally (using npm install <packagename> in the node_modules folder, run
Para desinstalar um pacote que você instalou localmente (usando `npm install <packagename>`) na pasta `node_modules`, execute
  
```
npm uninstall <package-name>
```

Usando a flag `-S` ou `--save`, esta operação também removerá a referência no
arquivo `package.json`.

Se o pacote for uma dependência de desenvolvimento listada nas devDependencies do
arquivo package.json, você deve usar as flags `-D` / `--save-dev` para removê-lo do arquivo:

```
npm uninstall -S <package-name>
npm uninstall -D <package-name>
```
Se o pacote está instalado globalmente, você precisa adicionar as flags `-g` / `--global`:

```
npm uninstall -g <package-name>
```

por exemplo:
```
npm uninstall -g webpack
```

Você pode executar este comando de qualquer lugar no seu sistema, não importando a pasta
onde você está atualmente.

## Pacotes globais ou locais


### Quando é melhor instalar um pacote globalmente? E por quê?

A principal diferença entre pacotes globais e locais é isso:

- `pacotes locais` são instalados no diretório onde você executou `npm install <package-name>`, e eles são colocados na pasta `node_modules`

- `pacotes globais` são todos colocados em um único lugar no seu sistema (exatamente onde depende do seu setup),  independente de onde você executou `npm install -g <package-name>`

No seu código, os dois são chamados da mesma maneira

```
require('package-name') ou import <package-name>
```

Em geral, todos os pacotes devem ser instalados localmente.

Isso garante que você possa ter dezenas de aplicativos em seu computador, todos executando como uma versão específica de cada pacote, se necessário.

A atualização de um pacote global faria com que todos os seus projetos usassem a nova versão e, como você pode
imaginar, isso pode causar pesadelos em termos de manutenção, pois alguns pacotes podem ter quebra de
compatibilidade com outras dependências.

Todos os projetos têm sua própria versão local de um pacote, mesmo que isso possa parecer um desperdício de
recursos, é um esforço mínimo se comparado às possíveis consequências negativas.

Um pacote deve ser instalado globalmente quando fornece um comando executável a partir da shell (CLI) e vai ser reutilizado em diferentes projetos.

Você também pode instalar comandos executáveis localmente e rodá-los usando `npx`, mas alguns
pacotes são melhores de serem instalados globalmente.

Exemplos dos pacotes globais mais populares

- `npm`
- `create-react-app`
- `vue-cli`
- `grunt-cli`
- `mocha`
- `react-native-cli`
- `gatsby-cli`
- `forever`
- `nodemon`


Você provavelmente já tem alguns pacotes instalados globalmente no seu sistema. Você pode ver
eles executando
```
npm list -g --depth 0
```
na sua linha de comando.

## npm dependencies e devDependencies

### Quando um pacote é uma dependência e quando é uma dependência de desenvolvedor?

Quando você instala um pacote npm usando o `npm install <package-name>`, você o instala como um
dependencxy.

O pacote é listado automaticamente no arquivo `package.json`, na lista de dependências (a partir do npm 5, você não precisa mais digitar `--save`).

Quando você adiciona as flags `-D` ou `--save-dev`, você o instala como uma dependência de desenvolvedor, que a adiciona à lista `devDependencies`.

As dependências de desenvolvimento são planejadas somente como pacotes de desenvolvimento, desnecessários
em produção.

Ao entrar em produção, se você digitar `npm install` e a pasta contiver um arquivo `package.json`, eles são instalados, pois o `npm` assume que esta é uma área de desenvolvimento.

Você precisa definir o sinalizador `--production` (`npm install --production`) para evitar a instalação dessas
dependências de desenvolvimento.

## npx

O npx é uma maneira muito legal de executar o código Nodex e fornece recursos muito úteis

Neste post, quero apresentar um comando muito poderoso que está disponível no npm
a partir da versão 5.2, lançada em julho de 2017: o `npx`.

Se você não deseja instalar o `npm`, pode instalar o `npx` como um pacote independente

O `npx` permite executar o código criado com o Node e publicado através do registro npm.


### Execute facilmente comandos locais

Os desenvolvedores do Node costumavam publicar a maioria dos comandos executáveis como pacotes globais.

Isso era um problema, porque você realmente não podia instalar versões diferentes do mesmo comando.

A execução de `npx commandname` localiza automaticamente a referência correta do comando dentro da
pasta `node_modules` de um projeto, sem precisar conhecer o caminho exato e sem exigir que o pacote seja instalado globalmente.


## Executando algum código usando uma versão diferente do Node

Use o `@` para especificar a versão e combine isso com o  pacote npm `node`:
```
npx node@6 -v #v6.14.3
npx node@8 -v #v8.11.3
```
Isso ajuda a evitar ferramentas como `nvm` ou outra versões de ferramentas de gerenciamento do Node

## Execute snippets de código diretamente de uma URL

O `npx` não limita você aos pacotes publicados no registro npm.

Você pode executar o código localizado em Github gist, por exemplo:

```
npx https://gist.github.com/zkat/4bc19503fe9e9309e2bfaa2c58074d32
```

Obviamente, você precisa ter cuidado ao executar um código que não controla, afinal, com grandes poderes vem grandes responsabilidades.

