# API - Introducción

## 1. Introducción

Este documento describe las API REST y los recursos proporcionados por Fiserv. Nuestros sistemas son confiables con gran desempeño y elasticidad. Como líder empresarial en el segmento de pagos, las API brindarán una oportunidad a los desarrolladores para aprovechar nuestro ecosistema de pagos y nuestra amplia infraestructura.

Nuestras API tienen rutas URI y utilizan autenticación HTTP nativa, lo que proporciona un acceso seguro a la información. Realice solicitudes HTTP utilizando métodos GET y POST, reciba respuestas en formato JSON. Esto permite que los desarrolladores utilicen cualquier lenguaje de programación para acceder a nuestras API, integrarlas de manera segura y crear varias aplicaciones diferentes.

## 2. Primer paso

Para comenzar a utilizar nuestras API, el desarrollador deberá seguir los pasos que se detallan a continuación:

1. Cree un usuario/contraseña en Dev Portal;
2. Cuando esté listo, la comprensión de las API y el desarrollo de la integración estén en progreso, solicite el Usuario/Contraseña del Token a través de la casilla **FiservDigitalBrazil@firstdatacorp.com.br.**Al enviar el correo electrónico, proporcione:
    * **Asunto**: Requisição de Usuário para Autenticação;
    * Cuerpo:
        * **Su nombre / Nombre de la empresa**
        * **Descripción de la Empresa**: Breve Descripción de su Empresa;
        * **Representante Fiserv**: Quien del equipo comercial Fiserv trabaja con su empresa;
        * **Institución***: A qué Institución se relaciona la integración, por ejemplo: BIN, Sicredi, etc.;
        * **Contrato de Servicio***: Contrato de Servicio registrado en Fiserv;
        * **Correo electrónico**: Correo electrónico de contacto institucional;
        * **Teléfono**: Teléfono de contacto institucional;
        * **Comentarios**: Cualquier comentario que desee agregar, relacionado con el proyecto de integración;
3. Utilice los valores de Usuario/Contraseña para generar un token de autenticación. El Token se utilizará para acceder a las API
4. Cree una aplicación en el menú [Aplicación][1] y seleccione los productos a los que desea tener acceso
5. Use los valores disponibles para "Clave de consumidor" y "Secreto de consumidor" en la página de la aplicación (serán necesarios para acceder a las API)

_* Si no conoce su Institución o Contrato de Servicio, por favor contacte a su representante de Fiserv._


### 2.1 Generación de token

Para generar el Token, debe acceder a la página con instrucciones específicas [Token Access](../api/?type=post&path=/token/).

**Aviso importante:**

* La custodia de las credenciales es responsabilidad del cliente;
* No compartir las Credenciales con quien no deba tener acceso a ellas;
* Almacene las credenciales de token en una bóveda específica. Evite almacenar en la misma base de datos de la aplicación que consumirá recursos;
* En caso de compromiso/pérdida de sus credenciales, debe contactarnos inmediatamente;

#### 2.1.1 Solicitud

Para recibir el Token, debe enviar una solicitud POST para "[Token Access](../api/?type=post&path=/token/)" con los siguientes parámetros:

| Content-Type | Local  | Type   | Value                                                                                  | 
|--------------|--------|--------|----------------------------------------------------------------------------------------|
| Content-Type | Header | String | application/x-www-form-urlencoded                                                      |
| Body         | Body   | String | grant_type=password&client_id=estabelecimento&username={user_name}&password={password} |

dónde:

* user_name: el nombre de usuario que obtuviste en el paso #1
* password: la contraseña que obtuviste en el paso #1

#### 2.1.2 Respuesta

La respuesta POST es un JSON con la siguiente información:

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

Donde access_token es el valor que se utilizará en todas las demás API.

Muestra de respuesta

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

## 3. Autenticación

Las API de Fiserv usan [autenticación HMAC][2].

Para poder acceder a cualquiera de los Códigos o APIs descritos en esta plataforma, se requerirán las credenciales enviadas por correo electrónico. ¿No tienes las credenciales? Cree su cuenta y solicite un Usuario/Contraseña enviando un e-mail a la casilla **FiservDigitalBrazil@firstdatacorp.com.br**.

Deberá enviar los siguientes parámetros en el encabezado de cada solicitud de API:

| Name              | Description                                     |
|-------------------|-------------------------------------------------|
| auth              | Generated Token                                 |
| Api-Key           | Key got from developer application              |
| Timestamp         | Date in "Unix epoch time" format - milisseconds |
| Message-Signature | HMAC authentication string                      |
| Client-Request-Id | Alphanumeric string randomly generated          |

