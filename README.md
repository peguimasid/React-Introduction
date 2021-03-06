# Introducao ao React

## Aula 1 - Conceitos do React 

1. É uma biblioteca para contruçāo de interfaces
2. Utilizado para construcao de ***Single-Page Applications*** (SPA)
3. Tudo fica dentro do JavaScript (CSS, HTML, Imagens)
4. ***React***: Biblioteca de contruçāo de interfaces React
5. ***ReactJS***: React no Browser (Web)
6. ***React Native***: React em Mobile

### Vantagens

1. Organizaçāo do Código que se da por ***Componentizaçāo***.
2. Divisāo de Responsabilidades, ou seja, o ***Backend*** só fica responsavel pelas regras de negócio, e o ***Frontend*** pela interface.
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

Ai se formos e rodarmos a aplicaçāo com `yarn dev` nosso `Hello World` estara la do mesmo jeito pois agora utilizamos o ***React*** para renderizar nosso texto.

Agora dentro de `src` criamos um arquivo chamado `App.js` que sera nosso primeiro componente da aplicaçāo, o chamado ***"Componente Raiz"***.

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


## Aula 4 - Importando CSS

Agora vamos colocar um pouco de ***CSS*** na nossa aplicaçāo e para isso vamos adicionar mais 2 loaders.

`yarn add style-loader css-loader -D`

Agora vamo em `webpack.config.js` e dentro de `rules` adicionamos essa nova:

```
{
  test: /\.css$/,
  use: [
    { loader: 'style-loader' },
    { loader: 'css-loader' }
  ]
}
```

Agora dentro de `src` criamos um arquivo `App.css` e adicionamos um estilo lá dentro, depois vamos em `src > App.js` e adicionamos:

`import './App.css'`

se iniciarmos o servidor agora veremos nosso estilo aplicado na página.

## Aula 5 - Importando imagens

Vamos importar agora um arquivo de imagem e para isso precisamos de mais um loader.

`yarn add file-loader -D`

Agora vamo em `webpack.config.js` e adicionamo mais uma `rule`:

```
{
  test: /.*\.(gif|png|jpe?g)$/i,
  use: {
    loader: 'file-loader'
  }
}
```

Agora dentro de `src` criamos uma pasta `assets` para colocarmos nossas imagens e la dentro colocamos uma imagem qualquer.

Depois vamos em `src > App.js`, importamos a imagem:

```
import <nome que vai dar pra imagem> from './assets/<nome da imagem>'
```
***Exemplo***: `import image from './assets/mobile.png';`

e a `function App()` vai ficar assim:

```
function App() {
  return <img src={<nome que deu pra imagem>} />
}
```
***Exemplo***: `return <img src={image} />`

se iniciarmos o servidor veremos nossa imagem lá.

## Aula 6 - Class Components

Podemos escrever um componente através de uma `function()` ou também através de uma ***Class*** que é o que vamos fazer:

primeiro vamos dentro de `src` e criamos uma pasta chamada `components` e dentro dela criamos um arquivo chamado `TechList.js` onde colocaremos um exemplo de lista.

Dentro de `TechList.js`:

```
import React, { Component } from 'react'

class List extends Component {
  render() {
    return (
      <ul>
        <li>Node.js</li>
        <li>ReactJS</li>
        <li>React Native</li>
      </ul>
    )
  }
}

export default TechList;
```

Agora vamos em `src > App.js` e importamos a `TechList`

`import TechList from './components/TechList'`

e a nossa `function App()` ficará assim:

```
function App() {
  return <TechList />
}
```

se formos agora no navegador veremos nossa `TechList` lá.

Podemos passar a `TechList` assim tambem:

```
import React, { Component } from 'react'

class TechList extends Component {
  state = {
    techs: [
      'Node.js',
      'ReactJS',
      'React Native'
    ]
  }

  render() {
    return (
      <ul>
        <li>Node.js</li>
        <li>ReactJS</li>
        <li>React Native</li>
      </ul>
    )
  }
}

export default TechList;
```

Mas pra isso precisamos intalar um outro plugin:

`yarn add @babel/plugin-proposal-class-properties`

E vamos depois dentro de `babel.config.js` e adicionamos:

```
plugins: [
    '@babel/plugins-proposal-class-properties'
  ]
```
Vamos aprender a listar os atributos passados dentro de `state` na ***Aula 7***

## Aula 7 - Estado & Imutabilidade

Para podermos referenciar o `state` que passamos na ultima aula temos que passar assim dentro da `<ul>`:

