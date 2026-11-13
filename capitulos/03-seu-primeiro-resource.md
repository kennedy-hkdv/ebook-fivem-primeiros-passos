# Capítulo 3 — Seu primeiro resource

Vamos criar um resource simples que cumprimenta o jogador e tem um comando.

## 1. Crie a pasta

Dentro de `resources`, crie `meu-primeiro-script`.

## 2. O `fxmanifest.lua`

Todo resource precisa deste arquivo. Ele diz ao FiveM o que carregar.

~~~lua
fx_version 'cerulean'
game 'gta5'
lua54 'yes'

author 'Seu Nome'
description 'Meu primeiro resource'
version '1.0.0'

client_script 'client.lua'
server_script 'server.lua'
~~~

- `fx_version` e `game` são obrigatórios.
- `lua54 'yes'` ativa o Lua 5.4.
- Use `client_scripts {...}` / `server_scripts {...}` para vários arquivos.

## 3. O `server.lua`

~~~lua
print('[meu-primeiro-script] iniciado no servidor!')
~~~

## 4. O `client.lua`

~~~lua
-- Mensagem no chat quando o jogador entra
AddEventHandler('playerSpawned', function()
  TriggerEvent('chat:addMessage', {
    args = { 'Servidor', 'Bem-vindo! Digite /ola para testar.' }
  })
end)

-- Comando /ola
RegisterCommand('ola', function(source, args, rawCommand)
  TriggerEvent('chat:addMessage', {
    args = { 'Servidor', 'Olá! Seu primeiro script está funcionando.' }
  })
end, false)
~~~

Explicando:

- `AddEventHandler` registra uma função para um evento.
- `RegisterCommand(nome, função, restrito)` cria um comando. O último parâmetro `false` deixa o comando liberado para todos; `true` exige permissão (ACE).
- `TriggerEvent('chat:addMessage', ...)` usa o resource de chat padrão para exibir texto.

## 5. Inicie o resource

No `server.cfg`, adicione:

~~~cfg
ensure meu-primeiro-script
~~~

Ou, com o servidor rodando, digite no console do servidor:

~~~
refresh
ensure meu-primeiro-script
~~~

Entre no jogo e digite `/ola` no chat.

## 6. Ciclo de desenvolvimento

1. Edite o código.
2. Salve.
3. No console: `restart meu-primeiro-script`.
4. Teste no jogo.

Se algo falhar, olhe o **console do servidor** (erros de server) e o **F8** dentro do jogo (erros de client).

## Erros comuns

| Sintoma | Causa provável |
| --- | --- |
| "Could not find resource" | Esqueceu o `refresh` ou a pasta está no lugar errado |
| Nada acontece ao digitar o comando | O script não foi iniciado (`ensure`) ou deu erro (veja o F8) |
| "attempt to call a nil value" | Nome de função/native escrito errado ou usado do lado errado (client vs server) |

## Desafios

1. Faça o comando `/ola` aceitar um nome: `/ola Maria` → "Olá, Maria!" (use `args[1]`).
2. Crie um comando `/coords` que mostra as coordenadas do jogador (pesquise a native `GetEntityCoords` e `PlayerPedId()`).
3. Mostre a mensagem de boas-vindas apenas uma vez por entrada.

---

⬅️ [Capítulo 2](02-montando-o-servidor.md) · ➡️ [Capítulo 4 — Eventos e comunicação](04-eventos-e-comunicacao.md)
