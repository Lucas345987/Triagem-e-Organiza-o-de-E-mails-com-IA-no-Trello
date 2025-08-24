# Projeto: Triagem e Organização de E-mails com IA no Trello

Este projeto descreve um fluxo de trabalho automatizado para ler, resumir e organizar e-mails recebidos, transformando-os em tarefas acionáveis em um quadro do Trello. A automação utiliza a inteligência artificial do Google Gemini para interpretar o conteúdo dos e-mails e direcioná-los para o setor correto.

## Sobre o Projeto

O objetivo principal desta automação é otimizar o gerenciamento da caixa de entrada, garantindo que nenhuma solicitação importante seja perdida e que cada e-mail seja tratado pela equipe correta. O sistema funciona como um assistente inteligente que lê cada e-mail, entende seu propósito, cria um resumo e o classifica, enviando a tarefa para o quadro Trello apropriado (Comercial, Financeiro ou Técnico).

## Funcionalidades Principais

  * **Monitoramento em Tempo Real:** O fluxo é iniciado instantaneamente (`Gmail Trigger`) assim que um novo e-mail chega na caixa de entrada monitorada.
  * **Inteligência Artificial Dupla:** Utiliza dois modelos `Google Gemini Chat` em sequência para uma análise aprofundada:
    1.  O primeiro modelo gera um resumo conciso do e-mail.
    2.  O segundo modelo classifica o resumo em uma das categorias predefinidas (Comercial, Financeiro, Técnico).
  * **Classificação Automática:** Com base no conteúdo do e-mail, o sistema decide qual é o departamento responsável pela demanda.
  * **Roteamento Inteligente (Switch):** Direciona a tarefa para a coluna correta do Trello de acordo com a classificação feita pela IA.
  * **Criação de Tarefas no Trello:** Cria automaticamente um novo card no Trello com o resumo do e-mail, garantindo que a equipe tenha todas as informações necessárias para agir.

## Fluxo de Trabalho Detalhado

O processo é linear e eficiente, seguindo os passos abaixo:

1.  **Gmail Trigger:** A automação é ativada no momento exato em que um novo e-mail é recebido.
2.  **Basic LLM Chain (Resumo):** O conteúdo completo do e-mail (corpo, assunto) é enviado para o primeiro modelo `Google Gemini Chat`. O prompt para este modelo é focado em extrair a essência da mensagem, por exemplo: *"Resuma o seguinte e-mail em poucas linhas, focando no principal pedido ou informação."*
3.  **Basic LLM Chain 1 (Classificação):** O resumo gerado na etapa anterior é passado para o segundo modelo `Google Gemini Chat`. O prompt aqui é focado em categorização, como: *"Classifique o texto a seguir em uma das três categorias: 'comercial', 'financeiro' ou 'tecnico'."*
4.  **Edit Fields:** Este nó provavelmente extrai e formata o resultado da classificação (a palavra "comercial", "financeiro" ou "tecnico") para ser usada na próxima etapa.
5.  **Switch (Roteamento):** Usando a categoria obtida na etapa anterior, este nó direciona o fluxo para um dos três caminhos possíveis:
      * Se a categoria for `comercial` -\> segue para o Trello do time Comercial.
      * Se a categoria for `financeiro` -\> segue para o Trello do time Financeiro.
      * Se a categoria for `tecnico` -\> segue para o Trello do time Técnico.
6.  **Criação de Card no Trello (Comercial / Financeiro / Técnico):** No final de cada caminho, um nó se conecta à API do Trello e cria um novo card no quadro e na lista correspondente. O título e a descrição do card contêm o resumo do e-mail, proporcionando contexto imediato para a equipe.
   
<img width="1426" height="507" alt="image" src="https://github.com/user-attachments/assets/c993ea14-bb58-47b1-abf6-3a874f7b60b3" />

## Tecnologias Utilizadas

  * **Plataforma de Automação:** n8n 
  * **Serviço de E-mail:** Gmail API (via Trigger).
  * **Inteligência Artificial:** Google Gemini Chat (dois modelos em cadeia).
  * **Gerenciamento de Projetos:** Trello API.
