# Contribuindo

Para contribuir com o pacote com a inserção de um novo
documento:

- Crie uma _issue_ dizendo sobre o documento que deseja
  inserir ao pacote;
  - Preferencialmente coloque links que ajudem a entender o
    algoritmo de geração e validação do documento.
- Realize os procedimentos padrões, sendo que na hora de
  criar a sua _branch_, referencie a sua _issue_;
- Realize o _pull request_ para a branch _main_.

## Sobre o código

Para novos documentos:

- Criar uma classe com as siglas do documento
  (herdando a classe pai `DocumentBase`);
- Importar a classe no `__init__.py`;
- Criar testes em `tests/`.

## Requisitos

- [uv](https://docs.astral.sh/uv/) para gerenciar
  dependências
- [Task](https://taskfile.dev/) para executar comandos
- Docker para lint

## Comandos

```shell
task build          # Builda a imagem Docker
task test           # Executa os testes
task test-coverage  # Executa os testes com cobertura
task type-check     # Verifica os tipos com ty
task lint           # Executa os linters
task lint-fix       # Corrige problemas de lint do Python
task ci             # Executa lint e testes
task shell          # Abre um shell no container
```
