## Quero criar uma automação no N8N para automação de controle de solicitações de suporte.

## Público ou responsável:
Equipe de suporte/TI.

## Resultado esperado:
Quando um colaborador enviar uma solicitação, o chamado deve ser registrado automaticamente, receber um número de protocolo e a equipe de suporte deve ser notificada para realizar o atendimento.

## Ferramentas envolvidas:
Google Forms — abertura do chamado
Google Sheets — controle dos chamados
Gmail — notificações
n8n — automação do processo

## Fluxo desejado:
1. Receber uma nova solicitação enviada pelo formulário.
2. Capturar nome, e-mail, setor, assunto e descrição do problema.
3. Verificar se todos os campos obrigatórios foram preenchidos.
4. Classificar o chamado de acordo com o assunto informado.
5. Gerar um número único de protocolo.
6. Registrar o chamado no Google Sheets.
7. Enviar um e-mail para a equipe de suporte com os dados do chamado.
8. Enviar um e-mail ao colaborador confirmando a abertura e informando o número do protocolo.
9. Caso existam informações obrigatórias ausentes, informar o colaborador sobre a necessidade de corrigir a solicitação.

## Regras importantes:
 * Nome, e-mail, setor e descrição do problema são obrigatórios.
 * Cada chamado deve possuir um protocolo único.
 * O chamado deve registrar data e hora da abertura.
 * Chamados com informações incompletas não devem ser registrados.
 * A equipe de suporte deve receber uma notificação para cada novo chamado válido.
 * O colaborador deve receber uma confirmação após o cadastro.
 * A automação deve permitir identificar chamados novos e chamados com erro.
 
 ### Nós que devem ser utilizados

1. **Google Forms / Webhook — Entrada do chamado**

   * Responsável por receber os dados enviados pelo colaborador.
   * Os principais dados são: nome, e-mail, setor, assunto e descrição do problema.

2. **Edit Fields (Set) — Organizar os dados**

   * Organiza os dados recebidos e cria informações adicionais, como data e hora da solicitação.
   * Também pode preparar os campos que serão enviados para os próximos nós.

3. **IF — Validar os dados**

   * Verifica se os campos obrigatórios foram preenchidos.
   * Validar se o e-mail possui um formato válido.
   * Se os dados forem inválidos, o fluxo segue para uma etapa de aviso ao usuário.
   * Se forem válidos, o fluxo continua normalmente.

4. **Code — Gerar protocolo**

   * Cria um número único para identificar o chamado.
   * O protocolo será utilizado tanto na planilha quanto nos e-mails enviados.

5. **Google Sheets — Registrar chamado**

   * Adiciona uma nova linha na planilha com as informações do chamado.
   * A planilha pode conter: protocolo, data, hora, nome, e-mail, setor, categoria, descrição e status.

6. **Gmail — Notificar equipe de suporte**

   * Envia um e-mail para a equipe de TI informando que um novo chamado foi aberto.
   * O e-mail deve conter o protocolo e as principais informações da solicitação.

7. **Gmail — Confirmar abertura para o colaborador**

   * Envia uma confirmação para o solicitante.
   * A mensagem informa que o chamado foi registrado e apresenta o número do protocolo.

8. **Gmail — Informar erro ou solicitação inválida**

   * Caso o formulário esteja incompleto ou apresente dados inválidos, o colaborador recebe uma mensagem explicando o problema.

### Lógica do workflow

O funcionamento será:

**Colaborador envia formulário**
↓
**n8n recebe os dados**
↓
**Organiza as informações**
↓
**Valida os dados**
↓
**Dados válidos?**

**Não** → Envia mensagem informando o problema → **Fim**

**Sim** → Gera protocolo → Registra no Google Sheets → Notifica equipe de TI → Envia confirmação ao colaborador → **Fim**

### Exemplo de estrutura

`Formulário/Webhook → Edit Fields → IF (Validação)`

**IF = Não**
`→ Gmail (Solicitação inválida)`

**IF = Sim**
`→ Code (Gerar protocolo) → Google Sheets → Gmail (Equipe de TI) → Gmail (Colaborador)`
