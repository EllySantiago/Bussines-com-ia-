# Pixel Art Studio: prompts para evoluir o app no Gemini

Conjunto de regras e prompts prontos para colar no Gemini, um por vez, a partir do app v9. Cada prompt traz o objetivo, o texto para enviar e um teste rápido para você conferir o resultado. Data: 12/10/2026.

## 1. Como usar

1. Abra uma conversa nova no Gemini. Cole primeiro o **Prompt base** (seção 2), depois o código completo do v9.
2. Envie **um prompt de melhoria por vez** (seção 3). Pedir tudo junto aumenta o risco de o Gemini quebrar o que já funcionava.
3. Teste com o checklist do prompt. Se passar, salve o arquivo como a próxima versão (v10, v11...) no seu repositório e use esse código no prompt seguinte.
4. Se algo quebrar, volte à versão anterior e reenvie o prompt com a descrição do erro e um print.
5. Ordem sugerida: P1, P2, P3, P4, P5 primeiro (corrigem o que existe e destravam a venda); depois P6 a P10; por fim P11 e P12.

Por que um de cada vez: modelos de IA tendem a reescrever partes que você não pediu quando recebem muitas mudanças juntas. Mudanças pequenas deixam fácil descobrir qual ajuste causou o problema.

## 2. Prompt base (colar sempre no início)

```
Você é um desenvolvedor front-end sênior. Vou te enviar um app web de conversão de imagem em pixel art e pedir melhorias uma de cada vez.

Regras fixas para todas as respostas:
1. Entregue sempre o arquivo HTML COMPLETO e funcional, em um único arquivo, com CSS e JavaScript embutidos. Nada de trechos soltos.
2. Não use bibliotecas externas, CDN, servidor nem requisições de rede. O app deve funcionar offline e nenhuma imagem pode sair do dispositivo.
3. Mantenha o visual e o layout atuais (tema escuro, duas colunas, botão de download embaixo da imagem, botão discreto de Dicas com modal). Só altere a interface onde o pedido exigir.
4. Não remova nenhuma função existente (galeria, câmera, modos, sliders, download, dicas).
5. Funcione bem no celular (toque, câmera traseira) e no computador.
6. Use nomes claros e comentários curtos em português nas partes alteradas.
7. Ao final, liste em até 8 linhas o que mudou e como testar.
8. Antes de responder, revise mentalmente casos extremos: imagem sem carregar, imagem retangular, imagem muito grande, PNG com transparência, celular na vertical.
9. Se o pedido for ambíguo, escolha a opção mais simples e diga qual escolheu.

Aguarde o código e o primeiro pedido.
```

## 3. Prompts de melhoria

### P1. Fundo transparente de verdade (substitui o "modo rascunho" atual)

Objetivo: PNG com fundo transparente sem furar áreas claras dentro do desenho.

```
Problema: o Modo Rascunho atual pinta o fundo de branco opaco. Quero fundo transparente. Antes ele ficava furado porque o limiar comparava todos os pixels da imagem, inclusive os claros dentro do desenho.

Peço: troque a lógica por remoção de fundo por preenchimento a partir das bordas (flood fill):
- Pegue a cor de fundo estimada nos cantos e bordas da imagem já reduzida.
- Percorra a partir das bordas e torne transparentes (alpha 0) apenas os pixels conectados à borda cuja cor esteja dentro de uma tolerância da cor de fundo.
- Pixels claros que ficam dentro do desenho, sem conexão com a borda, devem ser preservados.
- Renomeie o modo para "Remover fundo" e o slider para "Tolerância do fundo" (0 a 100, padrão 30).
- Adicione um fundo xadrez cinza e branco por trás da prévia para eu ver a transparência. Esse xadrez NÃO deve aparecer no PNG baixado.
- O PNG baixado deve manter o canal alpha.
```

Teste: faça o desenho de uma flor branca com miolo claro sobre papel branco. O fundo some e o miolo continua. O xadrez aparece na tela mas não no arquivo baixado.

### P2. Manter proporção (sem esticar a imagem)

```
Problema: imagens retangulares são esticadas para ficar quadradas ao reduzir.

Peço: adicione um seletor "Enquadramento" com duas opções:
1. "Recortar no centro" (padrão): corta o excesso para ficar quadrado, sem distorcer.
2. "Ajustar com margem": mantém a imagem inteira, centralizada, e preenche as sobras com transparência.
A grade final continua quadrada (resolução escolhida no slider). Nunca estique a imagem.
```

Teste: carregue uma foto horizontal. Um círculo desenhado deve continuar redondo.

### P3. Pixels mais nítidos (redução por bloco)

