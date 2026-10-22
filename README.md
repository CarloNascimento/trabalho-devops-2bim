#  Canil Patas Unidas - Trabalho Devops 2bim

Site institucional do **Canil Patas Unidas**, voltado à divulgação do trabalho do canil e ao incentivo à doação e adoção de animais. O projeto também foi usado para aplicar práticas de **DevOps**, com integração contínua, geração automática de changelog e publicação automatizada de releases.


##  Equipe

- Carlo Nascimento (Us: CarloNascimento) - RA 14664
- Thainá de Sá (Us: Thaina-de-Sa) - RA 250280

## Estrutura do repositório

```
.
├── .github/workflows/    # Workflows de automação (GitHub Actions)
├── CHANGELOG.md          # Histórico de mudanças (gerado automaticamente)
├── Canil Patas Unidas _ Doe e salve vidas.html   # Página principal do site
└── README.md             # Documentação do projeto
```

## Automações implementadas (GitHub Actions)

O repositório conta com três workflows na pasta `.github/workflows`:

| Workflow | Quando executa | O que faz |

| **CI** | A cada push no repositório | Valida o projeto automaticamente após cada alteração. |
| **Changelog** | A cada push na branch `main` | Gera e atualiza o arquivo `CHANGELOG.md` a partir do histórico de commits, agrupando por categoria (novidades, correções, documentação e outros). 
| **Release** | Ao publicar uma release (ou manualmente, via *Run workflow*) | Empacota o projeto em `.zip` e `.tar.gz` e anexa os arquivos à release correspondente. |

##  Changelog

O arquivo (./CHANGELOG.md) é atualizado automaticamente e organiza as mudanças por tipo. Para que o agrupamento funcione, as mensagens de commit seguem o padrão **Conventional Commits**:
	-Exemplo: `feat: adiciona seção de adoção de animais`

## Releases e versionamento

As versões do projeto seguem o Versionamento Semântico
- **MAIOR** (`2.0.0`): mudanças grandes que podem quebrar compatibilidade
- **MENOR** (`1.1.0`): novas funcionalidades compatíveis com a versão anterior
- **CORREÇÃO** (`1.0.1`): correções pequenas

### Como publicar uma nova versão

1. Acesse **Code → Releases → Draft a new release**.
2. Crie uma nova tag no formato exemplo: `v1.1.0`
3. Clique em **Generate release notes** para gerar as notas automaticamente.
4. Clique em **Publish release**.
5. O workflow **Release** é disparado e anexa à release os pacotes `canil-patas-unidas-v1.1.0.zip` e `canil-patas-unidas-v1.1.0.tar.gz`.

As versões publicadas estão disponíveis na aba do repositório.

## Como usar

1. Acesse a aba **Releases** e baixe o pacote `.zip` da versão desejada.
2. Extraia os arquivos.
3. Abra o arquivo `Canil Patas Unidas _ Doe e salve vidas.html` em qualquer navegador.

## Histórico de atividades realizadas

- Criação do repositório e do site do Canil Patas Unidas
- Configuração do workflow de **CI**
- Implementação do workflow de **Changelog** automático
- Implementação do workflow de **Release** com geração de pacotes `.zip` e `.tar.gz`
- Publicação da primeira versão (`v1.0.0`)
- Formalização da documentação no README

## Tecnologias e ferramentas

- HTML
- Git e GitHub
- GitHub Actions
- Conventional Commits
- Versionamento Semântico
