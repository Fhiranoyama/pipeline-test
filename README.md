# Pipeline Demo

## Como executar localmente

1. Requisitos: Node.js 20+
2. Instale dependências (não obrigatório para este exemplo):
   
   ```bash
   npm install
   ```
3. Execute o script principal:
   
   ```bash
   node index.js
   ```

## CI (Build)

Workflow: `pipeline-demo-main/.github/workflows/ci.yml`

- Dispara em `push` e `pull_request`
- Configura Node 20, instala dependências e executa `node index.js`

## Segurança

Foram adicionados dois jobs na pipeline de segurança: SAST e DAST.

Workflows:
- Raiz do repositório: `.github/workflows/security.yml`
- Dentro do projeto: `pipeline-demo-main/.github/workflows/security.yml`

### SAST (CodeQL)

- Linguagem: JavaScript
- Ação: `github/codeql-action`
- Queries: `security-extended` e `security-and-quality`
- Publica resultados em Security → Code scanning alerts

### DAST (OWASP ZAP Baseline)

- Ação: `zaproxy/action-baseline`
- Sobe um servidor HTTP estático em `http://127.0.0.1:8080` e executa o scan
- `fail_action: true` para falhar o job em caso de alertas

## Como disparar os pipelines

- Faça `push` para `main`, `develop` ou `feature/**`
- Abra um Pull Request
- Ou execute manualmente via "Run workflow" no GitHub Actions

## Ajustes comuns

- Para DAST contra uma aplicação Express real, troque o passo do servidor estático por `npm start` (ou o comando do seu servidor) e aponte o alvo do ZAP para a porta correspondente.
- Para usar Semgrep como SAST, substitua o job CodeQL por `returntocorp/semgrep-action` com o seu conjunto de regras.
