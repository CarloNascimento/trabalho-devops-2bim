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




	                                                                         #  Explicação Actions - Changelog / helloworld / release





jobs:								-> Uma única tarefa, chamada changelog, que roda numa máquina Ubuntu do GitHub.
  changelog:
    runs-on: ubuntu-latest



- uses: actions/checkout@v4				-> Baixa o repositório com todo o histórico de commits e tags. Aqui o fetch-depth: 0 é essencial: o script lê o histórico do Git para montar o changelog.
  with:
    fetch-depth: 0



- uses: actions/setup-python@v5 		-> Deixa o Python 3.12 pronto na máquina para rodar o script do próximo passo.
  with:
    python-version: "3.12"


- name: Gerar changelog 				-> O script Python está embutido direto no workflow (o <<'PY' envia o código para o python sem precisar de um arquivo separado). Em resumo, o script:
  run: |								Lê o histórico do Git (commits e tags) e agrupa os commits pelo tipo do prefixo (feat:, fix:, docs: etc., padrão Conventional Commits).
  python - --latest <<'PY'				Organiza por versão: uma seção “Não lançado” (commits depois da última tag) e uma seção para cada tag, com a data.
  ... (script Python) ...				Traduz os tipos para categorias em português (Adicionado, Corrigido, Documentação...) e destaca alterações incompatíveis (! ou BREAKING CHANGE).
  PY									Ignora commits de merge e os do próprio changelog ([skip ci]).
										Gera links para cada commit e para a comparação entre versões, usando o repositório atual.
										Lista os contribuidores com a contagem de commits.
										Cria dois arquivos: CHANGELOG.md (completo) e, por causa do --latest, RELEASE_NOTES.md (só a seção mais recente).
										
										
    
    

- name: Commitar CHANGELOG.md			-> Roda só quando o gatilho foi um push em branch. Configura o usuário do bot do GitHub, adiciona o CHANGELOG.md e, se houve mudança, faz commit e push. O [skip ci]
- if: github.ref_type == 'branch'		e o paths-ignore evitam que esse commit dispare o workflow de novo em loop.
										


- name: Criar release					-> Roda só quando o gatilho foi uma tag (ex.: v1.2.0). Usa o GitHub CLI (gh) para criar uma release com o nome da tag, usando o RELEASE_NOTES.md como descrição. O GH_TOKEN é o token if: github.ref_type == 'tag'			automático do GitHub que autoriza essa ação.
 env:
    GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  run: gh release create "$GITHUB_REF_NAME" -F RELEASE_NOTES.md --title "$GITHUB_REF_NAME" 

  

											Resumo
											Push na main/master: gera o CHANGELOG.md e commita de volta no repositório.
											Push de uma tag v*: gera as notas e cria a release no GitHub.
											Ligação com o release.yml

											Quando este workflow cria a release (passo 5), ele dispara o release.yml que você mostrou antes (on: release: types: [published]), que por sua vez anexa o .zip e o .tar.gz. Mas 											atenção: releases criadas com o GITHUB_TOKEN normalmente não disparam outros workflows, então esse encadeamento pode não acontecer automaticamente. Se isso ocorrer, vale rodar o 											release.yml manualmente (workflow_dispatch) ou usar um token pessoal (PAT) na criação da release.




-------------------------------------------------------------------------------------------------------------------------------------------------------------------



jobs:			-> é onde ficam as tarefas do workflow. Aqui há só uma, chamada build (o nome é livre).
  build:
    runs-on: ubuntu-latest    -> ubuntu-latest define que ela roda numa máquina virtual Linux (Ubuntu) do GitHub.


steps:			-> Lista de passos executados em ordem, dentro do job.
	- uses: actions/checkout@v4		-> Chama a action checkout, que baixa o código do repositório para a máquina. Sem isso, os próximos passos não teriam acesso aos seus arquivos.

	
- name: Run a one-line script 		-> executa um comando de terminal. E só imprime “Hello, world!” no log da execução.
  run: echo Hello, world!
 

- name: Run a multi-line script			-> O | permite escrever vários comandos, um por linha.
run: |									Aqui são dois echo que só imprimem mensagens no log.
  echo Add other actions to build,
  echo test, and deploy your project.

  


											 Resumo: este é o workflow de exemplo que o GitHub gera automaticamente (por isso o nome helloworld.yml). Ele roda a cada push ou pull request na main (ou manualmente), baixa o 											 código e imprime mensagens. Não faz build, teste nem deploy de verdade: serve só como modelo para você substituir esses passos pelos seus.





-------------------------------------------------------------------------------------------------------------------------------------------------------------------



jobs:							-> é onde ficam as tarefas do workflow. Aqui há só uma, chamada release (o nome é livre). O ubuntu-latest define que ela roda numa máquina virtual Linux (Ubuntu) fornecida pelo GitHub.
  release:
    runs-on: ubuntu-latest

steps:			-> Lista de passos executados em ordem, dentro do job. Se um falhar, os seguintes não rodam.


- uses: actions/checkout@v4		-> chama uma action pronta. A checkout baixa o código do repositório para a máquina. 0 baixa todo o histórico do Git (e não só o último commit). Aqui nem é estritamente necessário,
  with:     						já que o .git é excluído do pacote depois.
  fetch-depth: 0
    

- name: Criar pacotes (zip e tar.gz)
  run: | 								-> executa comandos de terminal (aqui, vários, por causa do |).
    VERSION="${{ github.event.release.tag_name || 'v1.0.0' }}"			-> pega a tag da release (ex.: v2.1.0). Se não existir (execução manual via workflow_dispatch), usa v1.0.0.
    NAME="canil-patas-unidas-${VERSION}"								-> monta o nome base, ex.: canil-patas-unidas-v2.1.0.
    mkdir -p dist														-> cria a pasta dist para guardar os arquivos gerados.
    zip -r "dist/${NAME}.zip" . -x ".git/*" ".github/*" "dist/*"		-> compacta tudo em .zip, ignorando .git, .github e dist.
    tar --exclude=".git" --exclude=".github" --exclude="dist" \			->  faz o mesmo, mas em .tar.gz, com as mesmas exclusões.
        -czf "dist/${NAME}.tar.gz" .



- name: Anexar pacotes na release									-> Usa a action softprops/action-gh-release, que envia arquivos para uma release do GitHub.
uses: softprops/action-gh-release@v2							    tag_name: em qual release anexar (mesma lógica da versão acima).
with:																files: dist/*: envia todos os arquivos da pasta dist (o .zip e o .tar.gz).
tag_name: ${{ github.event.release.tag_name || 'v1.0.0' }}
files: dist/*

 


											Isso só funciona porque no bloco permissions: contents: write (acima do jobs) o workflow tem permissão de escrever na release.

											Resumo: quando você publica uma release, o workflow baixa o código, gera um .zip e um .tar.gz do projeto e anexa os dois na própria release.
























