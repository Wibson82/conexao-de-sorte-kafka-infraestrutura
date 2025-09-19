## ✅ Checklist de Conformidade – Pipeline Kafka Infrastructure

- [x] Permissões globais reduzidas (`contents: read`, `id-token: write`); jobs elevam `packages`/`actions` somente quando indispensável.
- [x] Runner configurado com labels `[self-hosted, Linux, X64, srv649924, conexao-de-sorte-kafka-infraestrutura]` em todos os jobs.
- [x] `azure/login@v2` via OIDC com `${{ vars.AZURE_* }}` em jobs que interagem com Azure.
- [x] Nenhum segredo de aplicação armazenado no GitHub; inventário em `docs/secrets-usage-map.md`.
- [ ] `actionlint`/`docker compose config`/`hadolint` executados no CI ou localmente antes do deploy.
- [x] Deploy Swarm utiliza `docker stack deploy` com health checks e rollback configurados.
- [x] Documentação atualizada com comandos de validação e requisitos do runner Hostinger.
- [ ] Validação em staging (Hostinger) registrada após execuções relevantes.
