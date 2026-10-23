# CEMOA · Centro de Monitoramento — Regras de negócio

> **Para quem é este documento.** Para o geólogo do CEMOA, que é o dono das
> regras, e para o DDS (Setor de Desenvolvimento), que constrói o Centro de
> Monitoramento como submódulo do SISPDEC.
>
> **O que ele é.** O documento mestre da plataforma. A regra nasce aqui, antes
> de ir para o sistema de origem ou para o SISPDEC. Descreve o que cada tela
> faz, de onde tira os dados, de quanto em quanto tempo atualiza, o que grava
> e cada regra vigente. Não fala de código.
>
> **O que ele não é.** Não é o manual visual. A designer do DDS vai adaptar o
> layout; o que vale aqui é a regra, não a aparência.

| Item | Valor |
| --- | --- |
| Sistema de origem | Aplicação do CEMOA (`cemoa-centro-operacoes`) |
| Última conferência com o sistema de origem | 22/10/2026 (versão 0adae65) |
| Destino | SISPDEC · submódulo Centro de Monitoramento |
| Dono das regras | Geólogo do CEMOA |
| Responsável pelo desenvolvimento | DDS |

---

## Como usar este documento

1. **O geólogo não precisa mais mexer no sistema de origem.** Toda mudança de
   regra é feita aqui, por escrito.
