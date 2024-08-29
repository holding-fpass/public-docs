## Introdução

Webhooks também conhecidos como HTTP Callbacks são uma forma de se registrar para receber informações úteis em uma URL específica de sua escolha.

Quando ocorre uma alteração no estado de um recurso dentro da plataforma, por exemplo, uma transação é aprovada com sucesso um evento é gerado por essa ocorrência e enviado para os webhooks cadastrados.

Documentação do webhook: [EM BREVE]

**É de extrema necessidade que o seu sistema de recebimento de eventos construído para receber as notificações provenientes da plataforma, seguindo o paradigma de integração assíncrona, recebendo as mensagens e armazenando-as em filas para posterior consumo interno de seu sistema. Isso aumenta a disponibilidade e facilidade de manutenção e recuperação dessas integrações.**

### Como funcionam os disparos

#### Geração dos eventos
Independente de ter webhooks cadastrados, os eventos são gerados sempre que a sua ocasião de disparo é satisfeita. Esses eventos ficam disponíveis nas nossas APIs de listagem de eventos, apesar de desencorajarmos fortemente a utilização das APIs de leitura dos eventos através de crawling.

#### Disparo dos webhooks
Quando um evento é gerado, e existe webhooks cadastrados para receber tal tipo de evento, os disparos são agendados para alguns segundos após a geração. Essa espera se dá para que o seu sistema consiga finalizar as tarefas necessárias após a resposta da chamada que gerou o evento, evitando que os disparos sejam realizados antes de uma possível propagação de dados entre os componentes de software que compõem o seu sistema.


### Importante
A URL do seu webhook deve estar exposta (pública) para a internet, de forma que a plataforma a alcance e consiga enviar os eventos.

### Fluxo de retentativas de disparos
Uma vez que a primeira tentativa de entrega não obtém sucesso, a platforma efetuará novos disparos dentro de poucos instantes. Após um número máximo de 3 tentativas sem sucesso, o evento entra em estado de falha na entrega. É de responsabilidade do consumidor manter a resiliência dos eventos e a deduplicação de eventos.

### Timeouts
Durante o disparo de um evento para um de seus webhook, a platforma espera receber uma resposta em até 1 segundo. Caso esse tempo expire, fechamos a conexão

O evento de pagamento de transação captura os detalhes essenciais de uma transação dentro da plataforma. Este evento é fundamental para garantir que todas as partes interessadas sejam notificadas adequadamente sobre o status da transação, permitindo o processamento adequado e a manutenção de registros precisos.

A seguir, são descritas as principais propriedades associadas a este evento, incluindo identificadores únicos, informações detalhadas sobre a transação e dados específicos sobre o parceiro e a plataforma whitelabel. Essas propriedades são essenciais para o rastreamento, auditoria e gerenciamento de transações dentro do ecossistema da plataforma.

### 1. **transaction.canceled**
- **Descrição**: Este evento é disparado quando uma transação é cancelada (estornada). O cancelamento é feito pela plataforma devido a solicitações do usuário.

#### **Estrutura do Evento (JSON)**
```json
{
  "resourceId": "dfbecba9-bfb2-47e9-8797-23b2ca7a8505",
  "resourceType": "partner.event",
  "partnerId": "ef0c3466-a36a-49fe-a810-681e6ba9e505",
  "type": "transaction.canceled",
  "payload": {
    "resourceId": "492f8606-075d-402e-8fef-f44d823ec662",
    "resourceType": "transaction",
    "productId": "12b73a55-0186-42f8-9e07-5e7ebeac632d",
    "productType": "platform.subscription",
    "status": "canceled",
    "couponId": null,
    "transactionIds": [],
    "currency": "BRL",
    "description": null,
    "paymentMethod": "credit",
    "value": 49700,
    "splitProcessedAt": null,
    "refundedAt": null,
    "userId": "bd6b0ce2-3758-4feb-bacc-b86935a9e5e5",
    "metadata": null,
    "whitelabel": "test",
    "createdAt": "2020-04-30T10:20:00.000Z",
    "updatedAt": "2020-04-30T10:20:00.000Z"
  },
  "processedAt": null,
  "retries": 0,
  "metadata": null,
  "whitelabel": "test",
  "createdAt": "2020-04-30T10:20:00.000Z",
  "updatedAt": "2020-04-30T10:20:00.000Z"
}
```

