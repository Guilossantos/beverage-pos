# Governança do repositório

## Objetivo

Definir as regras do repositório do Beverage POS para proteger as branches principais, garantir validação automatizada e manter um histórico rastreável.

## Branches protegidas

### `main`

Representa as versões estáveis do projeto. Deve receber alterações somente por Pull Request.

### `develop`

Representa a branch de integração. As branches de funcionalidades e correções devem abrir Pull Requests para `develop`.

## Regras de proteção

As branches `main` e `develop` deverão:

- receber alterações somente por Pull Request;
- exigir que o pipeline de integração contínua seja aprovado;
- exigir que as discussões do Pull Request estejam resolvidas;
- impedir exclusões;
- impedir force push;
- impedir commits diretos;
- exigir que a branch esteja atualizada antes do merge.

Como o projeto possui atualmente um único desenvolvedor, não será exigida aprovação de outro usuário.

## Estratégia de merge

O projeto adotará `Squash and merge`.

Cada Pull Request será integrado como um único commit. As branches de trabalho poderão ser excluídas depois do merge.

## Convenções de branches

- `feature/*`: funcionalidades e melhorias;
- `fix/*`: correções;
- `release/*`: preparação de versões;
- `hotfix/*`: correções urgentes.

## Convenção de commits

Os commits deverão seguir Conventional Commits.

Exemplos:

```text
feat: adiciona cadastro de produtos
fix: impede estoque negativo
test: adiciona testes do serviço de vendas
docs: documenta modelo de domínio
ci: configura validação do backend
chore: atualiza configuração do projeto