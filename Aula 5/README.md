# Aula 5 - Treinamento - JavaScript e ReactJS

Erros ocorrem, queira você ou não. Mantendo isso em mente, o objetivo dessa aula é entender como lidar com essas situações adversas e evitar ao máximo que comprometam a experiência do usuário. Além disso, veremos como preparar nossa aplicação para o ambiente de produção.

- Tratamento de Exceções
    * Apresentando erros ao usuário
    * Error Boundary
- Testando a Aplicação
    * Testes Unitários
    * Jest
    * Testando componentes
- Build e deploy
    * Ambientes de desenvolvimento e produção
    * Opções de deploy

## Apresentando Erros ao Usuário

```jsx
import axios from 'axios'

const api = axios.create({
  baseURL: 'https://www.breakingbadapi.com/api'
});

...

api.get('/characters')
.then(res => ...)
.catch(error => {
    alert(error.message) // don't alert 
})

...

axios.interceptors.response.use(function (response) {
    // Do something with response data
    return response;
}, function (error) {
    alert(error.response.message) // don't alert
    return Promise.reject(error);
});
```

## Error Boundaries
 
> Error boundaries não capturam erros em:
> 
> - Manipuladores de evento (saiba mais)
> - Código assíncrono (ex. callbacks de setTimeout ou requestAnimationFrame)
> - Renderização no servidor
> - Erros lançados na própria error boundary (ao invés de em seus filhos)

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Atualiza o state para que a próxima renderização mostre a UI alternativa.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // Você também pode registrar o erro em um serviço de relatórios de erro
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // Você pode renderizar qualquer UI alternativa
      return <h1>Algo deu errado.</h1>;
    }

    return this.props.children; 
  }
}
```
[**Exemplo**](https://codepen.io/gaearon/pen/wqvxGa?editors=0010)

## Testando Nossa Aplicação

### [Jest](https://jestjs.io/docs/en/getting-started)

```jsx
function sum(a, b) {
  return a + b;
}
module.exports = sum;
```

```jsx
const sum = require('./sum');

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

### Testando Código React
```jsx
import React from 'react';
import {cleanup, fireEvent, render} from '@testing-library/react';
import CheckboxWithLabel from '../CheckboxWithLabel';

// Note: running cleanup afterEach is done automatically for you in @testing-library/react@9.0.0 or higher
// unmount and cleanup DOM after the test is finished.
afterEach(cleanup);

it('CheckboxWithLabel changes the text after click', () => {
  const {queryByLabelText, getByLabelText} = render(
    <CheckboxWithLabel labelOn="On" labelOff="Off" />,
  );

  expect(queryByLabelText(/off/i)).toBeTruthy();

  fireEvent.click(getByLabelText(/off/i));

  expect(queryByLabelText(/on/i)).toBeTruthy();
});
```

## Build

```sh
npm run build
```

## [HANDSON](https://codesandbox.io/s/admiring-shaw-u3qdg?file=/src/components/CharacterCard.js)