### 2. **transaction.disputed**
- **Descrição**: Este evento é disparado quando uma transação é contestada, ou seja, quando um comprador questiona a validade de uma cobrança realizada.

#### **Estrutura do Evento (JSON)**
```json
{
  "resourceId": "3d9ec97e-a4f7-4cb5-9ef7-4e5f9b1d7b8b",
  "resourceType": "partner.event",
  "partnerId": "bca7d6c4-1b8a-4f25-99f4-d6a06f3b5f02",
  "type": "transaction.disputed",
  "payload": {
    "resourceId": "9f6c4e52-0a74-4a38-a7c3-ef5f4f4b6a78",
    "resourceType": "transaction",
    "productId": "5e4c85b2-7d34-4694-ae62-8c9c4fb1a839",
    "productType": "platform.subscription",
    "status": "dispute",
    "couponId": null,
    "transactionIds": [],
    "currency": "BRL",
    "description": null,
    "paymentMethod": "credit",
    "value": 49700,
    "splitProcessedAt": null,
    "refundedAt": null,
    "userId": "8cf86a4d-57b9-45e1-9f21-2f8b788f11ef",
    "metadata": null,
    "whitelabel": "test",
    "createdAt": "2020-04-30T10:20:00.000Z",
    "updatedAt": "2020-04-30T10:20:00.000Z"
  },
  "processedAt": null,
  "retries": 0,
  "metadata": null,
  "whitelabel": "test",
  "createdAt": "2020-04-30T10:20:00.000Z",
  "updatedAt": "2020-04-30T10:20:00.000Z"
}
```

---

### 3. **transaction.dispute.succeeded**
- **Descrição**: Este evento é disparado quando uma disputa sobre uma transação é resolvida com sucesso, favorável ao usuário que contestou.

#### **Estrutura do Evento (JSON)**
```json
{
  "resourceId": "7d1ef752-17c3-434f-843e-c85a01a92d90",
  "resourceType": "partner.event",
  "partnerId": "bca7d6c4-1b8a-4f25-99f4-d6a06f3b5f02",
  "type": "transaction.dispute.succeeded",
  "payload": {
    "resourceId": "fbb23e8e-3d8a-4d7f-b9f4-1a7f123bb6d8",
    "resourceType": "transaction",
    "productId": "ecf6b3db-6c9e-4f5a-9a68-5a3e6b7322b3",
    "productType": "platform.subscription",
    "status": "dispute",
    "couponId": null,
    "transactionIds": [],
    "currency": "BRL",
    "description": "Disputa resolvida com sucesso",
    "paymentMethod": "credit",
    "value": 49700,
    "splitProcessedAt": null,
    "refundedAt": "2020-04-30T10:20:00.000Z",
    "userId": "8cf86a4d-57b9-45e1-9f21-2f8b788f11ef",
    "metadata": null,
    "whitelabel": "test",
    "createdAt": "2020-04-30T10:20:00.000Z",
    "updatedAt": "2020-04-30T10:20:00.000Z"
  },
  "processedAt": "2020-04-30T10:20:00.000Z",
  "retries": 0,
  "metadata": null,
  "whitelabel": "test",
  "createdAt": "2020-04-30T10:20:00.000Z",
  "updatedAt": "2020-04-30T10:20:00.000Z"
}
```

---

### 4. **transaction.failed**
- **Descrição**: Este evento é disparado quando uma transação falha, seja por problemas de autorização, saldo insuficiente, ou outros motivos que impedem a conclusão da transação.

#### **Estrutura do Evento (JSON)**
```json
{
  "resourceId": "8eb29c4b-1d7c-4f3a-8c9f-7e54c4b44b29",
  "resourceType": "partner.event",
  "partnerId": "bca7d6c4-1b8a-4f25-99f4-d6a06f3b5f02",
  "type": "transaction.failed",
  "payload": {
    "resourceId": "a3b2d7c4-5d8c-4a9f-b7d6-7a9f4b3e6d87",
    "resourceType": "transaction",
    "productId": "d3c9b6f7-4f6b-4a8b-b5d4-6e9c3f7a9d65",
    "productType": "platform.subscription",
    "status": "failed",
    "couponId": null,
    "transactionIds": [],
    "currency": "BRL",
    "description": "Falha na transação",
    "paymentMethod": "credit",
    "value": 49700,
    "splitProcessedAt": null,
    "refundedAt": null,
    "userId": "8cf86a4d-57b9-45e1-9f21-2f8b788f11ef",
    "metadata": null,
    "whitelabel": "test",
    "createdAt": "2020-04-30T10:20:00.000Z",
    "updatedAt": "2020-04-30T10:20:00.000Z"
  },
  "processedAt": null,
  "retries": 0,
  "metadata": null,
  "whitelabel": "test",
  "createdAt": "2020-04-30T10:20:00.000Z",
  "updatedAt": "2020-04-30T10:20:00.000Z"
}
```

