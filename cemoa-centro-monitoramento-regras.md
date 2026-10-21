# CEMOA · Centro de Monitoramento — Regras de negócio

> **Para quem é este documento.** Para o geólogo do CEMOA, que é o dono das
> regras, e para o DDS (Setor de Desenvolvimento), que constrói o Centro de
> Monitoramento como submódulo do SISPDEC.
>
> **O que ele é.** A ponte entre o CEMOA e o DDS. Aqui está o que cada tela faz,
> de onde tira os dados, de quanto em quanto tempo atualiza, o que grava e cada
> regra que hoje está fixa no sistema. Não fala de código.
>
> **O que ele não é.** Não é o manual visual. A designer do DDS vai adaptar o
> layout; o que vale aqui é a regra, não a aparência.

| Item | Valor |
| --- | --- |
| Sistema de origem | Aplicação do CEMOA (`cemoa-centro-operacoes`) |
| Última conferência com o sistema de origem | 21/10/2026 (versão 9a7b1a7) |
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

### Quem pode fazer o quê (hoje)

- Consultar todas as telas: qualquer pessoa com acesso.
- Classificar, desenhar manchas, lançar cota, emitir aviso: **qualquer operador
  autenticado**. Hoje não há diferença entre perfis na hora de gravar.
- Perfis conhecidos: Meteorologista plantonista, Geólogo (expediente), Chefe do
  Centro e Operacional.
- Até **6 operadores** conectados ao mesmo tempo no posto. Entrar com o mesmo
  usuário em outro terminal transfere a sessão.
- Na versão para celular, nada é editável: só consulta.

> No SISPDEC, cada ação vira uma permissão própria, configurável no controle de
> acesso (ver pendência G1).

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
- **Aviso global de qualidade do ar crítica.** Em todas as telas, uma faixa no
  topo lista os municípios com qualidade do ar **Muito Ruim ou Péssima** (pior
  sensor). Atualiza a cada 60 s. O operador pode recolher; ela volta sozinha se
  a lista de municípios ou os graus mudarem.
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
  o navegador estiver aberto. O som vem ligado; o operador pode silenciar e há
  um botão **Testar som** (desktop), que toca o aviso e fala "teste de som".
- 🔄 **Relógio de jornada** (cabeçalho do Painel de Alertas). Mostra a jornada
  do operador conectado, o tempo que falta e se está antes, em curso ou
  encerrada (horário de Manaus). Padrão: **Plantão 07–19** para meteorologista;
  **Expediente 09–16** para os demais perfis. O operador pode registrar um
  horário excepcional (início e fim em horas cheias, mínimo de 1 h), que fica
  marcado como excepcional.
- **Sala de situação.** Modo de tela cheia (só desktop) que esconde cabeçalho,
  lista e rodapé, deixando o mapa, os totais por grau com a ação correspondente
  e a faixa de alertas. Sai com o botão Operação ou Esc.
- **Exportação em PNG.** Os mapas oficiais são exportados como composição
  própria (não é print da tela), com rosa dos ventos, escala gráfica, legenda,
  fonte e horário de Manaus.

### Pendências gerais

- ❓ **G1 — Permissões por ação.** No SISPDEC, quem pode: classificar cada
  produto do painel, desenhar/apagar mancha, restaurar monitoramento, lançar
  cota no boletim, emitir aviso de 12 h, emitir aviso de 4 h, gerar passagem de
  turno? Hoje qualquer operador logado faz tudo.
- ❓ **G2 — Equipe do plantão.** O sistema de origem tem a equipe fixa no
  sistema (cinco meteorologistas no plantão, dois geólogos no expediente e o
  chefe do Centro). No SISPDEC a equipe vem do cadastro de usuários. Basta
  marcar quem é do plantão, ou há outra regra?
- ❓ **G3 — Limite de 6 operadores simultâneos.** Continua sendo uma regra do
  Centro ou era limitação técnica?
- ❓ **G4 — Jornada.** A jornada padrão do meteorologista é Plantão 07–19, mas
  o aviso de 12 h trabalha com turno diurno (07–19) e noturno (19–07). Existe
  jornada noturna para o relógio? O Expediente 09–16 vale para geólogo, chefe e
  operacional? O horário excepcional precisa ficar registrado (hoje fica só no
  navegador)?

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
- Uma linha-resumo: grau · mm (ou MP2,5) · temperatura · cota · validade.
- Texto de situação e riscos associados (ex.: chuva intensa na última hora,
  inundação no grau X do boletim, estiagem).
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
| 🔄 Vento a 10 m | Incêndio | Ligada |
| Avisos meteorológicos INMET vigentes | Só em Chuva intensa | Ligada (o operador pode desligar) |
| Setores R3/R4 (SGB/CPRM) | Movimento de massa e Erosão | Conforme o produto |

- Pluviômetro sem coordenada publicada é posicionado na sede do município (em
  círculo, se houver vários).
