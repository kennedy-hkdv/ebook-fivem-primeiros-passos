# Capítulo 1 — Como o FiveM funciona

Antes de escrever código, entenda as peças do jogo. Isso evita 80% da confusão de quem está começando.

## O que é o FiveM

O FiveM é uma modificação do GTA V que permite rodar **servidores multiplayer dedicados** com scripts, mapas e veículos customizados. Cada servidor define suas próprias regras por meio de *resources*.

## Resources

Um **resource** é uma pasta com arquivos que o servidor carrega: scripts, interface, mapas, modelos. Tudo no FiveM é um resource.

~~~
meu-script/
├── fxmanifest.lua   ← descreve o resource (obrigatório)
├── client.lua       ← código executado no computador do jogador
├── server.lua       ← código executado no servidor
└── config.lua       ← configurações (opcional)
~~~

## Client, Server e NUI

| Lado | Onde roda | Para que serve |
| --- | --- | --- |
| **Client** | PC de cada jogador | Tudo que é visual e local: personagem, câmera, veículos, teclas, marcadores |
| **Server** | Máquina do servidor | Regras, dados, banco de dados, economia, validações |
| **NUI** | Navegador embutido (HTML/CSS/JS) | Interfaces: menus, HUD, inventário |

Regra de ouro: **nunca confie no client**. O jogador controla o próprio computador; qualquer decisão importante (dar dinheiro, item, permissão) deve ser validada no **server**.

## Natives

**Natives** são funções do próprio jogo, como `GetEntityCoords`, `SetEntityHealth` ou `CreateVehicle`. Elas ficam documentadas em **docs.fivem.net/natives**. Algumas só funcionam no client, outras no server — a documentação informa o lado.

## Eventos

Os lados conversam por **eventos**: o client dispara um evento para o server e vice-versa (capítulo 4). Também existem eventos do próprio jogo, como `playerSpawned` ou `onResourceStart`.

## Frameworks

Muitos servidores usam um **framework** (como ESX, QBCore ou vRP) que fornece sistemas prontos — jogadores, dinheiro, inventário, empregos. Você pode:

- criar scripts **standalone** (sem framework): ótimos para aprender;
- ou integrar com o framework do servidor para o qual vai desenvolver.

Neste ebook usamos **standalone**, para você entender a base antes de qualquer framework.

## Onde consultar

- **Documentação oficial:** docs.fivem.net
- **Lista de natives:** docs.fivem.net/natives
- **Fórum da Cfx.re:** forum.cfx.re

## Checklist do capítulo

- [ ] Sei o que é um resource
- [ ] Sei a diferença entre client, server e NUI
- [ ] Sei por que a validação importante deve ficar no server

---

⬅️ [Índice](../README.md) · ➡️ [Capítulo 2 — Montando o servidor](02-montando-o-servidor.md)
