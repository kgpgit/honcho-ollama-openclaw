# Honcho OpenClaw - Fork com Suporte a Ollama

Fork do [plastic-labs/honcho](https://github.com/plastic-labs/honcho) modificado para usar Ollama localmente em vez de provedores cloud (Google, Anthropic, etc).

## Configurações Personalizadas

### Alterações realizadas

1. **`src/embedding_client.py`**: Adicionado suporte a provider `openai-compatible` para embeddings via Ollama
2. **`src/utils/types.py`**: Adicionado `openai-compatible` aos `SupportedProviders`
3. **`src/config.py`**: Adicionado `openai-compatible` ao `EMBEDDING_PROVIDER`
4. **`config.toml`**: Configurado para usar Ollama como provedor local

### Modelos configurados

- **LLM**: `llama3.2:latest`
- **Embedding**: `nomic-embed-text:latest`

## Atualizando do Repositório Original

Para atualizar este fork com as últimas alterações do repositório original (`plastic-labs/honcho`):

```bash
# 1. Buscar alterações do repositório original
git fetch upstream

# 2. Ver as alterações disponíveis
git log --oneline upstream/main ^main

# 3. Fazer merge das alterações
git merge upstream/main

# 4. Se houver conflitos, resolver manualmente
# Arquivos que DEVEM ser mantidos com nossas alterações:
#   - src/embedding_client.py (suporte openai-compatible)
#   - src/utils/types.py (SupportedProviders)
#   - src/config.py (EMBEDDING_PROVIDER)
#   - config.toml (configurações Ollama)

# 5. Commitar e pushar
git commit -m "Merge upstream changes"  # ou resolver conflitos
git push origin main
```

### Arquivos críticos a preservar

Após o merge, verifique estes arquivos:

1. **`src/embedding_client.py`**: Deve conter o bloco `elif self.provider == "openai-compatible":`
2. **`src/utils/types.py`**: Deve incluir `"openai-compatible"` no `SupportedProviders`
3. **`src/config.py`**: Deve incluir `"openai-compatible"` no `EMBEDDING_PROVIDER`
4. **`config.toml`**: Deve ter `PROVIDER = "custom"` e `OPENAI_COMPATIBLE_BASE_URL`

## Variáveis de Ambiente

```env
# Banco de dados
DB_CONNECTION_URI=postgresql+psycopg://postgres:honcho_local_pass@honcho-db:5432/honcho

# Auth desabilitado
AUTH_USE_AUTH=false

# Ollama como provider OpenAI-compatible
OPENAI_COMPATIBLE_API_KEY=ollama
OPENAI_COMPATIBLE_BASE_URL=http://192.168.0.98:11434/v1
```

## Build e Deploy

```bash
# Build local
docker build -t ghcr.io/kgpgit/honcho-ollama-openclaw:latest ./honcho-fork

# Push para GHCR (requer autenticação)
docker push ghcr.io/kgpgit/honcho-ollama-openclaw:latest
```

## Troubleshooting

Se o container não iniciar, verifique os logs:
```bash
docker compose logs honcho-server
```

Erros comuns:
- **Missing client**: Verificar se `config.toml` tem `PROVIDER = "custom"`
- **Embedding provider**: Verificar se `EMBEDDING_PROVIDER = "openai-compatible"`
