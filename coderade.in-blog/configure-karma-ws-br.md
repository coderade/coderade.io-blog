https://web.archive.org/web/20151127145620/http://br.coderade.in/configurando-karma-webstorm/]

Como configurar o Karma na IDE WebStorm
WebStorm é uma IDE da  empresa JetBrains  para desenvolvimento client-side e server-side com NodeJS.

Eu usei por algum tempo o editor Sublime Text (e continuo usando) para desenvolvimento web, mas após eu começar usar o WebStorm, e outras IDEs da JetBrains (atualmente eu uso PyCharm para desenvolvimento com Python, Intellij IDEA para Java, e a adorável WebStorm para Web) eu realmente não consigo voltar para IDEs como Eclipse, Netbeans(ew!) e outras muito usadas antigamente e atualmente.


Esse artigo é um curto guia de como configurar o  Karma na IDE WebStorm (porém, este guia deve funcionar nas IDEs Intellij IDEA, PyCharm, PhpStorm, RubyMine e outras IDEs da JetBrains).

Na versão 7 do WebStorm,  a  JetBrains adiciounou o suporte para o Karma – uma ferramenta simples e flexível para executar testes usando JavaScript, desenvolvido pelo grupo que também desenvolveu o AngularJS.

Primeiro, se você não possuí a IDE WebStorm instalada, você  pode baixar a versão para o seu S.O, nesse link: Download WebStorm.

Depois de instalar o IDE WebStorm, você precisa instalar o Karma. Karma é executada no Node.js e é disponível pelo pacote NPM. Então será necessário instalar o Node.js antes para executá-lo. Para instalar o NodeJs, de acordo com a documentação do Karma nos sistemas Mac ou Linux é recomendável usar o NVM. No Windows, deve-se  baixar o  Node.js do seu  site oficial.

Após instalar o NodeJS e  gerenciador de pacotes NPM, você  pode agora  instalar o Karma.

 Para instalar o Karma para executar  Testes unitários com Jasmine:
Para instalar Karma, você pode abrir o seu terminal favorito e  digitar no diretório do seu projeto:

Installing Karma
Shell
npm install karma
1
npm install karma
ou para instalar em um modo global (que possa ser usado em qualquer lugar) você pode digitar:

Install karma globally
Shell
npm install karma -g
1
npm install karma -g
Para executar os testes do Karma dentro do navegador Google Chrome você precisa instalar o módulo: (usando -g para instalar globalmente)

Install Karma Chrome launcher
Shell
npm install karma-chrome-launcher
1
npm install karma-chrome-launcher
e finalmente para permitir o Karma executar testes unitários usando  o framework Jasmine você precisa digitar:

Install Karma Jamine
Shell
npm install karma-jasmine
1
npm install karma-jasmine
Digitar o  comando  ./node_modules/karma/bin/karma start  todas vez que você quiser o Karma pode irritar um pouco e então você pode achar usual instalar o karma-cli em modo global.

Install Karma command line interface
Shell
npm install -g karma-cli
1
npm install -g karma-cli
E então, quando você quiser executar o Karma  você pode digitar simplesmente o comando  karma em qualquer local e ele sempre executará a versão local.

Após a instalação do Karma você precisará criar o arquivo de configuração do Karma para o seu projeto. Nesse arquivo você deverá colocar as configurações relacionadas ao seu projeto, com esse arquivo o Karma poderá executar os seus testes.

Para criar esse arquivo você precisa executar esse comando no diretório do seu projeto:

Shell
karma init
1
karma init
Karma irá fazer algumas perguntas relacionadas ao seu projeto, se você não sabe o quer responder, não se preocupe e use as respostas padrões.

Esse comando irá criar um arquivo parecido com o abaixo:

Karma configuration file
JavaScript
// Karma configuration
// Generated on Sun Oct 11 2015 20:56:38 GMT-0300 (BRT)