```
Problema: a redução usa drawImage direto, que faz média e deixa as bordas embaçadas e as cores sujas.

Peço: implemente a redução manualmente a partir da imagem em alta resolução:
- Divida a imagem enquadrada em uma grade NxN (N = resolução do slider).
- Para cada bloco, calcule a cor representativa. Ofereça um seletor "Amostragem" com: "Nítida" (cor mais frequente do bloco, com agrupamento aproximado das cores) e "Suave" (média do bloco). Padrão: Nítida.
- Preserve o alpha: se a maior parte do bloco for transparente, o pixel final é transparente.
- Mantenha o desempenho aceitável no celular com fotos grandes (reduza antes se a imagem passar de 2000 px no lado maior).
```

Teste: o contorno de um desenho fica reto e nítido; compare "Nítida" e "Suave" com a mesma foto.

### P4. Paleta real em vez de arredondar canais

```
Problema: o slider "Nível de cores" arredonda R, G e B separadamente, o que cria cores sem relação entre si.

Peço:
- Troque por quantização por paleta real, implementada em JavaScript puro (algoritmo median cut ou k-means simples). O slider passa a ser "Número de cores" (2 a 32).
- Mostre, abaixo da prévia, as cores da paleta usada como quadradinhos.
- Adicione o seletor "Paleta": "Extraída da imagem" (padrão), "Game Boy (4 tons de verde)", "16 cores clássicas" e "Paleta personalizada" (até 16 cores escolhidas por seletor de cor). Defina você mesmo as cores das paletas predefinidas; não copie paletas de terceiros que tenham licença restritiva.
- Quando uma paleta predefinida estiver ativa, mapeie cada pixel para a cor mais próxima dela (distância de cor perceptual simples).
```

Teste: com 6 cores, a imagem tem exatamente até 6 cores distintas e os quadradinhos batem com a imagem.

### P5. Exportar no tamanho certo

```
Problema: o download é sempre 512x512 e não serve para quem usa o sprite em jogos.

Peço: troque o botão de download por um bloco de exportação com opções:
- Tamanho nativo (ex.: 32x32), 4x, 8x e 16x, sempre ampliados por vizinho mais próximo, sem suavização.
- Botão "Baixar este tamanho" e botão "Baixar todos (4 PNGs)".
- Campo de texto para o nome do item e para o prefixo do kit. Nome do arquivo no formato prefixo_item_32x32.png, em letras minúsculas, sem acentos ou espaços.
- Mantenha o botão de download principal logo abaixo da imagem, como está hoje.
```

Teste: o PNG nativo abre com exatamente a resolução escolhida; o 8x é 8 vezes maior com bordas duras.

### P6. Contorno e dithering

```
Peço duas opções novas, desligadas por padrão:
1. "Contorno": adiciona um contorno de 1 pixel ao redor das partes não transparentes, com cor escolhida por seletor de cor. Deve respeitar a grade (1 pixel da arte, não 1 pixel da tela).
2. "Dithering": aplica pontilhado ordenado (matriz de Bayer) na quantização, com intensidade ajustável de 0 a 100.
Ambas devem funcionar junto com a paleta escolhida e com a transparência.
```

Teste: o contorno não aparece no que é fundo; o dithering não aparece com intensidade 0.

### P7. Várias imagens de uma vez, com ZIP

```
Peço processamento em lote:
- O seletor de arquivos aceita várias imagens. Mostre miniaturas pixeladas em uma grade, cada uma com um campo para o nome do item.
- Todas usam as mesmas configurações atuais. Um botão "Aplicar a todas" reprocessa tudo ao mudar um ajuste.
- Botão "Baixar kit (ZIP)" que gera um arquivo ZIP com pastas png_1x, png_4x e png_8x, nomes no padrão prefixo_itemNN_nome_tamanho.png.
- Como não posso usar bibliotecas externas, implemente você mesmo um gerador de ZIP simples (sem compressão, formato store) em JavaScript, com CRC32.
- Não trave o celular: processe uma imagem por vez.
```

Teste: 6 imagens geram um ZIP que abre normalmente no computador com as pastas e nomes certos.

### P8. Sprite sheet e capa de vitrine

```
Peço duas funções para o lote:
1. "Gerar sprite sheet": junta todos os itens em uma única imagem, em grade, com espaçamento de 1 pixel transparente entre eles, e inclui no ZIP junto com um arquivo spritesheet.json com nome, posição e tamanho de cada item.
2. "Gerar capa": cria uma imagem de 1200x800 com fundo de cor sólida escolhida, os itens ampliados e distribuídos de forma organizada, título do kit em texto (campo editado por mim) e uma marca d'água discreta com o meu nome (campo de texto). A capa é baixada separada do ZIP.
Use apenas fontes do sistema ou desenhe o texto com pixels, sem fontes externas.
```