---

### 5. **transaction.pre_authorization.succeeded**
- **Descrição**: Este evento é disparado quando uma pré-autorização de transação é realizada com sucesso, reservando o valor na conta do cliente antes da captura final.

#### **Estrutura do Evento (JSON)**
```json
{
  "resourceId": "1b7e2b5f-4c4a-44c2-8d9b-84b4fbc8c6c5",
  "resourceType": "partner.event",
  "partnerId": "bca7d6c4-1b8a-4f25-99f4-d6a06f3b5f02",
  "type": "transaction.pre_authorization.succeeded",
  "payload": {
    "resourceId": "ae9b6d3a-4c6b-4b8d-8f6e-2e7b7f5a9d65",
    "resourceType": "transaction",
    "productId": "e4b2d6f8-4f5b-4c8b-8d6f-2f7a3c6d8b5a",
    "productType": "platform.subscription",
    "status": "pre_authorized",
    "couponId": null,
    "transactionIds": [],
    "currency": "BRL",
    "description": "Pré-autorização da transação realizada com sucesso",
    "paymentMethod": "credit",
    "value": 49700,
    "splitProcessedAt": null,
    "refundedAt": null,
    "userId": "8cf86a4d-57b9-45e1-9f21-2f8b788f11ef",
    "metadata": null,
    "whitelabel": "test",
    "createdAt": "2020-04-30T10:20:00.000Z",
    "updatedAt": "2020-04-30T10:20:00.000Z"
  },
  "processedAt": null,
  "retries": 0,
  "metadata": null,
  "whitelabel": "test",
  "createdAt": "2020-04-30T10:20:00.000Z",
  "updatedAt": "2020-04-30T10:20:00.000Z"
}
```

---

### 6. **transaction.pre_authorized**
- **Descrição**: Este evento é disparado quando uma transação é pré-autorizada, indicando que o valor foi reservado mas ainda não capturado.

#### **Estrutura do Evento (JSON)**
```json
{
  "resourceId": "a9c3b5e6-8b6d-4a6f-9b7e-6d3f9b6a7e9f",
  "resourceType": "partner.event",
  "partnerId": "bca7d6c4-1b8a-4f25-99f4-d6a06f3b5f02",
  "type": "transaction.pre_authorized",
  "payload": {
    "resourceId": "6b4e5a7c-8f6d-4b9a-8c7d-2e7a4b5f8d6c",
    "resourceType": "transaction",
    "productId": "a9c6d5b7-4c6b-4b8a-8f5d-2e7a4b5f8c7d",
    "productType": "platform.subscription",
    "status": "pre_authorized",
    "couponId": null,
    "transactionIds": [],
    "currency": "BRL",
    "description": "Transação pré-autorizada",
    "paymentMethod": "credit",
    "value": 49700,
    "splitProcessedAt": null,
    "refundedAt": null,
    "userId": "8cf86a4d-57b9-45e1-9f21-2f8b788f11ef",
    "metadata": null,
    "whitelabel": "test",
    "createdAt": "2020-04-30T10:20:00.000Z",
    "updatedAt": "2020-04-30T10:20:00.000Z"
  },
  "processedAt": null,
  "retries": 0,
  "metadata": null,
  "whitelabel": "test",
  "createdAt": "2020-04-30T10:20:00.000Z",
  "updatedAt": "2020-04-30T10:20:00.000Z"
}
```

---

### 7. **transaction.reversed**
- **Descrição**: Este evento é disparado quando uma transação é revertida, ou seja, quando o valor cobrado é devolvido ao cliente após a conclusão da transação.

