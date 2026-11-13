# Capítulo 6 — Boas práticas e próximos passos

## Organização do resource

~~~
meu-script/
├── fxmanifest.lua
├── config.lua          ← valores ajustáveis
├── client/
│   └── main.lua
├── server/
│   └── main.lua
├── shared/
│   └── utils.lua       ← funções usadas por ambos os lados
└── html/               ← interface NUI (se houver)
~~~

Manifest correspondente:

~~~lua
shared_script 'config.lua'
client_script 'client/main.lua'
server_script 'server/main.lua'
~~~

## Configuração separada da lógica

Evite números "mágicos" espalhados pelo código. Centralize:

~~~lua
Config = {
  Idioma = 'pt-BR',
  DistanciaInteracao = 2.5,
  Preco = 150,
}
~~~

## Estilo de código

- Variáveis e funções **locais** sempre que possível.
- Nomes descritivos e consistentes (escolha `camelCase` ou `snake_case` e mantenha).
- Funções pequenas, com uma única responsabilidade.
- Comentários explicam o **porquê**.

## Depuração

- **Console do servidor:** erros do lado server.
- **F8 no jogo:** erros do lado client.
- Use `print()` para rastrear valores; remova os prints antes de publicar.
- Leia a mensagem de erro com calma: ela informa o arquivo e a linha.
- Teste uma coisa por vez e use `restart nome-do-resource` para reiniciar rapidamente.

## Controle de versão

Use **Git** e **GitHub**: guarde cada resource em um repositório, faça commits pequenos e com mensagens claras, e use *branches* para novas funcionalidades.

## Checklist antes de publicar um script

- [ ] Nenhum evento dá algo sem validação no server
- [ ] Sem `while true` sem `Wait`
- [ ] Consumo medido no `resmon`
- [ ] Configurações em `config.lua`
- [ ] README com instruções de instalação
- [ ] Prints de debug removidos

## 🚀 Próximos passos

1. **Interfaces:** aprenda NUI com HTML, CSS e JavaScript.
2. **Banco de dados:** persista dados com MySQL.
3. **Framework:** estude o framework do servidor em que quer trabalhar.
4. **Projetos:** monte um portfólio com 2 ou 3 scripts bem documentados no GitHub.

Continue para o ebook **[FiveM: NUI e MySQL](https://github.com/kennedy-hkdv/ebook-fivem-nui-e-mysql)** e veja também a lista de **[recursos úteis](https://github.com/kennedy-hkdv/fivem-recursos-uteis)**.

---

⬅️ [Capítulo 5](05-natives-threads-e-performance.md) · 🏠 [Índice](../README.md)
