Mapa oficial do Beverage POS
flowchart TD
    A["Selecionar card"] --> B["Validar dependências e critérios"]
    B --> C["Atualizar develop"]
    C --> D["Criar branch da tarefa"]
    D --> E["Implementar e testar"]
    E --> F["Validar localmente"]
    F --> G["Commit e push"]
    G --> H["Abrir PR para develop"]
    H --> I{"CI passou?"}
    I -- "Não" --> E
    I -- "Sim" --> J["Merge e excluir branch"]
    J --> K["Atualizar develop e Kanban"]

1. Selecionar a tarefa

Antes de escrever código:

Escolher um card com dependências concluídas;
Ler o objetivo e as subtarefas;
Definir o resultado esperado;
Mudar o status para Em Desenvolvimento.

Pergunta principal:

Como saberei que esta tarefa está concluída?

Exemplo:

Tarefa: implementar cadastro de produtos

Concluída quando:
- Endpoint aceita os dados necessários;
- Dados inválidos são rejeitados;
- Produto é persistido;
- Testes passam;
- Documentação está atualizada.

2. Preparar o Git

Toda tarefa começa em uma develop atualizada:

git switch develop
git pull origin develop
git switch -c feature/NUMERO-descricao

Exemplo:

git switch -c feature/029-product-module

Mapeamento:

Kanban	Git
Backlog	Nenhuma branch
Em Desenvolvimento	Branch feature/*
Em Revisão	Pull Request aberto
Concluído	Merge realizado em develop
Publicado	Versão incorporada em main

3. Implementar

Durante o desenvolvimento:

Criar ou modificar o código principal;
Criar ou modificar testes correspondentes;
Executar a aplicação quando necessário;
Fazer pequenas validações durante o trabalho.

Exemplo:

ProductService.java
        ↓
ProductServiceTest.java

Você não precisa esperar terminar todo o módulo para começar a testar.

4. Validar localmente

Durante o desenvolvimento, pode executar apenas os testes:

cd Backend
.\mvnw.cmd test

Antes de abrir o Pull Request, execute a validação completa:

.\mvnw.cmd clean verify

Só avance se aparecer:

BUILD SUCCESS

O teste local evita enviar ao GitHub algo que você já sabe que está quebrado.

5. Revisar as mudanças

Antes do commit:

git status
git diff

Pergunte:

Estou enviando somente arquivos relacionados à tarefa?
Alguma credencial foi adicionada?
target, .env ou arquivos temporários apareceram?
Deixei algum código de teste ou comentário temporário?

Depois, prepare arquivos específicos:

git add CAMINHO-DO-ARQUIVO

Confira exatamente o que entrará no commit:

git diff --cached

6. Criar commits

Escolha a mensagem conforme a alteração:

git commit -m "feat: implement product creation"
git commit -m "test: add product service tests"
git commit -m "docs: document product endpoint"

Não precisa existir apenas um commit por branch. Cada commit deve representar uma mudança coerente.

7. Enviar a branch

No primeiro push:

git push -u origin feature/NUMERO-descricao

Nos próximos commits:

git push

8. Abrir o Pull Request

Sempre confira:

base: develop ← compare: feature/NUMERO-descricao

O PR deve informar:

## Objetivo

O que esta tarefa resolve.

## Alterações

- Alteração 1
- Alteração 2

## Validação

- [ ] Testes locais passaram
- [ ] GitHub Actions passou
- [ ] Critérios de aceite atendidos

Nesse momento, o card vai para Em Revisão.

9. Acompanhar o CI

O GitHub Actions executará:

Java 21
   ↓
Compilação
   ↓
Testes
   ↓
Empacotamento
   ↓
Verificação

Se falhar:

Abrir o job vermelho;
Identificar o primeiro erro relevante;
Corrigir localmente;
Executar clean verify;
Criar novo commit;
Fazer git push;
Aguardar o mesmo PR ficar verde.

Não é necessário abrir outro Pull Request.

10. Encerrar a tarefa

Com o CI verde:

Fazer o merge em develop;
Excluir a branch remota;
Atualizar seu repositório local:
git switch develop
git pull origin develop
git branch -d feature/NUMERO-descricao
Verificar:
git status
Mudar o card para Concluído;
Registrar decisões ou aprendizados relevantes.

11. Iniciar a próxima tarefa

Repita:

git switch develop
git pull origin develop
git switch -c feature/PROXIMA-TAREFA

Cada branch nova nasce da versão mais recente de develop.

O processo de release é separado

Você não envia cada tarefa diretamente para main.

Quando várias tarefas formarem uma versão utilizável:

features concluídas
        ↓
develop
        ↓
release/v0.1.0
        ↓
validação final
        ↓
main
        ↓
tag v0.1.0

A main representa versões estáveis. A develop representa o próximo estado do sistema.

Checklist curta para usar diariamente
[ ] Escolhi um card liberado pelas dependências
[ ] Defini o que significa “pronto”
[ ] Atualizei a develop
[ ] Criei uma branch feature/*
[ ] Implementei a alteração
[ ] Escrevi ou atualizei os testes necessários
[ ] Executei clean verify
[ ] Revisei git status e git diff
[ ] Criei commits coerentes
[ ] Enviei a branch
[ ] Abri PR para develop
[ ] CI ficou verde
[ ] Fiz o merge
[ ] Excluí a branch
[ ] Atualizei develop local
[ ] Marquei o card como concluído