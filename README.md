# Nexters App · Lux Energia

App do time de SDRs Nexters: placar por temporada com prêmio, ligações lidas do GoTo, reuniões registradas em poucos cliques, mural do time e uma aba de gestão.

- **Site:** https://aurigconsultoria.github.io/arena-sdr/ (`index.html`, arquivo único, com a bandeira embutida)
- **Banco:** Supabase, projeto `fctmquzdqifzkirnzykw` (região São Paulo)
- **Ligações:** tarefa agendada lê o GoTo Analytics no Chrome do gestor de hora em hora, seg a sex, 08:02 às 19:02

## Acesso
Entre com e-mail e senha. Se a conta não existir, ela é criada na hora. SDRs são liberados automaticamente quando o e-mail está cadastrado em **Admin → Time**.

## Pontuação padrão (editável em Admin → Pontuação)

| Evento | Pts |
|---|---|
| Ligação (saída, 10 s ou mais) | +1 |
| Reunião agendada | +20 |
| Reunião aconteceu (verificada pelo gestor) | +10 |
| Qualidade A / B / C | +25 / +10 / 0 |
| No-show | −5 |
| Lead enviou fatura | +15 |
| Fechamento ACL | +100 |
| Fechamento GD | +67 |
| Bateu a meta semanal de ligações / reuniões | +30 / +50 |

## Fluxo da reunião
1. **SDR** toca em **+ Reunião**, escolhe a data, digita a empresa e responde duas perguntas: se já está no sistema e se tem fatura.
2. **Gestor** vê em Reuniões → *Aguardando sua verificação* e escolhe: Aconteceu, No-show, Remarcou ou Cancelou.
3. Se aconteceu, marca a qualidade (A, B ou C) e, quando fechar, o tipo de fechamento: ACL ou GD.

## Mural
Qualquer pessoa do time publica para todos. O gestor também pode mandar um recado só para um SDR e fixar publicações. Todo mundo pode reagir e comentar, e o mural atualiza ao vivo.

## Coaching sem microgerenciamento
Os pontos de atenção olham só a semana consolidada:
- ritmo de ligações abaixo de 60% do esperado (a partir de quarta)
- duas semanas seguidas sem reunião (nunca na 1ª semana do SDR)
- no-show recorrente
- reuniões esperando sua verificação
- metas batidas, para reconhecer
- leitura do GoTo atrasada

Não existe feed de ligação por ligação.

## Arquivos
- `index.html`: o app inteiro (gerado por `src/build.py` a partir de `src/app.template.html`)