```
render() {
    return (
      <ul>
        {this.state.techs.map(tech => <li key={tech}>{tech}</li>)}
      </ul>
    )
  }
 ```
 Isso pega o estado e faz um `map` nele, ou seja, separa cada elemento da ***Array*** em uma variavel propria chamada `tech` e fazemos um `li` pra cada `tech`.

 Agora vamos fazer um `input` que adiciona itens na nossa lista, mas se simplesmente colocarmos o `input` abaixo da `ul` ele dara um erro pois temos que ter uma `div` ou uma fragment tag (`<> </>`) em volta de todo nosso codigo. Veja o codigo abaixo e veja a explicaçāo para cada passo tomado. ***(Tire os comentarios)***

 ```
class TechList extends Component {    
  state = {
    newTech: '',
    techs: [
      'Node.js',
      'ReactJS',
      'React Native'
    ]
  }

  handleInputChange = e => {
    this.setState({ newTech: e.target.value })  *ARMAZENA O VALOR DE newTech*
  }

  handleSubmit = e => {
    e.preventDefault();

       this.setState({  *DEFINE UM NOVO VALOR PRA UM ESTADO*

       techs: [ ...this.state.techs, this.state.newTech ], 
       *ESTADO QUE QUER ALTERAR (techs) E O NOVO VALOR*

       newTech: ''
       *REDEFININDO O VALOR DE newTech*    
    })
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <ul>
          {this.state.techs.map(tech => <li key={tech}>{tech}</li>)}
        </ul>
        <input
         type="text"
         onChange={this.handleInputChange}
         value={this.state.newTech}
        />
        <button type="submit">Enviar</button>
      </form>
    )
  }
}
 ```

 Vamos por passos, primeiro ali no metodo `render()` nos passamos uma `form` por volta de todo conteudo ***HTML*** mas podia ser uma `div` ou `<> </>` depois nos criamos um `input`que tem um metodo `handleInputChange` que armazena o que digitarmos dentro dele com dentro de `newTech` com o metodo `this.setState()` depois temos um metodo `handleSubmit()` que primeiro previne qualquer comportamento padrao que o `onSubmit()` possa ter com o `e.preventDefault()` e depois nos usamos o `this.setState()` e passamos o valor que queremos alterar que é `techs` passamos todas as `techs` que ja estavam na ***Array*** e adicionamos a nova `this.state.newTech` e passamos o valor do `input` para vazio novamente. Lembrando que quando estamos tentando alterar algo dentro do `state` temos que passaro `setState()` e a `function` como sendo uma ***Arrow Function*** `= e =>` pois se nao nao conseguiremos acessar o valor de `this` dentro da `function`.

 ## Aula 8 - Removendo itens do estado

Vamos agora remover algo de dentro da ***Array*** e para isso precisamos fazer o seguinte no nosso codigo, o render que estava assim:

```
render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <ul>
          {this.state.techs.map(tech => <li key={tech}>{tech}</li>)}
        </ul>
        <input
         type="text"
         onChange={this.handleInputChange}
         value={this.state.newTech}
        />
        <button type="submit">Enviar</button>
      </form>
    )
  }
```

vai ficar assim:

```
render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <ul>
          {this.state.techs.map(tech => (
          <li key={tech}>
            {tech}
           * <button 
           *   onClick={ () => this.handleDelete(tech)} 
           *   type="button">Remover</button>
            </li>
          ))}
        </ul>
        <input
         type="text"
         onChange={this.handleInputChange}
         value={this.state.newTech}
        />
        <button type="submit">Enviar</button>
      </form>
    )
  }
```

Depois disso criamos uma ***Function*** `handleDelete`:

```
handleDelete = (tech) => {
    this.setState({ techs: this.state.techs.filter(t => t !== tech)})
  }
```

O que estamos fazendo ali é adicionar um botao que chama a function `handleDelete` ao clicarmos ela passa uma `tech` pra essa function e a mesma filtra passando todas a `techs` novamente menos aquela que removemos.

## Aula 9 - Propriedades do React

Vamos substituir os `<li>` por um componente que vai renderizar eles:

Primeiro vamos criar dentro de `src > components` um novo arquivo `TechItem.js`

Dentro de `TechItem.js`:

```
import React from 'react'

function TechItem ({ tech, onDelete}) {
  return(
    <li>
       {tech}
    <button onClick={onDelete} type="button">Remover</button>
    </li>
  )
}

export default TechItem;
```

E a nossa `ul` do `Techlist` que estava assim:

```
...
import TechItem from './TechItem';
...
```

