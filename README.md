# freeZapApi
## _Simples API para WhatsApp_


[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)


- Type some Markdown on the left
- See HTML in the right
- ✨Magic ✨

| Creditos | Site |
| ------ | ------ |
| Projeto-TInject| [https://github.com/mikelustosa/Projeto-TInject][PlDb] |
| Horse | [https://github.com/HashLoad/horse][PlGh] |
| RESTRequest4Delphi | [https://github.com/viniciussanchez/RESTRequest4Delphi][PlGd] |


## Features

Recursos / Resources
✔️ Enviar mensagens de texto - Send text message

❌ Enviar mensagens de texto com botões - Send text message with buttons (NEW)

✔️ Enviar mensagens de texto para números fora da agenda- Send text message

✔️ WebHook

✔️ Enviar MP3 - Send MP3

✔️ Enviar MP4 - Send MP4

✔️ Enviar IMG - Send IMG

✔️ Enviar RAR - Send RAR

✔️ Configurações de DDI - International number configuration

✔️ Checagem de conexão - check connection

❌ Enviar botões na conversa - Send buttons in chat(Not functional in WhatsApp Multi devices Beta)

## Installation

Baixe e extrair o conteúdo no diretório “C:\freeZapApi”, para iniciar basta executar o freeZapApi.exe, pronto agora só usar 😊.


## Endpoint

- ⚠️Atenção porta do serviço http "8089"

- Status [GET : http://localhost:8089/status]

```sh
Retorno JSON http code :200
{
  'status': 'Conectado'
}
```
```sh
Retorno JSON http code :400
{
  "error": "Exception",
  "description": "Desconectado"
}
```
- qrCode [GET : http://localhost:8089/qrcode]
```sh
Retorno JSON http code :200
{
  "qrcode": "iVBORw0KGgoAAAANSUhEUgAAAQgAAAEICAY.. (string base64)"
}
```
```sh
Retorno JSON http code :400
{
  "error": "Exception",
  "description": "Desconectado"
}
```

- send Text [POST : http://localhost:8089/sendtext]
```sh
Body JSON
{
	"fone" :"55DDNNNNNNNN@c.us",
	"texto": " Ola _Como_ pode ver esse ```texto``` esta com ~formatação~ para o *whatsapp* "
}
```
```sh
Retorno JSON http code :200
{
  "status": "Sucesso"
}
```
```sh
Retorno JSON http code :400
{
  "error": "Exception",
  "description": "Desconectado"
}
```

- send File [POST : http://localhost:8089/sendfile/55DDNNNNNNNN@c.us]
```code
Exemplo JavaScript
const form = new FormData();
form.append("files", "C:\\Users\\usuario\\Desktop\\foto.jpg");
form.append("files", "C:\\Users\\usuario\\Desktop\\pdf_sample_2.pdf");

fetch("http://localhost:8089/sendfile/55DDNNNNNNNN@c.us", {
  "method": "POST",
  "headers": {
    "Content-Type": "multipart/form-data; boundary=---011000010111000001101001"
  }
})
.then(response => {
  console.log(response);
})
.catch(err => {
  console.error(err);
});
```

```code
Exemplo PHP
<?php
$client = new http\Client;
$request = new http\Client\Request;

$body = new http\Message\Body;
$body->addForm(null, [
  [
    'name' => 'files',
    'type' => null,
    'file' => 'C:\\Users\\usuario\\Desktop\\foto.jpg',
    'data' => null
  ],
  [
    'name' => 'files',
    'type' => null,
    'file' => 'C:\\Users\\usuario\\Desktop\\pdf_sample_2.pdf',
    'data' => null
  ]
]);

$request->setRequestUrl('http://localhost:8089/sendfile/55DDNNNNNNNN@c.us');
$request->setRequestMethod('POST');
$request->setBody($body);

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```
```sh
Retorno JSON http code :200
{
  "upload_files_request": 1,
  "files": [
    {
      "filename": "foto.jpg",
      "size": 27851,
      "md5": "427b00a5dcce92d4dccb53e1260317db",
      "status": "ok"
    }
  ],
  "uploaded_files": 1
}
```
```sh
Retorno JSON http code :400
{
  "error": "Exception",
  "description": "Desconectado"
}
```

- WebHook [POST]
Para facilitar a integração com outras linguagens de programação foi implementado um serviço de WebHook toda vez que o WhatsApp receber uma mensagem do tipo chat ele enviar ou POST no formato JSON para a URL configurada, caso tenha falha no envio não será feita uma nova tentativa.

```sh
Request
{
  "chat_id": "55DDNNNNNNNN@c.us",
  "fone": "DDNNNNNNNN",
  "contact": "",
  "type": "chat",
  "message": "Mensagem recebida"
}
```

