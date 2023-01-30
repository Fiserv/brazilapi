# APIs - Introduction

## 1. Introduction

This document describes the APIs REST, and resources provided by Fiserv. Our systems are reliable with great performance and elasticity. As business leader in payment segment, the APIs will provide an opportunity to developers to take advantage of our ecosystem of payments and our extensive infrastructure.

Our APIs have paths URI and use native HTTP authentication, providing safety access to the information. Do HTTP requests using GET and POST methods, receive responses in JSON format. This enables that developers use any programming language to access our APIs, integrate in a safety manner and build various different applications.

## 2. First Step

To start using our APIs, the developer shall follow the steps listed below:

  1. Create an user/password in the Dev Portal;
  2. When ready the understanding of APIs and development of integration is in progress, please request the Token User/Password through the box **FiservDigitalBrazil@firstdatacorp.com.br.**When sending the e-mail, please, provide: 
      * **Subject**: Requisição de Usuário para Autenticação;
      * Body: 
          * **Your name / Company Name**
          * **Company Description**: Brief Description of your Company;
          * **Fiserv Representative**: Who from commercial Fiserv team works with your company;
          * **Institution***: What Institution the integration is related, for instance: BIN, Sicredi, etc.;
          * **Service Contract***: Service Contract registered within Fiserv;
          * **E-mail**: Institutional e-mail contact;
          * **Phone Number**: Institutional phone number contact;
          * **Comments**: Any comment you would like to add, related to the integration project;
  3. Use the values of User/Password to generate an Authentication Token. The Token will be used to access the APIs
  4. Create an application in [Application][1] menu, and select the products you wish to have access
  5. Use the available values for "Consumer Key" and "Consumer Secret" in application page (they will be required to access the APIs)

_* If you are not aware of your Institution or Service Contract, please contact your Fiserv representative._


### 2.1 Generating Token

To generate the Token, you should access the page with specific instructions [Token][2].

**Important&nbsp;Notice:**

* The custody of the credentials is the responsibility of the client;
* Do not share the Credentials with whom must not have access to them;
* Store the Token Credentials in an specific Vault. Avoid storing in the same application database that will consume resources;
* In case of compromise/lost your credentials, you must contact us, immediately;

#### 2.1.1 Request

To receive the Token, you need to send a POST request for "https://int.api.firstdata.com/sba/token" with the following parameters:

Content-Type | Local | Type | Value 
------------ | ----- | ---- | -----
Content-Type | Header| String| application/x-www-form-urlencoded
Body         | Body  | String| grant_type=password&client_id=estabelecimento&username={user_name}&password={password}

where:

* user_name: the user name you got in the step #1
* password: the password you got in the step #1

#### 2.1.2 Response

The POST response is a JSON with the following information:

Name| Type |Description | Sample
----|------|------------|-------
access_token | string | Token to be used in all other APIs | "eyJhbGciOiJS [...] swVHg6f8opW1DzuWsilvQ"
expires_in   | integer| Token expiration time|  62208000
refresh_expires_in| integer| Expiration time for refresh token| 1800
refresh_token|string|Token to be used to refresh the authentication| "eyJhbGciOiJS [...] G8LSL7qfJLmLU59O8sskw"
token_type|string|Token type|bearer
id_token| string|Token Id|"eyJhbGciOiJS [...] 3utG5l7qC0ieC--oLzGQ6w"
not-before-policy|integer||0
session-state|string|Session Status|"3a639a9b-2c42-472e-911b-209f9e01e077"
      
Where access_token is the value to be used to in all other APIs.

Sample of response

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

Fiserv APIs use [HMAC Authentication][3].

To be able to access any of the Codes or APIs described in this platform, it will require the credentials sent by e-mail. Don't have the credentials? [Create your account][4] and request an User/Password sending an e-mail to the box **FiservDigitalBrazil@firstdatacorp.com.br**.

You shall send the following parameters in header for each APIs request:

Name | Description 
---- | --------
auth | Generated Token
Api-Key| Key got from developer application
Timestamp|Date in "Unix epoch time" format - milisseconds
Message-Signature|HMAC authentication string
Client-Request-Id|Alphanumeric string randomly generated

### 3.1 auth

In **auth** parameter - header - the user must sent the value got in [Token details][5].

Token sample: "eyJhbGciOiJSUzI1NiIsInR5cCIgO [...] tFlCzaiEx06ETdkwoTC-CfzSaVA"

### 3.2 Api-Key

Key got from developer [application][1] page

Sample: "X0GW3QGOYFn4r7DHcVC8KuatUnNs6MGB"

