# Secrets Usage Map – Kafka Infrastructure

## Repository Variables (`vars`)
- `AZURE_CLIENT_ID` – OIDC application identifier consumed by `azure/login@v2`.
- `AZURE_TENANT_ID` – Azure AD tenant used during OIDC authentication.
- `AZURE_SUBSCRIPTION_ID` – Subscription granted access to the target Key Vault (if ever required).
- `AZURE_KEYVAULT_NAME` – Logical Key Vault name associated with this pipeline.
- `AZURE_KEYVAULT_ENDPOINT` – HTTPS endpoint for compatibility with tooling that expects the full URL.

> Mantenha todos os identificadores como **Repository Variables**. Nenhum segredo sensível permanece no GitHub.

## GitHub Secrets
- `GITHUB_TOKEN` – Token padrão com permissões endurecidas para GHCR/Docker quando necessário.

## Azure Key Vault Secrets
- Nenhum segredo é consumido pelo deploy do Kafka neste momento. Documente explicitamente aqui antes de adicionar qualquer allowlist.

## Jobs × Secret Usage
| Job | Propósito | Segredos/Variáveis | Observações |
| --- | --- | --- | --- |
| `validate-and-build` | Linters, validações de compose e upload de artefatos | `AZURE_*` (OIDC), `GITHUB_TOKEN` | Key Vault não é consultado; apenas validações e empacotamento de manifests. |
| `deploy-selfhosted` | Deploy Swarm no runner Hostinger + health checks | `AZURE_*` (OIDC), `GITHUB_TOKEN` | Nenhum segredo adicional; consumo do Key Vault permanece vazio. |

## Notas Operacionais
- Atualize este arquivo antes de adicionar qualquer segredo novo no Key Vault.
- Runners devem incluir as labels `[self-hosted, Linux, X64, srv649924, conexao-de-sorte-kafka-infraestrutura]`.
- Use `::add-mask::` sempre que expor valores sensíveis em variáveis ou outputs.
- Serviço roda como UID/GID 1000 dentro do Swarm; certifique-se de ajustar permissões do volume externo `kafka_data`.
