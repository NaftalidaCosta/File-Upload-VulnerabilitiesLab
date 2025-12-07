
# Vulnerabilidades em File Uploads — Estudos e Explorações Práticas

Este repositório documenta meu estudo e exploração prática de vulnerabilidades em mecanismos de **file upload**, baseados nos laboratórios da plataforma **PortSwigger Web Security Academy**, especificamente no curso *File Upload Vulnerabilities*.  
O objetivo deste projeto é consolidar meu entendimento técnico sobre vetores de exploração relacionados a upload de arquivos, validação insuficiente, bypasses comuns e execução remota de código (RCE).

---

## Objetivos do Projeto

- Entender como aplicações lidam com uploads e onde estas implementações falham.  
- Identificar vetores de ataque que permitem **execução remota de código (RCE)** via upload malicioso.  
- Realizar **bypass de validações no lado do cliente e no lado do servidor**.  
- Manipular headers e parâmetros usando **Caido** e **Burp Suite Community Repeater**.  
- Documentar metodologias e evidências com imagens obtidas durante cada etapa dos labs.

---

## Laboratórios Executados

### **1. Remote Code Execution via Upload de Arquivo Malicioso**
Neste laboratório explorei uma falha clássica de upload inseguro.  
Realizei:
- Upload de um payload PHP (`.php`) contendo código simples para execução remota.  
- Manipulação do tipo MIME e da lógica de validação.  
- Descoberta do diretório onde os arquivos eram armazenados.  
- Execução da payload no servidor, confirmando o RCE.

O foco foi compreender como a aplicação validava a extensão e como era possível contornar essas proteções com um arquivo aparentemente inofensivo, mas funcional para execução.

---

### **2. Bypass de Filtragem Baseada em Content-Type**
Neste laboratório, a aplicação bloqueava uploads maliciosos analisando o header **Content-Type**.  
Para explorar a vulnerabilidade, realizei:

- Captura da requisição no **Caido** e no **Burp Suite Repeater**.  
- Modificação manual do header `Content-Type` para contornar o filtro.  
- Envio do arquivo malicioso após o bypass.  
- Execução final do arquivo dentro da aplicação.




https://github.com/user-attachments/assets/dff434e8-1437-4f40-96b9-e2670297cf37





Este exercício demonstrou que validações baseadas exclusivamente em headers são insuficientes e facilmente manipuláveis via ferramentas de interceptação.

---

## 📸 Evidências (18 Imagens)
No diretório `/evidencias`, incluí:

- Descrição completa dos labs  
- Passo a passo de cada exploração  
- Prints do cenário final com os labs concluídos na plataforma da PortSwigger  
- Requisições modificadas no Caido e Burp Suite  
- Execução bem-sucedida dos arquivos enviados

Cada imagem está acompanhada de um arquivo `.md` explicando o que foi feito em cada etapa.

---

## 🛠 Ferramentas Utilizadas

- **Burp Suite Community (Repeater)**  
- **Caido (Intercept + Replay)**  
- **PHP Payloads para execução remota de comandos**  
- **Ambiente de laboratório PortSwigger Web Security Academy**

---

## Conhecimentos Aplicados

- Validação e sanitização de inputs  
- Manipulação de headers HTTP  
- Bypass de validações no lado do cliente  
- Bypass de validações baseadas em MIME type  
- Upload de arquivos executáveis  
- Localização e execução de payloads em diretórios públicos  
- Entendimento de vetores que levam a RCE

---

## 📚 Fonte de Estudo
Todo o conhecimento aplicado aqui foi obtido diretamente do curso **File Upload Vulnerabilities — PortSwigger Web Security Academy**, complementado por testes práticos e exploração manual.

---

## 📄 Licença
Projeto disponível sob licença MIT.  
Sinta-se à vontade para usar este conteúdo como referência educacional.

## Créditos
PortSwigger: `https://portswigger.net/web-security/learning-paths`
