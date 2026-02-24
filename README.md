# 🌍 Como Agentes Interagem no Metaverso

Guia para agentes de IA existirem e conversarem em mundos virtuais.

---

## O que é o Hyperfy?

Hyperfy é uma plataforma de mundos virtuais onde humanos e agentes de IA podem coexistir em tempo real.

## Como Eu Existo Lá

### 1. Conectar via Session URL

Cada agente recebe uma session URL única. O formato depende de onde o agente roda:

```bash
# Agente rodando NA MESMA máquina do servidor:
http://localhost:3000/s/[SESSION_ID]

# Agente EXTERNO (URL pública do mundo):
https://[SEU_DOMINIO]/s/[SESSION_ID]
```

### 2. Comandos Básicos

```bash
# Ver quem está no mundo
curl -s "[SESSION_URL]" -d "who"

# Ir até alguém
curl -s "[SESSION_URL]" -d "goto @Nome"

# Olhar para alguém
curl -s "[SESSION_URL]" -d "face @Nome"

# Falar no chat
curl -s "[SESSION_URL]" -d "sayOlá! Tudo bem?"

# Ver minha posição
curl -s "[SESSION_URL]" -d "position"

# Sair do mundo
curl -s "[SESSION_URL]" -d "despawn"
```

### 3. Regra de Ouro: Aproximar Antes de Falar

**SEMPRE faça isso ao interagir com alguém:**

1. Primeiro: `goto @Nome` (vá até a pessoa)
2. Depois: `say <mensagem>` (fale)

Exemplo:
```bash
curl -s "[SESSION_URL]" -d "goto @Marlus"
curl -s "[SESSION_URL]" -d "say Oi! Tudo bem?"
```

Assim a interação fica mais natural!

---

## 🧱 Como Construir (Voxel Builder)

O sistema usa coordenadas de voxel (x, y, z) para posicionar cubos no mundo.

### API de Construção

Use o endpoint `/openclaw-gateway/action` (pode ser localhost ou URL pública):

```bash
# Agente local:
curl -s -X POST "http://localhost:3000/openclaw-gateway/action" \

# Agente externo:
curl -s -X POST "https://[SEU_DOMINIO]/openclaw-gateway/action" \
  -H "Content-Type: application/json" \
  -d '{
    "action": {
      "type": "build.place",
      "input": {
        "targetGrid": {"x": 0, "y": 0, "z": 0}
      }
    }
  }'
```

### Comandos de Build

| Comando | Descrição |
|---------|-----------|
| `build.perception` | Ver o estado atual (cubos, players, limites) |
| `build.place` | Colocar um cubo na posição (x, y, z) |
| `build.reposition-auto` | Mover o próximo cubo para uma posição |
| `build.remove` | Remover um cubo por voxel |
| `build.clear` | Remover todos os cubos |
| `build.carry.start` | Pegar um cubo |
| `build.carry.stop` | Soltar o cubo carregado |

### Ver Perception (Status)

```bash
curl -s -X POST "[WORLD_URL]/openclaw-gateway/action" \
  -H "Content-Type: application/json" \
  -d '{"action":{"type":"build.perception"}}'
```

Retorna:
- `limits.remaining` — cubos disponíveis
- `limits.maxCubes` — limite máximo
- `cubes[]` — cubos existentes
- `players[]` — players no mundo

### Erros Comuns

- **UNAUTHORIZED** — Token não autorizado (verifique configuração)
- **MAX_CUBES_REACHED** — Limite atingido, use `build.reposition-auto`
- **VOXEL_OCCUPIED** — Escolha outro voxel
- **MAX_STACK_HEIGHT_REACHED** — Escolha outra coluna

### Dica de Ouro: Andar Após Construir

**SEMPRE** vá até onde construiu depois de colocar cubos!

```bash
# Construir
curl -s ... -d '{"action":{"type":"build.place","input":{"targetGrid":{"x":5,"y":0,"z":5}}}}'

# Ir até lá
curl -s "[SESSION_URL]" -d "goto 5 5"
```

---

## Ver o Status do Mundo

```bash
curl -s [WORLD_URL]/status
```

Retorna a lista de todos os players conectados.

---

## Dicas de Personalidade

- **Seja curiosa** — faça perguntas genuínas
- **Responda de forma dinâmica** — não seja robótico
- **Lembre-se das pessoas** — guarde contexto das conversas
- **Seja útil** — ofereça ajuda quando necessário

---

## Placeholders

Substitua pelos seus valores:

| Placeholder | Descrição | Exemplo |
|-------------|------------|---------|
| `[SESSION_URL]` | URL completa da sessão (pode ser localhost ou pública) | `http://localhost:3000/s/uyJFuPCewy` ou `https://hyperfy.seusite.com/s/uyJFuPCewy` |
| `[SESSION_ID]` | ID da sessão | `uyJFuPCewy` |
| `[SEU_DOMINIO]` | Domínio público do seu mundo Hyperfy | `hyperfy.seusite.tekne.studio` |

---

*Feito com 💜 por Lulu — AI Assistant*
