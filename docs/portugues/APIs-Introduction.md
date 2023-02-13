# APIs - Introdução

## 1. Introdução

Este documento descreve as APIs REST e os recursos fornecidos pela Fiserv. Nossos sistemas são confiáveis com ótimo desempenho e elasticidade. Como líderes de negócios no segmento de pagamentos, as APIs fornecerão uma oportunidade aos desenvolvedores de aproveitar nosso ecossistema de pagamentos e nossa ampla infraestrutura.

Nossas APIs possuem paths URI e utilizam autenticação HTTP nativa, proporcionando acesso seguro às informações. Faça solicitações HTTP usando os métodos GET e POST, receba respostas no formato JSON. Isso permite que os desenvolvedores usem qualquer linguagem de programação para acessar nossas APIs, integrar de forma segura e construir vários aplicativos diferentes.

## 2. Primeiro passo

Para começar a usar nossas APIs, o desenvolvedor deve seguir os passos listados abaixo:

1. Crie um usuário/senha no Portal do Desenvolvedor;
2. Quando estiver pronto o entendimento das APIs e o desenvolvimento da integração estiver em andamento, solicite o Usuário/Senha do Token através da caixa **FiservDigitalBrazil@firstdatacorp.com.br.**Ao enviar o e-mail, favor informar:
    * **Assunto**: Requisição de Usuário para Autenticação;
    * Corpo:
        * **Seu nome / Nome da empresa**
        * **Descrição da Empresa**: Breve Descrição da sua Empresa;
        * **Representante Fiserv**: Quem da equipe comercial Fiserv trabalha com sua empresa;
        * **Instituição***: A qual Instituição está relacionada a integração, por exemplo: BIN, Sicredi, etc.;
        * **Contrato de Prestação de Serviços***: Contrato de Prestação de Serviços registrado na Fiserv;
        * **E-mail**: E-mail institucional para contato;
        * **Telefone**: Telefone de contato institucional;
        * **Comentários**: Qualquer comentário que você queira adicionar, relacionado ao projeto de integração;
3. Use os valores de Usuário/Senha para gerar um Token de Autenticação. O token será usado para acessar as APIs
4. Crie um aplicativo no menu [Aplicativo][1] e selecione os produtos aos quais deseja ter acesso
5. Use os valores disponíveis para "Chave do consumidor" e "Segredo do consumidor" na página do aplicativo (eles serão necessários para acessar as APIs)

_* Se você não conhece sua Instituição ou Contrato de Serviço, entre em contato com seu representante da Fiserv._


### 2.1 Gerando Token

Para gerar o Token, você deve acessar a página com instruções específicas [Token Access](../api/?type=post&path=/token/).

**Aviso importante&nbsp;:**

* A guarda das credenciais é de responsabilidade do cliente;
* Não compartilhe as Credenciais com quem não deve ter acesso às mesmas;
* Armazene as credenciais de token em um cofre específico. Evite armazenar no mesmo banco de dados da aplicação que irá consumir recursos;
* Em caso de comprometimento/perda das suas credenciais, deverá contactar-nos, de imediato;

#### 2.1.1 Requisição

Para receber o Token, você precisa enviar uma solicitação POST para "[Token Access](../api/?type=post&path=/token/)" com os seguintes parâmetros:

| Content-Type | Local  | Type   | Value                                                                                  | 
|--------------|--------|--------|----------------------------------------------------------------------------------------|
| Content-Type | Header | String | application/x-www-form-urlencoded                                                      |
| Body         | Body   | String | grant_type=password&client_id=estabelecimento&username={user_name}&password={password} |

where:

* user_name: o nome de usuário que você obteve na etapa #1
* password: a senha que você obteve no passo #1

#### 2.1.2 Resposta

A resposta POST é um JSON com as seguintes informações:

