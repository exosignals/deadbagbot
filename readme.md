
# 🤖 Deadline RPG Bot (Telegram)

Um bot completo de RPG para Telegram, com gerenciamento de ficha, inventário, vida/sanidade, necessidades, rolagem de dados, armas, XP, ranking e administração — tudo conectado em banco PostgreSQL (Neon) para persistência real.

---

## 🚀 Funcionalidades Principais

- **Cadastro automático de jogadores:** `/start`
- **Ficha completa:** atributos, perícias, HP, SP, inventário, traumas, fome, sede e sono
- **Edição guiada da ficha:** `/editarficha`
- **Inventário com cálculo de peso, penalidades e sobrecarga**
- **Catálogo global de itens, armas e consumíveis**
- **Sistema de armas e munições:** tipos melee/range, recarga, balas, compatibilidade
- **Consumíveis:** efeitos de cura, dano, comida, bebida, munição, etc.
- **Necessidades:** fome, sede e sono afetam o personagem (com alertas automáticos)
- **Transferência e abandono de itens:** `/dar`, `/abandonar`, com confirmação
- **Testes de atributos/perícias (1d20 + bônus), rolagens livres (XdY+Z):** `/roll`
- **Registro de turnos para XP, streaks e ranking semanal**
- **Sistema de XP:** bônus por atividade, interações, streak
- **Rerolls diários:** gastáveis em /roll, reset às 6h
- **Sistema de traumas, terapia e coma**
- **Comandos administrativos completos**
- **Anti-spam automático**
- **API Flask embutida para healthcheck**

---

## 🧑‍💻 Ficha do Jogador

- **Atributos** (total: 20 pontos, cada um 1–6):
  - Força, Agilidade, Vitalidade, Raciocínio, Equilíbrio, Persuasão
- **Perícias** (total: 40 pontos, cada uma 1–6):
  - Luta, Resistência, Furtividade, Pontaria, Reflexo, Sobrevivência, Medicina, Improviso, Exploração, Intuição, Manipulação, Confiança
- **HP (Vida):** Máximo depende da Vitalidade
- **SP (Sanidade):** Máximo depende do Equilíbrio
- **Necessidades:** Fome, sede, sono (quanto maior, pior)
- **Traumas:** texto livre, pode ser gerado em colapsos de SP
- **Peso máximo:** conforme Força, com penalidades quando sobrecarregado
- **Inventário:** lista de itens, armas, munições, quantidades e pesos

---

## 🎲 Sistema de Rolagem

### **Testes de Atributos ou Perícias**

- **Comando:** `/roll <atributo|perícia>`
- **Mecânica:** Rola 1d20 + bônus do atributo/perícia + penalidade se sobrecarregado
- **Exemplo de uso:**
  ```
  /roll Força
  /roll Medicina
  ```
  **Saída típica:**
  ```
  🎲 /roll Força
  Rolagem: 1d20 → 13
  Bônus: +4
  Total: 17 → Sucesso
  ```

### **Rolagem Livre**

- **Comando:** `/roll XdY+Z`
- **Exemplo:** `/roll d6`, `/roll 2d8+3`
- **Limites:** até 5 dados, bônus até +10

---

## ⚖️ Peso Máximo & Penalidades

| Força | Peso Máx (kg) |
|-------|---------------|
| 1     | 5             |
| 2     | 10            |
| 3     | 15            |
| 4     | 20            |
| 5     | 25            |
| 6     | 30            |

- **Penalidade:** Sobrepeso reduz Força, Agilidade e Furtividade em até -3.
- **Comando:** `/inventario` mostra o peso total, penalidade e cada item.

---

## 🏪 Comandos de Itens & Inventário

- `/itens` — Lista o catálogo global de itens e armas
- `/additem Nome Peso` — (Admin) Adiciona item simples ao catálogo
- `/addarma Nome Peso melee/range Bônus [mun_atual/mun_max]` — (Admin) Adiciona arma
- `/addconsumivel Nome Peso [bônus] [armas_compat]` — (Admin) Adiciona consumível e define tipo/efeito
- `/delitem Nome` — (Admin) Remove item do catálogo
- `/inventario` — Mostra seu inventário e peso
- `/dar @jogador Item x2` — Envia itens para outro jogador (com confirmação)
- `/abandonar Item x1` — Descarta itens

