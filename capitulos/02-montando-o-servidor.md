# Capítulo 2 — Montando seu servidor de testes

Para testar seus scripts você precisa de um servidor local. O jeito mais simples é usar o **txAdmin**, painel de gerenciamento que já vem junto com o servidor.

> ⚠️ Os passos e telas podem mudar com novas versões. Em caso de dúvida, consulte **docs.fivem.net** (seção *Server Manual*).

## Requisitos

- Windows ou Linux (aqui usamos Windows para teste local)
- Uma conta na **Cfx.re** (gratuita)
- O jogo GTA V instalado para entrar no servidor e testar

## Passo a passo

1. **Baixe o servidor (FXServer artifacts)** na página oficial de downloads do FiveM e extraia em uma pasta, por exemplo `C:\FXServer\server`.
2. **Execute o `FXServer.exe`.** Uma janela de console abre e o painel txAdmin fica disponível no navegador (geralmente em `http://localhost:40120`). O console mostra um **PIN** para o primeiro acesso.
3. **Faça login** com sua conta Cfx.re e siga o assistente de configuração.
4. **Escolha uma receita (*recipe*)**. Para aprender, escolha uma base simples; o txAdmin baixa e configura tudo.
5. **Informe a chave de licença** do servidor, gerada gratuitamente no portal da Cfx.re.
6. **Inicie o servidor** pelo painel e conecte pelo FiveM usando `localhost:30120` (no menu de conexão direta).

## O arquivo `server.cfg`

É a configuração central do servidor. Os trechos mais importantes:

~~~cfg
sv_hostname "Meu Servidor de Testes"
sv_maxclients 4

# Resources que o servidor deve iniciar
ensure mapmanager
ensure chat
ensure meu-script
~~~

- `ensure nome` inicia o resource (e o reinicia se ele já estiver rodando).
- A **ordem** importa: coloque dependências antes de quem as usa.
- Comentários começam com `#`.

## Onde ficam os resources

Dentro da pasta do servidor existe a pasta `resources`. Seus scripts ficam nela, por exemplo:

~~~
resources/
└── [meus]/
    └── meu-script/
~~~

Pastas entre colchetes `[ ]` são apenas **categorias**: servem para organizar e podem ser iniciadas de uma vez com `ensure [meus]`.

## Comandos úteis no console do servidor

| Comando | O que faz |
| --- | --- |
| `refresh` | Relê a lista de resources do disco (use após criar um novo) |
| `start nome` | Inicia um resource |
| `stop nome` | Para um resource |
| `restart nome` | Reinicia um resource (ideal para testar alterações) |
| `ensure nome` | Inicia ou reinicia |

## Checklist do capítulo

- [ ] Servidor local iniciado pelo txAdmin
- [ ] Consegui entrar nele pelo FiveM
- [ ] Entendi `ensure`, `refresh` e `restart`

---

⬅️ [Capítulo 1](01-como-o-fivem-funciona.md) · ➡️ [Capítulo 3 — Seu primeiro resource](03-seu-primeiro-resource.md)