### 3.1 autenticación

En el parámetro **auth** - encabezado - el usuario debe enviar el valor obtenido en [Detalles del token][2].

Muestra de token: "eyJhbGciOiJSUzI1NiIsInR5cCIgO [...] tFlCzaiEx06ETdkwoTC-CfzSaVA"

### 3.2 Clave API

Clave obtenida de la página del desarrollador [aplicación][1]

Muestra: "X0GW3QGOYFn4r7DHcVC8KuatUnNs6MGB"

### 3.3 Marca de tiempo

Fecha actual en formato "Unix epoch time" en milisegundos, que consiste en un número en milisegundos desde las 00:00 hasta el 1 de enero de 1970.

Muestra: 1568914752721

### 3.4 Mensaje-Firma

Autenticación de cadena HMAC.

Para generar esta cadena, el usuario debe seguir los siguientes pasos:

1. Obtenga la clave de la aplicación del desarrollador - appKey
2. Obtenga la contraseña de la aplicación del desarrollador - secreto
3. Genere la fecha en formato de época Unix - milisegundos (Date)
4. Genere una cadena alfanumérica aleatoria (Client-Request-Id)
5. Cree el JSON que se enviará en el cuerpo (solicitud POST)
6. Concatene todos los elementos de instrucciones 1, 3, 4 y 5: appKey + Client + Date + JSON
7. Cifre el resultado del elemento n.° 6 (OpenSSL base64 usando el secreto, elemento n.° 2, como clave HMAC)
8. Proporcione la cadena #7 como parámetro "Message-Signature"

### 3.5 ID de solicitud de cliente

Cadena alfanumérica generada por el usuario.

Muestra: "6NYHj9Pgyxc84rmafpwG4fjhLwFfJYFVmxJOcgziD0QN3q6mGC"

## 4. Códigos de estado para API REST

Fiserv devuelve el estado del código estándar HTTP, para solicitudes exitosas y no exitosas. Consulte la siguiente tabla para obtener más detalles:

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

## 5. Solicitud OBTENER

### 5.1 Ejemplo de cartero

Siguiendo un ejemplo de una solicitud GET de CEP Verification, usando [Postman][3].

![Imagem mostrando a URL utilizada, com o CEP sendo igual a 04794000](../assets/images/cep-url.JPG "cep")

Los parámetros en el encabezado son:

![Headers metodo GET](../assets/images/new_headers_get_0.PNG "header")

La respuesta es:

![Print do Postman mostrando a resposta do request GET feito. Na resposta, é possível ver o resultado mostrando que o CEP é na cidade de São Paulo](../assets/images/reponse.JPG "response")

### 5.2 Muestra enPython 3

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

#### 5.2.2 Producción

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
    
### 5.3 Muestra en script

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
    
#### 5.3.2 Producción

```json

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   249    0   249    0     0     86      0 --:--:--  0:00:02 --:--:--    86
    {"code":200,"description":"Search done successfully!","cepResponse":{"state":"SP","city":"São Paulo","neighborhood":"Vila Gertrudes","address":"Avenida das Nações Unidas","complement":"- lado ímpar","zipCode":"04794000","genericZipCode":"Não"}}
```

## 6. Solicitud POST

### 6.1 Ejemplo en Postman

Siguiendo en solicitud POST para actualización de dirección en [Postman][6]

![Imagem mostrando a URL da API de atualização de endereços](../assets/images/address-url.JPG "post request")

Los parámetros en el encabezado son:

![Headers Método POST](../assets/images/new_headers_post.PNG "post header")

El JSON bory es:

![Imagem representando o JSON enviado no body do request POST](../assets/images/body_post.JPG "post body")

La respuesta es:

![Imagem mostrando o body da resposta do request POST](../assets/images/response_post.JPG "post response")

### 6.2 Muestra de Python 3

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
    

#### 6.2.2 Producción

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

### 6.3 Ejemplo de script de Shell

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
    
#### 6.3.2 Producción

```json
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   462    0   218  100   244    102    114  0:00:02  0:00:02 --:--:--   217
    {"processMessage":[{"institutionNumber":"00000003","userId":"100066","response":{"responseInformationList":[{"responseCode":"11","responseCodeDesc":"Update already executed"}]},"accounts":{"accountInformation":null}}]}
```

 [1]: APIsSupportDocuments.md
 [2]: TokenGenerationforWeb-services.md
 [3]: https://www.getpostman.com
