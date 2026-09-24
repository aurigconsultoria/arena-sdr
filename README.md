# Arena SDR · Lux Energia

Painel de gamificação e acompanhamento do time de SDRs: volume e qualidade de ligações (GoTo Connect), reuniões agendadas e classificadas, taxas de agendamento e fechamento, ranking por temporada com prêmio, e uma aba de gestão.

- **Front-end:** `index.html` (arquivo único, hospedado no GitHub Pages)
- **Back-end:** Supabase, projeto `Arena SDR - Lux` (`fctmquzdqifzkirnzykw`, região São Paulo). O banco, as regras e a sincronização já estão no ar.
- **Sincronização:** Edge Function `goto-sync`, que roda a cada 15 min, de segunda a sábado, das 7h às 20h

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

### 5. Integração com o GoTo Connect
Precisa de alguém com perfil **Admin** na conta GoTo da Lux.

1. Em https://developer.logmeininc.com/clients, clique em **Create client**:
   - Scopes: `cr.v1.read`
   - Em *Grant types*, habilite **Personal Access Token**
   - Guarde o **Client ID** e o **Client Secret**
2. Em https://myaccount.goto.com, vá em **Developer Tools** e clique em **Create token**, marcando o mesmo scope `cr.v1.read`. Guarde o **Personal Access Token**.
3. Pegue o **Account Key**: ele aparece no GoTo Admin (admin.goto.com) nas configurações da conta, ou pergunte ao suporte GoTo.
4. No painel, cole os quatro valores em **Admin → Integração GoTo** e clique em **Salvar credenciais**. Eles ficam criptografados no cofre do Supabase.
5. Clique em **Puxar histórico** para trazer os últimos 7, 14 ou 31 dias.
6. Se aparecer algo em **Admin → Time → Linhas do GoTo sem vínculo**, vincule cada linha ao SDR certo e puxe o histórico de novo.

Se a sincronização der erro, a mensagem aparece em *Integração GoTo* e em *Coaching → Pontos de atenção*.

> **Plano B:** enquanto a API não estiver liberada, exporte o histórico de chamadas do GoTo em CSV e importe em **Admin → Integração GoTo → Importar CSV**.

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
- `supabase/functions/goto-sync/index.ts`: código da sincronização (já publicado)
