# 🔥 Conexão de Sorte - Kafka Infrastructure

Deploy de Kafka standalone (Confluent 7.9) operado no Swarm da Hostinger (`srv649924`).

## 📋 Características
- Stack Swarm com volume externo `kafka_data` e rede `conexao-network-swarm`.
- Execução rootless (`user: 1000:1000`), `init` habilitado e `stop_grace_period` de 60s.
- Health check nativo via `kafka-broker-api-versions` + validações de logs/portas no pipeline.
- `update_config`/`rollback_config` configurados com `order: start-first` para zero downtime.

## 🚀 Deploy
```bash
# CI/CD (runner self-hosted)
git push origin main

# Manual (via runner)
docker stack deploy -c docker-compose.yml conexao-kafka
```

## 🔍 Validações Locais
```bash
actionlint -config-file .github/actionlint.yaml --shellcheck=
docker compose -f docker-compose.yml config -q
```
> `hadolint` indisponível neste ambiente. Sem Dockerfile, `docker build` é desnecessário.

## 🛡️ CI/CD & Segredos
- Runners: `[self-hosted, Linux, X64, srv649924, conexao-de-sorte-kafka-infraestrutura]`.
- Permissões globais: `contents: read`, `id-token: write`; sem segredos de aplicação no GitHub.
- `docs/secrets-usage-map.md` registra que o pipeline não consome Key Vault por enquanto.

## 📜 Histórico
Mudanças recentes documentadas em `HISTORICO-MUDANCAS.md`.

**Versão:** 1.1.0 – Atualizado em 19/09/2025.