```
<ul>
   {this.state.techs.map(tech => (
   <li key={tech}>
     {tech}
     <button 
       onClick={ () => this.handleDelete(tech)} 
       type="button">Remover</button>
     </li>
   ))}
</ul>
```
Vai ficar assim:

```
<ul>
  {this.state.techs.map(tech => (
    <TechItem 
      key={tech} // Identificador
      tech={tech} // parametro que passamos na funçao map()
      onDelete={() => this.handleDelete(tech)} // metodo handleDelete
      /> 
    ))}
</ul>
```
Quando vamos criar um componente temos que passar os parametros e funções dele (como propriedades) para o outro componente reconhecer e fazemos isso dentro tag `<TechItem />`, observe que os parametros `tech` e `onDelete` passamos como parametros tambem dentro do arquivo `TechItem.js`, o `key` so passamos por ser obrigatorio para identificaçāo do elemento.

## Aula 10 - Default Props & PropTypes

***DefaultProps*** - Vamos supor que a gente coloque o componente `<TechItem />` e esqueca de passar a propriedade `tech` dentro dele, podemos ai usar um conceito chamdo `defaultProps` que quando nao passarmos um valor pra uma propriedade ela assume um valor padrao que a gente escolhe.

***Exemplo***:

Passamos dentro de `TechList` o `<TechItem />` e nao passamos a propriedade `tech` pra ele. 

Em `TechItem`:

```
TechItem.defaultProps = {
  tech: `Oculto` // QUANDO NAO INFORMARMOS O VALOR SERA `OCULTO`
}
```

Ai em `TechList` passamos o componente `TechItem` para ser renderizado mais uma vez so que dessa vez sem passarmos propriedades nenhuma:

`<TechItem />`

ai se olharmos no navegador estara la escrito `Oculto`.

***PropTypes*** - É uma forma de validar as propriedades que nosso componente recebe.

Vamos supor que entre um novo desenvolvedor no time e que na nossa funçāo `onDelte` do tech item `function TechItem ({ tech, onDelete}) {` ele passe um texto ao inves de uma funçāo, seria bom o ***React*** avisar para ele que aquilo deveria ser uma funçāo.

Para isso precisamos instalar uma biblioteca chamada ***PropTypes***:

`yarn add prop-types`

Depois importamos ela dentro de `TechItem.js`:

`import PropTypes from 'prop-types'`

E para usar fazemos assim por exemplo:

`TechItem.js`

```
TechItem.propTypes = {
  tech: PropTypes.string,
  onDelete: PropTypes.func.isRequired,
}
```
ai estamos dizendo que a propriedade `tech` é do tipo ***String*** e a `onDelete` é uma funcao e é obrigatoria, e se esquecermos de passar o valor ou passarmos o valor de uma forma errada ele vai avisar a gente no console.

## Aula 11 - Ciclo de vida do componente

Podemos entender o ciclo de vida do componente como o momento em que ele ***Entra na tela*** e o ciclo se encerra a partir do momento em que ele ***Sai da tela***, temos três metodos principais que vamos usar no inicio para o ciclo de vida, sendo eles:

```
// Executado assim que o componente aparece na tela
  componentDidMount() {

  }

  // Executado sempre que houver alterações nas props ou estado
  componentDidUpdate(prevProps, prevState) {
    // this.props, this.state
  }

  // Executado quando o componente deixa de existir
  componentWillUnmount() {

  }
```
***Exemplo*** - Vamos usar esses metodos agora para usar o `localStorage`, ou seja, armazenar os dados no banco de dados do navegador.

Para isso usaremos primeiro o metodo `componentDidUpdate`:

```
componentDidUpdate(_, prevState) {
    if(prevState.techs !== this.state.techs){
      localStorage.setItem('techs', JSON.stringify(this.state.techs))
    }
  }
```

depois o `componentDidMount`:

```
componentDidMount() {
    const techs = localStorage.getItem('techs')

    if (techs) {
      this.setState({ techs: JSON.parse(techs)})
    }
  }
```

o que estamos fazendo é ali no `componentDidUpdate` verificar se mudou alguma coisa na ***Array*** de techs para poder atualizar e se tiver mudado nos armazenamos. Depois ali no `componentDidMount` pegar essas techs armazenadas e colocar numa ariavel nova chamada `techs` e depois exibir com o `parse()` do ***JSON***

## Aula 12 - Debugando React com DevTools


Se formos na extensões do Google Chrome e instalarmos uma chamada ***React Developer Tools*** nos teremos acesso a um inspecionario no console do navegador (Command + Option + J) que fala somente sobre nossos componentes do react!