---
title: Email-SES
createdAt: 2023-03-11T12:40:23.000Z
updatedAt: 2023-03-13T17:52:05.000Z
---

### Objetivo/Resumo

Possibilita o usuário enviar e-mails utilizando o Serviço de Email Simples(SES) da Amazon.
O Amazon SES é um provedor de serviços de e-mail na nuvem que pode ser integrado a qualquer aplicação para envio de e-mails em massa.
Para mais detalhes sobre o Amazon SES [clique aqui](https://aws.amazon.com/pt/ses/#:\~:text=O%20Amazon%20SES%20%C3%A9%20um,paga%20apenas%20pelo%20que%20usar.).

### Criando um fluxo utilizando Conector SES

Para criar um fluxo utilizando um conector de Email-SES é preciso criar uma conexão e configurar o fluxo.

### Criando conexão Email-SES

OBS: [Clique aqui]() e acesse nosso passo a passo para saber como criar uma conexão.

Estes são os seguintes parâmetros de configuração da conexão Email-SES que serão requisitados na hora da criação:

*   **Nome**: parâmetro padrão em qualquer criação de uma conexão, te ajudará a identificar melhor sua conexão.

*   **Descrição**: parâmetro padrão e opcional em qualquer criação de uma conexão, aqui você pode relatar detalhadamente para que fins servirá sua conexão.

*   **ID de acesso**: ou "Access Key" campo requerido para acesso ao serviço AWS.

*   **Chave Secreta**: ou "Secret Access Key" campo requerido para acesso ao serviço AWS.

*   **Região**: zona/local onde seu serviço será executado.



### Configurando fluxo Email-SES

Crie um fluxo e arraste o conector Email-SES para o canvas.

OBS: [Clique aqui]() e acesse nosso passo a passo para saber como criar um fluxo.

Selecione o conector e na aba de `Parametrização` e preencha os seguintes parâmetros de configuração:

*   **Remetente**: remetente do e-mail.

*   **Destinatários**: quem irá receber o e-mail, informe um ou mais destinatários.

*   **Assunto**: campo referente ao titulo do e-mail.

*   **Corpo**: conteúdo que sera enviado, aceita `template` ou texto fixo.

*Nota: Estes campos podem ser preenchidos usando funções de parâmetros dinâmicos da Fluid, para mais detalhes clique em *[](docId:0VQVPw6saqz2z1jGp8FwC)

Na aba `Propriedades` selecione a conexão desejada, dê um nome ao passo e se necessário detalhe uma descrição.

### Na prática

Neste tópico criaremos um exemplo com intuito de mostrar o conector Email-SES na prática. O fluxo terá dois passos, sendo eles random e envia-email.

![](https://archbee-image-uploads.s3.amazonaws.com/G1NTw6yAi4RDUYbsU8csp/JRi895oFXbftKaa4olvPz_fluxo-teste-email.png)

### Configurando passo random (conector HTTP)

O conector HTTP fará um `GET` em uma api pública chamada `random` acessando o endpoint `/api/v2/beers` que nos retornará dados de uma determinada cerveja.
Assim ficou as configurações do nosso conector HTTP na aba `Parametrização`:

![](https://archbee-image-uploads.s3.amazonaws.com/G1NTw6yAi4RDUYbsU8csp/FPEWcLFY8tiFm_Pw4POni_config-http-test-email.png)

Já na aba `Propriedades` informamos apenas um nome ao nosso passo, pois o get nessa api não precisa de nenhum tipo de conexão:

![](https://archbee-image-uploads.s3.amazonaws.com/G1NTw6yAi4RDUYbsU8csp/ljboLN522AeBGPSGwjdxW_config-http-test-email-propriedades.png)



*Nota: Para obter detalhes sobre nosso conector **HTTP** clique em ....*

### Fluxo de mensagens

### Entrada:

```text
Neste caso não possui dados de entrada
```

### Saída:

```json
{
    "id": 5838,
    "uid": "6e983d83-6330-4598-9f04-0c9ed28ae150",
    "brand": "Pabst Blue Ribbon",
    "name": "Stone Imperial Russian Stout",
    "style": "Amber Hybrid Beer",
    "hop": "Sterling",
    "yeast": "1332 - Northwest Ale",
    "malts": "Carapils",
    "ibu": "86 IBU",
    "alcohol": "5.2%",
    "blg": "9.1°Blg"
}
```

### Configurando passo envia-email (conector Email-SES)

O conector Email-SES enviará emails construindo o `corpo` através de um template e o `assunto` utilizaremos uma função de parâmetros dinamicos da fluid (*para mais detalhes clique em *)
Assim ficará nosso conector Email-SES na aba `Parametrização`:



![](https://archbee-image-uploads.s3.amazonaws.com/G1NTw6yAi4RDUYbsU8csp/FqNpYsvW2reFvidYAw7l0_image.png)

Foram selecionados dois destinatários:



![](https://archbee-image-uploads.s3.amazonaws.com/G1NTw6yAi4RDUYbsU8csp/0jn6ZNZMVuX6zsS798z27_destinatarios.png)

No Corpo foi selecionado `Template` e ultilizada a seguinte sintaxe:



![](https://archbee-image-uploads.s3.amazonaws.com/G1NTw6yAi4RDUYbsU8csp/LH8CNzsbqunqYcRiiJjLN_template-email-teste.png)

*Nota: Utilizamos a sintaxe do go template, para mais informações sobre a ferramenta clique em *

No `.state` fica alocado o resultado de saída dos passos anteriores, logo, se queremos utilizar os resultados do passo `random` especificamos o caminho onde buscar `.state.random` 

Na aba `Propriedades` informamos o nome do passo, a conexão criada com as credenciais da aws e a informação do remetente:

![](https://archbee-image-uploads.s3.amazonaws.com/G1NTw6yAi4RDUYbsU8csp/hiPXOwiML_WXlVREmolfV_envia-email-teste-propriedades.png)



### Fluxo de mensagens

### Entrada:

Depois de aplicado a configuração do template encima do JSON de saída do passo anterior, teremos os seguintes dados de entrada para o Email-SES:

```json
{
    "Subject": "Cerveja: Stone Imperial Russian Stout",
    "Body": "\u003ch3\u003e Dados \u003c/h3\u003e\n\n\u003cp\u003e cerveja fabricada por: Pabst Blue Ribbon \u003c/p\u003e\n\u003cp\u003e estilo: Amber Hybrid Beer \u003c/p\u003e\n\u003cp\u003e teor alcoólico: 5.2% \u003c/p\u003e\n\u003cp\u003e malte: Carapils \u003c/p\u003e\n\n",
    "Source": "integration@fluidapi.io"
}
```

### Saída:

```json
{
    "ID": "01000186b303e0c8-a014ddc2-0e17-4b0a-b117-a031860f2397-000000",
    "Message": "success"
}
```

### Resultado

Após disparar o fluxo podemos ver o resultado final nos logs que aparecerá no canvas, e clicando em detalhes teremos os dados da requisição e da resposta:

![](https://archbee-image-uploads.s3.amazonaws.com/G1NTw6yAi4RDUYbsU8csp/k3CMhpHnZpV6O4y-Z4OE1_fluxo-email-executado.png)



E se olharmos na caixa de entrada de um dos destinatarios do exemplo podemos ver o resultado final com o corpo formatado:



![](https://archbee-image-uploads.s3.amazonaws.com/G1NTw6yAi4RDUYbsU8csp/GHsLbY14LEAFVo937srCi_resultado-final-1.jpeg)



![](https://archbee-image-uploads.s3.amazonaws.com/G1NTw6yAi4RDUYbsU8csp/SqfePDKeMy-XViMtRLEuF_resultado-final-2.jpeg)