2. **Para mudar uma regra:** edite o texto da regra, marque a linha com
   `🔄 Alterada em DD/MM/AAAA` e registre a mudança no
   [Histórico de alterações](#histórico-de-alterações). O DDS lê e atualiza o
   SISPDEC.
3. **Para responder uma pendência:** escreva a resposta logo abaixo da pergunta,
   em `**Resposta do CEMOA:**`, e troque o ícone de `❓` para `✅`. O DDS move a
   regra confirmada para o corpo do módulo.
4. **Para criar um módulo novo do zero:** copie o
   [Modelo para módulo novo](#modelo-para-módulo-novo) no fim do documento e
   preencha. Só com esse texto o DDS deve conseguir desenvolver a tela.
5. **Divergências.** Onde o sistema de origem faz uma coisa e a documentação
   antiga dele diz outra, o comportamento atual do sistema foi descrito como
   regra vigente e a divergência foi para as **Pendências** do módulo. Nada foi
   decidido por conta do DDS.

**Legenda:** ✅ confirmado · ❓ pendente de resposta do CEMOA · 🔄 alterado ·
➕ regra nova (ainda não existe no sistema de origem).

---

## Sumário

- [Regras gerais (valem para os quatro módulos)](#regras-gerais-valem-para-os-quatro-módulos)
- [Módulo 1 · Painel de Alertas](#módulo-1--painel-de-alertas)
- [Módulo 2 · Boletim Hidrológico](#módulo-2--boletim-hidrológico)
- [Módulo 3 · Meteorologia](#módulo-3--meteorologia)
- [Módulo 4 · Gestão de Risco](#módulo-4--gestão-de-risco)
- [Anexo A · Fontes externas](#anexo-a--fontes-externas)
- [Anexo B · O que cada módulo grava](#anexo-b--o-que-cada-módulo-grava)
- [Anexo C · Frequência de atualização](#anexo-c--frequência-de-atualização)
- [Zonas de influência da chuva](#zonas-de-influência-da-chuva)
- [Zonas administrativas e vias de alagamento](#zonas-administrativas-e-vias-de-alagamento)
- [Modelo para módulo novo](#modelo-para-módulo-novo)
- [Histórico de alterações](#histórico-de-alterações)

---

## Regras gerais (valem para os quatro módulos)

### Território e horário

- O Centro monitora os **62 municípios do Amazonas**. Todo município aparece em
  todas as listas, mesmo sem sensor, sem leitura ou sem alerta.
- **Todo horário exibido é o de Manaus (UTC−4)**, inclusive em PNG exportado.
  Quando a fonte manda o horário em UTC ou de Brasília, ele é convertido.
- Os municípios são agrupados por **bacia** (no Painel de Alertas) e por
  **calha** (no Boletim). A correspondência bacia → calha está no Módulo 2.

### Escala de risco e ação correspondente

Usada em Chuva intensa, Alagamento, Movimento de massa e Erosão de margem
(Portaria MIDR nº 2.458/2026):

| Grau | Ação | Quando usar |
| --- | --- | --- |
| Baixo | Monitoramento | Normalidade, sem expectativa de impacto. Manter observação. |
| Moderado | Atenção | Pode evoluir para desastre, com impactos localizados ou restritos a grupos vulneráveis. Medidas preventivas. |
| Alto | Preparação | Risco relevante de desastre, com dano material, interrupção de serviço ou impacto à população. Preparar evacuação, abrigamento ou autoproteção. |
| Severo | Ação iminente | Risco de desastre com impacto à população; preparação para evacuação ou autoproteção. **Não usar para previsão genérica com mais de 2 h de antecedência.** |
| Extremo | Ação imediata | Risco iminente com impacto significativo. Evacuação ou abrigamento imediato. |

A qualidade do ar usa escala própria (Boa → Péssima), descrita no Módulo 1.
O Boletim Hidrológico usa só quatro graus (Baixo, Moderado, Alto, Severo).

### Quem pode fazer o quê

🔄 Alterada em 22/10/2026.

- Consultar todas as telas: qualquer pessoa com acesso. Consulta não exige login.
- Classificar, desenhar ou apagar mancha, restaurar monitoramento, lançar cota,
  emitir aviso de 12 h, emitir aviso de 4 h e gerar passagem de turno:
  **qualquer operador autenticado**. O papel não trava produto.
- No SISPDEC, um único papel de operador do Centro cobre essas gravações, até
  este documento criar outra regra.
- Na versão para celular, nada é editável: só consulta.
- 🔄 Vários operadores entram ao mesmo tempo. O teto de 6 era limitação técnica
  do sistema de origem, não regra do Centro. O sistema de origem ainda recusa
  o sétimo login; isso deixa de valer quando a atualização for feita.
- Classificar e editar continua exigindo operador autenticado.

**Papéis no cadastro:** meteorologista plantonista, meteorologista expediente,
geólogo de expediente, chefe, subchefe, operacional e operador IDAP (Interface
de Divulgação de Alertas Públicos). O sistema de origem ainda só conhece
meteorologista plantonista, geólogo de expediente, chefe e operacional.

**Quadro e jornada** (horário de Manaus):

| Seção | Quem | Jornada |
| --- | --- | --- |
| Chefe | CAP. QOABM Charlie Barroso | 09–16 |
| Subchefe | Sgt QCPBM Hirana | 09–16 |
| Adaptação às Mudanças Climáticas | SD QOPM Ananda | 09–16 |
| Monitoramento · meteorologia | FC Luan (A), FC Lenizia (B), FC Adriana (C), FC Gustavo (D). FC Karoline Pereira no administrativo o mês inteiro | Outubro/2026, escala publicada. 1º turno 07–19, repetindo D B A C. 2º turno 19–07, repetindo C D B A, e acaba às 07 do dia seguinte. Em 22/10: Lenizia 07–19 e Gustavo 19–07. Luan e Adriana de folga. O mês seguinte só entra quando a escala daquele mês for publicada |
| Monitoramento · geologia | FC Igor Jacaúna, FC Thayná Nascimento | 09–16 |
| Alertas · operadores IDAP | Equipe A Sgt Albarado, B Sgt Aldeci Reis, C Sgt Galvão, D Sgt Andréia Amorim | Outubro/2026: serviço de 24 h, das 07 às 07, na ordem A B C D. Sobreaviso é a equipe seguinte. Em 13/10 o Albarado fica até 23h30 e a Aldeci assume; em 14/10 a Aldeci fica até 19h e o Galvão assume até 07h de 15/10. Em 22/10 o serviço é o Galvão e o sobreaviso é a Andréia |

O relógio de outubro/2026 segue as escalas publicadas. A Karoline fica no
expediente 09–16 o mês inteiro. Nos outros quatro meteorologistas, só quem
está no turno do dia vê 07–19 ou 19–07; os demais veem folga.
O 2º turno do dia acaba às 07 da manhã seguinte. Na Seção de Alertas o serviço
é de 24 h, das 07 às 07, e o militar da equipe seguinte fica de sobreaviso. A
escala modificada vale nos dias 13 e 14 de outubro. 🔄 **Alterada em 23/10/2026.**
O mês seguinte entra no relógio só quando a escala daquele mês for publicada.
A ordem escrita em 22/10 (Gustavo em novembro, Adriana em dezembro, Lenizia em
janeiro, Luan em fevereiro) não é escala publicada e não entra no relógio. O
horário excepcional continua só no navegador até o SISPDEC gravar no servidor.

### Comportamentos comuns

- **Atualização automática só com a aba visível.** Se o operador troca de aba
  do navegador, a consulta para e volta assim que ele retorna.
- **"Sem leitura" é diferente de "zero" e de "sem sensor".** Toda tela separa
  os três casos e nunca pinta "sem leitura" como se fosse Baixo.
- **Fonte com falha não apaga a tela.** Se uma fonte externa falha, a tela
  continua com a última leitura boa e avisa "usando última leitura".
- **Saúde das fontes (rodapé de todas as telas).** Mostra cada fonte com
  semáforo pela idade da última leitura. Atualiza a cada 60 s.

  | Fonte | Fica amarela (atrasada) | Fica vermelha (indisponível) |
  | --- | --- | --- |
  | CEMADEN (chuva) | 20 min | 45 min |
  | App SELVA (qualidade do ar) | 20 min | 45 min |
  | ANA (cota) | 20 h | 30 h |
  | Painel hidrometeorológico CEMOA (Power BI) | 20 h | 30 h |
  | GOES-19 (satélite) | 30 min | 90 min |

  Fonte nunca consultada aparece como indisponível; isso não prova que o órgão
  está fora do ar.
- 🔄 **Aviso global de qualidade do ar crítica.** Em todas as telas, uma faixa no
  topo lista os municípios com qualidade do ar **Muito Ruim ou Péssima** (pior
  sensor). Atualiza a cada 60 s. Fica **7 segundos** e some sozinha. O operador
  pode recolher antes. Ela volta se a lista de municípios ou os graus mudarem.
- 🔄 **Aviso global de acumulado de chuva.** Em todas as telas, uma faixa no
  topo lista os municípios em que o acumulado medido já atinge **Alto, Severo
  ou Extremo** pelos limiares de **Alagamento (1 h)** ou de **Movimento de massa
  (24 h)** (tabelas no Módulo 1), ordenados do pior grau para o menor e do maior
  acumulado para o menor. Mostra até 4 nomes ("e mais N") e um atalho para
  abrir o produto no Painel. Atualiza a cada 60 s. O operador pode recolher;
  ela volta se a lista, os graus ou os acumulados mudarem.
- 🔄 **Aviso sonoro de chuva severa.** Em todas as telas, a cada 60 s o sistema
  verifica se o acumulado medido de algum município atingiu **Severo ou
  Extremo** pelos limiares de **Chuva intensa** (Severo: ≥ 40 mm/1 h ou ≥ 60 mm/6 h;
  Extremo: ≥ 60 mm/1 h ou ≥ 90 mm/6 h). Se sim, toca três bipes e mostra uma
  notificação com os municípios. Toca **uma vez por município e grau** enquanto
  o navegador estiver aberto; se a chuva continuar no mesmo grau, não repete.
  O som vem ligado; o operador pode silenciar. Não há botão de teste. Nenhum
  outro evento toca (nem vencimento, nem ar Péssima, nem alagamento).
- 🔄 **Relógio de jornada** (cabeçalho do Painel de Alertas). Mostra a jornada
  do operador conectado, o tempo que falta e se está antes, em curso ou
  encerrada (horário de Manaus). Em outubro/2026 o nome vem da escala do dia:
  meteorologista no 1º turno **07–19**, no 2º turno **19–07**, no administrativo
  **09–16**, ou folga. Na Seção de Alertas, serviço de **24 h** (07–07) ou
  sobreaviso. Chefe, subchefe, geólogo e Adaptação às Mudanças Climáticas
  permanecem no expediente **09–16**. O operador pode registrar um horário
  excepcional no expediente e no plantão de 12 h. Esse ajuste fica só no
  navegador até o SISPDEC gravar no servidor.
- 🔄 **Sala de situação.** Modo de tela cheia (só desktop) que esconde cabeçalho,
  lista e rodapé. Os totais por grau ficam numa faixa no topo do mapa, sem
  repetir a mesma contagem na legenda. A legenda guarda só o que o topo não
  mostra (aviso INMET, vento, focos, limiares de alagamento, setores R3/R4,
  sem leitura). Os três municípios de maior grau ficam numa linha, abaixo do
  botão de níveis de risco. Sai com o botão Operação ou Esc.
- **Exportação em PNG.** Os mapas oficiais são exportados como composição
  própria (não é print da tela), com rosa dos ventos, escala gráfica, legenda,
  fonte e horário de Manaus.

### Pendências gerais

- ✅ **G1 — Permissões por ação.** No SISPDEC, quem pode: classificar cada
  produto do painel, desenhar/apagar mancha, restaurar monitoramento, lançar
  cota no boletim, emitir aviso de 12 h, emitir aviso de 4 h, gerar passagem de
  turno? Hoje qualquer operador logado faz tudo.
  **Resposta do CEMOA:** Mantém-se assim. Qualquer operador autenticado faz
  todas essas gravações. Consulta não exige login. No celular nada é editável.
  O papel não trava produto. No SISPDEC, um único papel de operador do Centro
  cobre essas ações até este documento criar outra regra.
- ✅ **G2 — Equipe do plantão.** O sistema de origem tem a equipe fixa no
  sistema (cinco meteorologistas no plantão, dois geólogos no expediente e o
  chefe do Centro). No SISPDEC a equipe vem do cadastro de usuários. Basta
  marcar quem é do plantão, ou há outra regra?
  **Resposta do CEMOA:** Basta o papel no cadastro: meteorologista plantonista,
  meteorologista expediente, geólogo de expediente, chefe, subchefe,
  operacional ou operador IDAP. O quadro e o horário de cada um estão em
  "Quem pode fazer o quê".
- ✅ **G3 — Limite de 6 operadores simultâneos.** Continua sendo uma regra do
  Centro ou era limitação técnica?
  **Resposta do CEMOA:** Era limitação técnica. Vários operadores entram ao
  mesmo tempo. Edição e classificação só podem ser feitas por operador
  autenticado.
- ✅ **G4 — Jornada.** A jornada do meteorologista plantonista é 07–19; em
  seguida outro plantonista assume 19–07. Existe jornada noturna para o
  relógio? O expediente 09–16 vale para geólogo, chefe e operacional? O
  horário excepcional precisa ficar registrado (hoje fica só no navegador)?
  **Resposta do CEMOA:** Sim, o relógio tem jornada noturna. Em outubro/2026 a
  escala publicada manda: Karoline no expediente 09–16; os outros
  meteorologistas no 1º turno 07–19 ou no 2º turno 19–07, conforme o dia, e
  folga quando não estão escalados. A Seção de Alertas faz 24 h, das 07 às 07,
  com sobreaviso da equipe seguinte. Os dias 13 e 14 de outubro seguem a
  escala modificada. O mês seguinte entra quando a escala for publicada. O
  horário excepcional continua só no navegador até o SISPDEC gravar no
  servidor.

---

## Módulo 1 · Painel de Alertas

### Objetivo

Tela principal do plantão. Mostra, para os 62 municípios, o grau de alerta
vigente de cada produto, onde o operador decide e registra a classificação, e
o que precisa de ação agora (fila do plantão). A decisão é do operador; os
sensores são apoio, com as exceções automáticas descritas abaixo.

### O que a tela mostra

- **Seletor de produto:** Chuva intensa (padrão), Alagamento, Movimento de
  massa, Erosão de margem, Incêndio/Qualidade do ar.
- **Totais por grau** (um cartão por grau, com a quantidade e o percentual de
  municípios; a ação de cada grau fica na legenda). Clicar filtra a lista e o
  mapa.
- **Mapa** dos 62 municípios pintados pelo grau, com opacidade ajustável.
- **Lista lateral** de municípios, com a **fila do plantão** no topo.
- **Faixa de alertas** rolando com os alertas ativos.
- 🔄 **Resumo da situação** no topo (desktop), para o recorte aplicado (bacia
  ou calha): os **3 municípios com maior grau** do produto, o **pico** do
  produto (maior acumulado na última hora em Chuva intensa e Alagamento, em 24 h
  em Movimento de massa, maior MP2,5 em Incêndio) e, em Chuva intensa, o aviso
  do INMET vigente. Em Erosão de margem mostra "Grau definido pelo operador".
  Clicar num nome abre o município.
- **Faixa de monitoramento do produto** (resumo dos sensores do produto ativo)
  e **legenda dos limiares** automáticos.
- **Ficha do município** ao clicar (detalhe mais abaixo).
- **Passagem de turno** (resumo do turno, detalhe mais abaixo).

### Produtos de alerta

| Produto | Escala | Quem classifica | Fonte de apoio |
| --- | --- | --- | --- |
| Chuva intensa | Baixo → Extremo | **Só o operador.** O sensor apenas sugere na fila. | CEMADEN (+ ANA onde não há CEMADEN) |
| Alagamento | Baixo → Extremo | **Automático pelo acumulado de 1 h**; o operador pode alterar. | CEMADEN 1 h |
| Movimento de massa | Baixo → Extremo | **Automático pelo acumulado de 24 h**, só em município com área de risco mapeada; o operador pode alterar. | CEMADEN 24 h + setores SGB/CPRM |
| Erosão de margem (terras caídas) | Baixo → Extremo | **Só o operador.** Não há gatilho automático. | — |
| Incêndio/Qualidade do ar | Boa → Péssima | **Automático pelo pior sensor de MP2,5**; o operador classifica por cima. | App SELVA (PurpleAir de reserva) |

Sem classificação, todo município começa no nível mais baixo do produto (Baixo
ou Boa).

### Limiares que o sistema usa

**Chuva intensa — sugestão para a fila (não pinta o mapa)**

| Grau sugerido | 1 hora | ou 6 horas |
| --- | --- | --- |
| Moderado | ≥ 10 mm | ≥ 20 mm |
| Alto | ≥ 20 mm | ≥ 40 mm |
| Severo | ≥ 40 mm | ≥ 60 mm |
| Extremo | ≥ 60 mm | ≥ 90 mm |

**Alagamento — classificação automática (janela de 1 h)**

| Recorte | Moderado | Alto | Severo |
| --- | --- | --- | --- |
| 61 municípios (exceto Manaus) | 20 a 40 mm/h | 40 a 70 mm/h | acima de 70 mm/h |
| Manaus | — | — | acima de 20 mm/h |

**Movimento de massa — classificação automática (janela de 24 h)**

| Recorte | Moderado | Alto | Severo |
| --- | --- | --- | --- |
| 61 municípios (exceto Manaus) | 50 a 85 mm | 85 a 140 mm | acima de 140 mm |
| Manaus | — | — | acima de 30 mm |

- Só sobe em município com **setor de risco mapeado** (hoje 39 municípios, base
  SGB/CPRM). Sem setor mapeado, o município não sobe nem entra na fila.
- A classificação automática **nunca chega a Extremo**: Extremo é sempre do
  operador.

**Qualidade do ar — MP2,5 em tempo real (µg/m³)**

| Faixa | MP2,5 | Ação | No mapa |
| --- | --- | --- | --- |
| Boa | 0 a 12 | Monitoramento | Não pinta |
| Moderada | 12,1 a 35,4 | Atenção | Pinta |
| Ruim | 35,5 a 55,4 | Preparação | Pinta |
| Muito Ruim | 55,5 a 150,4 | Ação iminente | Pinta + aviso global |
| Péssima | acima de 150,4 | Ação imediata | Pinta + aviso global |

- Faixas iguais às do App SELVA/PurpleAir (padrão US AQI sobre o valor bruto).
- O valor do município é o do **pior sensor** em tempo real (leitura atual ou
  de 1 h). **A média de 24 h não define a faixa**; aparece só como referência
  na ficha.
- Sensores descartados: leitura acima de 500 µg/m³, sensor sem transmissão há
  mais de 2 h, sensor interno (indoor) e sensores marcados com defeito.
- **Lista de sensores com defeito** (ignorados no grau, no mapa e no aviso):
  `MPAM_BCA_01_PROMOTORIA` (Boca do Acre).
- **Lista de sensores âncora** (fora da malha, mas contam para o município):
  `SEMA_DCAM_44` → Nhamundá.
- Sensor fora de qualquer município conta para a sede mais próxima, até 55 km.

### Vigência (validade) dos alertas

- Ao classificar, o operador escolhe a duração: **2 h, 4 h, 6 h, 8 h, 10 h,
  12 h, 24 h ou 7 dias**. Duração sugerida pelo grau: Severo, Extremo, Muito
  Ruim e Péssima = 2 h; Alto e Ruim = 4 h; demais = 6 h.
- Prazo de referência pela Portaria (usado quando não há duração escolhida):
  Moderado 6 h · Alto e Ruim 4 h · Severo e Muito Ruim 2 h · Extremo e Péssima
  1 h.
- Cada alerta ativo tem cronômetro regressivo: normal, **atenção a 30 min**,
  **urgente a 10 min**, **vencido** ao zerar.
- **Alerta vencido não volta sozinho para Baixo.** Continua pintado e vai para a
  fila como "Vencido" até o operador renovar ou rebaixar.
- **Manchas (polígonos) vencidas somem do mapa** ao fim da validade.
- Classificação automática tem vigência própria: **Alagamento 6 h** e
  **Movimento de massa 12 h**. Dentro desse prazo, só sobe se o acumulado
  piorar; ao vencer, volta ao valor do sensor. Classificação do operador
  substitui a automática.
- Na Qualidade do ar, se o sensor estiver **pior** que o grau do operador, o
  mapa mostra o grau do sensor.

### Como o operador classifica (desktop, autenticado)

- **Por clique:** escolhe grau e duração e clica no município. Esc encerra.
- **Em lote:** escolhe grau e duração, cola a lista de nomes de municípios
  (por linha ou vírgula) e aplica.
- **Por mancha (polígono):** desenha uma área no mapa; só aquela área recebe a
  cor do grau, sem classificar o município inteiro. Pode apagar uma, várias ou
  todas.
- **Desfazer:** Ctrl+Z desfaz a última classificação ou exclusão de mancha.
- **Restaurar monitoramento:** remove as classificações do operador (de um
  produto ou de todos) e o grau automático volta a valer. É a única forma de
  "zerar" o painel.
- Toda classificação registra **quem**, **quando**, **grau anterior → novo** e
  **como** (clique, lote, desfazer, remoção, limpeza).

### Fila do plantão

Lista no topo da coluna lateral, por produto, em três tipos, nesta ordem:

| Tipo | Quando entra | Ordem dentro do tipo |
| --- | --- | --- |
| Vencido | Alerta ativo com prazo zerado | Mais vencido primeiro |
| Renovar | Alerta ativo com menos de 30 min, **ou** sensor sugerindo grau acima do atual | Menor prazo primeiro |
| Emitir | Município sem alerta e sensor sugerindo grau | Maior acumulado (ou pior MP2,5) primeiro |

- Cada item traz o motivo em texto (ex.: "1 h com 42 mm — limiar de chuva
  severa").
- Contagem resumida no topo da tela ("Plantão: 2 Vencido · 1 Renovar") e
  filtro da fila por tipo (Todos, Vencido, Renovar, Emitir).
- 🔄 **Vencimento não toca mais som** (retirado em 21/10/2026). O som do
  plantão passou a ser só o de chuva severa (ver Regras gerais).

### Alerta "novo" e "agravado"

- Contam a partir do **início do turno atual** (07:00 ou 19:00 de Manaus).
- **Novo:** o município não tinha alerta e passou a ter neste turno.
- **Agravado:** já tinha alerta e subiu de grau neste turno.

### Ficha do município

Ao clicar num município:

- Grau atual, ação, cronômetro e "Classificado por" (operador ou automático, com
  a fonte).
- Uma linha-resumo: grau · mm (ou MP2,5) · temperatura · validade.
- 🔄 A cota aparece em linha própria: valor em metros (ou "sem leitura") e a
  calha. Não traz o grau de inundação do boletim. O fenômeno de chuva no
  painel chama-se Alagamento; inundação é o cenário do Boletim Hidrológico.
- Texto de situação e riscos associados (ex.: chuva intensa na última hora,
  estiagem). O grau de inundação fica no boletim, pelo link "Cota no boletim".
- **Chuva CEMADEN por estação** (1 h, 6 h, 24 h, 72 h e 96 h), com link para o
  gráfico oficial do pluviômetro.
- **Qualidade do ar por monitor** (no produto de incêndio).
- **Temperatura e previsão INMET** (24 h, 48 h, 72 h e 5 dias).
- **Território:** população (Censo 2022), áreas de risco mapeadas, pessoas em
  área de risco e link para o mapa de áreas de risco CPRM do próprio SISPDEC.
- **Histórico de classificações** do município.

### Camadas do mapa

| Camada | Disponível em | Começa |
| --- | --- | --- |
| Sedes municipais | Todos os produtos | Ligada |
| Comunidades rurais | Todos os produtos | Desligada (agrupadas em zoom amplo) |
| Comunidades indígenas | Todos os produtos | Desligada |
| Pontos de medição: pluviômetros (Chuva, Alagamento, Movimento de massa) ou sensores de MP2,5 (Incêndio) | Conforme o produto | Desligada (um único botão liga os pontos do produto ativo) |
| Focos de calor (15 dias) | Incêndio | 🔄 Desligada. Só consulta o INPE quando ligada. Pinta em degradê vermelho pelo total de focos |
| 🔄 Vento a 10 m | Chuva intensa e Incêndio | Ligada. O mesmo traço curto nos dois produtos |
| ➕ Chuva caindo | Só em Chuva intensa | Ligada quando há milímetros em 1 h ou em 6 h no Amazonas |
| Avisos meteorológicos INMET vigentes | Só em Chuva intensa | Ligada (o operador pode desligar) |
| Setores R3/R4 (SGB/CPRM) | Movimento de massa e Erosão | Conforme o produto |
| ➕ Manaus: bairros, curvas de 10 m, zonas e vias com alagamento | Chuva, Alagamento, Movimento de massa e o mapa do boletim | Desligadas, dentro da seção Manaus, que nasce fechada |

- Pluviômetro sem coordenada publicada é posicionado na sede do município (em
  círculo, se houver vários).
- No mapa de chuva, município **sem estação** fica cinza e não conta como Baixo.
- **Focos de calor não mudam o grau de qualidade do ar** no painel; são camada
  de contexto (mas entram no cálculo de risco, Módulo 4).
- 🔄 **Vento** (alterada em 22/10/2026): a mesma marca vale para Chuva intensa
  e Incêndio. A grade continua a do modelo sobre o Amazonas (cerca de 2,2° de
  latitude por 3,2° de longitude; pontos a mais de 170 km de qualquer sede
  ficam de fora). Cada ponto leva um traço curto. 🔄 **Alterada em 23/10/2026.**
  No computador a marca fica visível, no tamanho atual, sem transparência. No
  celular a marca fica menor. **O traço corre para onde o vento sopra agora.** O
  ciclo não desce de 1,8 s; vento mais forte ainda termina antes, até esse
  piso. Quem pede menos movimento no sistema vê a marca parada. Cor pela
  velocidade: azul abaixo de 10 km/h, laranja de 10 a 25 km/h, vinho acima de
  25 km/h. A legenda mostra a hora do dado. É vento do modelo Open-Meteo a
  10 m, não medido. O mesmo botão liga e desliga nos dois produtos.
- ➕ **Chuva caindo** (22/10/2026): no produto Chuva intensa, se alguma estação
  do Amazonas tem acumulado maior que zero em 1 h ou em 6 h, o mapa mostra
  gotas curtas e transparentes por cima do território. O acumulado de 24 h
  sozinho não liga a animação. Some nos outros produtos e quando o sistema
  pede menos movimento. Não entra no PNG e não emite alerta.
- No produto Incêndio, a legenda explica: a cor do município é a classificação
  do ar e o ponto é o MP2,5 do sensor. Na sala de situação essa frase de cor
  não se repete, porque a faixa do topo já mostra os graus; fica a frase do
  ponto.

### Passagem de plantão

🔄 Documento interno de transmissão do turno, gerado pelo operador ao fim do
turno. Estrutura:

1. **Cabeçalho:** "Passagem de plantão" · Governo do Estado do Amazonas ·
   Defesa Civil do Amazonas · Centro de Monitoramento e Alerta — CEMOA.
2. **Identificação:** horário de Manaus, responsável, recorte (filtro
   aplicado) e natureza ("documento interno de transmissão do turno").
3. **O que foi feito** (vindo do histórico de classificações). Se o histórico
   não puder ser lido, o texto diz isso em vez de afirmar que nada foi feito.
4. **Alertas ativos**, agrupados por produto e calha.
5. Três campos livres: **comentário**, **intercorrências** e **avisos gerais**.
6. **Ciência:** "O turno de saída registra a situação descrita neste
   documento. O turno que assume declara ciência ao recebê-lo", com linhas para
   o turno de saída e o turno que assume.

Exporta em texto e em PDF. O PDF tem faixa institucional no topo, rodapé "uso
interno da operação" e numeração de páginas.

### Atualização

| O quê | Frequência |
| --- | --- |
| Alertas, chuva, cotas e qualidade do ar (pacote da tela) | 20 s |
| Avisos INMET vigentes | 1 h |
| Faixa global de qualidade do ar crítica | 60 s |
| 🔄 Faixa global de acumulado de chuva | 60 s |
| 🔄 Verificação do som de chuva severa | 60 s |
| 🔄 Vento (Chuva intensa e Incêndio) | Ao abrir o produto, sem repetir (a fonte renova a cada 30 min) |
| 🔄 Focos de calor | Ao ligar a camada (a fonte renova a cada 6 h) |
| Rodapé de saúde das fontes | 60 s |
| Cronômetros | a cada segundo, sem consulta |

### O que grava

- Classificação do operador por produto e município (grau, quem, quando,
  duração).
- Manchas desenhadas (polígono, grau, quem, quando, duração).
- Histórico de classificações (auditoria).
- Abertura/encerramento do turno do plantão e passagem de turno (no sistema de
  origem ficam só no navegador do operador).

### Fontes

- **CEMADEN** — pluviômetros do Amazonas (1 h, 6 h, 24 h, 72 h, 96 h). O valor
  do município é o da **estação com maior acumulado**.
- **ANA telemetria** — chuva de reserva para município sem CEMADEN (hoje
  Barcelos, Tefé e Santa Isabel do Rio Negro), consultada a cada 15 min; só
  vale leitura das últimas 3 h.
- **App SELVA** — MP2,5 (pior sensor). **PurpleAir** se o SELVA falhar.
- **INPE BDQueimadas** — focos de calor, satélite AQUA_M-T, bioma Amazônia,
  Amazonas, últimos 15 dias.
- 🔄 **Open-Meteo** — vento atual a 10 m (velocidade e direção) na grade do
  Amazonas.
- **INMET** — avisos meteorológicos vigentes (polígonos que cobrem o Amazonas),
  temperatura da estação mais próxima e previsão por município.
- **SGB/CPRM + Casa Civil NT 1/2023** — setores R3/R4 e pessoas em área de
  risco (arquivo fixo, não consultado ao vivo).
- **IBGE Censo 2022** — população, sedes e comunidades (arquivo fixo).

### Pendências do Painel de Alertas

- ✅ **P1.1 — Faixas de MP2,5.** O sistema usa 0–12 / 12,1–35,4 / 35,5–55,4 /
  55,5–150,4 / >150,4 (padrão PurpleAir). A documentação antiga do CEMOA dizia
  0–15 / 15–50 / 50–75 / 75–125 / >125. Qual vale?
  **Resposta do CEMOA:** As duas valem a em tempo real do Appselva e a antiga do CEMOA com níveis em 24h 
- ✅ **P1.2 — Operador x sensor na Qualidade do ar.** Hoje, se o sensor estiver
  pior que o grau do operador, o mapa mostra o sensor (o operador não consegue
  manter um grau abaixo do sensor). A documentação antiga diz que "o operador
  prevalece sobre o sensor". Qual é a regra?
  **Resposta do CEMOA:** O mapa mostra o pior entre operador e sensor. O
  operador pode elevar acima do sensor. Se o sensor ficar pior, o mapa sobe
  para o sensor. O operador não segura um grau abaixo do sensor.
- ✅ **P1.3 — Duração sugerida do Extremo.** A Portaria indica 1 h para Extremo,
  mas a duração sugerida na hora de classificar é 2 h. Manter 2 h ou 1 h?
  **Resposta do CEMOA:** A sugestão na classificação continua 2 h para
  Extremo. A 1 h da Portaria só entra quando ninguém escolhe duração.
- ✅ **P1.4 — Duração de 12 h.** O sistema oferece 12 h entre as opções; a
  documentação antiga não lista. Manter?
  **Resposta do CEMOA:** Manter 12 h na lista.
- ✅ **P1.5 — Alerta vencido x mancha vencida.** O alerta do município vencido
  continua pintado até o operador agir, mas a mancha vencida some sozinha. É
  intencional?
  **Resposta do CEMOA:** Não o alerta pode sumir junto. 
- ✅ **P1.6 — Chuva intensa automática.** Hoje os limiares de chuva intensa só
  sugerem na fila. Deve continuar só sugestão, ou passar a pintar como
  Alagamento e Movimento de massa?
  **Resposta do CEMOA:** Continua só sugestão na fila e no som. Não pinta o
  mapa. Quem pinta Chuva intensa é o operador.
- ✅ **P1.7 — Extremo automático.** Alagamento e Movimento de massa sobem no
  máximo até Severo. Há limiar para Extremo?
  **Resposta do CEMOA:** Não há limiar automático de Extremo. Extremo é só do
  operador.
- ✅ **P1.8 — Erosão de margem sem gatilho.** O produto é 100% manual. Deve usar
  algum dado (velocidade de descida do rio, grau de estiagem do boletim)?
  **Resposta do CEMOA:** Sim pode usar esses parâmetros por enquanto para o gatiilho de erosão
- ✅ **P1.9 — Atualização das listas de sensores.** Quem mantém a lista de
  sensores com defeito e de sensores âncora, e com que frequência? No SISPDEC
  isso pode virar cadastro na tela.
  **Resposta do CEMOA:** As listas vigentes estão no sistema: defeito
  `MPAM_BCA_01_PROMOTORIA`; âncora `SEMA_DCAM_44` para Nhamundá. Quem inclui
  ou tira é o CEMOA, por alteração deste documento. Não há rotina periódica.
  No SISPDEC pode virar cadastro, com a mesma lista de partida.
- ✅ **P1.10 — Base de setores de risco.** A lista de municípios com setor
  mapeado (39) vem de arquivo fixo SGB/CPRM. Qual a data dessa base e quem avisa
  quando sair atualização?
  **Resposta do CEMOA:** A base vigente é o recorte operacional CEMOA ·
  SGB/CPRM de setores mapeados (deslizamento, movimento de massa ou erosão de
  margem). O arquivo não traz data de vigência. Quem avisa atualização é o
  CEMOA (geologia), substituindo o arquivo e registrando a data neste
  documento. Ausência de mapeamento não significa ausência de risco residual.
- ✅ **P1.11 — Município sem pluviômetro.** São Sebastião do Uatumã segue sem
  CEMADEN e sem estação ANA de reserva. Existe outra fonte para ele?
  **Resposta do CEMOA:** Não há outra fonte ligada. O município segue sem
  leitura de chuva até este documento apontar uma estação real.
- ✅ **P1.12 — Faixa de acumulado x setor mapeado.** A faixa global de
  acumulado lista Movimento de massa em **qualquer** município que passe do
  limiar de 24 h, mesmo **sem setor de risco mapeado**. Já a classificação
  automática do mapa só sobe onde há setor mapeado. A faixa deve seguir a mesma
  regra do mapa?
  **Resposta do CEMOA:** As duas ficam diferentes. A faixa avisa qualquer
  município acima do limiar de 24 h. O mapa só sobe Movimento de massa onde há
  setor mapeado.
- ✅ **P1.13 — Som só para chuva severa.** Desde 21/10/2026 o vencimento de
  alerta e o fim do aviso de 12 h não tocam mais som; só a chuva severa toca. É
  intencional? Algum outro evento deve tocar (ex.: ar Péssima, acumulado de
  alagamento)?
  **Resposta do CEMOA:** É intencional. Só a chuva intensa em Severo ou
  Extremo toca. Ar Péssima, alagamento e vencimento não tocam.
- ✅ **P1.14 — Repetição do som.** O som toca uma vez por município e grau
  enquanto o navegador está aberto; se a chuva continuar severa, não repete. No
  SISPDEC, deve repetir de tempos em tempos enquanto a condição durar?
  **Resposta do CEMOA:** Não repete enquanto o mesmo município permanecer no
  mesmo grau. Um grau novo volta a tocar.
- ✅ **P1.15 — Vento previsto.** O vento vem do modelo Open-Meteo (não é
  medição) e aparece só no produto Incêndio. O CEMOA aceita essa fonte? Deve
  aparecer em outros produtos (ex.: Chuva intensa)?
  **Resposta do CEMOA:** Aceita o Open-Meteo só no Incêndio, com os traços
  animados na direção em que sopra. Não entra nos outros produtos.
  🔄 **Alterada em 22/10/2026.** O vento passou a aparecer também em Chuva
  intensa, com o mesmo traço curto dos dois produtos. A frase anterior, que
  limitava o vento ao Incêndio, fica substituída por esta.

---

## Módulo 2 · Boletim Hidrológico

### Objetivo

Acompanhar a **estiagem** e a **inundação** (cheia lenta dos rios) por
município e calha: cota do dia, tendência, comparação com as cotas de
referência e com os extremos históricos, e o grau oficial do boletim do CEMOA.
Também gera o mapa oficial em PNG de estiagem e de inundação.

### O que a tela mostra

- **Seletor de cenário:** Estiagem (vazante) ou Inundação (enchente). O mapa,
  os totais e a lista mudam conforme o cenário.
- 🔄 **Totais por grau:** Baixo, Moderado, Alto, Severo, com leitura e sem
  leitura. O rodapé de cada um desses cartões mostra só a porcentagem do
  recorte. O cartão de municípios mostra Total ou Recorte.
- **Mapa** dos municípios pelo grau do cenário escolhido.
- **Lista por calha**, ordenada dentro da calha do pior grau para o melhor.
- **Linha do tempo** para voltar até 6 dias atrás.
- **Faixa** rolando com as estações.
- **Ficha da estação** ao clicar (detalhe abaixo).
- Filtros: calha, grau, com/sem leitura, município, busca por nome, calha ou
  código da estação.

### Graus

| Grau | Ação | Cor |
| --- | --- | --- |
| Baixo | Monitoramento | Verde |
| Moderado | Atenção | Amarelo |
| Alto | Preparação | Laranja |
| Severo | Ação iminente | Vermelho |
| Sem leitura | — | Cinza (município sem cota no dia operacional) |

Não existe Extremo no boletim.

### Regras

- **O grau vem do boletim oficial do CEMOA**, um grau para estiagem e outro
  para inundação por município. **A cota ao vivo não recalcula o grau.** O
  operador ajusta o grau manualmente quando necessário.
- **As cotas de referência** (Moderado e Alto, por cenário) aparecem como
  linhas no gráfico da estação, como apoio visual. Hoje existem para 23 dos 62
  municípios.
- **Dia do boletim:** a partir das **16:00 de Manaus** o boletim é o do dia
  atual; antes disso, o do dia anterior.
- **Cota do dia:** é a leitura das **07:00 de Manaus**, ou a mais próxima desse
  horário no mesmo dia.
- **Ordem de prioridade da cota** (a última vence):
  1. Boletim base do CEMOA;
  2. Painel hidrometeorológico CEMOA (Power BI), cota mais próxima das 07:00;
  3. Telemetria ANA do dia (estação automática);
  4. Lançamento manual do operador.
- **Situação da leitura** mostrada em cada estação: "Atualizado" (cota de
  hoje), "Dado de DD/MM" (série antiga), "Sem leitura", "Sem estação". Município
  sem cota de hoje **não herda a cor de ontem sem aviso**.
- **Tendência:** subindo, baixando, estável, vazante ou sem tendência.
- **Série:** até 90 dias por estação, exibindo a última semana.
- **Projeção:** reta pelos últimos 5 pontos da série, projetada em +3, +5 e +7
  dias. Só aparece com pelo menos 3 leituras. É apoio, não previsão oficial.
- **Extremos históricos:** no cenário de inundação mostra a máxima histórica
  (cota e data); no de estiagem, a mínima histórica, e a distância da cota
  atual até ela.
- **Fonte da cota** aparece na ficha: estação automática (ANA), painel
  hidrometeorológico CEMOA, boletim CEMOA (Defesa Civil AM) ou não informada.

### Calhas

Alto Solimões · Baixo Amazonas · Baixo Solimões · Juruá · Madeira · Médio
Amazonas · Médio Solimões · Negro · Purus.

Correspondência bacia (Painel de Alertas) → calha (Boletim): Japurá → Médio
Solimões; Rio Negro → Negro; as demais têm o mesmo nome.

### Ano hidrológico

🔄 **Alterada em 23/10/2026.** O ano da calha tem dois picos e, entre eles, o
processo que leva de um ao outro. A tabela abaixo é a média. Azul é o pico de
enchente. Laranja é o pico de vazante (estiagem). O mês em branco não é um
terceiro regime: é o processo em curso.

No Juruá, a média do pico de vazante é agosto e setembro. Depois dele, outubro,
novembro, dezembro, janeiro, fevereiro, março e abril são de enchente; março e
abril são o pico. A partir de maio começa o processo de vazante, até o pico de
agosto e setembro. Nas outras calhas a fase muda de mês, mas a sequência é a
mesma: depois do pico de vazante vem a enchente, até o pico de enchente;
depois desse pico começa a vazante, até o pico de vazante.

🔄 **Alterada em 23/10/2026.** O mês da máxima e o mês da mínima são os meses
de pico em que aquela estação permanece na enchente ou na vazante. A máxima
recorrente é o pico de enchente. A mínima recorrente é o pico de vazante. O
mês sai do histórico da própria estação: em cada ano fechado, a data em que a
cota atingiu o extremo, e o mês que mais se repete. A tabela da calha é a média
dessa recorrência. Ela não troca o cenário no dia 1º, nem antecipa o pico que
a estação ainda não atingiu.

Depois desse período de pico começa o processo de transição. Depois da máxima,
a estação sai da enchente e entra na vazante. Depois da mínima, sai da vazante
e entra na enchente. O rio e a época mudam o mês, não a sequência.

A tabela não traz Japurá. No boletim, Japurá continua na lista do Médio
Solimões; isso não copia para Japurá os picos do Médio Solimões. O boletim
lista o Negro numa calha só; a média abaixo separa Alto Negro e Baixo Negro.

| Calha | Pico de enchente | Pico de vazante |
| --- | --- | --- |
| Juruá | março e abril | agosto e setembro |
| Purus | março e abril | setembro e outubro |
| Madeira | abril e maio | setembro e outubro |
| Alto Solimões | abril e maio | setembro, outubro e novembro |
| Médio Solimões | maio e junho | outubro e novembro |
| Baixo Solimões | maio e junho | outubro e novembro |
| Médio Amazonas | maio e junho | outubro e novembro |
| Baixo Amazonas | maio e junho | outubro e novembro |
| Alto Negro | junho e julho | janeiro e fevereiro |
| Baixo Negro | maio e junho | outubro, novembro e dezembro |

Na cheia (o processo de enchente, do fim do pico de vazante até o fim do pico
de enchente) o fator de alerta de inundação se inverte, como em P4.4. ➕ O
sistema de origem ainda não aplica esta regra: o fator de inundação continua
sempre "sem alerta" (0,30).

### Ficha da estação

Município, rio, calha, cota atual, variação em 24 h, estação/código, fonte da
cota e horário da leitura, máxima ou mínima histórica, grau de estiagem e de
inundação lado a lado (com as cotas de referência), situação da leitura e
gráfico da série com as linhas de referência e a projeção.

### Lançamento manual (operador autenticado)

- Por estação ou **em lote** (lista de nomes de municípios).
- Pode lançar: grau de estiagem, grau de inundação, cota com data, ou marcar
  "sem leitura". Pode lançar só o grau, sem cota.
- O lançamento fica gravado e prevalece sobre as fontes automáticas.

### Atualização

| O quê | Frequência |
| --- | --- |
| Tela do boletim | 25 s |
| Consulta à ANA (cota) | 1 vez por dia, às 16:00 de Manaus |
| Consulta ao painel hidrometeorológico CEMOA | 07:00 e 16:00 de Brasília |

### O que grava

- Lançamentos manuais por estação: grau de estiagem, grau de inundação, cotas
  por data, quem lançou e quando.

### Fontes

- **Boletim base do CEMOA** — grau oficial de estiagem e inundação, cotas de
  referência, extremos históricos, rios e calhas. Hoje é um arquivo do sistema
  de origem com data de referência de 01/09/2026.
- **ANA** — telemetria das estações automáticas (cota em metros).
- **Painel hidrometeorológico CEMOA (Power BI/Fabric)** — cota municipal.
- **SGB e SEMA/DC-AM** — citados como origem no boletim base.

### Pendências do Boletim Hidrológico

- ✅ **P2.1 — Grau automático pela cota.** Hoje o grau não muda com a cota; só
  com o boletim base ou lançamento manual. Deve ser calculado comparando a cota
  do dia com as cotas de referência? Se sim, como tratar os 39 municípios sem
  referência?
  **Resposta do CEMOA:** A cota não recalcula o grau. O grau é o do boletim
  base ou o lançado pelo operador. 
- ✅ **P2.2 — Origem oficial do grau no SISPDEC.** O boletim base do sistema de
  origem está parado em 01/09/2026. No SISPDEC, quem informa o grau oficial
  diário e em que tela? (O SISPDEC já tem cadastro de cotas por calha no
  módulo CEMOA; confirmar se é o mesmo dado.)
  **Resposta do CEMOA:** Os dados de cota são os mesmos, porém o grau de risco é estabelecido pelos técnicos do CEMOA.
- ✅ **P2.3 — Escala sem Extremo.** O boletim usa quatro graus e o painel cinco.
  Confirmar que inundação e estiagem não têm Extremo.
  **Resposta do CEMOA:** Confirmado. Inundação e estiagem têm Baixo,
  Moderado, Alto e Severo por enquanto.
- ✅ **P2.4 — Projeção de +3/+5/+7 dias.** A reta simples sobre 5 pontos é aceita
  pelo CEMOA como apoio ao plantão, ou deve sair da tela?
  **Resposta do CEMOA:** Permanece na ficha da estação como apoio, não como
  previsão oficial. Exige pelo menos três leituras e usa os últimos cinco
  pontos.
- ✅ **P2.5 — Prioridade da cota.** Confirmar a ordem: boletim < Power BI < ANA
  < operador.
  **Resposta do CEMOA:** Confirmada. A última da lista vence, nesta ordem:
  boletim base, Power BI (cota mais próxima das 07:00), telemetria ANA do dia,
  lançamento do operador.
- ✅ **P2.6 — "Mudanças em 24 h".** A tela tem uma área de mudanças das últimas
  24 h que hoje está sempre vazia. O que ela deve listar?
  **Resposta do CEMOA:** Lista mudanças de grau oficial do boletim (município,
  estiagem ou inundação, de qual grau para qual). Está vazia porque o boletim
  base não tem linhas nessa lista, não porque a área não tenha função. Sem
  linhas, o texto é "Nenhuma alteração neste recorte."
- ✅ **P2.7 — Cenário padrão.** Qual cenário a tela abre por padrão (estiagem ou
  inundação)? Muda conforme a época do ano?
  **Resposta do CEMOA:** Abre conforme época do ano, como estamos em período de estiagem abre primeiro a estiagem

---

## Módulo 3 · Meteorologia

### Objetivo

Reunir a situação meteorológica do Amazonas para o plantão: chuva observada,
previsão, temperatura, imagem de satélite, contexto climático e a emissão dos
avisos meteorológicos (plantão de 12 h e aviso gráfico de 4 h). Esta tela não
classifica alerta; quem classifica é o Painel de Alertas.

### Abas

#### Chuva (observada)

- Mapa dos municípios em **degradê de azul** pelo acumulado da janela
  escolhida: **1 h, 6 h, 24 h ou 72 h**.

  | Acumulado | Cor |
  | --- | --- |
  | Sem pluviômetro | Cinza |
  | 0 mm | Quase branco |
  | até 5 mm | Azul muito claro |
  | 5 a 12,5 mm | Azul claro |
  | 12,5 a 25 mm | Azul médio |
  | 25 a 50 mm | Azul forte |
  | 50 mm ou mais | Azul escuro |

- Indicadores: municípios com chuva na janela, maior acumulado (e onde),
  municípios com leitura, municípios com chuva intensa em 1 h (**≥ 20 mm**).
- Ranking dos municípios mais chuvosos.
- **Satélite GOES-19, infravermelho realçado (canal 13):** recorte do Amazonas
  com limites municipais, hora da cena em Manaus e **loop da última hora**
  (6 cenas). Aviso quando a cena tem mais de 30 min.
- Exportação do mapa em PNG.

#### Previsão

- Mapa por município com a variável escolhida: **chuva 6 h, chuva 24 h,
  temperatura, probabilidade de chuva 6 h, probabilidade 24 h**.
- Indicadores: municípios com chuva prevista em 24 h, maior chuva prevista em
  24 h, maior probabilidade, temperatura agora.
- Tabela por município com chuva 6 h/24 h, probabilidade e máxima/mínima do dia.
- Probabilidade em degradê: <20%, 20–40%, 40–60%, 60–80%, ≥80%.

#### Temperatura

- Temperatura **instantânea** só das estações INMET que estão transmitindo, com
  hora da leitura e umidade.
- **Não classifica onda de calor**: o estado não tem estações suficientes.
- Cor do ponto (só leitura): <26 °C azul · 26–30 verde · 30–33 amarelo · 33–36
  laranja · ≥36 vermelho.

#### Clima

- **MERGE/CPTEC:** anomalia de chuva do **mês anterior e do mês atual**
  (vermelho = déficit, azul = excesso), **dias sem chuva no mês** e **maior
  sequência de dias secos no mês**.
- É **contexto de estiagem**; não classifica chuva nem alerta.
- O CPTEC publica os índices do mês já fechado; se o mês atual não existe, a
  tela mostra o mais recente disponível.

### Aviso meteorológico do plantão (12 h)

- Acompanha os **turnos de 12 h**: diurno **07:00–19:00** e noturno
  **19:00–07:00** (Manaus).
- O aviso emitido **vale até o fim do turno em que foi emitido**, não 12 h
  corridas a partir da emissão.
- Sinalização do cartão: **amarelo** quando falta 1 h para o fim do turno;
  **urgente** a 15 min ou vencido (pede nova emissão).
- Só operador identificado emite; aviso sem nome de operador não é aceito.
  Campo de observação opcional.
- O aviso de 12 h é compartilhado entre todos os postos.
- 🔄 **Não há mais som no fim da vigência** (retirado em 21/10/2026). O cartão
  continua mudando de cor.

### Aviso gráfico (4 h)

- Janelas fixas de 4 h (Manaus): **02–06, 06–10, 10–14, 14–18, 18–22, 22–02**.
  O aviso vale até o fim da janela em que foi montado.
- **Código sequencial no formato N/ANO**, reiniciando em 1 a cada ano.
- Conteúdo: código, período de validade, texto do cenário (há um texto padrão
  para começar), **calhas atingidas** e **evolução potencial** (marcadas numa
  lista), e a imagem GOES-19 recortada no Amazonas com limites municipais.
- Lista de calhas do aviso: Alto, Médio e Baixo Solimões; Alto, Médio e Baixo
  Amazonas; Alto, Médio e Baixo Rio Negro; Alto, Médio e Baixo Juruá; Alto,
  Médio e Baixo Purus; Madeira; Japurá.
- Sai em **PNG institucional em formato retrato**.
- Sem operador identificado sai como **rascunho** ("não publicado · sem revisão
  de operador"), sem numerar oficialmente.
- Se a cena GOES tiver mais de 30 min, o sistema avisa e o PNG leva a hora da
  imagem, não a da emissão. Sem cena, avisa que o PNG sai sem satélite.
- 🔄 **Alterada em 23/10/2026.** Emitir o aviso de 4 h não registra o aviso de plantão de 12 h. Cada plantão emite no máximo 3 avisos meteorológicos; em 24 h, no máximo 6. ➕ Essa cota e a separação dos dois avisos ainda não estão no sistema de origem: lá, emitir o de 4 h ainda grava o de 12 h.

### Atualização

| O quê | Frequência |
| --- | --- |
| Chuva observada | 20 s |
| Aviso de 12 h (estado compartilhado) | 20 s |
| Previsão | 30 min |
| Temperatura | 10 min, só com a aba Temperatura aberta |
| GOES-19 | ao abrir e no botão atualizar (cena nova a cada 10 min) |
| MERGE | ao abrir (imagem guardada por 1 h) |

### O que grava

- Aviso de plantão de 12 h (quem, quando, observação).
- Aviso gráfico de 4 h: no sistema de origem fica **só no navegador** de quem
  emitiu (inclusive o último código usado).

### Fontes

- **CEMADEN** e **ANA telemetria** — chuva observada.
- **GOES-19 · CPTEC/INPE** — infravermelho realçado, canal 13 (repositório
  DISSM).
- **Open-Meteo** — previsão por município (chuva, probabilidade e
  temperatura, 2 dias).
- **INMET** — temperatura das estações automáticas e convencionais, e previsão
  Prevmet na ficha do município (Painel de Alertas).
- **MERGE/CPTEC (data.inpe.br/dados/merge)** — anomalia mensal e índices de
  dias secos.

### Pendências da Meteorologia

- ✅ **P3.1 — Numeração do aviso de 4 h.** Hoje o último código fica só no
  navegador de quem emitiu; dois postos podem gerar o mesmo número. No SISPDEC a
  numeração será central. Confirmar: sequência única do estado por ano?
  **Resposta do CEMOA:** O formato desejado é N/ANO, zerando a cada ano, numa
  sequência única do estado. No sistema de origem o último código continua só
  no navegador de quem emitiu. A sequência central é entrega do SISPDEC, ainda
  não existe na origem.
- ✅ **P3.2 — Lista de calhas do aviso.** O aviso usa 17 calhas (Alto/Médio/Baixo)
  e o boletim usa 9. As duas listas estão corretas e devem continuar diferentes?
  **Resposta do CEMOA:** Sim. As duas listas permanecem. O aviso parte a calha
  em Alto, Médio e Baixo; o boletim usa as nove calhas.
- ✅ **P3.3 — Fonte da previsão.** O mapa de previsão usa Open-Meteo; a ficha do
  município usa INMET Prevmet. O CEMOA aceita o Open-Meteo como fonte oficial
  do mapa?
  **Resposta do CEMOA:** Aceita as duas, cada uma no seu lugar. Mapa de
  previsão: Open-Meteo. Ficha do município: INMET Prevmet.
- ✅ **P3.4 — Aviso de 4 h x aviso de 12 h.** Emitir o de 4 h renova o de 12 h
  automaticamente. É intencional?
  **Resposta do CEMOA:** Não. O aviso de 4 h não renova o de 12 h. Cada plantão emite no máximo 3 avisos meteorológicos, e 6 em 24 h. ➕ Ainda não está no sistema de origem.
- ✅ **P3.5 — Histórico dos avisos.** Os avisos de 4 h precisam ficar
  consultáveis (lista, reimpressão do PNG)? Hoje não ficam.
  **Resposta do CEMOA:** No sistema vigente não ficam. A emissão gera o PNG
  na hora e não guarda lista para reimpressão.
- ✅ **P3.6 — Escala de temperatura.** As faixas de cor (26/30/33/36 °C) são
  definição do CEMOA?
  **Resposta do CEMOA:** Sim
---

## Módulo 4 · Gestão de Risco

### Objetivo

Priorizar os 62 municípios pela metodologia de risco do CEMOA (IVM, IRE e IRG),
combinando vulnerabilidade estrutural, ameaça por tipo de evento, exposição,
histórico (agravo) e o que está acontecendo agora (fator de alerta). Serve para
ordenar e priorizar; **não muda as cores do Painel de Alertas nem do Boletim**.

### Abas

| Aba | Conteúdo |
| --- | --- |
| Operação | Mapa em degradê pelo indicador escolhido (IRG ou um dos seis IRE), ranking e ficha municipal, exportação em PNG |
| Análise | Pizzas e barras por nível, por prioridade e por evento crítico; top 15 do IRG |
| Território | Mapa temático de população, densidade, % rural, % crianças, % idosos e % indígena (Censo 2022) |
| Logística | Classe de acesso de cada município (rodovia, aeródromo, fluvial) e calha |
| Decretos | Total histórico de decretos de estiagem ou de inundação por município, em mapa e lista |
| Dados | Tabela completa ordenável e download em CSV |

Indicadores no topo: municípios, quantos em N6 Extremo, N5 Crítico, P1
confirmado e rebaixados. Filtros: calha, prioridade, classe do IVM, busca e
recortes rápidos (N6, N5, P1, rebaixados).

### Peças da metodologia

| Peça | O que é | De onde vem |
| --- | --- | --- |
| IVM | Vulnerabilidade estrutural do município | Valor pronto, importado do aplicativo original do CEMOA. Não é recalculado. |
| Ameaça | Parâmetro fixo por tipo de evento | Tabela abaixo |
| FS | Fator de sensibilidade por evento | Tabela abaixo |
| FE | Fator de exposição | População em área de risco, adensamento, ruralidade e terra indígena |
| FA | Fator de alerta | O que está acontecendo agora (classificação e sensores) |
| Agravo | Histórico e criticidade por evento | Nota de 0 a 9 importada; na inundação, decretos |
| IRE | Índice de risco de cada um dos seis eventos | Fórmula abaixo |
| IRG | Índice de risco global do município | Fórmula abaixo |

Eventos: **Estiagem, Inundação, Incêndio/Qualidade do ar, Erosão, Movimento de
massa, Chuvas.** Eventos súbitos: Incêndio/QAr, Movimento de massa e Chuvas.

### Fórmulas (como o sistema calcula hoje)

```text
IRE (evento) = (IVM + Ameaça) × FS × FE × FA  +  Agravo        (máximo 60)

IRG = 0,6 × maior IRE  +  0,2 × média dos 6 IRE  +  0,2 × IVM escalado   (máximo 60)

IVM escalado = (IVM ÷ 21) × 60
```

- O FA e o FE multiplicam só a parte dinâmica; o **agravo é somado por fora**.
- Valores arredondados em duas casas.

### Classes do IVM

| Classe | Nome | IVM |
| --- | --- | --- |
| D | Muito Alta | 19 ou mais |
| C | Alta | 15 a 18,9 |
| B | Média | 10 a 14,9 |
| A | Baixa | abaixo de 10 |

### Parâmetros

**Ameaça**

| Evento | Base | Manaus |
| --- | --- | --- |
| Estiagem | 8 | 8 |
| Inundação | 8 | 8 |
| Incêndio/QAr | 8 (+8 se município PIMF) | 15 (+8 PIMF = 23) |
| Erosão | 6 | 6 |
| Movimento de massa | 5 | 12 |
| Chuvas | 5 | 15 |

Municípios prioritários do PIMF (Plano Integrado de Manejo do Fogo): 29
municípios, lista mantida no sistema.

**Fator de sensibilidade (FS):** Estiagem 1,1 · Inundação 0,9 · Incêndio/QAr
1,1 · Erosão 1,1 · Movimento de massa 0,9 · Chuvas 1,0.

**Fator de alerta (FA)**

| Nível | FA |
| --- | --- |
| Sem alerta | 0,30 |
| Moderado | 0,70 |
| Alto (ou ar Ruim) | 1,00 |
| Severo (ou ar Muito Ruim) | 1,30 |
| Extremo (ou ar Péssima) | 1,60 |

**De onde sai o nível de alerta de cada evento (vale o pior):**

| Evento | Entradas |
| --- | --- |
| Estiagem | Grau de estiagem do Boletim Hidrológico |
| Inundação | Sempre "Sem alerta" (0,30) |
| Chuvas | Classificação de Chuva intensa, classificação de Alagamento e limiar automático de alagamento (1 h) |
| Movimento de massa | Classificação de Movimento de massa e limiar automático de 24 h |
| Erosão | Classificação de Erosão de margem e grau de estiagem do Boletim |
| Incêndio/QAr | 🔄 Se o município tem **sensor de MP2,5 com leitura** ou **classificação manual do operador**: o pior entre os dois, e os focos **não** entram. Só sem sensor e sem classificação manual os focos de calor definem o nível |

**Faixa de focos de calor** (AQUA-Tarde, 15 dias) para o FA de incêndio: 0 =
Sem alerta · 1 a 4 = Moderado · 5 a 14 = Alto · 15 a 39 = Severo · 40 ou mais =
Extremo. *Esta faixa foi criada na operação do Centro e não faz parte da
metodologia original.* 🔄 Desde 21/10/2026 ela só vale para município **sem
sensor de ar e sem classificação manual**: um sensor que mostra ar Moderado
não fica Extremo só porque a contagem de focos é alta.

**Agravo**

- Estiagem: nota importada (0 a 9) + bônus contextual, limitado a 9. Bônus
  contextual: 2 se rural ≥ 35% ou terra indígena ≥ 30%; 1 se rural ≥ 15% ou
  terra indígena ≥ 10%; vale o maior.
- Inundação: proporcional ao total de decretos de inundação do município em
  relação ao município com mais decretos, até 11 pontos. Com FA 0,30 isso
  mantém o IRE de inundação abaixo de 20 (Baixo) e serve só para ordenar o
  ranking.
- Demais eventos: nota importada (0 a 9).

**Exposição (FE)**

- Estiagem: aumenta com % rural, % de terra indígena e quanto menor o
  município em relação a Manaus (até +60%).
- Inundação: sempre 1,0.
- Demais eventos: 1,0 se não houver população em área de risco R3/R4; caso
  contrário, cresce com a proporção e o total de pessoas em área de risco e com
  o adensamento urbano, com pesos diferentes para Chuvas/Movimento de massa,
  Incêndio e Erosão.
- População em área de risco: o **maior** valor entre o levantamento do
  aplicativo original e o levantamento SGB/CPRM + Casa Civil (Censo 2022).

### Níveis e prioridade

| IRG | Nível | Nome | Prioridade |
| --- | --- | --- | --- |
| 58 ou mais | 6 | Extremo | P1 |
| 50 a 57,9 | 5 | Crítico | P1 |
| 40 a 49,9 | 4 | Alto | P2 |
| 30 a 39,9 | 3 | Elevado | P3 |
| 20 a 29,9 | 2 | Moderado | P4 |
| abaixo de 20 | 1 | Baixo | P4 |

**Confirmação de P1:** o município precisa ter pelo menos **2 eventos em nível
5 ou mais**, ou **1 em nível 5 ou mais e 1 em nível 4**. Além disso:

- **Nível 6 (Extremo):** fica Extremo só com confirmação **e** algum evento com
  alerta Extremo. Com confirmação e alerta Severo, desce para Crítico. Nos
  outros casos, desce para Alto (P2).
- **Nível 5 (Crítico):** fica Crítico com confirmação **e** algum alerta ativo.
  Sem isso, desce para Alto (P2).
- "Rebaixado" = tinha pontuação de P1 confirmada, mas nenhum alerta ativo.
- A cor no mapa segue o IRG calculado, mesmo quando a prioridade é rebaixada.

### Logística (aba própria)

- **Modal:** rodoviário para os municípios da Região Metropolitana ligados a
  Manaus por estrada (Autazes, Careiro, Careiro da Várzea, Iranduba,
  Itacoatiara, Itapiranga, Manacapuru, Manaquiri, Manaus, Novo Airão,
  Presidente Figueiredo, Rio Preto da Eva, Silves); fluvial para os demais.
- **Classe de acesso** (nesta ordem): hub aéreo e rodoviário (Manaus) ·
  rodovia e aeródromo público · rodovia sem aeródromo público · fluvial com
  aeródromo público · fluvial com pista privada · somente fluvial.
- **Posto logístico** (ordem de facilidade de acesso) por pontos: rodovia 55 (ou
  20 sem rodovia); aeródromo público +30; com operação por instrumentos +8;
  com operação noturna +6; aeródromo de maior porte +6; aeroporto de Manaus +15;
  só pista privada +10 a +22 conforme pavimento e comprimento (≥ 1.000 m).

### Atualização

| O quê | Frequência |
| --- | --- |
| Índices, chuva e cotas | 20 s |

### O que grava

Nada. O módulo só calcula e mostra. Exporta PNG (mapa oficial com top 10) e
CSV.

### Fontes

- **Parâmetros da metodologia** (IVM, % terra indígena, agravo por evento,
  calha, setores R3/R4 originais): aplicativo original do CEMOA, arquivo fixo.
- **IBGE Censo 2022:** população, % rural, densidade, faixas etárias, %
  indígena.
- **SGB/CPRM + Casa Civil NT 1/2023:** pessoas em área de risco.
- **Decretos de estiagem e inundação:** arquivo fixo com total, anos e último
  ano por município.
- **ANAC:** aeródromos públicos (01/10/2026) e privados (14/10/2026).
- **Ao vivo:** classificações do Painel de Alertas, grau do Boletim, CEMADEN,
  App SELVA e focos INPE.

### Pendências da Gestão de Risco

- ✅ **P4.1 — Pesos do IRG.** O sistema usa 0,6 × maior IRE + 0,2 × média +
  0,2 × IVM escalado. O caderno metodológico antigo diz 0,7 × maior IRE + 0,3 ×
  média. Qual vale?
  **Resposta do CEMOA:** Vale a fórmula do sistema: 0,6 × maior IRE + 0,2 ×
  média dos seis IRE + 0,2 × IVM escalado, teto 60. O peso do caderno antigo
  não vale.
- ✅ **P4.2 — Posição do agravo.** O sistema soma o agravo **depois** de FE e FA.
  O caderno antigo coloca o agravo **dentro**, multiplicado por FE e FA. Qual
  vale?
  **Resposta do CEMOA:** Vale a conta do sistema: o agravo entra depois de FE
  e FA, não dentro do produto.
- ✅ **P4.3 — FA de Estiagem e Erosão.** A documentação antiga diz FA fixo de
  1,0 para esses eventos. O sistema usa o grau de estiagem do boletim (Baixo =
  0,30). Qual vale?
  **Resposta do CEMOA:** Vale o grau de estiagem do boletim. FA fixo de 1,0
  não vale.
- ✅ **P4.4 — FA de Inundação.** O sistema usa sempre "Sem alerta" (0,30), sem
  olhar o grau de inundação do boletim. Isso deve mudar na época de cheia?
  **Resposta do CEMOA:** Muda: na cheia o cenário se inverte. A média dos picos está no ano hidrológico do Boletim. O que inicia a transição é a estação atingir o próprio pico de vazante ou de enchente, não o calendário sozinho. ➕ O sistema de origem ainda usa sempre "sem alerta" (0,30).
- ✅ **P4.5 — Faixa de focos de calor.** A faixa 1–4 / 5–14 / 15–39 / ≥40 foi
  criada na operação e não tem validação institucional. Desde 21/10/2026 ela só
  vale para município sem sensor de ar e sem classificação manual. Manter,
  ajustar ou retirar do cálculo?
  **Resposta do CEMOA:** Manter, só quando o município não tem leitura de
  MP2,5 e não tem classificação manual. Sensor e operador prevalecem.
- ✅ **P4.6 — Cores dos níveis.** A documentação antiga lista cores diferentes
  das usadas no sistema. Qual paleta é a oficial (ou fica com a designer do
  DDS)?
  **Resposta do CEMOA:** Fica com a designer do DDS
- ✅ **P4.7 — Cor do rebaixado.** Município rebaixado de P1 para P2 aparece como
  "Alto" mas com a cor do Crítico. É intencional?
  **Resposta do CEMOA:** Sim. O rótulo acompanha o nível novo; a cor guarda o
  nível de origem enquanto o rebaixamento estiver vigente.
- ✅ **P4.8 — Empate no teto de 60.** Municípios acima do teto empatam no
  ranking. Precisa de critério de desempate?
  **Resposta do CEMOA:** Não. Quem passa de 60 empata no teto. O desempate é que o municipio com o pior IVM fica na frente
- ✅ **P4.9 — Japurá e Maraã.** O caderno antigo diz que "não foram
  reclassificados". O que precisa ser feito com eles?
  **Resposta do CEMOA:** Entram na mesma fórmula dos demais municípios. Não
  há reclassificação pendente.
- ✅ **P4.10 — Recomendações por município.** Existe uma base de recomendações
  do CEMOA no sistema de origem que não é usada em nenhuma tela. Deve aparecer
  na ficha municipal?
  **Resposta do CEMOA:**  Continua fora da ficha até este documento criar essa regra.
- ✅ **P4.11 — Atualização dos parâmetros.** Quem atualiza IVM, agravos,
  decretos e lista PIMF, e com que frequência? No SISPDEC isso pode virar
  cadastro.
  **Resposta do CEMOA:** Quem atualiza é o CEMOA, por
  substituição do arquivo e registro neste documento. Não há rotina periódica
  no sistema mas pode ser feita mensalmente ou caso tenha no SISPDEC esse registro pelo DR
  - ✅ **P4.12 — Pontuação logística.** Os pontos da classe de acesso são definição
  do CEMOA? Devem ser revistos?
  **Resposta do CEMOA:** Os pontos podem ser revistos.

---

## Anexo A · Fontes externas

| Fonte | Endereço | O que traz | Usado em |
| --- | --- | --- | --- |
| CEMADEN | `resources.cemaden.gov.br/graficos/interativo/getJson2.php?uf=AM` | Chuva 1 h a 96 h por pluviômetro | Painel, Meteorologia, Risco |
| CEMADEN (gráfico) | `resources.cemaden.gov.br/graficos/interativo/grafico_CEMADEN.php` | Link do gráfico do pluviômetro | Ficha do Painel |
| ANA telemetria | `telemetriaws1.ana.gov.br/ServiceANA.asmx/DadosHidrometeorologicos` | Cota e chuva das estações automáticas | Boletim, Painel |
| Painel hidrometeorológico CEMOA | Relatório público Power BI/Fabric | Cota municipal | Boletim |
| App SELVA | `appselva.com.br/api.php?route=purpleair` | MP2,5 por sensor | Painel, Risco, aviso global |
| PurpleAir | `api.purpleair.com/v1/sensors` (exige chave) | MP2,5 (reserva do SELVA) | Painel |
| INPE BDQueimadas | `dataserver-coids.inpe.br/queimadas/.../focos_diario_br_AAAAMMDD.csv` | Focos diários (AQUA_M-T) | Painel, Risco |
| INMET avisos | `apiprevmet3.inmet.gov.br/avisos/ativos` | Avisos vigentes com polígono | Painel |
| INMET Prevmet | `apiprevmet3.inmet.gov.br/previsao/{IBGE}` e `/estacao/proxima` | Previsão e estação mais próxima | Ficha do Painel |
| INMET estações | `apitempo.inmet.gov.br/estacoes/T` e `/M` | Temperatura instantânea | Meteorologia |
| Open-Meteo | `api.open-meteo.com/v1/forecast` | Previsão por município; 🔄 vento atual a 10 m | Meteorologia; Painel (Chuva intensa e Incêndio) |
| GOES-19 · CPTEC/INPE | `satelite.cptec.inpe.br/repositoriogoes/goes19/...ams_ret_ch13_baixa/` | Imagem infravermelho canal 13 | Meteorologia, aviso 4 h |
| MERGE · CPTEC | `ftp.cptec.inpe.br/modelos/tempo/MERGE/FIG/CLIMATOLOGY/...` | Anomalia mensal, dias secos | Meteorologia |
| IBGE Censo 2022 | Arquivo fixo | População, sedes, comunidades, malha | Todos |
| SGB/CPRM + Casa Civil NT 1/2023 | Arquivo fixo | Setores R3/R4 e pessoas em risco | Painel, Risco |
| ANAC | Arquivo fixo | Aeródromos públicos e privados | Risco |
| Decretos | Arquivo fixo | Decretos de estiagem e inundação | Risco |

---

## Anexo B · O que cada módulo grava

| Módulo | Registro | Campos principais |
| --- | --- | --- |
| Painel de Alertas | Classificação do operador | Produto, município, grau, grau anterior, quem, quando, duração |
| Painel de Alertas | Mancha (polígono) | Produto, grau, área desenhada, municípios atingidos, quem, quando, duração |
| Painel de Alertas | Histórico de classificações | Quando, produto, município, grau anterior → novo, quem, como |
| Painel de Alertas | Turno do plantão / passagem de plantão | Turno, quem abriu, quando abriu e encerrou, texto da passagem |
| Painel de Alertas | 🔄 Horário excepcional de jornada | Perfil de jornada, hora de início e de fim |
| Boletim Hidrológico | Lançamento manual | Estação, grau de estiagem, grau de inundação, cotas por data, quem, quando |
| Meteorologia | Aviso de plantão 12 h | Quem, quando, observação (validade = fim do turno) |
| Meteorologia | Aviso gráfico 4 h | Código, validade, texto, calhas, evolução, cena GOES, quem |
| Gestão de Risco | — | Não grava |
| Todos | Sessão de operador no posto | Quem, último sinal de presença (a cada 20 s) |

No sistema de origem, a passagem de plantão, a abertura de turno, o horário
excepcional de jornada e o aviso de 4 h ficam só no navegador. No SISPDEC devem ser gravados no servidor.

---

## Anexo C · Frequência de atualização

| Tela / dado | Tela consulta a cada | A fonte é renovada a cada |
| --- | --- | --- |
| Painel de Alertas (alertas, chuva, cotas, ar) | 20 s | Chuva 2 min · Ar 1 min · Cota 1x/dia |
| Avisos INMET no mapa | 1 h | 1x por dia (Manaus) |
| Faixa global de ar crítico | 60 s | 1 min |
| 🔄 Faixa global de acumulado de chuva | 60 s | 2 min |
| 🔄 Som de chuva severa | 60 s | 2 min |
| 🔄 Vento no Painel (Chuva intensa e Incêndio) | ao abrir o produto | 30 min |
| Rodapé de saúde das fontes | 60 s | — |
| Boletim Hidrológico | 25 s | ANA 16:00 Manaus · Power BI 07:00 e 16:00 Brasília |
| Meteorologia · chuva | 20 s | 2 min |
| Meteorologia · aviso 12 h | 20 s | — |
| Meteorologia · previsão | 30 min | 30 min |
| Meteorologia · temperatura | 10 min (só com a aba aberta) | 10 min |
| Meteorologia · GOES-19 | ao abrir / botão | cena a cada 10 min |
| Meteorologia · MERGE | ao abrir | mensal |
| Focos de calor | 🔄 ao ligar a camada | 6 h |
| Gestão de Risco | 20 s | conforme as fontes acima |

---

## Zonas de influência da chuva

🔄 **Alterada em 22/10/2026.** A camada entrou no mapa do Painel de Alertas.
Não cria um painel novo, não substitui as áreas de risco já cadastradas e não
inventa estação, limiar ou polígono. Se um dado não existir, a tela diz a
limitação e usa o que o sistema já tem.

A geometria vigente fora de Manaus é um círculo de raio fixo, o mesmo para Moderado, Alto, Severo e Extremo. As opções são 5, 10 e 15 km; o padrão é 5 km. Onde o catálogo das PCDs do CEMADEN traz a coordenada, o ponto é esse. Nos demais municípios, o círculo usa a localidade que o mapa já usa ou, na falta dela, a sede municipal.

O desenho da zona, no mapa e na exportação, é sempre o raio de perigo: o círculo fixo. Ele não cresce com o grau. 🔄 **Alterada em 22/10/2026.** Cada raio tem um ponto no centro da estação e a marca dos quilômetros na borda norte. O ponto continua clicável quando dois círculos se sobrepõem. O clique abre a ficha. Se o círculo já ocupa boa parte da tela, a vista não salta; se não ocupa, o mapa aproxima o círculo. O mapa também pode ligar, à parte, os bairros de Manaus e as curvas de 10 m lidas na grade das curvas SIRGAS 2000. Essa grade tem passo de cerca de 440 m; as linhas não são as curvas originais. Em Manaus, os bairros que cruzam o raio e cuja cota mínima, medida nessa grade, não fica acima da cota da estação entram na análise. O clique num desses bairros destaca o nome e a população; não troca o raio e não emite alerta. O bairro inteiro não é tratado como se tivesse recebido o mesmo acumulado. A ficha soma a população residente de 2022 desses bairros. O número vem da agregação preliminar dos setores censitários do IBGE publicada pela Perspectiva em abril de 2024; o IBGE não publica tabela oficial de bairros. Bairro sem correspondência fica sem dado e fora da soma. Se a cota ou o bairro não fecharem a lista, a ficha diz isso e o círculo permanece.

Círculo na sede, ou leitura sem hora ou com mais de 45 minutos, aparece na ficha como delimitação não confiável. A classificação reusa os limiares vigentes. 🔄 **Alterada em 22/10/2026.** Movimento de massa continua medido em 24 h e alagamento em 1 h, com os mesmos milímetros. No raio de perigo e na ficha da zona, o grau para em Severo: a faixa Moderado do produto e as faixas Alto e Severo são desenhadas como Severo. Em São Paulo de Olivença, uma leitura entre 50 e 85 mm em 24 h fica Severo no raio. O mapa do município não muda: a classificação automática continua no máximo em Severo. Estação em Baixo não gera zona. A camada nasce desligada. KMZ e GeoJSON saem só quando o operador pede. O raio não gera PNG próprio. 🔄 **Alterada em 22/10/2026.**

A análise da chuva continua no Módulo 3. A distribuição espacial e a
exportação entram no mapa do Módulo 1.

### Objetivo

Separar o que cada módulo faz e ligar os dois:

- o Módulo de Meteorologia analisa a chuva e entrega o resultado processado;
- o Painel de Alertas mostra, no mapa que já existe, onde essa chuva pode
  estar atingindo áreas de risco, e permite exportar essa região.

Nenhuma zona dispara alerta oficial sozinha.

### Regras

**1. Meteorologia continua dona da análise.**

No Módulo 3 permanecem: consulta dos pluviômetros já cadastrados; acumulados
nos intervalos que o sistema já calcula; intensidade média, quando a série
tiver tempo bastante para estimá-la; duração e evolução da chuva; qualidade,
atualidade e continuidade das leituras; entrega desse resultado ao Painel de
Alertas. O Painel não refaz essas contas. Reusa a mesma fonte e o mesmo
resultado.

**2. Camada no Painel de Alertas.**

No mapa do Painel, uma camada chamada **Zonas de influência da chuva**. Ela:

- usa as localizações dos pluviômetros que já existem;
- com a camada ligada, desenha no mapa só os pluviômetros que estão registrando chuva (acumulado maior que zero em 1 h ou em 6 h), com o valor ao lado do ponto;
- consome o resultado processado da Meteorologia;
- desenha uma estimativa da distribuição espacial da precipitação;
- classifica com os limiares já vigentes neste documento;
- sobrepõe essa geometria aos polígonos de risco já cadastrados;
- deixa o operador ver a área potencialmente atingida e escolher polígonos e
  setores de interesse;
- pode ser ligada e desligada.

Não cria limiar novo e não altera os atuais.

**3. Metodologia espacial.**

A primeira geometria é preliminar e configurável. Na ficha e na exportação ela
se identifica como **estimativa de influência da precipitação**. A estrutura
fica pronta para, quando houver dado, usar pluviômetros vizinhos, interpolação,
radar ou estimativa de satélite. Essas vias não entram enquanto o dado não
existir.

Não tratar o interior de um buffer como se tivesse recebido o mesmo acumulado.
Não aumentar o raio só porque o grau passou de Moderado para Alto, Severo ou
Extremo. O tamanho da zona não é o grau. 🔄 **Alterada em 23/10/2026.** No raio
de movimento de massa e de alagamento, o desenho e a ficha param em Severo:
a faixa Moderado do produto e as faixas Alto e Severo são desenhadas como
Severo. Chuva intensa pode continuar em Extremo. O mapa do município não usa
esse recorte e a classificação automática continua no máximo em Severo. Quando
a leitura não permitir um contorno confiável, a interface diz isso, em vez de
desenhar uma zona segura.

**4. Ficha da zona.**

Ao selecionar uma zona, a ficha mostra o que o dado tiver:

- estação ou estações relacionadas;
- acumulado;
- intensidade média, quando houver;
- intervalo temporal;
- data e hora da leitura;
- classificação vigente;
- método usado para gerar a geometria;
- polígonos de risco interceptados;
- municípios, bairros ou setores, conforme o cadastro existente;
- qualidade e limitações da estimativa.

A zona selecionada fica destacada. O mapa aproxima o círculo quando ele ainda não ocupa boa parte da tela. O
operador pode selecionar, uma a uma, as áreas de risco interceptadas. A ficha
do município continua aberta junto com a do bairro e a do raio; as três ficam
por cima do mapa e não mudam o enquadramento. Isso não emite alerta oficial.

**5. PNG do raio.**

🔄 **Alterada em 22/10/2026.** O raio de perigo não gera PNG. KMZ e GeoJSON
continuam na ficha. O PNG institucional dos outros mapas (painel, boletim,
meteorologia e gestão de risco) permanece.

**6. Exportar zona KMZ.**

Ação **Exportar zona KMZ**. O arquivo traz a geometria que o mapa usou, para
abrir em programa geográfico (Google Earth e equivalentes). Dá para exportar
uma zona, várias zonas, e, quando o operador pedir, os polígonos de risco
interceptados. Metadados disponíveis: estação, acumulado, classificação,
período, data e método. Geometrias válidas, coordenadas no sistema adequado e
cores das classes já vigentes. O mesmo conteúdo também sai em GeoJSON.

**7. Integração.**

As funções atuais dos dois módulos permanecem. Zona nova ou atualizada não
envia alerta. A camada atualiza quando chega observação válida nova. Leitura
velha ou insuficiente aparece como limitação, não como zona fechada. O mapa
não ganha geometria que não ajude a leitura. Desktop e celular: ligar e
desligar a camada, selecionar a zona e ler a ficha. 🔄 **Alterada em 22/10/2026.**
No celular a ficha do raio fica na base do mapa e ocupa menos da metade da
área, para o círculo continuar visível. Bairro, pluviômetro, via e zona
administrativa ficam na parte de cima e não cobrem essa ficha. O ponto central
tem área de toque maior. A chuva que cai no mapa fica só sobre o mapa, com
menos gotas, e a marca de vento fica menor. No celular continua valendo a
regra geral: nada é editável; exportar é consulta, não classificação.

### Quem pode fazer o quê

- Ver a camada, selecionar a zona, ler a ficha, exportar KMZ e GeoJSON — operador
  autenticado no desktop; no celular, a mesma consulta, sem gravar alerta.
- Classificar ou emitir alerta — continua só pelo fluxo já descrito no
  Módulo 1. A zona não é um atalho.

### Atualização

| O quê | Frequência |
| --- | --- |
| Resultado de chuva usado pela zona | o mesmo ritmo já vigente da Meteorologia e do CEMADEN |
| Geometria da zona | quando chega observação válida nova |
| Leitura velha ou insuficiente | desenha a estimativa e a ficha diz que a delimitação não é confiável |

### O que grava

Nada de alerta. KMZ e GeoJSON são arquivos baixados pelo operador. Não há envio
automático. O raio não gera PNG.

### Fontes

As que o sistema já usa: pluviômetros e acumulados do Módulo 3, limiares deste
documento, polígonos e setores de risco já cadastrados. Nenhuma estação, faixa
ou polígono novo entra com esta atualização.

### Testes de aceitação

1. Acumulado e intensidade continuam calculados na Meteorologia.
2. O Painel consome esse resultado e não repete a conta.
3. A camada usa os pluviômetros e os polígonos de risco que já existem.
4. A classificação usa os limiares vigentes, sem faixa nova.
5. O operador seleciona uma região potencialmente atingida.
6. 🔄 **Alterada em 23/10/2026.** O mapa aproxima o círculo só quando ele ainda não ocupa boa parte da tela.
7. O raio não gera PNG. KMZ e GeoJSON levam o círculo desenhado no mapa.
8. O KMZ abre em programa geográfico com a geometria desenhada no mapa.
9. Desktop e celular: ligar a camada, selecionar e ler a ficha.
10. Classificar, fila, som, boletim, avisos e gestão de risco seguem como
    estão neste documento.

### Entrega

A atualização entra no projeto existente. O registro da entrega lista o que
foi reusado, a metodologia espacial adotada e os testes acima. Se faltar
dado para um método mais fino, a entrega fica na geometria preliminar
configurável e escreve a limitação aqui, com data.

---

## Zonas administrativas e vias de alagamento

➕ Camadas opcionais do mapa. Bairros, curvas de nível, zonas e vias nascem desligadas e nenhuma emite alerta. As quatro ficam na seção **Manaus** do menu de camadas. A seção nasce fechada. Quando alguma está ligada, o cabeçalho mostra quantas estão ligadas.

- **Zonas de Manaus.** Seis polígonos dos arquivos `Zona_Norte`, `Zona_Sul`, `Zona_Leste`, `Zona_Oeste`, `Zona_Centro_Sul` e `Zona_Centro_Oeste`. Esses arquivos não trazem atributos. A ficha calcula a área do polígono e junta os bairros cujo centro cai dentro dele: quantidade, população do Censo 2022 quando existe, e cotas mínima e média já gravadas no bairro. O clique no nome do bairro abre a ficha do bairro.
- **Vias com alagamento.** Só entram avenidas nomeadas em reportagem ou nota da Seminf e encontradas no cadastro `ARRUAMENTO_MANAUS`: Torquato Tapajós, Max Teixeira, André Araújo e Djalma Batista (G1, 19/03/2025); Autaz Mirim (Seminf, abril de 2026); Avenida Brasil (Em Tempo, 25/03/2026). A linha é o eixo do cadastro de fevereiro de 2000 e janeiro de 2002, não o trecho medido do alagamento. O arquivo inteiro do arruamento não vai para o navegador.

---

## Modelo para módulo novo

Copie este bloco, troque os textos entre `<>` e preencha. Se algo não se
aplica, escreva "não se aplica" em vez de apagar o item.

```markdown
## Módulo N · <Nome do módulo>

### Objetivo
<Para que serve a tela e quem usa. Que decisão ela ajuda a tomar.>

### O que a tela mostra
- <Bloco 1: o que aparece e para quê>
- <Bloco 2>

### Regras
- <Regra 1: quando acontece X, o sistema faz Y.>
- <Limiares, faixas, cores, prazos — de preferência em tabela.>

### Quem pode fazer o quê
- <Ação> — <perfil>

### Atualização
| O quê | Frequência |
| --- | --- |
| <dado> | <a cada N minutos / ao abrir / 1x por dia às HH:MM> |

### O que grava
- <Registro, campos, quem grava. Ou "Nada.">

### Fontes
- **<Órgão / sistema>** — <endereço ou arquivo> — <o que traz>

### Pendências
- ❓ **PN.1 — <assunto>.** <pergunta>
```

---

## Histórico de alterações

| Data | Quem | Módulo | O que mudou |
| --- | --- | --- | --- |
| 21/10/2026 | DDS | Todos | Levantamento inicial a partir do sistema de origem (versão de 19/10/2026), com pendências para o CEMOA. |
| 21/10/2026 | DDS | Gerais, Painel, Meteorologia, Risco | Conferência com a versão 9a7b1a7 do sistema de origem. **Regras novas:** faixa global de acumulado de chuva, som de chuva severa (substitui o som de vencimento e o de fim do aviso de 12 h), relógio de jornada, resumo da situação no Painel, camada de vento no Incêndio, passagem de plantão em formato de documento com ciência. **Regra alterada:** fator de alerta de Incêndio na Gestão de Risco (sensor e operador prevalecem sobre focos); camada de focos passa a começar desligada. **Só visual (sem mudança de regra):** cartões de totais mostram só a quantidade e o percentual, cor do Moderado legível em fundo claro, lista e fila mais compactas. Pendências novas: G4, P1.12 a P1.15. |
| 22/10/2026 | CEMOA | Documento mestre | Este arquivo passa a ser o documento mestre da plataforma (versão 0adae65). **Respostas:** G1 a G4, P1.1 a P1.15, P2.1 a P2.7, P3.1 a P3.6 e P4.1 a P4.12. Onde o caderno antigo divergia, vale o sistema vigente. **Regras alteradas:** aviso de ar crítica some em 7 s; som só de chuva severa ou extrema, sem botão de teste e sem repetir o mesmo grau; sala de situação com os graus só na faixa do topo; vento corre para onde sopra; cota da ficha sem o grau de inundação. **Só visual:** rodapé dos cartões do boletim só com a porcentagem; ponto do sensor de ar menor. **Próxima atualização (ainda não construída):** zonas de influência da chuva no Painel, com ficha, PNG e KMZ, sem limiar novo e sem alerta automático. |
| 22/10/2026 | CEMOA | Gerais | Respostas de G2, G3 e G4 revistas pelo CEMOA e levadas para "Quem pode fazer o quê". **Papéis:** meteorologista plantonista, meteorologista expediente, geólogo de expediente, chefe, subchefe, operacional e operador IDAP. **Quadro:** Barroso e Hirana (09–16); Ananda (09–16); meteorologista expediente 09–16; meteorologistas Lenizia, Luan, Karoline, Gustavo e Adriana em turnos 07–19 e 19–07; geólogos Igor e Thayná (09–16); IDAP Aldeci, Andreia, Albarado e Galvão (07–19 e 19–07). **Postos:** o teto de 6 era limitação técnica; vários operadores entram ao mesmo tempo, e só operador autenticado classifica ou edita. O sistema de origem ainda não tem esses papéis, o turno 19–07 no relógio nem a retirada do teto. |
| 22/10/2026 | CEMOA | Gerais | Escala dos meteorologistas: entre Lenizia, Luan, Karoline, Gustavo e Adriana, um fica no expediente 09–16 no mês e os outros quatro no plantão. No mês seguinte, outro assume o expediente e quem saiu volta ao plantão. O documento não fixa quem está no expediente neste mês. |
| 22/10/2026 | CEMOA | Gerais | Expediente de outubro/2026: FC Karoline Pereira (09–16). Lenizia, Luan, Gustavo e Adriana no plantão. |
| 22/10/2026 | CEMOA | Gerais | O relógio passou a aplicar a troca mensal e o turno 19–07. Em outubro/2026 a Karoline fica no expediente 09–16. Os outros meteorologistas veem Plantão 07–19 e, das 19 às 07, Plantão 19–07. A ordem da troca é Gustavo (novembro/2026), Adriana (dezembro), Lenizia (janeiro/2027), Luan (fevereiro) e de novo a Karoline. |
| 22/10/2026 | CEMOA | Gerais | As escalas publicadas de outubro/2026 substituem a fila mensal. Meteorologia: Karoline no administrativo; turnos de 12 h nas equipes A Luan, B Lenizia, C Adriana e D Gustavo (1º turno DBAC, 2º turno CDBA). Alertas: 24 h das 07 às 07 nas equipes A Albarado, B Aldeci, C Galvão e D Andréia, com a modificação dos dias 13 e 14. Quem não está escalado vê folga; a equipe seguinte vê sobreaviso. |
| 22/10/2026 | CEMOA | Painel de Alertas | Zonas de influência da chuva no mapa existente. A Meteorologia continua dona do acumulado e da intensidade. O Painel só desenha. Geometria: círculo de 5, 10 ou 15 km, igual em todos os graus, na posição que o mapa já usa para o pluviômetro. Sem limiar novo e sem alerta automático. Ficha, KMZ e GeoJSON. |
| 22/10/2026 | CEMOA | Painel de Alertas | Manaus usa a coordenada do catálogo CEMADEN e o recorte por bairros abaixo da cota. O raio fica acima do município, com contorno branco, ponto central e marca de km. Bairros, curvas, zonas e vias ficam na seção Manaus, desligadas. Chuva intensa anima gotas quando há mm em 1 h ou 6 h. O vento a 10 m também vale nesse mapa. No raio de massa e alagamento o desenho para em Severo. O raio não gera PNG. |
| 23/10/2026 | CEMOA | Documento mestre | Arestas de texto alinhadas a regras já decididas: o README deixa de tratar a zona como não construída; a metodologia do raio repete que massa e alagamento param em Severo; GeoJSON deixa de ser condicional. A linha solta do histórico de 22/10 voltou para a tabela. |
| 23/10/2026 | CEMOA | Meteorologia, Risco, Painel, Gerais | Respostas do CEMOA. O aviso de 4 h não registra o de 12 h; no máximo 3 avisos por plantão e 6 em 24 h, ainda não construído na origem. Na cheia o fator de inundação se inverte. No computador a marca de vento fica visível; no celular, menor. O mapa só aproxima o círculo quando ele ainda não ocupa a tela. Novembro não entra no relógio até a escala daquele mês ser publicada. |
| 23/10/2026 | CEMOA | Boletim, Risco | Ano hidrológico. A tabela é a média do pico de enchente e do pico de vazante por calha. Entre os picos corre o processo. No Juruá, depois de agosto e setembro vêm outubro a abril de enchente (março e abril no pico) e maio abre a vazante. O que rege a estação é a tendência depois que ela atinge o próprio pico. Japurá não herda o Médio Solimões. Ainda não construído na origem. |
| 23/10/2026 | CEMOA | Boletim | O mês da máxima recorrente é o pico em que a estação permanece na enchente. O mês da mínima recorrente é o pico em que permanece na vazante. Depois desse período começa a transição: da enchente para a vazante, ou da vazante para a enchente, conforme o rio e a época. |
