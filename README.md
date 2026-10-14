# Bussines-com-ia-
minhas criações com inteligencias artificiais classificadas por aplicativo e resultado produzido, além de observações e anotações sobre o fluxo de criação, historico de versões disponivel
## objetivos:
### acompanhar a evolução das ideias a cada prompt
### acompanhar as mudanças e possibilidades de melhoria encontradas conforme contexto inicial vai sendo passado a outro modelo de ia

## testes
- cada versão ( V.U , Versão para Uso, V1 ate V9 ) em seu uso prático
- a versão com o claude foi feita e testada com o proprio claude e revisada ja pronta
- a versão gemini e claude foi para fins de teste do claude gerando requisitos e o gemini como o dev da linha de produção
- ferramenta para transformar fotos e desenhos em pixel arte, funciona offline e aceita imagens tanto da camêra quanto da galeria
## observações 
- tempo pra fazer e revisar : cerca de 6h
- pontos de melhoria do processo: ter alguma ferramenta para testes, começar com documento de testes simples.
- outro ponto de melhoria para economizar tempo, usar dois chats do gemini, um pra refinação e pesquisa e outro pra puro codigo bruto, debater com o chat de refinamento enquanto o de dev trabalha.
- pontos a serem analisados, conferência de fontes e delirio da ia, gemini se mostrou muito confiante em respostas sem checagem em fontes
- uma correção pra questao do refinamento é usar a versão web disponibilizada pelo navegador (versao gemini que auxilia pesquisas google e não versao web site proprio), nessa versão fica disponivel fontes, tanto em videos quanto sites para verificação manual.
- claude, melhora e economia de tokens gritante apos configuraçao de skills personalizadas oferecidas pela anthropic, skills utilizadas visualmente notaveis: revisão jurídica, dev frontend, revisão de texto, revisão financeira e habilidade de marketing.

## possibilidades de melhoria do fluxo 13/10/2026
- estudar como funcionam as integrações omniroute para serem mimetizadas na tomada de decisão humana.
- estudar ferramentas e seu funcionamento para gerenciamento de fluxo de ia.
- ferramentas de segurança e desenvolvimento.
- obs: esses pontos são para melhorar o senso crítico humano sobre o que a ia esta fazendo e executando, pra execução de código mais pesado copilot é a melhor decisão observada.
## copilot e observações pra uso
- no futuro na inserção do copilot no projeto o fluxo deve alternar de claude prompt(com revisão humana)>gemini dev> claude revisor de execução para gemini prompt e refinamento da ideia> claude revisor de documentos e de projeto proposto> copilot fica responsável por toda a parte de desenvolvimento e a checagem de código fica manual( o claude separa a aplicação por prompt e a cada implementação a aplicação é testada manualmente e o código é revisado manualmente pra o caso de inserção de código malicioso via contaminação da ia no prompt ou no próprio copilot, até mesmo na própria máquina).
- simplificação da cadeia de comando necessária, definição de tarefas necessita de mais precisão.