module.exports = function(config) {
  config.set({

    // base path that will be used to resolve all patterns (eg. files, exclude)
    basePath: '',


    // frameworks to use
    // available frameworks: https://npmjs.org/browse/keyword/karma-adapter
    frameworks: ['jasmine'],


    // list of files / patterns to load in the browser
    files: [
      '**/*.js'
    ],


    // list of files to exclude
    exclude: [
    ],


    // preprocess matching files before serving them to the browser
    // available preprocessors: https://npmjs.org/browse/keyword/karma-preprocessor
    preprocessors: {
    },


    // test results reporter to use
    // possible values: 'dots', 'progress'
    // available reporters: https://npmjs.org/browse/keyword/karma-reporter
    reporters: ['progress'],


    // web server port
    port: 9876,


    // enable / disable colors in the output (reporters and logs)
    colors: true,Remembering to change the "<path>" to the path of your Karma configuration file.


    // level of logging
    // possible values: config.LOG_DISABLE || config.LOG_ERROR || config.LOG_WARN || config.LOG_INFO || config.LOG_DEBUGte: MultiMarkdown
    logLevel: config.LOG_INFO,


    // enable / disable watching file and executing tests whenever any file changes
    autoWatch: true,


    // start these browsers
    // available browser launchers: https://npmjs.org/browse/keyword/karma-launcher
    browsers: ['Chrome'],


    // Continuous Integration mode
    // if true, Karma captures browsers, runs the tests and exits
    singleRun: false
  })
}
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
// Karma configuration
// Generated on Sun Oct 11 2015 20:56:38 GMT-0300 (BRT)
 
module.exports = function(config) {
  config.set({
 
    // base path that will be used to resolve all patterns (eg. files, exclude)
    basePath: '',
 
 
    // frameworks to use
    // available frameworks: https://npmjs.org/browse/keyword/karma-adapter
    frameworks: ['jasmine'],
 
 
    // list of files / patterns to load in the browser
    files: [
      '**/*.js'
    ],
 
 
    // list of files to exclude
    exclude: [
    ],
 
 
    // preprocess matching files before serving them to the browser
    // available preprocessors: https://npmjs.org/browse/keyword/karma-preprocessor
    preprocessors: {
    },
 
 
    // test results reporter to use
    // possible values: 'dots', 'progress'
    // available reporters: https://npmjs.org/browse/keyword/karma-reporter
    reporters: ['progress'],
 
 
    // web server port
    port: 9876,
 
 
    // enable / disable colors in the output (reporters and logs)
    colors: true,Remembering to change the "<path>" to the path of your Karma configuration file.
 
 
    // level of logging
    // possible values: config.LOG_DISABLE || config.LOG_ERROR || config.LOG_WARN || config.LOG_INFO || config.LOG_DEBUGte: MultiMarkdown
    logLevel: config.LOG_INFO,
 
 
    // enable / disable watching file and executing tests whenever any file changes
    autoWatch: true,
 
 
    // start these browsers
    // available browser launchers: https://npmjs.org/browse/keyword/karma-launcher
    browsers: ['Chrome'],
 
 
    // Continuous Integration mode
    // if true, Karma captures browsers, runs the tests and exits
    singleRun: false
  })
}
Após isso o Karma já estará preparado para executar os seus testes. uau!

Se você não necessita configurar o Karma na IDE WebStorm  e simplesmente executar seus testes você pode digitar:

Shell
karma start <path>/karma.conf.js
1
karma start <path>/karma.conf.js
Lembrando de alterar o valor “<path>” para o diretório do seu arquivo de configuração do Karma.

Agora você pode configurar a o Karma na sua IDE WebStorm.

Para fazer isso, com o Karma executando (of course!) clique na opção Run, e depois Edit Configurations.

output_dSMTsM

Isso irá abrir o menu Run/Debug Configurations.

Para criar o executador do seus testes do Karma, clique na opção Add new configuration  (Alt + insert | Botão com o símbolo de MAIS) e selecionar o Karma.

output_kfWo5x

Essa janela será aberta:
Screenshot from 2015-11-16 13:07:08

Então você poderá criar a configuração do Karma, preenchendo as seguintes opções:

Name: nome da sua configuração;
Configuration file:  caminho do seu arquivo de configuração do Karma;
Browsers to start: normalmente, essa opção deve ser feita no seu arquivo de configuração do Karma, mas você pode mudá-la aqui  para que ela seja sobrescrita.
Node interpreter: esta opção  será preenchida por padrão pela IDE, mas se ele não preencher ou se você quiser alterar esta opção , você deve passar o caminho do diretório do NodeJS.
Karma package: essa opção também será preenchida por padrão pela WebStorm, mas você pode mudar para o diretório do módulo do Karma.

Depois de mudar essas configurações você pode salvá-las.

Agora,  você pode executar os seus testes selecionando na opção Select Run/Debug Configuration a configuração do Karma que você acabou de criar.

runing_tests

Nos próximos artigos, Eu irei mostrar como criar e executar Teste unitários e  testes e2e (End-to-End) usando esse stack de ferramentas e a linguagem Javascript.