| Name               | Type    | Description                                    | Sample                                      |
|--------------------|---------|------------------------------------------------|---------------------------------------------|
| access_token       | string  | Token to be used in all other APIs             | "eyJhbGciOiJS [...] swVHg6f8opW1DzuWsilvQ"  |
| expires_in         | integer | Token expiration time                          | 62208000                                    |
| refresh_expires_in | integer | Expiration time for refresh token              | 1800                                        |
| refresh_token      | string  | Token to be used to refresh the authentication | "eyJhbGciOiJS [...] G8LSL7qfJLmLU59O8sskw"  |
| token_type         | string  | Token type                                     | bearer                                      |
| id_token           | string  | Token Id                                       | "eyJhbGciOiJS [...] 3utG5l7qC0ieC--oLzGQ6w" |
| not-before-policy  | integer || 0                                              |     |
| session-state      | string  | Session Status                                 | "3a639a9b-2c42-472e-911b-209f9e01e077"      |

Onde access_token é o valor a ser usado em todas as outras APIs.

Exemplo de resposta

```json
{
  "access_token": "eyJhbGciOiJS [...] swVHg6f8opW1DzuWsilvQ",
  "expires_in": 62208000,
  "refresh_expires_in": 1800,
  "refresh_token": "eyJhbGciOiJS [...] G8LSL7qfJLmLU59O8sskw",
  "token_type": "bearer",
  "id_token": "eyJhbGciOiJS [...] 3utG5l7qC0ieC--oLzGQ6w",
  "not-before-policy": 0,
  "session_state": "3a639a9b-2c42-472e-911b-209f9e01e077"
}
```

## 3. Authentication

As APIs Fiserv usam [Autenticação HMAC][2].

Para poder aceder a qualquer um dos Códigos ou APIs descritos nesta plataforma, serão necessárias as credenciais enviadas por e-mail. Não tem as credenciais? Crie sua conta e solicite um Usuário/Senha enviando um e-mail para a caixa **FiservDigitalBrazil@firstdatacorp.com.br**.

Você deve enviar os seguintes parâmetros no cabeçalho para cada solicitação de APIs:

| Name              | Description                                     |
|-------------------|-------------------------------------------------|
| auth              | Generated Token                                 |
| Api-Key           | Key got from developer application              |
| Timestamp         | Date in "Unix epoch time" format - milisseconds |
| Message-Signature | HMAC authentication string                      |
| Client-Request-Id | Alphanumeric string randomly generated          |

### 3.1 auth

No parâmetro **auth** - header - o usuário deve enviar o valor obtido em [Token details][2].

Amostra de Token  "eyJhbGciOiJSUzI1NiIsInR5cCIgO [...] tFlCzaiEx06ETdkwoTC-CfzSaVA"

### 3.2 Api-Key

Chave obtida da página do desenvolvedor [application][1]

Sample: "X0GW3QGOYFn4r7DHcVC8KuatUnNs6MGB"

### 3.3 Timestamp

Data atual no formato "Unix epoch time" em milissegundos, que consiste em um número em milissegundos desde 00:00 - 1º de janeiro de 1970.

Amostra: 1568914752721

### 3.4 Message-Signature

Autenticação de string HMAC.

Para gerar esta string, o usuário precisa seguir os passos abaixo:

1. Obtenha a chave do aplicativo do desenvolvedor - appKey
2. Obtenha a senha do aplicativo do desenvolvedor - segredo
3. Gere a data no formato Unix epoch - milissegundos (Date)
4. Gere uma string alfanumérica aleatória (Client-Request-Id)
5. Crie o JSON que será enviado no corpo (solicitação POST)
6. Concatene todas as instruções itens 1, 3, 4 e 5: appKey + Client + Date + JSON
7. Criptografe o resultado do item #6 (OpenSSL base64 usando o segredo - item #2 - como chave HMAC)
8. Forneça a string #7 como parâmetro "Message-Signature"

### 3.5 Client-Request-Id

String alfanumérica gerada pelo usuário.

Amostra: "6NYHj9Pgyxc84rmafpwG4fjhLwFfJYFVmxJOcgziD0QN3q6mGC"

## 4. Códigos de status para APIs REST

Fiserv retorna o status do código padrão HTTP, para solicitações bem-sucedidas e malsucedidas. Veja a tabela abaixo para mais detalhes:

