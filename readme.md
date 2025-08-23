
# 🤖 Deadline RPG Bot (Telegram)

Um bot completo para gerenciamento de fichas de RPG, inventário, vida/sanidade, rolagens de dados, armas e progressão por XP no Telegram.  
**Totalmente integrado com PostgreSQL (Neon) para persistência de dados, mesmo após deploys no Render.**

---

## 🚀 Funcionalidades

- Registro automático de jogadores: `/start`
- Ficha completa com atributos, perícias, vida e sanidade: `/ficha`
- Edição de ficha com distribuição de pontos: `/editarficha`
- Inventário com cálculo de peso e penalidades: `/inventario`
- Catálogo global de itens e armas: `/itens`
- Sistema de armas e munições: carregamento, balas, tipo melee/ranged
- Consumíveis com efeitos: cura, dano, munição etc.
- Transferência e abandono de itens: `/dar`, `/abandonar`
- Testes de perícias, atributos e rolagens manuais: `/roll`
- Registro de turnos para XP: `/turno`
- Sistema de XP por atividade, com streaks e ranking semanal
- Comandos administrativos para controle total do sistema
- Anti-spam embutido para comandos repetidos
- Rerolls diários com reset automático às 6h

---

## 🧑‍💻 Ficha do Jogador

- **Atributos** (máx. 20 pts): Força, Destreza, Constituição, Inteligência, Sabedoria, Carisma  
- **Perícias** (máx. 40 pts): Percepção, Persuasão, Medicina, Furtividade, Intimidação, Investigação, Armas de fogo, Armas brancas, Sobrevivência, Cultura, Intuição, Tecnologia  
- **HP (Vida)**: Máximo inicial 40  
- **SP (Sanidade)**: Máximo inicial 40  
- **XP**: Ganha por turnos, interações e streaks  
- **Ranking semanal**: Reset toda segunda às 6h, enviado aos admins

---

## 🏋️‍♂️ Peso Máximo por Força

| Força | Peso Máx (kg) |
|-------|---------------|
| 1     | 5             |
| 2     | 10            |
| 3     | 15            |
| 4     | 20            |
| 5     | 25            |
| 6     | 30            |

- Penalidades por sobrepeso: até -3 em testes físicos

---

## 🎮 Comandos Disponíveis

### 👤 Jogadores
- `/start` → Cria seu personagem  
- `/ficha` → Exibe sua ficha atual  
- `/editarficha` → Edita atributos e perícias  
- `/inventario` → Mostra seu inventário  
- `/roll <XdY+Z|atributo|perícia>` → Faz rolagem de dados ou testes  
- `/reroll` → Usa um dos 3 rerolls diários  
- `/turno <texto>` → Registra um turno de roleplay (mín. 499 caracteres, 1/dia)  

### 📦 Itens e Inventário
- `/itens` → Lista o catálogo de itens e armas  
- `/dar @jogador NomeDoItem x2` → Envia itens (com confirmação)  
- `/abandonar NomeDoItem x1` → Descarta itens do inventário  
- `/consumir NomeDoItem` → Usa consumível (cura, munição etc)  
- `/recarregar NomeDaArma` → Recarrega arma com munição compatível  

### 💀 Saúde e Sanidade
- `/dano hp|sp [@jogador]` → Aplica dano físico ou mental  
- `/cura @jogador NomeDoKit` → Usa item de cura  
- `/terapia @jogador` → Recupera sanidade via terapia  
- `/coma` → Marca jogador como em coma  
- `/ajudar @jogador` → Tenta reanimar jogador em coma  

### 👑 Administradores
- `/additem Nome Peso` → Adiciona item simples  
- `/addconsumivel Nome Peso [bônus] [armas_compat]` → Consumível com efeito  
- `/addarma Nome Peso melee/range Bônus [munição_atual/munição_max]` → Adiciona arma  
- `/delitem Nome` → Remove item do catálogo  
- `/verficha @jogador` → Consulta ficha completa de um jogador

---

## 📈 Sistema de XP e Ranking

- **XP por turnos**: baseado no tamanho do texto (mín. 499 caracteres)  
- **Streaks**: bônus por turnos consecutivos  
- **Interações mútuas**: bônus ao mencionar outros jogadores  
- **Ranking semanal**: reseta às segundas às 06h e é enviado para os administradores  

---

## 🛠️ Como rodar no Render com Neon

1. Suba os arquivos (`bot.py`, `requirements.txt`, `readme.md`) no seu repositório GitHub.
2. Crie um banco PostgreSQL gratuito no [Neon](https://neon.tech) e copie a **Database URL**.
3. No [Render](https://render.com), crie um **Web Service** e conecte ao seu repo.
4. Adicione as seguintes variáveis de ambiente:

```env
BOT_TOKEN=seu_token_aqui
NEON_DATABASE_URL=postgres://...
ADMINS=123456789,987654321
````

5. Confirme que `psycopg2-binary` está no seu `requirements.txt`.
6. No campo **Start Command**, coloque:

```bash
python bot.py
```

---

## 📦 Dependências

* `python-telegram-bot`
* `psycopg2-binary`
* `flask`

---

## 💡 Observações

* Todos os dados dos jogadores são salvos no banco Neon/PostgreSQL.
* O catálogo de itens é global, o inventário é individual.
* Rerolls e XP são resetados automaticamente às 6h da manhã.
* Ranking semanal é resetado às segundas.
* O bot aceita comandos por texto ou menus interativos do Telegram.

---

## 🤝 Contribuição

Pull requests e sugestões são bem-vindos!
Feito para RPGs por [exosignals](https://github.com/exosignals)

```
