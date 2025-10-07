# 🤖 Deadline RPG Bot (Telegram)

Um bot COMPLETO de RPG para Telegram, 100% Python, com gerenciamento avançado de ficha, inventário, vida/sanidade, necessidades, rolagens, armas, XP, ranking, administração e persistência real via banco PostgreSQL (Neon). Ideal para campanhas narrativas, roleplay e gerenciamento de mesa — tudo automatizado!

---

## 🚀 FUNCIONALIDADES PRINCIPAIS

- **Cadastro automático:** `/start`
- **Ficha de personagem:** atributos, perícias, HP, SP, inventário, traumas, fome, sede, sono, peso máximo e penalidades
- **Edição guiada:** `/editarficha` com template fácil de preencher
- **Inventário inteligente:** cálculo de peso, penalidades, sobrecarga, armas, munições, consumíveis
- **Catálogo global:** admins podem cadastrar/remover itens, armas e consumíveis
- **Sistema de armas:** melee/range, recarga, balas, compatibilidade por tipo de munição
- **Consumíveis com efeitos:** cura, dano, comida, bebida, munição, etc.
- **Necessidades reais:** fome, sede, sono afetam o personagem; alertas automáticos quando críticos
- **Transferência e abandono de itens:** `/dar`, `/abandonar`, ambos com confirmação
- **Testes de atributos/perícias (1d20+bônus) e rolagens livres (XdY+Z):** `/roll`
- **Registro de turnos para XP:** streaks, bônus por menções mútuas, ranking semanal automatizado
- **Sistema de XP:** bônus por atividade, streak, interações
- **Rerolls diários:** para refazer rolagens, reset às 6h
- **Sistema de traumas, terapia e coma**
- **Comandos administrativos completos**
- **Anti-spam automático**
- **API Flask embutida para healthcheck/deploy**

---

## 🧑‍💻 FICHA DO JOGADOR

- **Atributos** (total: 20 pontos, cada um 1–6):
  - Força, Agilidade, Vitalidade, Raciocínio, Equilíbrio, Persuasão
- **Perícias** (total: 40 pontos, cada uma 1–6):
  - Luta, Resistência, Furtividade, Pontaria, Reflexo, Sobrevivência, Medicina, Improviso, Exploração, Intuição, Manipulação, Confiança
- **HP (Vida):** Máximo depende da Vitalidade
- **SP (Sanidade):** Máximo depende do Equilíbrio
- **Necessidades:** Fome, sede, sono (quanto maior, pior)
- **Traumas:** texto livre, pode ser gerado em colapsos de SP
- **Peso máximo:** conforme Força, com penalidades por sobrecarga
- **Inventário:** itens, armas, munições, quantidades, pesos, efeitos

---

## 🎲 SISTEMA DE ROLAGEM

### Testes de Atributos ou Perícias

- **Comando:** `/roll <atributo|perícia>`
- **Mecânica:** 1d20 + bônus do atributo/perícia + penalidade se sobrecarregado
- **Exemplo:**
  ```
  /roll Força
  /roll Medicina
  ```
  **Saída:**
  ```
  🎲 /roll Força
  Rolagem: 1d20 → 13
  Bônus: +4
  Total: 17 → Sucesso
  ```

### Rolagem Livre

- **Comando:** `/roll XdY+Z`
- **Exemplo:** `/roll d6`, `/roll 2d8+3`
- **Limite:** até 5 dados, bônus máximo +10

---

## ⚖️ PESO MÁXIMO & PENALIDADES

| Força | Peso Máx (kg) |
|-------|---------------|
| 1     | 5             |
| 2     | 10            |
| 3     | 15            |
| 4     | 20            |
| 5     | 25            |
| 6     | 30            |

- **Penalidade:** Sobrepeso reduz Força, Agilidade e Furtividade (até -3).
- **Comando:** `/inventario` mostra o peso total, penalidade e cada item.

---

## 🏪 COMANDOS DE ITENS & INVENTÁRIO

- `/itens` — Lista catálogo global de itens e armas
- `/additem Nome Peso` — (Admin) Adiciona item ao catálogo
- `/addarma Nome Peso melee/range Bônus [mun_atual/mun_max]` — (Admin) Adiciona arma ao catálogo
- `/addconsumivel Nome Peso [bônus] [armas_compat]` — (Admin) Adiciona consumível ao catálogo, define efeito/tipo
- `/delitem Nome` — (Admin) Remove item do catálogo
- `/inventario` — Mostra inventário e peso do jogador
- `/dar @jogador Item x2` — Envia item para outro jogador, com confirmação
- `/abandonar Item x1` — Descarta item do inventário