| Code | Description           | Meaning                                                                                                                                                      |
|------|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 200  | OK                    | Successfully                                                                                                                                                 |
| 400  | Bad Request           | Validation Error. The server was not able to process the request, due to any problem that might be coming from customer side. Ex: parameters or body request |
| 401  | Unauthorized          | Authentication error related to HMAC or Token. Be sure the access Token is valid and not expired                                                             |
| 403  | Forbidden             | The user is not authorized to access the APIs                                                                                                                |
| 404  | Not found             | APIs URI is incorrect or the resource might not be found                                                                                                     |
| 409  | Conflict              | The request cannot be completed due to a conflict with destination resource                                                                                  |
| 500  | Internal Server Error | Fiserv server might be down or unresponsive                                                                                                                  |
| 502  | Bad Gateway           | The server (gateway or proxy) received an invalide response by an internal server                                                                            |

## 5. Requisição GET

### 5.1 Amostra no Postman

Segue um exemplo de uma Solicitação GET da Verificação CEP, usando [Postman][3].

![Imagem mostrando a URL utilizada, com o CEP sendo igual a 04794000](../assets/images/cep-url.JPG "cep")

Os parâmetros no cabeçalho são:

![Headers metodo GET](../assets/images/new_headers_get_0.PNG "header")

A resposta é:

![Print do Postman mostrando a resposta do request GET feito. Na resposta, é possível ver o resultado mostrando que o CEP é na cidade de São Paulo](../assets/images/reponse.JPG "response")

### 5.2 Amostra em Python 3

#### 5.2.1 Código

```python
    from hashlib import sha256
    import base64
    import time
    import hmac
    import requests
    import random
    import string
    import json
    
    # appkey extraída da página da aplicação do desenvolvedor
    appkey = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    
    # senha extraída da página da aplicação do desenvolvedor
    secret = "XXXXXXXXXXXXXXXX"
    
    # url das APIs
    url = "https://int.api.firstdata.com"
    
    # endpoint da API
    endpoint = "/sba/cep-service/cep/04794000"
    
    # token obtido através da API de Token
    token = 'eyJhbGciOiJSUzI1NiIsIn[...]dPdcIGGIEz76SP6GoHcdYTJQnmBYg'
    
    # data no formato "Unix epoch time" em milissegundos
    dt = str(int(time.time() * 1000))
    
    # string alfanumérica gerada pelo usuário
    client_request_id = ''.join(random.choice(string.ascii_uppercase +
                                              string.ascii_lowercase +
                                              string.digits) for _ in range(50))
    
    # concatenação de strings
    string_to_sign = appkey + client_request_id + dt
    
    # encriptação da string usando hmac com sha256
    encoded_signature = base64.b64encode(hmac.new(secret.encode(),
                                         string_to_sign.encode(),
                                         sha256).digest()).decode()
    
    # string que será enviada como parâmetro "Message-Signature" no header
    hmac_auth = 'encoded_signature'
    
    # dicionário "headers" com todos os parâmetros que serão enviados no request
    headers = {
        'auth': token,
        'Api-Key': appkey,
        'Timestamp': dt,
        'Message-Signature': hmac_auth,
        'Client-Request-Id': client_request_id,
    }
    
    # envio do request e impressão do resultado (status e body)
    r = requests.get(url + endpoint, headers=headers)
    print(f'Status: {r.status_code}\n'
          f'Body: {json.dumps(r.json(), indent=4, sort_keys=True, ensure_ascii=False)}')
``` 

#### 5.2.2 Saída

```json
    Status: 200
    Body: {
        "cepResponse": {
            "address": "Avenida das Nações Unidas",
            "city": "São Paulo",
            "complement": "- lado ímpar",
            "genericZipCode": "Não",
            "neighborhood": "Vila Gertrudes",
            "state": "SP",
            "zipCode": "04794000"
        },
        "code": 200,
        "description": "Search done successfully!"
    }
```
    
### 5.3 Amostra em Shell script

#### 5.3.1 Código