---

## 🍽️ Necessidades (Fome, Sede, Sono)

- **Consumir comida/bebida:** `/consumir NomeDoItem`
  - Exemplo: `/consumir Barrinha x2`
- **Dormir:** `/dormir 6` (horas)
  - Recupera sono e parte do HP/SP; aumenta fome e sede
- **Alertas automáticos** são enviados quando fome, sede ou sono atingem valores críticos.

---

## 💀 Saúde, Sanidade, Curar, Dano, Terapia

- `/dano hp|sp [@alvo] [arma/consumível/perícia]` — Aplica dano físico ou mental
- `/cura [@alvo] NomeDoKitOuConsumível` — Usa item de cura (kit ou consumível)
- `/terapia @alvo` — Recupera SP via Manipulação
- `/coma` — Realiza teste de coma ao zerar HP
- `/ajudar @alvo NomeDoKit` — Aplica bônus no próximo teste de coma do alvo

---

## 📈 Sistema de XP, Turnos e Ranking

- **Turnos de roleplay:** `/turno <texto>`
  - Gera XP conforme tamanho do texto (mín. 499 caracteres)
  - Bônus por streak (dias consecutivos) e menções mútuas
- **XP semanal:** `/xp` mostra seu XP, streak e detalhes
- **Ranking:** `/ranking` ou botão no /xp mostra o top 10 da semana
- **Reset:** Ranking semanal reseta (e avisa admins) toda segunda às 6h

---

## 🔄 Rerolls

- **Você tem até 3 rerolls diários** para refazer rolagens de teste
- **Comando:** `/reroll <atributo|perícia>`
- **Reset:** todos os rerolls são renovados às 6h

---

## 👑 Comandos Especiais de Admin

- `/verficha @jogador` — Consulta ficha completa de outro jogador
- `/status [@jogador]` — Consulta status de necessidades de outro jogador
- `/additem`, `/addarma`, `/addconsumivel`, `/delitem` — Gerenciamento de catálogo

---

## 📦 Exemplos de Uso

### **Cadastro e Ficha**

```
/start
/ficha
/editarficha
```

### **Edição de Ficha**

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

### **Transferência de Itens**

```
/dar @fulano Barrinha x2
```
O destinatário recebe um botão de confirmação.

### **Consumo e Necessidades**

```
/consumir Água
/dormir 8
/status
```

### **Dano e Cura**

```
/dano hp @fulano Faca
/cura @fulano Kit Básico
```

### **Rolagem Customizada**

```
/roll d20+2
/roll 3d6
```

---

## 🛠️ Deploy no Render + Neon (PostgreSQL)

1. Suba `bot.py`, `requirements.txt`, `readme.md` no seu repositório.
2. Crie um banco gratuito no [Neon](https://neon.tech), copie a **Database URL**.
3. No [Render](https://render.com), crie um **Web Service** ligado ao seu repo.
4. Adicione variáveis de ambiente:

```
BOT_TOKEN=seu_token
NEON_DATABASE_URL=postgres://...
ADMINS=123456789,987654321
```

5. Confirme que `psycopg2-binary` está em `requirements.txt`.
6. No campo **Start Command**, coloque:

```
python bot.py
```

---

## 📦 Dependências

- `python-telegram-bot`
- `psycopg2-binary`
- `flask`

---

## 💡 Observações

- Todos os dados dos jogadores são persistidos em banco
- Inventário e catálogo são separados
- Rerolls e XP resetam às 6h
- Ranking semanal é automatizado
- Bot aceita comandos de texto e menus do Telegram

---

## 🤝 Contribuição

Pull requests e sugestões são bem-vindos!

Feito por [exosignals](https://github.com/exosignals)
```

**Pronto! README.md atualizado com explicação de comandos, fluxos e exemplos, completamente alinhado com o seu código atual.**
Se precisar de exemplos ainda mais detalhados ou de um tutorial de ficha, posso complementar!