#### **Estrutura do Evento (JSON)**
```json
{
  "resourceId": "b3e4d6f7-5c7a-4b8d-8f5e-2d7b4a6f8c7e",
  "resourceType": "partner.event",
  "partnerId": "bca7d6c4-1b8a-4f25-99f4-d6a06f3b5f02",
  "type": "transaction.reversed",
  "payload": {
    "resourceId": "9d6b7f8a-4c6d-4b8a-9f7c-2e6b3c7d9f4a",
    "resourceType": "transaction",
    "productId": "d4c7b6f8-5c6d-4b8a-9e7f-2d7c4f6b9a8d",
    "productType": "platform.subscription",
    "status": "refunded",
    "couponId": null,
    "transactionIds": [],
    "currency": "BRL",
    "description": "Transação revertida",
    "paymentMethod": "credit",
    "value": 49700,
    "splitProcessedAt": null,
    "refundedAt": "2020-04-30T10:20:00.000Z",
    "userId": "8cf86a4d-57b9-45e1-9f21-2f8b788f11ef",
    "metadata": null,
    "whitelabel": "test",
    "createdAt": "2020-04-30T10:20:00.000Z",
    "updatedAt": "2020-04-30T10:20:00.000Z"
  },
  "processedAt": "2020-04-30T10:20:00.000Z",
  "retries": 0,
  "metadata": null,
  "whitelabel": "test",
  "createdAt": "2020-04-30T10:20:00.000Z",
  "updatedAt": "2020-04-30T10:20:00.000Z"
}
```

---

### 8. **transaction.succeeded**
- **Descrição**: Este evento é disparado quando uma transação é concluída com sucesso, indicando que o pagamento foi processado e o valor foi capturado.

#### **Estrutura do Evento (JSON)**
```json
{
  "resourceId": "d4b7f5c8-5c8b-4d8a-9f7e-6c9b7d6f9a8e",
  "resourceType": "partner.event",
  "partnerId": "bca7d6c4-1b8a-4f25-99f4-d6a06f3b5f02",
  "type": "transaction.succeeded",
  "payload": {
    "resourceId": "e6d7f5c9-4d8a-4b8f-9e6c-2f7b6d9f8a7e",
    "resourceType": "transaction",
    "productId": "d4c7b6f8-5c6d-4b8a-9e7f-2d7c4f6b9a8d",
    "productType": "platform.subscription",
    "status": "succeeded",
    "couponId": null,
    "transactionIds": [],
    "currency": "BRL",
    "description": "Transação concluída com sucesso",
    "paymentMethod": "credit",
    "value": 49700,
    "splitProcessedAt": null,
    "refundedAt": null,
    "userId": "8cf86a4d-57b9-45e1-9f21-2f8b788f11ef",
    "metadata": null,
    "whitelabel": "test",
    "createdAt": "2020-04-30T10:20:00.000Z",
    "updatedAt": "2020-04-30T10:20:00.000Z"
  },
  "processedAt": null,
  "retries": 0,
  "metadata": null,
  "whitelabel": "test",
  "createdAt": "2020-04-30T10:20:00.000Z",
  "updatedAt": "2020-04-30T10:20:00.000Z"
}
```

---

### 9. **transaction.updated**
- **Descrição**: Este evento é disparado quando uma transação existente é atualizada, podendo incluir mudanças de status, valores, ou informações de pagamento.

#### **Estrutura do Evento (JSON)**
```json
{
  "resourceId": "e6d7f5c9-4d8a-4b8f-9e6c-2f7b6d9f8a7e",
  "resourceType": "partner.event",
  "partnerId": "bca7d6c4-1b8a-4f25-99f4-d6a06f3b5f02",
  "type": "transaction.updated",
  "payload": {
    "resourceId": "d4c7b6f8-5c6d-4b8a-9e7f-2d7c4f6b9a8d",
    "resourceType": "transaction",
    "productId": "e6d7f5c9-4d8a-4b8f-9e6c-2f7b6d9f8a7e",
    "productType": "platform.subscription",
    "status": "created",
    "couponId": null,
    "transactionIds": [],
    "currency": "BRL",
    "description": "Transação atualizada",
    "paymentMethod": "credit",
    "value": 49700,
    "splitProcessedAt": null,
    "refundedAt": null,
    "userId": "8cf86a4d-57b9-45e1-9f21-2f8b788f11ef",
    "metadata": null,
    "whitelabel": "test",
    "createdAt": "2020-04-30T10:20:00.000Z",
    "updatedAt": "2020-04-30T10:20:00.000Z"
  },
  "processedAt": null,
  "retries": 0,
  "metadata": null,
  "whitelabel": "test",
  "createdAt": "2020-04-30T10:20:00.000Z",
  "updatedAt": "2020-04-30T10:20:00.000Z"
}
```

---

