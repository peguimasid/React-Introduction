# Introducao ao React

## Aula 1 - Conceitos do React 

1. É uma biblioteca para contruçāo de interfaces
2. Utilizado para construcao de ***Single-Page Applications*** (SPA)
3. Tudo fica dentro do JavaScript (CSS, HTML, Imagens)
4. ***React***: Biblioteca de contrucao de interfaces React
5. ***ReactJS***: React no Browser (Web)
6. ***React Native***: React em Mobile

### Vantagens

1. Organizacao do Código que se da por ***Componentizaçāo***.
2. Divisāo de Responsabilidades, ou seja, o ***Backend*** só fica responsalvel pelas regras de negocio, e o ***Frontend*** pela interface.
3. O ***Mobile*** e o ***Web*** podem usar o mesmo ***Backend*** (API REST).
4. Programaçāo Declarativa

### Babel/Webpack

1. O browser nāo entende todo esse código que passamos no React.
2. O ***Babel*** pega esse código JS e converte de uma forma que o browser entenda.
### O Webpack possui varias funções:
  1. Criaçao do bundle, que é um arquivo com todo códifo da nossa aplicação.
  2. Ensinar ao JavaScript como importar arquivos CSS, Imagens e etc.
  3. Live reload com Webpack Dev Server.

## Aula 2 - Configurando estrutura

1. Criamos uma pasta para o projeto.
2. Rodamos `yarn init -y`

 Vamos agora instalar algumas bibliotecas que vamos utilizar no projeto:

1. `yarn add @babel/core @babel/preset-env @babel/preset-react webpack webpack-cli -D`
2. `yarn add react react-dom`

Criamos um arquivo `babel.config.js`:

Dentro de `babel.config.js`: 

```
module.exports = {
  presets: [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}
```

Depois criamos um arquivo `webpack.config.js`:

Dentro de `webpack.config.js`:

```
const path = require('path');

module.exports = {
  entry: path.resolve(__dirname, 'src', 'index.js'),
  output: {
    path: path.resolve(__dirname, 'public'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      }
    ]
  }
}
```

Rodamos `yarn add babel-loader -D`

Criamos uma pasta `src` na raiz e dentro dessa pasta um arquivo `index.js`.

`index.js`:

```
const soma = (a, b) => a + b;

alert(soma(1, 3));
```

Criamos uma pasta `public` na raiz do projeto.

Dentro de `package.json` passamos:

```
"scripts": {
    "build": "webpack --mode development"
  },
```

Depois disso vamos no terminal e rodamos `yarn build` e se deu tudo certo vai ter criado automaticamente um arquivo chamado `bundle.js` dentro da pasta `public`.

Agora dentro da pasta `public` criamos um arquivo `index.html`:

Dentro de `index.html`:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ReactJS</title>
</head>
<body>
  <h1>Hello World</h1>

  <script src="./bundle.js"></script>
</body>
</html>
```
Agora se abrirmos o arquivo `index.html` no navegador ele mostrara um alerta.

Rodamos no terminal `yarn add webpack-dev-server -D`

depois vamos no `webpack.config.js` e adicionamos:

```
devServer: {
    contentBase: path.resolve(__dirname, 'public')
  },
```

e vamos no `package.json` e adicionamos dentro dos `scripts`:

`"dev": "webpack-dev-server --mode development"`

e o outro `script` ficara assim para quando formos rodar online nossa aplicacao: 

`"build": "webpack --mode production",`

e se rodarmos agora `yarn dev` e entrarmos em `localhost:8080` nosso projeto estara rodando la e nos retornara nosso alerta e qualquer atualizacao que fizermos no arquivo ele atualizara automaticamente sem precisarmos reiniciar o servidor.

## Aula 3 - Criando componente raiz

O que vamos fazer agora é finalmente comecar a utilizar o ***React*** na nossa aplicaçāo.

Vamos em `src > index.js`, excluimos tudo que tava la e adicionamos as configurações:

```
import React from 'react';
import { render } from 'react-dom';

render(<h1>Hello World</h1>, document.getElementById('app'));
```

e no `public > index.html` substituimos:

`<h1>Hello World</h1>`

por 

`<div id='app'></div>`

Ai se formos e rodarmos a aplicacao com `yarn dev` nosso `Hello World` estara la do mesmo jeito pois agora utilizamos o ***React*** para renderizar nosso texto.

Agora dentro de `src` criamos um arquivo chamado `App.js` que sera nosso primeiro componente da aplicacao, o chamado ***"Componente Raiz"***.

Dentro de `App.js`:

```
import React from 'react';

function App() {
  return <h1>Hello World</h1>
}

export default App;
```

Vamos no `index.js`:

```
...
import App from './App';
...
render(<App />, document.getElementById('app'));
```

Se rodarmos nossa aplicaçāo agora veremos nosso `Hello World` la, dessa vez renderizado utilizando um componente do ***React***.