- No mapa de chuva, município **sem estação** fica cinza e não conta como Baixo.
- **Focos de calor não mudam o grau de qualidade do ar** no painel; são camada
  de contexto (mas entram no cálculo de risco, Módulo 4).
- 🔄 **Vento:** setas sobre uma grade de pontos do modelo que cobre o
  Amazonas (cerca de 2,2° de latitude por 3,2° de longitude; pontos a mais de
  170 km de qualquer sede ficam de fora). **A seta aponta para onde o vento
  sopra.** Cor pela velocidade: azul abaixo de 10 km/h, laranja de 10 a 25 km/h,
  vinho acima de 25 km/h. A legenda mostra a hora do dado. É vento previsto
  pelo modelo, não medido.
- No produto Incêndio, a legenda explica: a cor do município é a classificação
  do ar e o ponto é o MP2,5 do sensor.

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
| 🔄 Vento (produto Incêndio) | Ao abrir o produto, sem repetir (a fonte renova a cada 30 min) |
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

- ❓ **P1.1 — Faixas de MP2,5.** O sistema usa 0–12 / 12,1–35,4 / 35,5–55,4 /
  55,5–150,4 / >150,4 (padrão PurpleAir). A documentação antiga do CEMOA dizia
  0–15 / 15–50 / 50–75 / 75–125 / >125. Qual vale?
- ❓ **P1.2 — Operador x sensor na Qualidade do ar.** Hoje, se o sensor estiver
  pior que o grau do operador, o mapa mostra o sensor (o operador não consegue
  manter um grau abaixo do sensor). A documentação antiga diz que "o operador
  prevalece sobre o sensor". Qual é a regra?
- ❓ **P1.3 — Duração sugerida do Extremo.** A Portaria indica 1 h para Extremo,
  mas a duração sugerida na hora de classificar é 2 h. Manter 2 h ou 1 h?
- ❓ **P1.4 — Duração de 12 h.** O sistema oferece 12 h entre as opções; a
  documentação antiga não lista. Manter?
- ❓ **P1.5 — Alerta vencido x mancha vencida.** O alerta do município vencido
  continua pintado até o operador agir, mas a mancha vencida some sozinha. É
  intencional?
- ❓ **P1.6 — Chuva intensa automática.** Hoje os limiares de chuva intensa só
  sugerem na fila. Deve continuar só sugestão, ou passar a pintar como
  Alagamento e Movimento de massa?
- ❓ **P1.7 — Extremo automático.** Alagamento e Movimento de massa sobem no
  máximo até Severo. Há limiar para Extremo?
- ❓ **P1.8 — Erosão de margem sem gatilho.** O produto é 100% manual. Deve usar
  algum dado (velocidade de descida do rio, grau de estiagem do boletim)?
- ❓ **P1.9 — Atualização das listas de sensores.** Quem mantém a lista de
  sensores com defeito e de sensores âncora, e com que frequência? No SISPDEC
  isso pode virar cadastro na tela.
- ❓ **P1.10 — Base de setores de risco.** A lista de municípios com setor
  mapeado (39) vem de arquivo fixo SGB/CPRM. Qual a data dessa base e quem avisa
  quando sair atualização?
- ❓ **P1.11 — Município sem pluviômetro.** São Sebastião do Uatumã segue sem
  CEMADEN e sem estação ANA de reserva. Existe outra fonte para ele?
- ❓ **P1.12 — Faixa de acumulado x setor mapeado.** A faixa global de
  acumulado lista Movimento de massa em **qualquer** município que passe do
  limiar de 24 h, mesmo **sem setor de risco mapeado**. Já a classificação
  automática do mapa só sobe onde há setor mapeado. A faixa deve seguir a mesma
  regra do mapa?
- ❓ **P1.13 — Som só para chuva severa.** Desde 21/10/2026 o vencimento de
  alerta e o fim do aviso de 12 h não tocam mais som; só a chuva severa toca. É
  intencional? Algum outro evento deve tocar (ex.: ar Péssima, acumulado de
  alagamento)?
- ❓ **P1.14 — Repetição do som.** O som toca uma vez por município e grau
  enquanto o navegador está aberto; se a chuva continuar severa, não repete. No
  SISPDEC, deve repetir de tempos em tempos enquanto a condição durar?
- ❓ **P1.15 — Vento previsto.** O vento vem do modelo Open-Meteo (não é
  medição) e aparece só no produto Incêndio. O CEMOA aceita essa fonte? Deve
  aparecer em outros produtos (ex.: Chuva intensa)?

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
- **Totais por grau:** Baixo, Moderado, Alto, Severo, com leitura e sem
  leitura.
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

- ❓ **P2.1 — Grau automático pela cota.** Hoje o grau não muda com a cota; só
  com o boletim base ou lançamento manual. Deve ser calculado comparando a cota
  do dia com as cotas de referência? Se sim, como tratar os 39 municípios sem
  referência?
