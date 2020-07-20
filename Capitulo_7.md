<h1 align="center"> HTTP </h1>

- [HTTP](#http)
- [Como as requisições HTTP funcionam](#http-requests)
- [Construa um servidor HTTP](#http-server)
- [Fazendo requisições HTTP](#http-requests)
- [Axios](#axios)
- [Websockets](#websockets)
- [HTTPS, conexões seguras](#secure-connections)

## <a name="http"></a> HTTP

**Uma descrição detalhada de como funciona o protocolo HTTP e a Web.**

O HTTP (Protocolo de Transfêrencia de Hipertexto) é um dos protocolos de aplicação do TCP/IP, o conjuto de protocolos que alimenta a Internet.

Deixe me corrigir isso: Ele não é apenas um dos protocolos, ele é o mais bem sucedido e popular entre eles.

HTTP é o que faz a "World Wide Web" funcionar, fornecendo aos navegadores uma linguagem para se comunicarem com os servidores remotos que hospedam as páginas da Web.

O HTTP foi padronizado pela primeira vez em 1991, como resultado do trabalho que Tim Berners-Lee realizou no CERN, o Centro Europeu de Pesquisa Nuclear, criado em 1989.

O objetivo era permitir que os pesquisadores trocassem e interligassem seus artigos com facilidade. Era um meio para a comunidade científica trabalhar melhor.

Naquela época, as principais aplicações da Internet eram basicamente:  FTP (Protocolo de Transferência de Arquivos), Email, e Usenet (Grupo de notícias, abandonado nos dias de hoje).

Em 1993, Mosaic, o primeiro navegador gráfico da web, foi lançando e as coisas dispararam a partir daí.

A Web se tornou a aplicação indispensável da Internet. 

Com o tempo, a Web e o ecossistema ao seu redor evoluíram dramaticamente, mas o básico ainda permanece. Um exemplo dessa evolução: O HTTP agora oferece, além de páginas da Web, APIs REST, um método comum de acessar programaticamente um serviço pela Internet.

O HTTP sofreu uma pequena revisão em 1997 com o HTTP/1.1, e em 2015 seu sucessor, o HTTP/2, foi padronizado e agora está sendo implementado pelos principais servidores da Web usados em todo o mundo.

O protocolo HTTP é considerado inseguro, como qualquer outro protocolo (SMTP, FTP...) não servido por uma conexão criptografada. É por isso que hoje em dia existe um grande empurrão no uso do HTTPS, que é um HTTP acrescentado por TLS.

Dito isto, os blocos de construção do HTTP/2 e HTTPS têm suas raízes no HTTP e neste artigo vou apresentar como o HTTP funciona.

### Documentos HTML

O HTTP é a maneira dos **navegadores da Web**, como Chrome, Firefox, Edge e muitos outros (daqui em diante chamados de clientes) se comunicarem com os **servidores da Web**.

O nome Protocolo de Transferência de Hipertexto (HTTP) deriva da necessidade de transferir não apenas arquivos, como no FTP (Protocolo de Transferência de Arquivos), mas também hipertextos, que seriam escritos usando HTML, representados graficamente pelo navegador com uma boa apresentação e links interativos.

Os links, juntamente com a facilidade de criação de novas páginas web, foram as principais causas que impulsionaram a adoção do protocolo. 

O HTTP é o que transfere esses arquivos de hipertexto (e, como veremos também, imagens e outros tipos de arquivos) pela rede.

### Hyperlinks

Dentro do navegador Web, um documento pode apontar para outro documento usando links.

Um link é composto por uma primeira parte que determina o protocolo e o endereço do servidor, através de um nome de domínio ou um IP.

Está parte não é exclusiva do HTTP, é claro.

Depois, há a parte do documento. Qualquer coisa anexada à parte do endereço representa o caminho do documento.

Por exemplo,  `https://flaviocopes.com/http/` o endereço deste documento é:

- `https` é o protocolo.
- `flaviocopes.com` é o nome de domínio que aponta para o meu servidor.
- `http/` é a URL do documento que é relativo ao caminho da raiz do servidor.

Outro exemplo de endereço: `https://flaviocopes.com/page/privacy/` e, neste caso, a URL do documento é `/page/privacy`.

O servidor da Web é responsável por interpretar a requisição e, uma vez analisada, atender com a resposta correta.

### Uma requisição

O que há em uma requisição?

A primeira coisa é **a URL**, que já vimos antes.

Quando inserimos um endereço e pressionamos enter em nosso navegador, por baixo dos panos, o servidor envia para o endereço IP correto uma requisição como esta:

```
  GET /a-page
```

onde /a-page é o URL que você requisitou.

A segunda coisa é `o método HTTP` (também chamado de verbo).

Nos começo, o HTTP definiu três deles:

- `GET`
- `POST`
- `HEAD`

e o HTTP/1.1 introduziu:

- `PUT`
- `DELETE`
- `OPTIONS`
- `TRACE`

Vamos vê-los em detalhes em um minuto.

A terceira coisa que compõe uma requisição é um conjunto de **cabeçalhos HTTP**.

Os cabeçalhos são um conjunto de pares chave:valor que são usados para se comunicar com as informações específicas predefinidas pelo servidor, para que ele possa saber o que queremos dizer.

Eu os descrevi em detalhes na lista de cabeçalhos de requisição HTTP.

Dê uma olhada rápida nessa lista. Todos esses cabeçalhos são opcionais, exceto `Host`.

### Métodos HTTP

#### `GET`

GET é o método mais usado aqui. É o que é usado quando você digita uma URL na barra de endereços do navegador ou quando você clica em um link.

Ele pede ao servidor para enviar o recurso solicitado como resposta.

#### `HEAD`

HEAD é como o GET, mas ele diz ao servidor para não enviar o corpo da resposta de volta, apenas os cabeçalhos.

#### `POST`

O cliente usa o método POST para enviar dados para o servidor. Normalmente é usado em formulários, mas também é usando ao interagir com uma API REST. 

#### `PUT`

O método PUT visa criar um recurso nessa URL específica, com os parâmetros passados no corpo da requisição. Principalmente usado em APIs REST.

#### `DELETE`

O método DELETE é chamado em uma URL para solicitar a exclusão desse recurso. Principalmente usado em APIs REST.

#### `OPTIONS`

Quando um servidor recebe uma requisição OPTIONS, ele deve enviar de volta a lista de métodos HTTP permitidos para esse URL específico.

#### `TRACE`

Retorna ao cliente a requisição que foi recebida. Usado para fins de diagnóstico por debug.

### Comunicação Cliente/Servidor HTTP

O HTTP, como a maioria dos protocolos que pertencem ao conjunto TCP/IP, é um protocolo sem estado.

Servidores não têm ideia de qual é o estado atual do cliente. Tudo o que eles se importam é em receber a requisição e atender isso.

Qualquer requisição anterior não faz sentido nesse contexto, isso possibilita que um servidor da Web seja muito mais rápido, pois há menos para processar e oferecendo largura de banda para lidar com muitas solicitações simultâneas.

O HTTP também é muito enxuto e a comunicação, em termos gerais, é muito rápida. Isso contrasta com os protocolos mais usados no momento em que o HTTP foi introduzido: TCP, POP/SMTP e os protocolos de correio, que envolvem muitos `handshaking` e confirmações de recebimento.

Os navegadores gráficos abstraem toda essa comunicação, mas a ilustraremos aqui para fins de aprendizado.

Uma mensagem é composta por uma primeira linha, que começa com o método HTTP, depois contém o caminho relativo do recurso e a versão do protocolo:

`GET /a-page HTTP/1.1`

Depois disso, precisamos adicionar cabeçalhos de requisição HTTP. Como mencionado acima, existem muitos cabeçalhos, mas o único obrigatório é o `Host`

```
GET /a-page HTTP/1.1
Host: flaviocopes.com
```

Como você pode testar isso ? Usando **Telnet**. Esta é uma ferramenta de linha de comando que nos permite conectar-se a qualquer servidor e enviar-lhe comandos.

Abra seu terminal e digite `telnet flaviocopes.com 80`

Isto irá abrir um terminal, que informará:

```
Trying 178.128.202.129...
Connected to flaviocopes.com.
Escape character is '^]'.
```

Você está conectado ao servidor Netlify que alimenta meu blog. Agora você pode digitar:


```
GET /axios/ HTTP/1.1
Host: flaviocopes.com
```

e apertar enter em um linha vazia para disparar a requisição.

A resposta será:

```
HTTP/1.1 301 Moved Permanently
Cache-Control: public, max-age=0, must-revalidate
Content-Length: 46
Content-Type: text/plain
Date: Sun, 29 Jul 2018 14:07:07 GMT
Location: https://flaviocopes.com/axios/
Age: 0
Connection: keep-alive
Server: Netlify

Redirecting to https://flaviocopes.com/axios/
```

Veja, está é uma resposta HTTP que recebemos do servidor. É uma requisição 301 movida permanentemente. Consulte a lista de códigos de status HTTP para saber mais sobre os códigos de status.

Ela basicamente nos diz que o recurso foi movido permanentemente para outro local.

Por quê? Porque nos conectamos à porta 80, que é o padrão para o HTTP, mas no meu servidor eu configurei um redirecionamento automático para HTTPS.

O novo local é especificado no `Location` dentro do cabeçalho de resposta HTTP.

Existem outros cabeçalhos, todos descritos na lista de cabeçalhos de resposta HTTP.

Tanto na requisição quanto na resposta, uma linha vazia separa o cabeçalho de requisição do corpo da requisição. Neste caso, o corpo da requisição contém a string:

`Redirecting to https://flaviocopes.com/axios/`

Com o tamanho de 46 bytes de comprimento, conforme especificado no cabeçalho `Content-Length`. Isto é mostrado no 
navegador quando você abre a página, enquanto ele automaticamente redireciona você para a localização correta.

Nesse caso, estamos usando o `telnet`, a ferramenta de baixo nível que podemos usar para conectar-se a qualquer servidor, portanto não teremos nenhum tipo de redirecionamento automático.

Vamos fazer esse processo novamente, agora conectando à porta 443, que é a porta padrão do protocolo HTTPS. Não podemos usar o `telnet` por causa do `handshaking` SSL que deve acontecer.

Vamos simplificar as coisas e usar `curl`, outra ferramenta de linha de comando. Não podemos digitar diretamente a requisição HTTP, mas vamos ver a resposta:

`curl -i https://flaviocopes.com/axios/`

é isso que receberemos em de volta:

```
HTTP/1.1 200 OK
Cache-Control: public, max-age=0, must-revalidate
Content-Type: text/html; charset=UTF-8
Date: Sun, 29 Jul 2018 14:20:45 GMT
Etag: "de3153d6eacef2299964de09db154b32-ssl"
Strict-Transport-Security: max-age=31536000
Age: 152
Content-Length: 9797
Connection: keep-alive
Server: Netlify

<!DOCTYPE html>
<html prefix="og: http://ogp.me/ns#" lang="en">
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<title>HTTP requests using Axios</title>
....
```

Eu cortei a resposta, mas você pode ver que o HTML da página está sendo retornado agora.

### Outros recursos

Um servidor HTTP não transfere somente arquivos HTML, transfere também muitos outros tipos diferentes de arquivos, como CSS, JS, SVG, PNG, JPG.

Isso depende da configuração.

O HTTP também é perfeitamente capaz de transferir esses arquivos, deixando o cliente informado sobre o tipo de arquivo, para que ele interprete-os da maneira correta.

É assim que a Web funciona: quando uma página HTML é recuperada pelo navegador, ela é interpretada e qualquer outro recurso necessário para exibir as propriedades (CSS, JavaScript, imagens, ...), são recuperados por meio de requisições HTTP adicionais para o mesmo servidor.

## <a name="http-requests"></a> Como as requisições HTTP funcionam

**O que acontece quando você digita uma URL no navegador, do início ao fim**

- [O protocolo HTTP](#http-protocol)
- [Eu analiso apenas requisições de URL](#url-requests)
- [Estamos nos referindo ao macOS/Linux](#things-relate)
- [Fase de pesquisa do DNS](#dns-lookup)
  - [gethostbyname](#gethostbyname)
- [Handshaking de requisição TCP](#tcp-request-handshaking)
- [Enviando a requisição](#sending-the-request)
  - [A linha da requisição](#request-line)
  - [O cabeçalho da requisição](#request-header)
  - [O corpo da requisição](#request-body)
- [A resposta](#the-response)
- [Analisar o HTML](#parse-html)

> Este artigo descreve como os navegadores executam requisições usando o protocolo HTTP/1.1

Se você já fez uma entrevista, pode ter sido perguntado: "o que acontece quando você digita algo na caixa de pesquisa do Google e pressiona enter". 

Ela é uma das perguntas mais populares. As pessoas só querem ver se você consegue explicar alguns conceitos básicos e se você tem alguma ideia de como a Internet realmente funciona.

Nesta postagem, analisarei o que acontece quando você digita uma URL na barra de endereços do seu navegador e pressiona enter.

É um tópico muito interessante de ser dissecado em uma postagem de blog, pois toca em muitas tecnologias nas quais posso mergulhar em postagens separadas.

Essa é uma tecnologia que raramente é alterada e alimenta um dos ecossistemas mais complexos e amplos já construídos por seres humanos.

### <a name="http-protocol"></a> O protocolo HTTP

Primeiro, eu menciono o HTTPS em particular, porque as coisas são diferentes de uma conexão HTTPS.

### <a name="url-requests"></a> Eu analiso somente requisições da URL

Os navegadores modernos tem a capacidade de saber se o que você escreveu na barra de endereço é uma URL real ou um termo de pesquisa e usarão o mecanismo de pesquisa padrão se não for uma URL válida.

Suponho que você digite um URL real.

Quando você insere o URL e pressiona enter, o navegador primeiro cria o URL completo.

Se você acabou de inserir um domínio, como `flaviocopes.com`, o navegador por padrão irá adicionar `HTTP://` a ele, padronizando com protocolo HTTP.

### <a name="things-relate"></a> Estamos nos referindo ao macOS/Linux

Apenas para sua informação. O Windows pode fazer algumas coisas de maneira ligeiramente diferente.

### <a name="dns-lookup"></a> Fase de pesquisa do DNS

O navegador inicia a pesquisa de DNS para obter o endereço IP do servidor.

O nome de domínio é um atalho útil para nós, humanos, mas a Internet está organizada de tal maneira que os computadores podem procurar a localização exata de um servidor através do seu endereço IP, que é um conjunto de números como o `222.324.3.1` (IPv4).

Primeiro, ele verifica o cache local do DNS para ver se o domínio já foi consultado recentemente.

O Chrome possui um prático visualizador de cache DNS, que você pode ver em `chrome://net-internals/#dns`

Se nada for encontrado lá, o navegador usará o resolvedor de DNS, usando a chamada de sistema `gethostbyname` POSIX para recuperar as informações do host.

#### <a name="gethostbyname"></a> gethostbyname

O `gethostbyname` procura primeiro o arquivo de hosts local, que no macOS ou Linux está localizado em `/etc/hosts`, para verificar se o sistema fornece as informações localmente.

Se ele não fornecer nenhuma informação sobre o domínio, o sistema fará uma requisição ao servidor DNS.

O endereço do servidor DNS é armazenado nas preferências do sistema.

Esses são 2 servidores DNS populares:

- `8.8.8.8`: o servidor DNS público do Google

- `1.1.1.1`: o servidor DNS do CloudFlare

A maioria das pessoas usa o servidor DNS fornecido pelo provedor da Internet.

O navegador executa a requisição DNS usando o protocolo UDP.

TCP e UDP são dois dos protocolos fundamentais da rede de computadores. Eles ficam no mesmo nível conceitual, mas o TCP é orientado à conexão, enquanto o UDP é um protocolo sem conexão, mais leve, usado para enviar mensagens com pouca sobrecarga.

>Como a requisição UDP é realizada não está no escopo deste tutorial

O servidor DNS pode ter o IP do domínio na cache. Caso contrário, ele solicitará ao servidor DNS raiz. Esse é um sistema (composto por 13 servidores reais, distribuídos em todo o planeta) que impulsiona toda a Internet.

O servidor DNS não sabe o endereço de todo e qualquer nome de domínio no planeta.

O que ele sabe é onde estão os resolvedores de DNS de nível superior.

Um domínio de nível superior é a extensão do domínio: `.com`, `.it`, `.pizza` e assim por diante.

Depois que o servidor DNS raiz recebe a requisição, ele a encaminha para o servidor DNS de domínio de nível superior (`TLD`).

Digamos que você esteja procurando por `flaviocopes.com`. O servidor DNS do domínio raiz retorna o IP do servidor TLD `.com`.

Agora, nosso resolvedor de DNS armazenará em cache o IP desse servidor de TLD, para que ele não precise solicitar novamente o servidor DNS raiz.

O servidor DNS do TLD terá os endereços IP dos Servidores de Nome com autoridade para o domínio que estamos procurando.

>Como? Quando você compra um domínio, o registrador de domínios envia os servidores TDL apropriados para o mesmo nome. Quando você atualiza os servidores de nomes (por exemplo, quando altera o provedor de hospedagem), essas informações são atualizadas automaticamente pelo seu registrador de domínio.

Esses são os servidores DNS do provedor de hospedagem. Eles geralmente são mais de 1, para servir como backup.

Por exemplo:

- `ns1.dreamhost.com`
- `ns2.dreamhost.com`
- `ns3.dreamhost.com`

O resolvedor de DNS começa com o primeiro e tenta solicitar o IP do domínio (com o subdomínio) que você está procurando.

Essa é a melhor fonte de verdade para o endereço de IP.

Agora que temos o endereço IP, podemos continuar nossa jornada.

### <a name="tcp-request-handshaking"></a> Handshaking de requisição TCP

Com o endereço de IP do servidor disponível, agora o navegador pode iniciar uma conexão TCP.

Uma conexão TCP requer um pouco de `handshaking` antes de poder ser totalmente inicializada e você comece a enviar dados.

Depois que a conexão for estabelecida, podemos enviar a requisição.

### <a name="sending-the-request"></a> Enviando a requisição

A requisição é um documento de texto simples, estruturado de maneira precisa, determinada pelo protocolo de comunicação.

É composto por 3 partes: 

- O linha da requisição
- O cabeçalho da requisição
- O corpo da requisição

#### <a name="request-line"></a> A linha da requisição

A linha de requisição define, em uma única linha: 

- O método HTTP
- A localização do recurso
- A versão do protocolo

Exemplo:

`GET /HTTP/1.1`

#### <a name="request-header"></a> O cabeçalho da requisição

O cabeçalho da requisição é um conjunto de pares `campo:valor` que definem determinados valores.

Existem 2 campos obrigatórios, um deles é o `Host` e o outro é o `Connection`, todos os outros campos são opcionais:

```
Host: flaviocopes.com
Connection: close
```

`Host` indica o nome do domínio que queremos atingir, enquanto `Connection` é sempre definido como `close`, a menos que a conexão precise ser mantida aberta.

Alguns dos campos de cabeçalho mais usados são:

- `Origin`
- `Accept`
- `Accept-Encoding`
- `Cookie`
- `Cache-Control`
- `Dnt`

mas existem muitos outros.

A parte do cabeçalho é finalizada por uma linha em branco.

#### <a name="request-body"></a> O corpo da requisição

O corpo da requisição é opcional, não usado em requisições GET, mas muito usado em requisições POST e, as vezes, em outros verbos também. Ele pode conter dados no formato JSON.

Como agora estamos analisando uma requisição GET, o corpo está em branco e não o examinaremos mais.

### <a name="the-response"></a> A resposta

Depois que a requisição é enviada, o servidor a processa e manda de volta uma resposta.

A resposta começa com o código de status e a mensagem de status. Se a requisição for bem-sucedida e retornar um 200, ela começará com:

`200 OK`

A requisição pode retornar um código de status diferente e uma mensagem, como uma dessas:

```
404 Not Found
403 Forbidden
301 Moved Permanently
500 Internal Server Error
304 Not Modified
401 Unauthorized
```

Então, A resposta contém uma lista de cabeçalhos HTTP e o corpo da resposta (que, como estamos fazendo a solicitação no navegador, será HTML).

### <a name="parse-html"></a> Analisar o HTML

O navegador agora recebeu o HTML e começa a analisá-lo. Ele repetirá exatamente o mesmo processo que fizemos para todos os recursos exigidos pela página: 

- Arquivos CSS
- Imagens
- O favicon
- Arquivos JavaScript
- ...

Como os navegadores renderizam a página, ela está fora do escopo, é importante entender que o processo que descrevi não é apenas para as páginas HTML, mas para qualquer item veiculado por HTTP.

## <a name="http-server"></a> Construindo um servidor HTTP

**Como criar um servidor HTTP com Node.js**

Aqui está o servidor da Web que usamos como uma aplicação Node "Olá Mundo" na [introdução do Node.js](https://flaviocopes.com/nodejs/)

```
const http = require('http')

const port = 3000

const server = http.createServer((req, res) => {  
  res.statusCode = 200
  res.setHeader('Content-Type', 'text/plain')
  res.end('Hello World\n')
})

server.listen(port, () => {
  console.log(`Server running at http://${hostname}:${port}/`)
})
```

Vamos analisar brevemente. Nós incluímos o [módulo `http`](https://nodejs.org/api/http.html).

Usamos o módulo para criar um servidor HTTP. 

O servidor está configurado para escutar na porta especificada, `3000`. Quando o servidor está pronto, a função callback (de retorno de chamada) `listen` é chamada.


A função callback que passamos é aquela que será executada a cada requisição recebida. Sempre que uma nova requisição é recebida o evento [`requisição`](https://nodejs.org/api/http.html#http_event_request)  é chamado, fornecendo dois objetos: uma requisição (um objeto [http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage)) e uma resposta (um [http.ServerResponseobject](https://nodejs.org/api/http.html#http_class_http_serverresponse)).

O `request` fornece os detalhes da requisição. Por meio dele, acessamos os cabeçalhos e os dados da requisição.

O `response` é usado para preencher os dados que retornaremos ao cliente.

Neste caso com:

`res.statusCode = 200`

definimos a propriedade statusCode como 200, para indicar uma resposta bem sucedida.

Também definimos o cabeçalho Content-Type:

`res.setHeader('Content-Type', 'text/plain')`

e finalizamos a resposta, adicionando o conteúdo como argumento a `end()`:

`res.end('Hello World\n')`

## <a name="http-requests"></a> Fazendo requisições HTTP

**Como executar requisições HTTP com Node.js usando GET, POST, PUT e DELETE**

>Eu uso o termo HTTP, mas HTTPS é o que deve ser usado em todos os lugares, portanto, esses exemplos usam HTTPS em vez de HTTP.

### Fazer uma requisição GET

```
const https = require('https')

const options = {
  hostname: 'flaviocopes.com',  
  port: 443,  
  path: '/todos',  
  method: 'GET'
}

const req = https.request(options, (res) => {
  console.log(`statusCode: ${res.statusCode}`)  
  
  res.on('data', (d) => {    
    process.stdout.write(d)  
  })
})

req.on('error', (error) => {
  console.error(error)
})

req.end()
```

### Fazer uma requisição POST

```
const https = require('https')

const data = JSON.stringify({  
  todo: 'Buy the milk'
})

const options = {  
  hostname: 'flaviocopes.com',  
  port: 443,  
  path: '/todos',  
  method: 'POST',  
  headers: {
    'Content-Type': 'application/json',
    'Content-Length': data.length  
  }
}

const req = https.request(options, (res) => {
  console.log(`statusCode: ${res.statusCode}`)  
  
  res.on('data', (d) => {    
    process.stdout.write(d)  
  })
})

req.on('error', (error) => {
  console.error(error)
})

req.write(data)
req.end()
```

### PUT e DELETE

Requisições PUT e DELETE usam o mesmo formato da requisição POST e apenas alteram o valor `options.method`.

## <a name="axios"></a> Axios

**O Axios é uma biblioteca JavaScript muito conveniente para executar requisições HTTP no Node.js**

- [Introdução](introduction-axios)
- [Instalação](installation-axios)
- [A API do Axios](axios-api)
- [Requisições GET](axios-get)
- [Adicionar parâmetros as requisições GET](axios-parameters)
- [Requisições POST](axios-post)

### <a name="introduction-axios"></a> Introdução

O Axios é uma biblioteca JavaScript muito popular que você pode usar para executar requisições HTTP, que funciona nas duas plataformas Browser e [Node.js](https://flaviocopes.com/nodejs/).

![Axios](/images/Capitulo-7/axios.png)

Ele suporta todos os navegadores modernos, incluindo o suporte para o IE8 e superior.

É baseado em `Promises`, isso permite escrever código async/await para executar requisições [XHR](https://flaviocopes.com/xhr/) com muita facilidade.

O uso do Axios possui algumas vantagens sobre o [Fetch API](https://flaviocopes.com/fetch-api/) nativo:

- suporta navegadores mais antigos (Fetch precisa de um polyfill)
- tem uma maneira de abortar uma solicitação
- tem uma maneira de definir um tempo limite de resposta 
- possui proteção CSRF integrada
- suporta o progresso do upload
- executa transformação automática de dados JSON 
- funciona no Node.js

### <a name="installation-axios"></a> Instalação

Axios pode ser instalado usando [npm](https://flaviocopes.com/npm/):

`npm install axios`

ou [yarn](https://flaviocopes.com/yarn/):

`yarn add axios`

ou simplesmente adicionando na sua página usando unpkg.com:

`<scriptsrc="https://unpkg.com/axios/dist/axios.min.js"></script>`

### <a name="axios-api"></a> A API do Axios

Você pode iniciar uma requisição HTTP a partir do objeto `axios`:

```
axios({  
  url: 'https://dog.ceo/api/breeds/list/all',  
  method: 'get',  
  data: {    
    foo: 'bar'  
  }
})
```

mas por conveniência, você geralmente usará:

- `axios.get()`
- `axios.post()`

(como no jQuery, você usaria `$.get()` e `$.post()` em vez de `$.ajax()`)

O Axios oferece métodos para todos os verbos HTTP, que apesar de menos populares, ainda são usados:

- `axios.delete()`
- `axios.put()`
- `axios.patch()`
- `axios.options()`

e um método para obter os cabeçalhos HTTP de uma requisição, descartando o corpo:

- `axios.head()`

### <a name="axios-get"></a> Requisições GET

Uma maneira conveniente de usar o Axios é usar a sintaxe async/await moderna (ES2017).

Esta é um exemplo de consulta em Node.js a [Dog API](https://dog.ceo/) para recuperar uma lista de todas as raças de cães usando `axios.get()`, contando as raças.

```
const axios = require('axios')

const getBreeds = async () => {
  try {
    returnawait axios.get('https://dog.ceo/api/breeds/list/all')  
  } catch (error) {
    console.error(error)  
  }
}

const countBreeds = async () => {
  const breeds = await getBreeds()
  
  if (breeds.data.message) {
    console.log(`Got ${Object.entries(breeds.data.message).length} breeds`)  
  }
}

countBreeds()
```

Se você não quiser usar async/await, você pode usar a sintaxe de [Promises](https://flaviocopes.com/javascript-promises/):

```
const axios = require('axios')

const getBreeds = () => {
  try {
    return axios.get('https://dog.ceo/api/breeds/list/all')  
  } catch (error) {
    console.error(error)  
  }
}

const countBreeds = async () => {
  const breeds = getBreeds()
  .then(response => {
    if (response.data.message) {
      console.log(
        `Got ${Object.entries(response.data.message).length} breeds`        
      )      
    }    
  })    
  .catch(error => {
    console.log(error)    
  })
}

countBreeds()

```

### <a name="axios-parameters"></a> Adicionar parâmetros as requisições GET

Uma requisição GET pode conter parâmetros na URL, assim: `https://site.com/?foo=bar`.

Com o Axios, você pode fazer isso simplesmente usando esse URL:

`axios.get('https://site.com/?foo=bar')`

ou você pode usar uma propriedade `params` nas opções:

```
axios.get('https://site.com/', {  
  params: {    
    foo: 'bar'  
  }
})
```

### <a name="axios-post"></a> Requisições POST

Executar uma requisição POST é como fazer uma requisição GET, mas em vez de `axios.get`, você usa `axios.post`:

`axios.post('https://site.com/')`

Um objeto contendo os parâmetros POST e um segundo argumento:

```
axios.post('https://site.com/', {  
  foo: 'bar'
})
```

## <a name="websockets"></a> Websockets

**Os WebSockets são uma alternativa à comunicação HTTP em aplicações Web. Eles oferecem um canal de comunicação bidirecional de longa duração entre cliente e servidor**

WebSockets são uma alternativa a comunicação HTTP em aplicações Web.

Eles oferecem um canal de comunicação bidirecional de longa duração entre cliente e servidor.

Uma vez estabelecido, o canal é mantido aberto, oferecendo uma conexão muito rápida com baixa latência e sobrecarga.

### Suporte do navegador para WebSockets

Os WebSockets são suportados por todos os navegadores modernos.

![Browser Support](/images/Capitulo-7/browser-support.png)

### Como os WebSockets diferem do HTTP

O HTTP é um protocolo muito diferente e uma maneira diferente de se comunicar.

Ele é um protocolo de requisição/resposta: o servidor retorna alguns dados quando o cliente solicita.

Diferenças do WebSocket para o HTTP: 

- o servidor pode enviar uma mensagem sem que o cliente solicite algo explicitamente
- o cliente e o servidor podem conversar simultaneamente
- pouquíssima sobrecarga de dados precisa ser trocado para enviar mensagens. Isso significa uma comunicação de baixa latência.

Os WebSockets são ótimos para comunicações em tempo real e de longa duração.

O HTTP é ótimo para trocas ocasionais de dados e interações iniciadas pelo cliente.

O HTTP é muito mais simples de implementar, enquanto WebSockets exigem um pouco mais de estrutura.

### WebSockets seguros

Sempre use o protocolo seguro e criptografado para WebSockets, `wss://`.

`ws://` refere-se à versão insegura do WebSockets (o `http://` do WebSockets) e deve ser desviada por razões óbvias.

### Criar uma nova conexão WebSockets

```
const url = 'wss://myserver.com/something'
const connection = new WebSocket(url)
```

`connection` é um objeto [WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket).

Quando a conexão é estabelecida com sucesso, o evento `open` é acionado.

Para ouvi-lo é necessário atribuir uma função callback à propriedade `onopen` do objeto ` connection`:

```
connection.onopen = () => {
  //...  
}
```
Se houver algum erro, a função callback `onerror` é acionada:

```
connection.onerror = error => {
  console.log(`WebSocket error: ${error}`)
}
```

### Enviando dados para o servidor usando WebSockets

Quando a conexão estiver aberta, você poderá enviar dados para o servidor.

Você pode fazer isso convenientemente dentro da função callback `onopen`:

```
connection.onopen = () => {  
  connection.send('hey')
}
```

### Recebendo dados do servidor usando WebSockets

Ouça com uma função callback `onmessage`, que é chamado quando o evento da mensagem é recebido:

```
connection.onmessage = e => {
  console.log(e.data)
}
```

### Implementar um servidor WebSockes no Node.js

[ws](https://github.com/websockets/ws) é uma biblioteca popular de WebSockets para [Node.js](https://flaviocopes.com/nodejs).

Vamos usá-lo para criar um servidor WebSockets. Também pode ser usado para implementar um cliente e usar WebSockets para se comunicar entre dois serviços de back-end.

Instale-o facilmente usando:

```
yarn init
yarn add ws
```

O código que você precisa escrever é muito pequeno:

```
const WebSocket = require('ws')

const wss = new WebSocket.Server({ port: 8080 })

wss.on('connection', ws => {  
  ws.on('message', message => {
    console.log(`Received message => ${message}`)  
  })    
  ws.send('ho!')
})
```

Este código cria um novo servidor na porta 8080 (a porta padrão do WebSockets) e adiciona uma função callback quando uma conexão é estabelecida, enviando `ho!` para o cliente e registrando as mensagens que recebe.

### Veja um exemplo ao vivo no Glitch

Aqui está um exemplo ao vivo de um servidor WebSockets: [https://glitch.com/edit/#!/flavio-websockets-server-example](https://glitch.com/edit/#!/flavio-websockets-server-example)

Aqui está um cliente WebSockets que interage com o servidor: [https://glitch.com/edit/#!/flavio-websockets-client-example](https://glitch.com/edit/#!/flavio-websockets-client-example])

## <a name="secure-connections"></a> HTTPS, conexões seguras

**O protocolo HTTPS é uma extensão do HTTP, o Protocolo de Transferência de Hypertexto, que fornece uma comunicação segura**

O HTTP é inseguro por design.

Quando você abre o navegador e solicita que um servidor envie uma página da Web, seus dados realizam duas viagens: uma do navegador para o servidor da Web e a outra do servidor da Web para o navegador. 

Então, dependendo do conteúdo da página da Web, você pode precisar de mais conexões necessárias para obter os arquivos CSS, JavaScript, imagens e assim por diante.

Durante qualquer uma dessas conexões, qualquer rede que seus dados estão passando podem ser **inspecionadas** e **manipuladas**.

As consequências podem ser graves: você pode ter todas as suas atividades de rede monitoradas e registradas. Em uma terceira parte, você nem sabe que elas existem, algumas redes [podem injetar anúncios](https://justinsomnia.org/2012/04/hotel-wifi-javascript-injection/) e você pode estar sujeito a um ataque intermediário, uma ameaça à segurança na qual o invasor pode manipular seus dados e até mesmo se passar por seu computador na rede. É muito fácil alguém ouvir apenas pacotes HTTP sendo transmitidos por uma rede Wi-Fi pública e não criptografada.

O HTTPS visa solucionar o problema na raiz: toda a comunicação entre o navegador e o servidor da Web é criptografada.

Privacidade e segurança são uma das principais preocupações da Internet de hoje. Alguns anos atrás, você poderia fugir usando apenas uma conexão criptografada em páginas protegidas, ou durante um checkout de um E-Commerce. Pelo alto custo e dificuldades do certificado SSL, a maioria dos sites apenas usava HTTP.

Hoje, o HTTPS é um requisito em qualquer site. Mais de 50% de toda a Web o usa agora. O Google Chrome começou recentemente a marcar sites HTTP como inseguros, apenas para fornecer uma razão válida para que o HTTPS seja obrigatório (e forçado) em todos os seus sites.

Ao usar HTTP, a porta padrão do servidor é 80, no HTTPS é 443. Não é necessário adicionar de forma explicita se o servidor usa a porta padrão, é claro.

Às vezes, o HTTPS também é chamado de HTTP com SSL ou HTTP com TLS.

A diferença entre os dois é simples: TLS é o sucessor do SSL.

Ao usar HTTPS, as únicas coisas que não são criptografadas são o domínio do servidor da web e a porta do servidor.

Todas as outras informações, incluindo o caminho do recurso, cabeçalhos, cookies e parâmetros de consulta, são criptografadas.

Não analisarei em detalhes como o protocolo TLS funciona por baixo dos panos, mas se você pensar que está sendo colocado uma boa quantidade adicional de dados, você estaria certo.

Qualquer cálculo adicionado ao processamento de recursos de rede causa sobrecarga no cliente, no servidor e no tamanho dos pacotes transmitidos.

Entretanto, o HTTPS permite o uso do mais novo protocolo **HTTP/2**, que possui uma vantagem enorme sobre o HTTP/1.1: é muito mais rápido.

Por quê? Há muitas razões, uma é a compactação de cabeçalho, a outra é a multiplexação de recursos. Um deles é o envio pelo servidor: o servidor pode enviar mais recursos quando um recurso é solicitado. Portanto, se o navegador solicita uma página, ele também receberá todos os recursos necessários (imagens, CSS, JS). 

Detalhes à parte, o HTTP/2 é uma grande melhoria em relação ao HTTP/1.1 **e requer HTTPS**. Isso significa que o HTTPS, apesar de ter a sobrecarga de criptografia, é muito mais rápido que o HTTP, caso as coisas estejam configuradas corretamente e com uma instalação moderna.

