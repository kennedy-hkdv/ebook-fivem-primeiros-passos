# Capítulo 4 — Eventos e comunicação client ↔ server

Client e server são programas separados. Eles se comunicam por **eventos**.

## Eventos locais

Disparados e ouvidos no **mesmo lado**:

~~~lua
AddEventHandler('meuEvento', function(texto)
  print('Recebi:', texto)
end)

TriggerEvent('meuEvento', 'olá')
~~~

## Eventos de rede

Para atravessar a rede, o evento precisa ser registrado com `RegisterNetEvent`.

### Client → Server

**client.lua**

~~~lua
RegisterCommand('pedirbonus', function()
  TriggerServerEvent('meuscript:pedirBonus')
end, false)
~~~

**server.lua**

~~~lua
RegisterNetEvent('meuscript:pedirBonus', function()
  local src = source          -- ID do jogador que disparou
  print(('Jogador %d pediu bônus'):format(src))
end)
~~~

### Server → Client

**server.lua**

~~~lua
-- para UM jogador
TriggerClientEvent('meuscript:mensagem', src, 'Você recebeu um bônus!')

-- para TODOS (-1)
TriggerClientEvent('meuscript:mensagem', -1, 'Anúncio geral!')
~~~

**client.lua**

~~~lua
RegisterNetEvent('meuscript:mensagem', function(texto)
  TriggerEvent('chat:addMessage', { args = { 'Servidor', texto } })
end)
~~~

## A variável `source`

No server, dentro de um evento de rede, `source` é o **ID do jogador** que disparou o evento. Ele é definido pelo próprio FiveM e **não pode ser falsificado** pelo cliente. Guarde-o logo no início:

~~~lua
RegisterNetEvent('meuscript:acao', function()
  local src = source   -- guarde em uma variável local
  -- ...
end)
~~~

> Dentro de funções assíncronas (como depois de um `Wait`), `source` pode mudar. Por isso, copie para uma variável `local` antes.

## Nomeando eventos

Use um **prefixo** com o nome do resource: `meuscript:acao`. Isso evita conflitos com outros scripts.

## 🔒 Segurança: nunca confie no client

O client pode ser manipulado por trapaceiros. Eles podem disparar seus eventos de rede com **qualquer valor**.

❌ **Errado** — o client decide quanto dinheiro receber:

~~~lua
RegisterNetEvent('loja:pagar', function(valor)
  darDinheiro(source, valor)   -- o trapaceiro envia 999999999
end)
~~~

✅ **Certo** — o server decide e valida:

~~~lua
local PRECO_ITEM = 100

RegisterNetEvent('loja:comprar', function(itemId)
  local src = source
  if type(itemId) ~= 'number' then return end   -- valida o tipo
  -- o server confere preço, saldo, distância, etc.
  cobrar(src, PRECO_ITEM)
end)
~~~

Regras práticas:

1. **Valide todo argumento** recebido do client (tipo, faixa de valores).
2. **Calcule no server**: preço, quantidade, recompensa.
3. **Confira o contexto**: o jogador está perto da loja? Tem o item? Tem permissão?
4. Evite criar eventos que "dão" algo diretamente sem checagem.

## Desafios

1. Crie o comando `/bonus` que dispara um evento para o server, que responde com uma mensagem ao jogador.
2. Faça o server enviar um anúncio para todos quando um jogador usar `/anunciar texto` (restrinja com permissão).
3. Revise um evento seu e liste o que um trapaceiro poderia enviar para se beneficiar.

---

⬅️ [Capítulo 3](03-seu-primeiro-resource.md) · ➡️ [Capítulo 5 — Natives, threads e performance](05-natives-threads-e-performance.md)