- ❓ **P2.2 — Origem oficial do grau no SISPDEC.** O boletim base do sistema de
  origem está parado em 01/09/2026. No SISPDEC, quem informa o grau oficial
  diário e em que tela? (O SISPDEC já tem cadastro de cotas por calha no
  módulo CEMOA; confirmar se é o mesmo dado.)
- ❓ **P2.3 — Escala sem Extremo.** O boletim usa quatro graus e o painel cinco.
  Confirmar que inundação e estiagem não têm Extremo.
- ❓ **P2.4 — Projeção de +3/+5/+7 dias.** A reta simples sobre 5 pontos é aceita
  pelo CEMOA como apoio ao plantão, ou deve sair da tela?
- ❓ **P2.5 — Prioridade da cota.** Confirmar a ordem: boletim < Power BI < ANA
  < operador.
- ❓ **P2.6 — "Mudanças em 24 h".** A tela tem uma área de mudanças das últimas
  24 h que hoje está sempre vazia. O que ela deve listar?
- ❓ **P2.7 — Cenário padrão.** Qual cenário a tela abre por padrão (estiagem ou
  inundação)? Muda conforme a época do ano?

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
- Emitir o aviso de 4 h também registra o aviso de plantão de 12 h.

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

- ❓ **P3.1 — Numeração do aviso de 4 h.** Hoje o último código fica só no
  navegador de quem emitiu; dois postos podem gerar o mesmo número. No SISPDEC a
  numeração será central. Confirmar: sequência única do estado por ano?
- ❓ **P3.2 — Lista de calhas do aviso.** O aviso usa 17 calhas (Alto/Médio/Baixo)
  e o boletim usa 9. As duas listas estão corretas e devem continuar diferentes?
- ❓ **P3.3 — Fonte da previsão.** O mapa de previsão usa Open-Meteo; a ficha do
  município usa INMET Prevmet. O CEMOA aceita o Open-Meteo como fonte oficial
  do mapa?
- ❓ **P3.4 — Aviso de 4 h x aviso de 12 h.** Emitir o de 4 h renova o de 12 h
  automaticamente. É intencional?
- ❓ **P3.5 — Histórico dos avisos.** Os avisos de 4 h precisam ficar
  consultáveis (lista, reimpressão do PNG)? Hoje não ficam.
- ❓ **P3.6 — Escala de temperatura.** As faixas de cor (26/30/33/36 °C) são
  definição do CEMOA?

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

- ❓ **P4.1 — Pesos do IRG.** O sistema usa 0,6 × maior IRE + 0,2 × média +
  0,2 × IVM escalado. O caderno metodológico antigo diz 0,7 × maior IRE + 0,3 ×
  média. Qual vale?
- ❓ **P4.2 — Posição do agravo.** O sistema soma o agravo **depois** de FE e FA.
  O caderno antigo coloca o agravo **dentro**, multiplicado por FE e FA. Qual
  vale?
- ❓ **P4.3 — FA de Estiagem e Erosão.** A documentação antiga diz FA fixo de
  1,0 para esses eventos. O sistema usa o grau de estiagem do boletim (Baixo =
  0,30). Qual vale?
- ❓ **P4.4 — FA de Inundação.** O sistema usa sempre "Sem alerta" (0,30), sem
  olhar o grau de inundação do boletim. Isso deve mudar na época de cheia?
- ❓ **P4.5 — Faixa de focos de calor.** A faixa 1–4 / 5–14 / 15–39 / ≥40 foi
  criada na operação e não tem validação institucional. Desde 21/10/2026 ela só
  vale para município sem sensor de ar e sem classificação manual. Manter,
  ajustar ou retirar do cálculo?
- ❓ **P4.6 — Cores dos níveis.** A documentação antiga lista cores diferentes
  das usadas no sistema. Qual paleta é a oficial (ou fica com a designer do
  DDS)?
- ❓ **P4.7 — Cor do rebaixado.** Município rebaixado de P1 para P2 aparece como
  "Alto" mas com a cor do Crítico. É intencional?
- ❓ **P4.8 — Empate no teto de 60.** Municípios acima do teto empatam no
  ranking. Precisa de critério de desempate?
- ❓ **P4.9 — Japurá e Maraã.** O caderno antigo diz que "não foram
  reclassificados". O que precisa ser feito com eles?
- ❓ **P4.10 — Recomendações por município.** Existe uma base de recomendações
  do CEMOA no sistema de origem que não é usada em nenhuma tela. Deve aparecer
  na ficha municipal?
- ❓ **P4.11 — Atualização dos parâmetros.** Quem atualiza IVM, agravos,
  decretos e lista PIMF, e com que frequência? No SISPDEC isso pode virar
  cadastro.
- ❓ **P4.12 — Pontuação logística.** Os pontos da classe de acesso são definição
  do CEMOA? Devem ser revistos?

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
| Open-Meteo | `api.open-meteo.com/v1/forecast` | Previsão por município; 🔄 vento atual a 10 m | Meteorologia; Painel (Incêndio) |
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
| 🔄 Vento no Painel (Incêndio) | ao abrir o produto | 30 min |
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
