## ✅ Checklist de Conformidade – Pipeline Kafka Infrastructure

- [ ] Permissões globais reduzidas (`contents: read`, `id-token: write`); jobs elevam `packages`/`actions` somente quando indispensável.
- [ ] Runner configurado com labels `[self-hosted, Linux, X64, srv649924, conexao-de-sorte-kafka-infraestrutura]` em todos os jobs.
- [ ] `azure/login@v2` via OIDC com `${{ vars.AZURE_* }}` em jobs que interagem com Azure.
- [ ] Nenhum segredo de aplicação armazenado no GitHub; inventário em `docs/secrets-usage-map.md`.
- [ ] `actionlint`/`docker compose config`/`hadolint` executados no CI ou localmente antes do deploy.
- [ ] Deploy Swarm utiliza `docker stack deploy` com health checks e rollback configurados.
- [ ] Documentação atualizada com comandos de validação e requisitos do runner Hostinger.
- [ ] Validação em staging (Hostinger) registrada após execuções relevantes.
