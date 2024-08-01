## Introdução

A plataforma Fpass é um sistema de educação. O principal objetivo do Single Sign-On (SSO) é permitir que usuários façam login na plataforma Fpass a partir de outro aplicativo, proporcionando uma experiência de usuário mais fluida e segura.

## Visão Geral do SSO

O SSO é baseado em JSON Web Tokens (JWT). Abaixo estão listadas e detalhadas as propriedades necessárias no payload do JWT:

- `userId`: (string) Identificador único do usuário.
- `whitelabel`: (string) Nome do parceiro ou aplicação.
- `ssoVersion`: (string) Deve ser "v2.0.0".
- `iss`: (string) Emissor do token.

## JWT

JWT (JSON Web Token) é um padrão aberto (RFC 7519) que define uma maneira compacta e segura de transmitir informações entre as partes como um objeto JSON. Estas informações podem ser verificadas e confiáveis porque são assinadas digitalmente. Para mais informações, visite [jwt.io](https://jwt.io).

## Requisitos Técnicos

Para implementar o SSO, são necessários os seguintes requisitos técnicos:

1. Conhecimento da chave secreta compartilhada entre o parceiro e a Fpass.
2. Capacidade de gerar um JWT utilizando essa chave secreta.

## Passos para Implementação

### Gerar o JWT

Para gerar um JWT, utilize a chave secreta fornecida pela Fpass. Abaixo estão exemplos de código em Python e Node.js para gerar um JWT com o seguinte payload:

```json
{
    "userId": "exemploUserId",
    "whitelabel": "exemploWhitelabel",
    "ssoVersion": "v2.0.0",
    "iss": "exemploIss"
}
```

#### Exemplo em Python

```python
import jwt
import datetime

# Definindo a chave secreta
SECRET_KEY = 'sua_chave_secreta'

# Criando o payload
payload = {
    'userId': 'exemploUserId',
    'whitelabel': 'exemploWhitelabel',
    'ssoVersion': 'v2.0.0',
    'iss': 'exemploIss',
    'exp': datetime.datetime.utcnow() + datetime.timedelta(hours=1)  # Token expira em 1 hora
}

# Gerando o JWT
token = jwt.encode(payload, SECRET_KEY, algorithm='HS256')

print(token)
```

#### Exemplo em Node.js

```javascript
const jwt = require('jsonwebtoken');

// Definindo a chave secreta
const SECRET_KEY = 'sua_chave_secreta';

// Criando o payload
const payload = {
    userId: 'exemploUserId',
    whitelabel: 'exemploWhitelabel',
    ssoVersion: 'v2.0.0',
    iss: 'exemploIss',
    exp: Math.floor(Date.now() / 1000) + (60 * 60) // Token expira em 1 hora
};

// Gerando o JWT
const token = jwt.sign(payload, SECRET_KEY);

console.log(token);
```

### Redirecionar o Usuário

Após gerar o JWT, redirecione o usuário para a URL da Fpass com o JWT gerado. Abaixo estão exemplos de código para esse redirecionamento em Python e Node.js.

#### Exemplo em Python

```python
from flask import Flask, redirect

app = Flask(__name__)

@app.route('/login')
def login():
    # Gerando o JWT
    token = jwt.encode(payload, SECRET_KEY, algorithm='HS256')
    # Redirecionando o usuário
    return redirect(f'https://fpass.com.br?jwt={token}')

if __name__ == '__main__':
    app.run()
```

#### Exemplo em Node.js

```javascript
const express = require('express');
const app = express();

app.get('/login', (req, res) => {
    // Gerando o JWT
    const token = jwt.sign(payload, SECRET_KEY);
    // Redirecionando o usuário
    res.redirect(`https://fpass.com.br?jwt=${token}`);
});

app.listen(3000, () => {
    console.log('Servidor rodando na porta 3000');
});
```

## Testes e Solução de Problemas

### Testes

Para garantir que a integração do SSO está funcionando corretamente, siga os seguintes passos:

1. Gere o JWT com os dados corretos.
2. Redirecione o usuário para a URL da Fpass com o JWT gerado.
3. Verifique se o usuário é autenticado corretamente na plataforma.

### Solução de Problemas

Problemas comuns que podem ocorrer durante a integração:

- **Token Expirado**: Certifique-se de que o campo `exp` no payload está configurado corretamente.
- **Assinatura Inválida**: Verifique se a chave secreta utilizada para gerar o JWT está correta.
- **Payload Incorreto**: Assegure-se de que todos os campos necessários estão presentes no payload.

## Conclusão

Seguindo os passos detalhados nesta documentação, você poderá implementar o SSO na plataforma Fpass de maneira eficiente e segura. Certifique-se de seguir todos os passos corretamente para garantir uma integração bem-sucedida.
