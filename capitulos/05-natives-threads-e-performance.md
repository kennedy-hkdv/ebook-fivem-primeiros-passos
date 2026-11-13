# Capítulo 5 — Natives, threads e performance

## Natives

Natives são as funções do jogo. Exemplos comuns (client):

~~~lua
local ped = PlayerPedId()                 -- personagem do jogador
local coords = GetEntityCoords(ped)       -- posição (vector3)
local vida = GetEntityHealth(ped)
SetEntityHealth(ped, 200)
~~~

Como ler a documentação de uma native em **docs.fivem.net/natives**:

1. Veja o **nome** e os **parâmetros** com seus tipos.
2. Veja o que ela **retorna**.
3. Confira se é **client** ou **server**.

## Vetores

Posições usam `vector3`:

~~~lua
local pos = vector3(25.0, -1347.0, 29.5)
local dist = #(GetEntityCoords(PlayerPedId()) - pos)   -- distância entre dois pontos
~~~

O operador `#` aplicado à diferença de dois vetores retorna a distância.

## Threads

Para tarefas que precisam rodar repetidamente, use `CreateThread`:

~~~lua
CreateThread(function()
  while true do
    Wait(1000)
    print('Passou 1 segundo')
  end
end)
~~~

**Nunca** crie um `while true` sem `Wait`: ele trava o jogo.

## Performance: o erro nº 1 dos iniciantes

Um loop com `Wait(0)` roda **a cada frame** (dezenas de vezes por segundo). Usá-lo o tempo todo deixa o resource pesado — você verá isso no **resmon** (`resmon 1` no F8), que mostra o consumo de cada resource em ms.

### Técnica: espera dinâmica

Só rode a cada frame quando for **realmente necessário**:

~~~lua
local local_loja = vector3(25.0, -1347.0, 29.5)

CreateThread(function()
  while true do
    local espera = 1000                       -- padrão: verifica 1x por segundo
    local pos = GetEntityCoords(PlayerPedId())

    if #(pos - local_loja) < 10.0 then       -- só perto da loja…
      espera = 0                              -- …verifica a cada frame
      DrawMarker(1, local_loja.x, local_loja.y, local_loja.z - 1.0,
        0.0, 0.0, 0.0, 0.0, 0.0, 0.0,
        1.5, 1.5, 0.5, 0, 150, 255, 120,
        false, false, 2, false, nil, nil, false)
    end

    Wait(espera)
  end
end)
~~~

Resultado: longe da loja o script quase não consome nada.

### Outras dicas

- Guarde em variáveis o que não muda (como `PlayerPedId()` em loops que rodam raramente).
- Prefira **eventos** a loops de verificação (em vez de perguntar "mudou?" toda hora, reaja quando acontecer).
- Evite criar objetos, tabelas ou strings grandes dentro de loops de frame.

## Desafios

1. Crie um marcador em um local do mapa, visível só quando o jogador estiver a menos de 20 metros.
2. Meça o consumo do seu resource no `resmon` com `Wait(0)` fixo e depois com espera dinâmica. Compare.
3. Mostre uma mensagem quando o jogador entrar na área do marcador (e só uma vez).

---

⬅️ [Capítulo 4](04-eventos-e-comunicacao.md) · ➡️ [Capítulo 6 — Boas práticas e próximos passos](06-boas-praticas-e-proximos-passos.md)
