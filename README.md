# Barber Shop

A demonstração está disponível [aqui] (https://barber-shop-53333.web.app/)

Credenciais de administrador:
- username: demo@demo.com
- password: demo2020

Sistema de reservas feito com ecossistema Vue + Firebase (funções de nuvem, auth, firestore, hosting). Este exemplo é inspirado nos requisitos de uma barbearia, mas é aplicável em muitos cenários.

O usuário pode fazer uma reserva obtendo um código. Este código pode ser usado para cancelar a reserva.

Você pode implantar um sistema de reserva sem um servidor, mas tenha em mente que isso é apenas um playground.

Comentários e contribuições serão apreciados. Estou fazendo isso apenas para me divertir.

## Configuração do projeto

Clone este repositório
```sh
$ git clone https://github.com/ibonkonesa/barber-shop.git
```

You must create a new Firebase project. Cloud Firestore location must be set to us-central (default). If you choose other location, you must update src/store/user/location variable. Go to database section and create a new cloud firestore database. 


Este repo tem duas pernas: funções de nuvem (atua como um servidor de back-end, fornecendo autenticação e acionamento quando as reservas são escritas) e um Vue.JS SPA que permite aos usuários fazer e verificar as reservas.

O código de funções da nuvem está localizado na pasta de funções da nuvem. Na raiz desta pasta, há um arquivo que você deve editar.

Renomeie .firebaserc.example para .firebaserc e atualize o valor de projects.default com seu ID de projeto do Firebase. Você pode vê-lo na configuração do projeto no console do Firebase.

A pasta pública é um link simbólico para a pasta dist. Esta pasta ("pública") será publicada quando você implantar seu projeto Firebase (este recurso é chamado de "Hospedagem". Quando você cria um projeto Firebase você obtém algum espaço de hospedagem. Você pode vincular seu domínio a esta hospedagem. Mais tarde eu explicarei isso melhorar.).

A pasta de funções contém dois arquivos importantes:

-index: aqui é onde está o código do servidor. Você está livre para atualizar / melhorar este código. Basicamente, existem 3 funções:

  * createBooking. É um gatilho lançado quando uma reserva é criada no banco de dados. Esta função notificará se você configurar o envio de correio.
  * createToken. Esta é uma função http. Ele retorna um token usado pelo webapp para modificar uma reserva
  
-serviceAccountKey.json: Se você clonar este repo, este arquivo não deve existir. É o arquivo de configuração do lado do servidor (lembre-se, a função de nuvem atua como um servidor).
Você precisa criar uma nova conta de serviço no Firebase console. Vá para Configurações> Conta de serviço e gere uma nova conta privada. Coloque o arquivo gerado em cloud-functions / functions / serviceAccountKey.json

Depois de configurar as funções de nuvem, você deve configurar o aplicativo front end. O projeto Vue.JS está localizado na pasta src. Existe um arquivo chamado config / firebase.js.example. Renomeie este arquivo para firebase.js e atualize a variável de configuração com os dados fornecidos adicionando um novo aplicativo da web no console do projeto Firebase. Basta atualizar a variável de configuração.

A programação e a configuração do aplicativo (título, descrição) são definidas na pasta config. Nomes de constantes são autodescritivos;)

Os usuários administradores devem ser criados no Firebase console e devem ser criados usando o método de assinatura de e-mail / senha. Todos os usuários criados com este método estarão habilitados para esta área.

Agora você pode implantar este projeto.

## Enviando

Você pode configurar o envio automático de e-mail quando um cliente faz uma reserva.

Basta renomear /cloud-functions/functions/mailing.example
para /cloud-functions/functions/mailing.js e colocar as credenciais do remetente do gmail e o e-mail do destinatário.
Lembre-se de ativar os "aplicativos menos seguros do Gmail" (https://myaccount.google.com/lesssecureapps).
Se você estiver usando 2FA no e-mail do remetente, terá que criar uma senha específica do aplicativo para fazer o mailer funcionar. Este [link] (https://community.nodemailer.com/using-gmail/) pode ser útil.

## Desenvolvimento

### Com Docker

Basta aplicar o arquivo docker-compose incluído e ir para http: // localhost: 8080

```sh
$ docker-compose up
```

### Localmente

Npm:

```sh
$ npm install
$ npm run serve
```

Fio:

```sh
$ yarn install
$ yarn serve
```

O servidor hot-reload estará disponível em http://localhost: 8080

## Implantar aplicativo

Depois de configurar o projeto, você pode implantar o projeto.

### Aplicativo

Este é um projeto baseado em vue-cli. Instale os pacotes npm:

```sh
$ npm install
```

Inicie o comando de compilação:

```sh
$ npm run build --prod
```



### Funções de nuvem e hospedagem

As funções da nuvem (triggers e http) devem ser publicadas. Você precisará do pacote firebase-tools. Instale-o:

```sh
$ npm install -g firebase-tools
```

Entre na pasta de funções da nuvem:

```sh
$ cd cloud-functions
```


Insira a pasta de funções e instale os pacotes npm:

```sh
funções $ cd
$ npm install
$ cd ..
```

O aplicativo de frontend será criado na pasta / dist. Verifique se há um link simbólico em cloud-functions / public apontando para a pasta / dist. Se este link não existir, execute:

```sh
$ ln -s ../dist public
```


Faça login no firebase. A credencial do console do Firebase (Google) será necessária:

```sh
$ firebase login
```

Depois de fazer login, você pode implantar funções e hospedagem:

```sh
$ firebase deploy
```

Um url será impresso no terminal e o sistema de reservas estará acessível. Você pode vincular um domínio personalizado no console do Firebase.

## Pendência

- Aplicativo móvel (Quasar? Ionic?)
- Refatorar armazenamentos com assíncrono / aguardar

   
## CONTRIBUIÇÃO

Comentários e melhorias serão apreciados.

## LICENÇA

Este repo pode ser clonado, bifurcado, melhorado sem referência. Se você gostar e quiser que eu continue desenvolvendo, pode me dar uma estrela neste repositório :)