---

## 🍽️ NECESSIDADES (FOME, SEDE, SONO)

- **Consumir comida/bebida:** `/consumir NomeDoItem`
  - Exemplo: `/consumir Barrinha x2`
- **Dormir:** `/dormir 6` (horas)
  - Recupera sono e parte do HP/SP; aumenta fome e sede
- **Alertas automáticos** para valores críticos de necessidades

---

## 💀 SAÚDE, SANIDADE, CURA, DANO, TERAPIA

- `/dano hp|sp [@alvo] [arma/consumível/perícia]` — Aplica dano físico ou mental
- `/cura [@alvo] NomeDoKitOuConsumível` — Usa kit ou consumível de cura
- `/terapia @alvo` — Recupera SP via Manipulação
- `/inconsciente` — Teste de coma ao zerar HP (1/dia)
- `/ajudar @alvo NomeDoKit` — Aplica bônus no teste de coma do alvo

---

## 📈 XP, TURNOS E RANKING

- **Turnos de roleplay:** `/turno <texto>`
  - Gera XP conforme tamanho do texto (mín. 499 caracteres)
  - Bônus por streak (dias consecutivos) e menções mútuas
- **XP semanal:** `/xp` mostra XP, streak e detalhes do jogador
- **Ranking:** `/ranking` ou botão no /xp mostra o top 10 da semana
- **Reset:** Ranking semanal reseta e avisa admins toda segunda às 6h

---

## 🔄 REROLLS

- **Até 3 rerolls diários** para refazer rolagens de teste
- **Comando:** `/reroll <atributo|perícia>`
- **Reset automático** às 6h

---

## 👑 ADMINISTRAÇÃO

- `/verficha @jogador` — Consulta ficha completa de outro jogador
- `/status [@jogador]` — Consulta necessidades/status de outro jogador
- `/liberar @jogador` / `/desliberar @jogador` — Gerencia acesso
- `/additem`, `/addarma`, `/addconsumivel`, `/delitem` — Gerenciamento do catálogo

---

## 📦 EXEMPLOS DE USO

### Cadastro e Ficha
```
/start
/ficha
/editarficha
```

### Edição de Ficha
O comando `/editarficha` envia um template:
```
Força: 4
Agilidade: 3
Vitalidade: 5
Raciocínio: 3
Equilíbrio: 2
Persuasão: 3
Luta: 4
...
```
Altere, cole e envie para atualizar sua ficha.

### Transferência de Itens
```
/dar @fulano Barrinha x2
```
O destinatário recebe botão de confirmação.

### Consumo e Necessidades
```
/consumir Água
/dormir 8
/status
```

### Dano e Cura
```
/dano hp @fulano Faca
/cura @fulano Kit Básico
```

### Rolagem Customizada
```
/roll d20+2
/roll 3d6
```

---

## 🛠️ DEPLOY NO RENDER + NEON (POSTGRESQL)

1. Suba `testinhobot.py`, `requirements.txt`, `readme.md` no seu repositório.
2. Crie banco gratuito no [Neon](https://neon.tech), copie a **Database URL**.
3. No [Render](https://render.com), crie um **Web Service** ligado ao repo.
4. Adicione variáveis de ambiente:
   ```
   BOT_TOKEN=seu_token
   NEON_DATABASE_URL=postgres://...
   ADMINS=123456789,987654321
   ```
5. Confirme que `psycopg2-binary` está em `requirements.txt`.
6. No campo **Start Command**, coloque:
   ```
   python testinhobot.py
   ```

---

## 📦 DEPENDÊNCIAS

- `python-telegram-bot`
- `psycopg2-binary`
- `flask`

---

## 💡 OBSERVAÇÕES

- Dados dos jogadores são persistidos em banco PostgreSQL
- Inventário e catálogo globais separados
- Rerolls e XP resetam automaticamente às 6h
- Ranking semanal é automatizado
- Bot aceita comandos de texto e menus do Telegram
- Estrutura extensível para novas funcionalidades

---

## 🤝 CONTRIBUIÇÃO

Pull requests e sugestões são super bem-vindos!

Feito por [exosignals](https://github.com/exosignals)