### 10. **transaction.void.failed**
- **Descrição**: Este evento é disparado quando uma tentativa de anulação de uma transação falha, indicando que a transação não pôde ser cancelada ou revertida.

#### **Estrutura do Evento (JSON)**
```json
{
  "resourceId": "d4c7b6f8-5c6d-4b8a-9e7f-2d7c4f6b9a8d",
  "resourceType": "partner.event",
  "partnerId": "bca7d6c4-1b8a-4f25-99f4-d6a06f3b5f02",
  "type": "transaction.void.failed",
  "payload": {
    "resourceId": "e6d7f5c9-4d8a-4b8f-9e6c-2f7b6d9f8a7e",
    "resourceType": "transaction",
    "productId": "d4c7b6f8-5c6d-4b8a-9e7f-2d7c4f6b9a8d",
    "productType": "platform.subscription",
    "status": "succeeded",
    "couponId": null,
    "transactionIds": [],
    "currency": "BRL",
    "description": "Tentativa de anulação da transação falhou",
    "paymentMethod": "credit",
    "value": 49700,
    "splitProcessedAt": null,
    "refundedAt": null,
    "userId": "8cf86a4d-57b9-45e1-9f21-2f8b788f11ef",
    "metadata": null,
    "whitelabel": "test",
    "createdAt": "2020-04-30T10:20:00.000Z",
    "updatedAt": "2020-04-30T10:20:00.000Z"
  },
  "processedAt": null,
  "retries": 0,
  "metadata": null,
  "whitelabel": "test",
  "createdAt": "2020-04-30T10:20:00.000Z",
  "updatedAt": "2020-04-30T10:20:00.000Z"
}
```

---

### 11. **transaction.void.succeeded**
- **Descrição**: Este evento é disparado quando uma tentativa de anulação de uma transação é bem-sucedida, indicando que a transação foi cancelada ou revertida com sucesso.

#### **Estrutura do Evento (JSON)**
```json
{
  "resourceId": "e6d7f5c9-4d8a-4b8f-9e6c-2f7b6d9f8a7e",
  "resourceType": "partner.event",
  "partnerId": "bca7d6c4-1b8a-4f25-99f4-d6a06f3b5f02",
  "type": "transaction.void.succeeded",
  "payload": {
    "resourceId": "d4c7b6f8-5c6d-4b8a-9e7f-2d7c4f6b9a8d",
    "resourceType": "transaction",
    "productId": "e6d7f5c9-4d8a-4b8f-9e6c-2f7b6d9f8a7e",
    "productType": "platform.subscription",
    "status": "canceled",
    "couponId": null,
    "transactionIds": [],
    "currency": "BRL",
    "description": "Anulação da transação realizada com sucesso",
    "paymentMethod": "credit",
    "value": 49700,
    "splitProcessedAt": null,
    "refundedAt": "2020-04-30T10:20:00.000Z",
    "userId": "8cf86a4d-57b9-45e1-9f21-2f8b788f11ef",
    "metadata": null,
    "whitelabel": "test",
    "createdAt": "2020-04-30T10:20:00.000Z",
    "updatedAt": "2020-04-30T10:20:00.000Z"
  },
  "processedAt": "2020-04-30T10:20:00.000Z",
  "retries": 0,
  "metadata": null,
  "whitelabel": "test",
  "createdAt": "2020-04-30T10:20:00.000Z",
  "updatedAt": "2020-04-30T10:20:00.000Z"
}
```

### 12. **transaction.charged_back**
- **Descrição**: Este evento é disparado quando uma transação sofre um chargeback. Isso ocorre quando o comprador contesta a cobrança e o valor é devolvido.

#### **Estrutura do Evento (JSON)**
```json
{
  "resourceId": "aebec7e1-4bfa-4c91-8f71-ecf2e8c7bb0f",
  "resourceType": "partner.event",
  "partnerId": "bca7d6c4-1b8a-4f25-99f4-d6a06f3b5f02",
  "type": "transaction.charged_back",
  "payload": {
    "resourceId": "59a4c612-dc2e-4e88-9c58-fdbebc37f1e8",
    "resourceType": "transaction",
    "productId": "24c073d4-3f42-4a9c-b649-2e9f0f734dbf",
    "productType": "platform.subscription",
    "status": "charged_back",
    "couponId": null,
    "transactionIds": [],
    "currency": "BRL",
    "description": "Transação sofreu chargeback",
    "paymentMethod": "credit",
    "value": 49700,
    "splitProcessedAt": null,
    "refundedAt": "2020-04-30T10:20:00.000Z",
    "userId": "8cf86a4d-57b9-45e1-9f21-2f8b788f11ef",
    "metadata": null,
    "whitelabel": "test",
    "createdAt": "2020-04-30T10:20:00.000Z",
    "updatedAt": "2020-04-30T10:20:00.000Z"
  },
  "processedAt": null,
  "retries": 0,
  "metadata": null,
  "whitelabel": "test",
  "createdAt": "2020-04-30T10:20:00.000Z",
  "updatedAt": "2020-04-30T10:20:00.000Z"
}
```

