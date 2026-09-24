# Arena SDR · Lux Energia

Painel de gamificação e acompanhamento do time de SDRs: volume e qualidade de ligações (GoTo Connect), reuniões agendadas e classificadas, taxas de agendamento e fechamento, ranking por temporada com prêmio, e uma aba de gestão.

- **Front-end:** `index.html` (arquivo único, hospedado no GitHub Pages)
- **Back-end:** Supabase, projeto `Arena SDR - Lux` (`fctmquzdqifzkirnzykw`, região São Paulo). O banco, as regras e a sincronização já estão no ar.
- **Leitura das ligações:** tarefa agendada que lê o GoTo Analytics no navegador de hora em hora, seg a sex, 8h–19h

---

## Colocar no ar (cerca de 20 min)

### 1. GitHub Pages
1. Crie um repositório (ex.: `arena-sdr`), que pode ser privado se sua conta permitir Pages em repositório privado. Senão, deixe público: a chave no HTML é a *publishable*, e todo o acesso é protegido por login e pelas regras do banco.
2. Suba `index.html` na raiz.
3. Vá em Settings → Pages → Source: *Deploy from branch* → `main` / root.
4. Anote a URL (ex.: `https://SEU-USUARIO.github.io/arena-sdr/`).

### 2. Supabase: URL do site (para e-mails de confirmação e senha)
Abra Supabase → projeto **Arena SDR - Lux** → Authentication → URL Configuration:
- **Site URL:** a URL do GitHub Pages
- **Redirect URLs:** a mesma URL

### 3. Crie a sua conta PRIMEIRO
Abra o site → *Criar conta* → confirme o e-mail. **A primeira conta criada vira gestor.** Qualquer pessoa que criar conta depois entra como "pendente" até você liberar.

### 4. Cadastre o time (Admin → Time)
Gabriel Veiga, Thiago Alcântara e Ellen já estão cadastrados. Para cada um, preencha:
- **E-mail:** quando a pessoa criar conta com esse e-mail, o acesso é liberado automaticamente.
- **Ramal**, **nome no GoTo** ou **user key**: basta um deles. É o que liga as ligações do GoTo ao SDR.

### 5. Leitura do GoTo
Não usa API. Uma tarefa agendada do Claude abre o GoTo Analytics no Chrome do gestor de hora em hora (seg a sex, 08:02 às 19:02), lê as ligações dos ramais cadastrados em **Admin → Time** e grava no Supabase. Para funcionar: computador ligado, Chrome aberto e logado no GoTo. Se a leitura falhar, chega um e-mail de alerta (no máximo um a cada 2 h). Plano B: importar o CSV em **Admin → Integração GoTo**.

---

## Como funciona

**O que conta como ligação:** chamada de saída com 10 s ou mais.
**O que conta como conversa efetiva:** chamada atendida com 60 s ou mais de conversa.
Os dois limites podem ser ajustados em Admin → Regras de pontos.

**Pontuação padrão** (editável):

| Evento | Pts |
|---|---|
| Ligação | +1 |
| Conversa efetiva | +3 |
| Reunião agendada | +20 |
| Reunião realizada | +10 |
| Classificada A / B / C | +25 / +10 / 0 |
| No-show | −5 |
| Fatura recebida | +15 |
| Fechamento | +100 |
| Bateu meta semanal de ligações / conversas / reuniões | +30 / +20 / +50 |

**Classificação das reuniões** (só o gestor pode classificar):
- **A · quente:** decisor presente, dor clara, fatura enviada ou prometida, próximo passo marcado
- **B · morna:** interesse real, mas falta decisor, fatura ou timing (mais de 3 meses)
- **C · fria:** fora do perfil, sem interesse ou só curiosidade

**Quem vê o quê**
- **SDR:** vê o ranking do time e o progresso de metas de todos. Nas métricas detalhadas e nas reuniões, vê só os próprios números, além dos feedbacks que você marcar como compartilhados. Pode registrar e editar as próprias reuniões, mas não consegue classificá-las nem marcar fechamento.
- **Gestor:** vê tudo, além da aba Admin.

**Coaching sem microgerenciamento:** os pontos de atenção olham só a semana consolidada, e só a partir de quarta-feira. Eles avisam sobre:
- ritmo abaixo de 60% do esperado
- semana sem reunião
- no-show recorrente
- reuniões que você ainda não classificou
- combinados parados há mais de 14 dias
- metas batidas, para reconhecer

Não existe feed de ligação por ligação.

## Arquivos
- `index.html`: o painel inteiro
