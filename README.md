# Paciência

Paciência Klondike minimalista, feita em um único arquivo HTML, sem dependências. Funciona no celular e no computador e só sorteia jogos que têm solução.

## Recursos

- **Klondike clássico**, com compra de 1 ou de 3 cartas (escolha em *Opções*).
- **Só gera jogos vencíveis**: cada distribuição passa por um solucionador embutido antes de a partida começar.
- **Jogue como preferir**: arraste as cartas ou toque em uma carta para movê-la automaticamente para o melhor destino.
- **Dica**: destaca uma jogada possível.
- **Desfazer**: botão na tela ou `Ctrl/Cmd + Z`.
- **Reiniciar**: volta ao começo da *mesma* distribuição. **Novo jogo** embaralha tudo de novo.
- **Organização automática**: quando não sobra nenhuma carta oculta nas pilhas, o jogo manda as cartas para as fundações sozinho.
- **4 temas**: Névoa, Grafite, Floresta e Marinho.
- **Estatísticas**: partidas, vitórias, recorde de pontos e melhor tempo.
- **Salva o andamento** automaticamente no navegador; você pode fechar a aba e continuar depois.
- Interface em português do Brasil.

## Como jogar

O objetivo é levar todas as 52 cartas para as quatro fundações (no topo), formando sequências do Ás ao Rei de cada naipe.

- Nas sete pilhas de baixo, monte sequências **decrescentes e em cores alternadas** (por exemplo, um 6 vermelho sobre um 7 preto).
- Só um **Rei** (ou uma sequência começando por um Rei) pode ocupar uma coluna vazia.
- Clique no monte para virar cartas para o descarte. Quando o monte acaba, clicar nele de novo recicla o descarte.
- Cartas viradas para cima podem ser movidas em sequência. Ao esvaziar a parte oculta de uma pilha, a próxima carta é virada.

### Pontuação

| Ação | Pontos |
| --- | --- |
| Carta para a fundação | +10 |
| Carta do descarte para uma pilha | +5 |
| Virar uma carta oculta de uma pilha | +5 |
| Carta da fundação de volta para uma pilha | −15 |
| Reciclar o descarte | −100 (compra de 1) ou −20 (compra de 3) |
| Bônus de tempo ao vencer (partidas com mais de 30 s) | 700000 ÷ tempo em segundos |

A pontuação nunca fica negativa. A organização automática não tem custo.

## Como usar

Não há instalação nem etapa de build. Baixe o repositório e abra o `index.html` no navegador.

Para publicar com o **GitHub Pages**: em *Settings → Pages*, escolha a branch principal e a pasta raiz. O endereço fica no formato `https://SEU-USUARIO.github.io/NOME-DO-REPOSITORIO/`.

## Detalhes técnicos

- HTML, CSS e JavaScript puros em um único arquivo (`index.html`, cerca de 37 KB).
- Movimentos de arrastar usam *pointer events*, então o mesmo código serve para mouse e toque.
- O progresso, as opções e as estatísticas ficam no `localStorage` do navegador (chaves `pac.game`, `pac.cfg` e `pac.stats`). Limpar os dados do site apaga tudo isso.

### Como o jogo garante que há solução

Antes de cada partida, o jogo embaralha, monta a distribuição e roda uma busca em profundidade com memorização de estados para tentar vencê-la, enxergando todas as cartas. Se não achar solução (ou estourar o limite de 8000 estados ou 400 ms), sorteia outra distribuição.

Pontos que vale conhecer:

- **"Tem solução" não significa "fácil".** O solucionador vê as cartas ocultas; quem joga não vê. A vitória existe, mas pode exigir jogadas bem pensadas.
- O solucionador é conservador: ele não usa o movimento de tirar cartas da fundação de volta para as pilhas. Todas as soluções que ele encontra são jogáveis, mas ele pode descartar uma distribuição que só seria vencível com esse movimento.
- Há um limite de 2,5 segundos para encontrar uma distribuição. Se estourar (algo que não apareceu nos meus testes), o jogo usa a última sorteada para não travar.
- O **Reiniciar** depende de a distribuição inicial ter sido salva. Partidas salvas por versões anteriores do jogo não a têm; nelas, o botão avisa e sugere *Novo jogo*.
- A organização automática só move cartas para as fundações. Se uma carta precisar sair de cima de outra para liberar a próxima, o jogo para e deixa a jogada com você.

## Como este projeto foi feito

O jogo foi desenvolvido por **Tainara Martins** com o apoio do **Claude**, assistente de IA da Anthropic. O processo foi conversacional: eu defini o que queria (botão de reiniciar, organização automática ao acabarem as cartas ocultas e geração apenas de jogos vencíveis), o Claude escreveu e alterou o código, e eu testei o resultado jogando.

O que foi e o que não foi verificado:

- Os testes foram **manuais**, feitos por mim jogando.
- O solucionador e a organização automática também foram exercitados em simulações rápidas rodadas fora do navegador, durante o desenvolvimento. Esses scripts **não fazem parte do repositório**, e o projeto **não tem suíte de testes automatizados**.
- Não há garantia de funcionamento em todos os navegadores e dispositivos.

Contribuições, relatos de problemas e sugestões são bem-vindos pelas *issues*.

## Licença

Distribuído sob a [licença MIT](LICENSE). Você pode usar, copiar, modificar e distribuir o código, mantendo o aviso de copyright.

© 2026 Tainara Martins