```shell
    #!/bin/bash
    
    # appkey extraída da página da aplicação do desenvolvedor
    APPKEY="XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" # Api Key (ou chave publica) gerada pelo Apigee
    
    # senha extraída da página da aplicação do desenvolvedor
    SECRET="XXXXXXXXXXXXXXXX" # Chave secreta gerada pelo APIGEE
    
    # url da API
    URL="https://int.api.firstdata.com/sba/cep-service/cep/04794000"
    
    # data no formato "Unix epoch time" em milissegundos
    DATE=`Date +%s%3N` #Data em formato epoch
    
    # string alfanumérica gerada pelo usuário
    CLIENT_REQUEST_ID=`head /dev/urandom | tr -dc A-Za-z0-9 | head -c 50 ; echo ''`
    
    # token obtido através da API de Token
    TOKEN="eyJhbGciOiJSUzI1NiIsIn[...]dPdcIGGIEz76SP6GoHcdYTJQnmBYg"
    
    # concatenação de strings
    STRING_TO_SIGN=$APPKEY$CLIENT_REQUEST_ID$DATE
    
    # encriptação da string usando hmac com sha256
    ENCODED_SIGNATURE=$(echo -n $STRING_TO_SIGN | openssl sha256 -binary -hmac $SECRET | base64) #Estamos com suporte ao sha256 somente
    
    # string que será enviada como parâmetro "Message-Signature" no header
    HMAC_AUTH=$ENCODED_SIGNATURE
    
    # envio do request com parâmetros no header
    curl -H "auth: $TOKEN" -H "Api-Key: $APPKEY" -H "Timestamp: $DATE" -H "Message-Signature: $HMAC_AUTH" -H "Client-Request-Id: $CLIENT_REQUEST_ID" $URL
```
    
#### 5.3.2 Saída

```json

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   249    0   249    0     0     86      0 --:--:--  0:00:02 --:--:--    86
    {"code":200,"description":"Search done successfully!","cepResponse":{"state":"SP","city":"São Paulo","neighborhood":"Vila Gertrudes","address":"Avenida das Nações Unidas","complement":"- lado ímpar","zipCode":"04794000","genericZipCode":"Não"}}
```

## 6. Requisição POST

### 6.1 Amostra no Postman

Seguindo na solicitação POST para atualização de endereço em [Postman][6]

![Imagem mostrando a URL da API de atualização de endereços](../assets/images/address-url.JPG "post request")

Os parâmetros no cabeçalho são:

![Headers Método POST](../assets/images/new_headers_post.PNG "post header")

O corpo JSON é:

![Imagem representando o JSON enviado no body do request POST](../assets/images/body_post.JPG "post body")

A resposta é:

![Imagem mostrando o body da resposta do request POST](../assets/images/response_post.JPG "post response")

### 6.2 Amostra em Python 3

#### 6.2.1 Código

```python
    from hashlib import sha256
    import base64
    import time
    import hmac
    import requests
    import random
    import string
    import json
    
    # appkey extraída da página da aplicação do desenvolvedor
    appkey = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    
    # senha extraída da página da aplicação do desenvolvedor
    secret = "XXXXXXXXXXXXXXXX"
    
    # url das APIs
    url = "https://int.api.firstdata.com"
    
    # endpoint da API
    endpoint = "/wsm/merchantinfo/accounts"
    
    # body que será enviado (requests POST)
    body = {
        "paymentAccountInfo":
        [
            {
                "requestType": "U",
                "institutionNumber": "00000003",
                "merchantId": "89450179",
                "accountTypeId": "063",
                "bankNumber": "341",
                "branchNumber": "002978",
                "rbsAccountNumber": "248111",
                "paymentAccountNumber": "89450179",
                "userId": "100066"
            }
        ]
    }
    json_body = json.dumps(body)
    
    # token obtido através da API de Token
    token = "505f890a-2498-472c-9033-d46e3eb18206"
    
    # data no formato "Unix epoch time" em milissegundos
    dt = str(int(time.time() * 1000))
    
    # string alfanumérica gerada pelo usuário
    client_request_id = ''.join(random.choice(string.ascii_uppercase +
                                              string.ascii_lowercase +
                                              string.digits) for _ in range(50))
    
    # concatenação de strings
    string_to_sign = appkey + client_request_id + dt + json_body
    
    # encriptação da string usando hmac com sha256
    encoded_signature = base64.b64encode(hmac.new(secret.encode(),
                                         string_to_sign.encode(),
                                         sha256).digest()).decode()
    
    # string que será enviada como parâmetro "Message-Signature" no header
    hmac_auth = encoded_signature
    
    # dicionário "headers" com todos os parâmetros que serão enviados no request
    headers = {
        'auth': token,
        'Api-Key': appkey,
        'Timestamp': dt,
        'Message-Signature': hmac_auth,
        'Client-Request-Id': client_request_id,
        'Content-Type': 'application/json'
    }
    
    # envio do request e impressão do resultado (status e body)
    r = requests.post(url + endpoint, json=body, headers=headers)
    print(f'Status: {r.status_code}\n'
          f'Body: {json.dumps(r.json(), indent=4, sort_keys=True, ensure_ascii=False)}')
```
    