### 3.3 Timestamp

Current date in "Unix epoch time" format in milliseconds, that consists in a number in milliseconds since from 00:00 - 1st January, 1970.

Sample: 1568914752721

### 3.4 Message-Signature

HMAC string authentication.

To generate this string, the user needs to follow the below steps:

  1. Get the key from developer application - appKey
  2. Get the password from the developer application - secret
  3. Generate the date in Unix epoch format - milliseconds (Date)
  4. Generate a randomly alphanumeric string (Client-Request-Id)
  5. Create the JSON that will be sent in body (POST request)
  6. Concatenate all the instructions items 1, 3, 4 e 5: appKey + Client + Date + JSON
  7. Encrypt the result from item #6 (OpenSSL base64 using the secret - item #2 - as HMAC key)
  8. Provide the string #7 as Parameter "Message-Signature"

### 3.5 Client-Request-Id

Alphanumeric string generated by the user.

Sample: "6NYHj9Pgyxc84rmafpwG4fjhLwFfJYFVmxJOcgziD0QN3q6mGC"

## 4. Status Codes for APIs REST

Fiserv returns HTTP standard code status, for successful and unsuccessful requests. See the below table for more detais:

Code| Description | Meaning
----| ------ | --------
200 | OK |Successfully
400 | Bad Request| Validation Error. The server was not able to process the request, due to any problem that might be coming from customer side. Ex: parameters or body request
401 | Unauthorized | Authentication error related to HMAC or Token. Be sure the access Token is valid and not expired
403 | Forbidden | The user is not authorized to access the APIs
404 | Not found |APIs URI is incorrect or the resource might not be found
409 | Conflict| The request cannot be completed due to a conflict with destination resource
500 | Internal Server Error|Fiserv server might be down or unresponsive
502 | Bad Gateway | The server (gateway or proxy) received an invalide response by an internal server
      
## 5. Request GET

### 5.1 Postman Sample

Following an example of a GET Request from CEP Verification, using [Postman][6].

![Imagem mostrando a URL utilizada, com o CEP sendo igual a 04794000](../assets/images/cep-url.JPG "cep")

The parameters in header are:

![Headers metodo GET](../assets/images/new_headers_get_0.PNG "header")

The response is:

![Print do Postman mostrando a resposta do request GET feito. Na resposta, é possível ver o resultado mostrando que o CEP é na cidade de São Paulo](../assets/images/reponse.JPG "response")

### 5.2 Sample in Python 3

#### 5.2.1 Code

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

#### 5.2.2 Output

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
    
### 5.3 Sample in Shell script

#### 5.3.1 Code

```json
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
    
#### 5.3.2 Output

```json

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   249    0   249    0     0     86      0 --:--:--  0:00:02 --:--:--    86
    {"code":200,"description":"Search done successfully!","cepResponse":{"state":"SP","city":"São Paulo","neighborhood":"Vila Gertrudes","address":"Avenida das Nações Unidas","complement":"- lado ímpar","zipCode":"04794000","genericZipCode":"Não"}}
```

## 6. Request POST

### 6.1 Sample in Postman

Following in POST request for address update in [Postman][6]

![Imagem mostrando a URL da API de atualização de endereços](../assets/images/address-url.JPG "post request")

The parameters in header are:

![Headers Método POST](../assets/images/new_headers_post.PNG "post header")

The JSON bory is:

![Imagem representando o JSON enviado no body do request POST](../assets/images/body_post.JPG "post body")

The response is:

![Imagem mostrando o body da resposta do request POST](../assets/images/response_post.JPG "post response")

### 6.2 Sample of Python 3

#### 6.2.1 Code

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
    

#### 6.2.2 Output

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

### 6.3 Sample of Shell script

#### 6.3.1 Code

```json
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
    
#### 6.3.2 Output

```json
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   462    0   218  100   244    102    114  0:00:02  0:00:02 --:--:--   217
    {"processMessage":[{"institutionNumber":"00000003","userId":"100066","response":{"responseInformationList":[{"responseCode":"11","responseCodeDesc":"Update already executed"}]},"accounts":{"accountInformation":null}}]}
```

 [1]: https://docs-qa.firstdata.com/org/brazilapi/user/apps
 [2]: https://docs-qa.firstdata.com/org/brazilapi/node/1146
 [3]: https://tools.ietf.org/html/draft-cavage-http-signatures-00#section-2.1.2.2
 [4]: https://docs-qa.firstdata.com/org/brazilapi/user/register
 [5]: #gerando-token-sba
 [6]: https://www.getpostman.com
