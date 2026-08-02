# 🚀 OmniRoute AI Gateway (VPS Infrastructure)

Este repositório contém a configuração de Docker Compose para deploy do **OmniRoute AI Gateway** via Coolify, atuando como roteador central de LLMs, gerenciador de cache semântico/prompt caching e compressor de tokens para ambientes de desenvolvimento com IA.

---

## 🏗 Arquitetura

- **Domínio Público:** `https://ai.sigapro.com`
- **Orquestrador:** Coolify (VPS)
- **SSL / Proxy:** Cloudflare DNS + Caddy/Traefik (Coolify)
- **Serviço:** OmniRoute (Porta `20128`)

---

## 🛠 Como Fazer o Deploy no Coolify

1. **Criar Serviço:**
   - No Coolify, adicione uma nova aplicação selecionando este repositório Git.
   - Escolha o tipo de Build: **Docker Compose**.

2. **Variáveis de Ambiente:**
   - Copie o conteúdo de `.env.example` para as variáveis de ambiente da aplicação no Coolify e defina seu `ADMIN_KEY`.

3. **Configuração de Domínio e FQDN:**
   - No painel do Coolify, defina o FQDN como `https://ai.sigapro.com`.
   - Garanta que no painel do Cloudflare o registro `A` de `ai` em `sigapro.com` esteja apontando para o IP da sua VPS (com proxy habilitado ou apenas DNS).

4. **Deploy:**
   - Clique em **Deploy**. O Coolify vai subir a imagem `diegosouzapw/omniroute:latest` e mapear o volume `/app/data` para persistência.

---

## 🔌 Conectando seus Harnesses Locais

### 1. OpenCode / CLIs Compatíveis (OpenAI Format)
Configure suas ferramentas locais apontando a **Base URL** para o seu Gateway:

- **Base URL:** `https://ai.sigapro.com/v1`
- **API Key:** `<Sua ADMIN_KEY ou token gerado no Dashboard>`
- **Model:** `auto` (ou `auto/coding`, `auto/cheap`, `auto/fast`)

### 2. Oh My Pi (`omp`)
Adicione o provedor customizado no arquivo `~/.omp/agent/models.yml`:

```yaml
providers:
  sigapro-gateway:
    baseUrl: https://ai.sigapro.com/v1
    api: openai-completions
    apiKey: sua_admin_key_aqui
    models:
      - id: auto
        name: Auto Smart Router
      - id: auto/coding
        name: Auto Coding Combo

📊 Recursos Ativos

    Prompt Caching: Roteamento cache-optimized para reaproveitamento de contextos de constituição e regras.
    Compressão de Tokens: Motores RTK e Caveman ativos para enxugar outputs de terminal e código sem perder precisão.
    Auto-Combo & Failover: Roteamento resiliente entre 290+ provedores com fallback automático em picos de erro (429/500).


---