---

### 13. **transaction.commission.succeeded**
- **Descrição**: Este evento é disparado quando o cálculo da comissão sobre uma transação é realizado com sucesso. Esse cálculo é importante para parceiros que recebem comissões baseadas em transações.

#### **Estrutura do Evento (JSON)**
```json
{
  "resourceId": "4e9b1f3b-8df9-4828-9a5b-373e5a745bdd",
  "resourceType": "partner.event",
  "partnerId": "bca7d6c4-1b8a-4f25-99f4-d6a06f3b5f02",
  "type": "transaction.commission.succeeded",
  "payload": {
    "resourceId": "59a4c612-dc2e-4e88-9c58-fdbebc37f1e8",
    "resourceType": "transaction",
    "productId": "24c073d4-3f42-4a9c-b649-2e9f0f734dbf",
    "productType": "platform.subscription",
    "status": "succeeded",
    "couponId": null,
    "transactionIds": [],
    "currency": "BRL",
    "description": "Comissão calculada com sucesso",
    "paymentMethod": "credit",
    "value": 49700,
    "commissionValue": 4970,
    "splitProcessedAt": null,
    "refundedAt": null,
    "userId": "8cf86a4d-57b9-45e1-9f21-2f8b788f11ef",
    "metadata": null,
    "whitelabel": "test",
    "createdAt": "2020-04-30T10:20:00.000Z",
    "updatedAt": "2020-04-30T10:20:00.000Z"
  },
  "processedAt": null,
  "retries": 0,
  "metadata": null,
  "whitelabel": "test",
  "createdAt": "2020-04-30T10:20:00.000Z",
  "updatedAt": "2020-04-30T10:20:00.000Z"
}
```

---

### 14. **transaction.created**
- **Descrição**: Este evento é disparado quando uma nova transação é criada no sistema. Este evento é usado para registrar o início de uma transação.

#### **Estrutura do Evento (JSON)**
```json
{
  "resourceId": "cfe24b4e-6ff4-46f1-8c48-5d3d0ec7efc6",
  "resourceType": "partner.event",
  "partnerId": "bca7d6c4-1b8a-4f25-99f4-d6a06f3b5f02",
  "type": "transaction.created",
  "payload": {
    "resourceId": "76d8fbb9-bf60-42fe-a6bc-9c7a4bf59367",
    "resourceType": "transaction",
    "productId": "3f2f7b4a-8c7b-4371-9a74-f7e27ab4fbcd",
    "productType": "platform.subscription",
    "status": "created",
    "couponId": "e25b7fb3-8d6f-48f3-b5f4-0dfe479b5b4d",
    "transactionIds": [],
    "currency": "BRL",
    "description": "Nova transação criada",
    "paymentMethod": "credit",
    "value": 49700,
    "splitProcessedAt": null,
    "refundedAt": null,
    "userId": "8cf86a4d-57b9-45e1-9f21-2f8b788f11ef",
    "metadata": null,
    "whitelabel": "test",
    "createdAt": "2020-04-30T10:20:00.000Z",
    "updatedAt": "2020-04-30T10:20:00.000Z"
  },
  "processedAt": null,
  "retries": 0,
  "metadata": null,
  "whitelabel": "test",
  "createdAt": "2020-04-30T10:20:00.000Z",
  "updatedAt": "2020-04-30T10:20:00.000Z"
}
```

## Conclusão
Esta documentação foi elaborada para fornecer uma visão abrangente e detalhada dos eventos relacionados a transações de pagamento dentro da plataforma.
Seguindo os passos detalhados nesta documentação, você poderá implementar o sistema de pagamentos da plataforma Fpass de maneira eficiente e segura. Certifique-se de seguir todos os passos corretamente para garantir uma integração bem-sucedida.
