# Bussines-com-ia-
minhas criações com inteligencias artificiais classificadas por aplicativo e resultado produzido, além de observações e anotações sobre o fluxo de criação, historico de versões disponivel
## objetivos:
### acompanhar a evolução das ideias a cada prompt
### acompanhar as mudanças e possibilidades de melhoria encontradas conforme contexto inicial vai sendo passado a outro modelo de ia

## testes
- cada versao ( V.U , Versao para Uso, V1 ate V9 ) em seu uso pratico
- a versao com o claude foi feita e testada com o proprio claude e revisada ja pronta
- a versao gemini e claude foi para fins de teste do claude gerando requisitos e o gemini como o dev da linha de produção
- ferramenta para transformar fotos e desenhos em pixel arte, funciona offline e aceita imagens tanto da camera quanto da galeria
## observaçoes 
- tempo pra fazer e revisar : cerca de 6h
- pontos de melhoria do processo: ter alguma ferramenta para testes, começar com documento de testes simples
- outro ponto de melhoria para economizar tempo, usar dois chats do gemini, um pra refinação e pesquisa e outro pra puro codigo bruto, debater com o chat de refinamento enquanto o de dev trabalha
- pontos a serem analisados, conferencia de fontes e delirio da ia, gemini se mostrou muito confiante em respostas sem checagem em fontes
- uma correção pra questao do refinamento é usar a versao web disponibilizada pelo navegador (versao gemini que auxilia pesquisas google e não versao web site proprio), nessa versao fica disponivel fontes, tanto em videos quanto sites para verificação manual
- claude, melhora e economia de tokens gritante apos configuraçao de skills personalizadas oferecidas pela anthropic, skills utilizadas visualmente notaveis: revisao juridica, dev frontend, revisao de texto, revisao financeira e habilidade de marketing.

## possibilidades de melhoria do fluxo 13/10/2026
- estudar como funcionam as integrações omniroute para serem mimetizadas na tomada de decisão humana
- estudar ferramentas e seu funcionamento para gerenciamento de fluxo de ia
- ferramentas de segurança e desenvolvimento
- obs: esses pontos são para melhorar o senso critico humano sobre o que a ia esta fazendo e executando, pra execuçao de codigo mais pesado copilot é a melhor decisão observada
## copilot e observaçoes pra uso
- no futuro na inserção do copilot no projeto o fluxo deve alternar de claude prompt(com revisao humana)>gemini dev> claude revisor de execução para gemini prompt e refinamento da ideia> claude revisor de documentos e de projeto proposto> copilot fica responsavel por toda a parte de desenvolvimento e a checagem de codigo fica manual( o claude separa a aplicação por prompt e a cada implementaçao a aplicação é testada manualmente e o codigo é revisado manualmente pra o caso de inserçao de codigo malicioso via contaminação da ia no prompt ou no proprio copilot, ate mesmo na propria maquina).
- simplificação da cadeia de comando necessaria, definição de tarefas necessita de mais precisão