Teste: a capa baixada tem 1200x800, mostra os itens sem borrão e leva a marca d'água.

### P9. Gerador de licença e leia-me

```
Peço um botão "Gerar documentos do kit" que inclui no ZIP dois arquivos de texto:
1. LEIA-ME.txt: nome do kit, autora, quantidade de itens, resolução nativa, número de cores, lista de arquivos e pastas, data, e como usar o sprite sheet.
2. LICENCA.txt: monte a partir de campos que eu preencho (nome da autora, nome do kit, tipo: Pessoal ou Comercial, contato). Estrutura: o que o comprador pode fazer, o que não pode (revender, redistribuir os arquivos originais, usar para treinar modelos de IA), que a autoria permanece com a autora, ausência de garantia e um aviso curto de que o texto é um modelo.
Escreva os textos em português claro e deixe tudo fácil de editar no código.
```

Atenção: o texto gerado pelo Gemini é um modelo. Leia, ajuste ao que você realmente quer permitir e, se o kit render bem, peça revisão a um advogado.

### P10. Fotos de celular na orientação certa

```
Problema: fotos tiradas no celular às vezes aparecem giradas.
Peço: respeite a orientação da foto ao carregar (use createImageBitmap com imageOrientation from-image quando disponível) e adicione dois botões pequenos para girar 90 graus à esquerda e à direita. O resultado deve atualizar na hora.
```

Teste: uma foto vertical aparece em pé; os botões giram sem esticar.

### P11. Salvar e repetir configurações

```
Peço um painel discreto "Presets":
- Salvar as configurações atuais (modo, resolução, cores, paleta, contorno, dithering, enquadramento, tolerância do fundo) com um nome.
- Escolher um preset salvo para reaplicar tudo.
- Exportar e importar presets como arquivo JSON, para eu manter o mesmo estilo em todos os kits.
- Guarde os presets em localStorage e proteja com try/catch, caso o navegador bloqueie.
```

Teste: salve um preset, recarregue a página e reaplique.

### P12. Versão pronta para vender

```
Peço o acabamento para distribuir o app como produto:
- Tela "Sobre" discreta (acessível por um link pequeno no rodapé) com nome do app, número da versão, minha marca, aviso de que todo processamento acontece no dispositivo e nenhuma imagem é enviada, e um resumo da licença de uso do app.
- Aviso claro quando o navegador não permitir a câmera ou o localStorage, sem quebrar o app.
- Revisão de acessibilidade básica: rótulos nos controles, foco visível, contraste suficiente.
- Mensagens de erro amigáveis para arquivo que não é imagem ou imagem muito grande.
- Confirme no código que não há nenhuma chamada de rede nem recurso externo, e liste o que verificou.
```

Teste: abra o arquivo com o Wi-Fi desligado; tudo funciona.

### P13. Revisão final como QA

```
Agora aja como engenheiro de QA. Leia o código completo e faça:
1. Uma lista de bugs prováveis, do mais grave ao mais leve, com a linha ou função.
2. Uma lista de 15 casos de teste manuais (imagem pequena, imagem enorme, PNG transparente, foto vertical, lote com 20 imagens, modo escuro do celular, cancelar a câmera etc.), com o resultado esperado de cada um.
3. Correções para os bugs graves, entregando o arquivo completo atualizado.
```

## 4. Regras gerais de qualidade para qualquer versão

- Cada versão nova deve passar nos testes das versões anteriores. Guarde a lista de testes no repositório.
- Nenhum recurso externo: o app deve abrir sem internet.
- A interface continua leve e simples, como você pediu: nada de painéis grandes ou textos longos fixos.
- Toda opção nova vem desligada ou com padrão seguro, para não mudar o resultado que você já aprovou.
- Registre no README de cada versão: o prompt usado, o que mudou e o que ficou pendente, como você já faz no repositório.

## 5. Quando o Gemini errar

- **Reescreveu partes que você não pediu:** volte à versão anterior e reenvie com a frase "altere somente X; não modifique mais nada".
- **Entregou só um trecho:** responda "envie o arquivo HTML completo, sem omitir nenhuma parte".
- **O resultado visual não mudou:** envie um print do antes e do depois e pergunte qual variável está sendo ignorada (foi o que aconteceu com o slider de paleta na v2).
- **Piorou:** peça que ele explique em três linhas por que a mudança deveria ajudar e depois compare com o teste. Isso ajuda você a entender a técnica e a pedir o próximo ajuste com mais precisão.