#### 6.2.2 Saida

```json
    Status: 200
    Body: {
        "processMessage": [
            {
                "accounts": {
                    "accountInformation": null
                },
                "institutionNumber": "00000003",
                "response": {
                    "responseInformationList": [
                        {
                            "responseCode": "11",
                            "responseCodeDesc": "Update already executed"
                        }
                    ]
                },
                "userId": "100066"
            }
        ]
    }
```

### 6.3 Amostra em Shell script

#### 6.3.1 Código

```shell
    #!/bin/bash
    
    # appkey extraída da página da aplicação do desenvolvedor
    APPKEY="XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" # Api Key (ou chave publica) gerada pelo Apigee
    
    # senha extraída da página da aplicação do desenvolvedor
    SECRET="XXXXXXXXXXXXXXXX" # Chave secreta gerada pelo APIGEE
    
    # url da API
    URL="https://int.api.firstdata.com/wsm/merchantinfo/accounts"
    
    # body que será enviado (JSON)
    JSON='{"paymentAccountInfo":[{"requestType":"U","institutionNumber":"00000003","merchantId":"89450179","accountTypeId":"063","bankNumber":"341","branchNumber":"002978","rbsAccountNumber":"248111","paymentAccountNumber":"89450179","userId":"100066"}]}'
    
    # data no formato "Unix epoch time" em milissegundos
    DATE=`Date +%s%3N` #Data em formato epoch
    
    # string alfanumérica gerada pelo usuário
    CLIENT_REQUEST_ID=`head /dev/urandom | tr -dc A-Za-z0-9 | head -c 50 ; echo ''`
    
    # token obtido através da API de Token
    TOKEN="cc1b5188-7cc8-4640-b7b5-44113215d8a3"
    
    # concatenação de strings
    STRING_TO_SIGN=$APPKEY$CLIENT_REQUEST_ID$DATE$JSON
    
    # encriptação da string usando hmac com sha256
    ENCODED_SIGNATURE=$(echo -n $STRING_TO_SIGN | openssl sha256 -binary -hmac $SECRET | base64) #Estamos com suporte ao sha256 somente
    
    # string que será enviada como parâmetro "Message-Signature" no header
    HMAC_AUTH=$ENCODED_SIGNATURE
    
    # envio do request com parâmetros no header e o body
    curl -H "auth: $TOKEN" -H "Api-Key: $APPKEY" -H "Timestamp: $DATE" -H "Message-Signature: $HMAC_AUTH" -H "Client-Request-Id: $CLIENT_REQUEST_ID" -H "Content-Type: application/json" -d "$JSON" -X POST $URL
```
    
#### 6.3.2 Saída

```json
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   462    0   218  100   244    102    114  0:00:02  0:00:02 --:--:--   217
    {"processMessage":[{"institutionNumber":"00000003","userId":"100066","response":{"responseInformationList":[{"responseCode":"11","responseCodeDesc":"Update already executed"}]},"accounts":{"accountInformation":null}}]}
```

 [1]: APIsSupportDocuments.md
 [2]: TokenGenerationforWeb-services.md
 [3]: https://www.getpostman.com
