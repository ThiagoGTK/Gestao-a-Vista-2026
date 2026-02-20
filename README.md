# 📊 Gestão à Vista - 2026

Documentação completa do modelo semântico Power BI para acompanhamento de vendas, metas e desempenho comercial.

---

## 📋 Visão Geral

| Propriedade | Valor |
|---|---|
| **Nome do Arquivo** | Gestão à Vista - 2026 |
| **Modo Padrão** | Import |
| **Cultura** | pt-BR |
| **Última Modificação da Estrutura** | 18/02/2026 |
| **Inteligência de Tempo** | Habilitada |
| **Total de Tabelas** | 33 (excl. tabelas de data automáticas) |
| **Total de Medidas** | 40 |
| **Total de Relacionamentos** | 37 |

---

## 🗂️ Estrutura de Tabelas

### Tabelas de Fatos

#### `Vendas`
Tabela central de vendas. Relaciona-se com `Clientes`, `Canais de Venda`, `Agentes de Vendas` e `Calendario`.

| Colunas principais | Descrição |
|---|---|
| `Chave Venda` | Chave primária |
| `Data` | Data da venda (relacionada ao Calendário) |
| `Código Canal de Venda` | FK para Canais de Venda |
| `Chave Agente de Tele-venda` | Identifica vendas indiretas (TeleVendas) |

---

#### `Movimentos`
Tabela de movimentos/itens de venda com 44 colunas. Granularidade em nível de item.

| Colunas principais | Descrição |
|---|---|
| `CHAVE_VENDA` | FK para Vendas |
| `CHAVE_PRODUTO` | FK para Produtos |
| `MOV_AGVCOD` | FK para Agentes de Vendas |
| `MOV_EMPCOD` | FK para Empresas |
| `MOV_TPMCOD` | FK para Tipos de Movimentos |
| `MOV_DATAHORA` | Data/hora do movimento |
| `Valor de Venda` | Valor do item vendido |

---

#### `Valor a Faturar`
Pedidos/vendas ainda não faturados (a faturar no mês). Relaciona-se com `Agentes de Vendas` e `Calendario`.

| Colunas principais | Descrição |
|---|---|
| `Data Emissão` | Data do pedido (FK para Calendário) |
| `Código Agente de Venda` | FK para Agentes de Vendas |
| `Valor Total` | Valor a ser faturado |

---

### Tabelas Dimensão

#### `Clientes` (18 colunas)
Cadastro de clientes com informações geográficas e de segmentação.

| Colunas principais | Descrição |
|---|---|
| `Chave Região` | FK para Regiões |
| `Chave Segmento` | FK para Segmento dos Clientes |
| `CLI_DTALT` | Data de alteração (FK ativa para Calendário) |
| `CLI_DTCAD` | Data de cadastro (FK inativa para Calendário) |
| `DTHR` | FK para tabela de datas local |

---

#### `Produtos` (15 colunas)
Cadastro de produtos com hierarquia de classificação.

| Colunas principais | Descrição |
|---|---|
| `Chave Produto` | Chave primária |
| `Chave Categoria do Produto` | FK para Categoria Produtos |
| `Chave Marca` | FK para Marca de Produtos |
| `Chave Linha` | FK para Linha de Produtos |
| `Chave Divisão` | FK para Divisões de Produtos |

---

#### `Calendario` (24 colunas)
Tabela calendário principal do modelo.

| Colunas relevantes | Descrição |
|---|---|
| `Data` | Chave primária (PK) |
| `MesNumero` | Número do mês (1–12) |
| `MesNome` | Nome do mês em português |
| `Ano` | Ano |
| `Dia` | Dia do mês |
| `InicioDoMes` | Primeiro dia do mês |
| `FimDoMes` | Último dia do mês |
| `EhFimDeSemana` | Boolean — fim de semana |
| `EhDiaUtil` | Boolean — dia útil |

---

#### `Agentes de Vendas` (5 colunas)
Cadastro dos agentes/vendedores.

| Colunas principais | Descrição |
|---|---|
| `Chave Agente de Venda` | Chave primária |
| `Código Agente de Venda` | Código numérico do agente |
| `Canal de Venda` | Canal ao qual o agente pertence |

---

#### `Supervisores` (4 colunas)
Cadastro de supervisores comerciais.

| Colunas principais | Descrição |
|---|---|
| `CHAVE` | Chave primária |
| `AGEVEN_COD` | Código do agente/supervisor |

---

#### `Canais de Venda` (2 colunas)
Dimensão de canais de venda.

| Colunas | Descrição |
|---|---|
| `Canal de Venda - Código` | Chave primária |
| `Canal de Venda - Nome` | Descrição do canal |

---

#### `Regiões` (3 colunas)
Dimensão geográfica de regiões.

---

#### `Segmento dos Clientes` (3 colunas)
Classificação dos clientes por segmento.

---

#### `Cidades` (12 colunas)
Cadastro geográfico de cidades.

---

#### `Empresas` (2 colunas)
Cadastro de empresas/filiais.

---

#### `Tipos de Movimentos` (2 colunas)
Classificação dos tipos de movimentos.

---

#### `Condição de Pagamento` (2 colunas)
Condições de pagamento cadastradas.

---

#### `Categoria Produtos` / `Marca de Produtos` / `Linha de Produtos` / `Divisões de Produtos`
Hierarquia de classificação de produtos com chaves de relacionamento para a tabela `Produtos`.

---

#### `Dim CanalEixo` (3 colunas)
Tabela auxiliar para segmentação dinâmica de canais nos visuais.

| Colunas | Descrição |
|---|---|
| `Grupo` | Nome do canal agrupado |
| `CodCanal` | Código específico do sub-canal (pode ser BLANK para canais simples) |
| `Eixo` | Rótulo de exibição |

---

### Tabelas de Relacionamento (Bridge)

| Tabela | Propósito |
|---|---|
| `Supervisores x Agentes` | N:N entre supervisores e agentes |
| `Agentes de Venda X Clientes` | Relacionamento agente ↔ cliente |
| `Agente de Venda X UV x Negócio` | Mapeamento agente ↔ unidade de venda ↔ negócio |

---

### Tabelas de Metas

Todas as tabelas de metas possuem colunas de meta por mês (Janeiro a Dezembro) e código do agente para filtro.

| Tabela | Canal / Segmento |
|---|---|
| `Metas Diárias Mensais` | Metas globais do negócio por mês |
| `Distribuição Metas 2026 Canais` | Metas distribuídas por canal e negócio |
| `Metas Vendedores E-commerce` | Time de e-commerce |
| `Metas Vendedores Outside Sales BC` | Outside Sales - Balneário Camboriú |
| `Metas Vendedores Philozon BC` | Inside Sales Philozon - BC |
| `Metas Vendedores Philozon SP` | Inside Sales Philozon - SP |
| `Metas Vendedores Prescritor` | Canal Prescritor |
| `Metas Vendedores V D Ozoncare` | Vendas Diretas Ozoncare |

---

## 📐 Relacionamentos

### Diagrama Resumido

```
Calendario ◄──── Vendas ──────────► Canais de Venda
                   │
                   ▼
              Movimentos ──────────► Produtos ──► Categoria Produtos
                   │                          ──► Marca de Produtos
                   │                          ──► Linha de Produtos
                   │                          ──► Divisões de Produtos
                   │
                   ├──────────────► Agentes de Vendas ◄── Supervisores x Agentes ──► Supervisores
                   │                      ▲
                   │                      │
                   ├──────────────► Agente de Venda X UV x Negócio ──► Metas (múltiplas)
                   │
                   └──────────────► Tipos de Movimentos
                                 └► Empresas

Valor a Faturar ──► Calendario
                └──► Agentes de Vendas

Clientes ──────────► Regiões
         ──────────► Segmento dos Clientes
         ──────────► Calendario (ativa: CLI_DTALT | inativa: CLI_DTCAD)
```

### Tabela Completa de Relacionamentos

| De (Tabela) | De (Coluna) | Para (Tabela) | Para (Coluna) | Cardinalidade | Filtro | Ativo |
|---|---|---|---|---|---|---|
| Clientes | Chave Região | Regiões | Chave Região | N:1 | Único | ✅ |
| Clientes | Chave Segmento | Segmento dos Clientes | Chave Segmento | N:1 | Único | ✅ |
| Clientes | CLI_DTALT | Calendario | Data | N:1 | Único | ✅ |
| Clientes | CLI_DTCAD | Calendario | Data | N:1 | Único | ❌ |
| Vendas | Data | Calendario | Data | N:1 | Único | ✅ |
| Vendas | Código Canal de Venda | Canais de Venda | Canal de Venda - Código | N:1 | Único | ✅ |
| Movimentos | CHAVE_VENDA | Vendas | Chave Venda | N:1 | Único | ✅ |
| Movimentos | CHAVE_PRODUTO | Produtos | Chave Produto | N:1 | Único | ✅ |
| Movimentos | MOV_AGVCOD | Agentes de Vendas | Chave Agente de Venda | N:1 | Único | ✅ |
| Movimentos | MOV_EMPCOD | Empresas | Código | N:1 | Único | ✅ |
| Movimentos | MOV_TPMCOD | Tipos de Movimentos | Código Tipo de Movimento | N:1 | Único | ✅ |
| Produtos | Chave Categoria do Produto | Categoria Produtos | Chave Categoria Produto | N:1 | Único | ✅ |
| Produtos | Chave Marca | Marca de Produtos | Código Marca | N:1 | Único | ✅ |
| Produtos | Chave Linha | Linha de Produtos | Chave Linha | N:1 | Único | ✅ |
| Produtos | Chave Divisão | Divisões de Produtos | Chave Divisão | N:1 | Único | ✅ |
| Valor a Faturar | Data Emissão | Calendario | Data | N:1 | Único | ✅ |
| Valor a Faturar | Código Agente de Venda | Agentes de Vendas | Código Agente de Venda | N:1 | Único | ✅ |
| Supervisores x Agentes | Chave Agente de Venda | Agentes de Vendas | Chave Agente de Venda | N:1 | Único | ✅ |
| Supervisores x Agentes | Chave Supervisor | Supervisores | CHAVE | N:1 | Único | ✅ |
| Agentes de Venda X Clientes | Chave Agente de Venda | Agentes de Vendas | Chave Agente de Venda | N:1 | Único | ✅ |
| Agente de Venda X UV x Negócio | Agente de Venda - Código | Agentes de Vendas | Código Agente de Venda | N:N | Ambos | ✅ |
| Agente de Venda X UV x Negócio | Agente de Venda - Código | Metas Vendedores E-commerce | Código Agente | 1:1 | Ambos | ✅ |
| Agente de Venda X UV x Negócio | Agente de Venda - Código | Metas Vendedores Outside Sales BC | Código Agente | 1:1 | Ambos | ✅ |
| Agente de Venda X UV x Negócio | Agente de Venda - Código | Metas Vendedores Philozon BC | Código Agente | 1:1 | Ambos | ✅ |
| Agente de Venda X UV x Negócio | Agente de Venda - Código | Metas Vendedores Philozon SP | Código Agente | 1:1 | Ambos | ✅ |
| Agente de Venda X UV x Negócio | Agente de Venda - Código | Metas Vendedores Prescritor | Código Agente | 1:1 | Ambos | ✅ |
| Agente de Venda X UV x Negócio | Agente de Venda - Código | Metas Vendedores V D Ozoncare | Código Agente | 1:1 | Ambos | ✅ |
| Distribuição Metas 2026 Canais | Código Agente Líder | Supervisores | AGEVEN_COD | N:1 | Único | ✅ |

---

## 🧮 Medidas DAX

Todas as medidas estão centralizadas na tabela `AA Medidas`.

---

### 📁 Medidas Tabela Vendas

#### `Valor Total de Venda`
```dax
SUM(Movimentos[Valor de Venda])
```
> Soma bruta dos valores de todos os movimentos.

---

#### `Vlr. Vendas Diretas`
```dax
SUMX(
    FILTER(Vendas, ISBLANK(Vendas[Chave Agente de Tele-venda]) = TRUE),
    'AA Medidas'[Valor Total de Venda]
)
```
> Vendas onde não há agente de televenda (venda direta pelo vendedor externo).

---

#### `Vlr. Vendas Indiretas`
```dax
SUMX(
    FILTER(Vendas, ISBLANK(Vendas[Chave Agente de Tele-venda]) = FALSE),
    'AA Medidas'[Valor Total de Venda]
)
```
> Vendas originadas pelo canal de TeleVendas.

---

#### `Valor Total`
```dax
COALESCE([Vlr. Vendas Diretas], 0) + COALESCE([Vlr. Vendas Indiretas], 0)
```
> Valor total de vendas (diretas + indiretas). Formato: `R$ #,0.00`

---

#### `Valor Total + Faturar`
```dax
COALESCE([Valor Total], 0) + COALESCE(SUM('Valor a Faturar'[Valor Total]), 0)
```
> Valor consolidado: vendas já lançadas + pedidos a faturar.

---

#### `Vendas Dia Mes Atual`
```dax
VAR Resultado =
    CALCULATE(
        [Valor Total + Faturar],
        'Calendario'[MesNumero] = MONTH(TODAY()),
        'Calendario'[Dia] = DAY(TODAY())
    )
RETURN COALESCE(Resultado, 0)
```
> Vendas do dia atual no mês corrente.

---

#### `Vendas Mes Atual`
```dax
CALCULATE(
    [Valor Total],
    'Calendario'[MesNumero] = MONTH(TODAY())
)
```
> Total de vendas no mês corrente.

---

#### `Vendas Mês Passado`
```dax
VAR DataRef = EDATE(TODAY(), -1)
RETURN
CALCULATE(
    [Valor Total],
    'Calendario'[MesNumero] = MONTH(DataRef),
    'Calendario'[Ano]       = YEAR(DataRef)
)
```
> Total de vendas no mês anterior ao atual.

---

#### `Valor Total Marca Ozoncare`
```dax
CALCULATE([Valor Total], 'Marca de Produtos'[Marca produto] = "Ozoncare")
```

#### `Valor Total Marca Philozon`
```dax
CALCULATE([Valor Total], 'Marca de Produtos'[Marca produto] = "Philozon")
```

---

#### `Meta Mensal Canal`
```dax
VAR MesSel = MONTH(TODAY())
VAR CanalEixo = SELECTEDVALUE('Agentes de Vendas'[Canal de Venda])
RETURN
CALCULATE(
    SWITCH(MesSel, 1, SUM(...[Janeiro]), 2, SUM(...[Fevereiro]), ... 12, SUM(...[Dezembro])),
    ALL('Distribuição Metas 2026 Canais'),
    KEEPFILTERS('Distribuição Metas 2026 Canais'[Canal] = CanalEixo)
)
```
> Meta mensal filtrada pelo canal selecionado no contexto.

---

#### `Cor Faturado Atual`
```dax
VAR Valor = [Valor Total + Faturar]
RETURN
SWITCH(TRUE(),
    Valor < 0,      "#c79b56",   -- Bronze
    Valor <= 50000, "#c0c0c0",   -- Silver
                    "#d4af37"    -- Gold
)
```
> Retorna cor hexadecimal conforme faixa de valor. Usado em formatação condicional.

---

### 📁 Medidas Tabela Valor a Faturar

#### `Valor Faturar Mês Atual`
```dax
CALCULATE(
    COALESCE(SUM('Valor a Faturar'[Valor Total]), 0),
    MONTH('Vendas'[Data]) = MONTH(TODAY())
)
```
> Pedidos a faturar no mês atual.

---

### 📁 Medidas Tabela Movimentos

#### `Valor Total de Venda`
```dax
SUM(Movimentos[Valor de Venda])
```

---

### 📁 Medidas Tabela Metas Diárias Mensais

#### `Meta Mensal (Mês Atual)`
```dax
VAR MesAtual = MONTH(TODAY())
RETURN
CALCULATE(
    MAX('Metas Diárias Mensais'[Meta Mensal]),
    FILTER(ALL('Metas Diárias Mensais'), VALUE('Metas Diárias Mensais'[Mês Num]) = MesAtual)
)
```

#### `Meta Diária (Mês Atual)`
```dax
VAR MesAtual = MONTH(TODAY())
RETURN
CALCULATE(
    MAX('Metas Diárias Mensais'[Meta Diária]),
    FILTER(ALL('Metas Diárias Mensais'), 'Metas Diárias Mensais'[Mês Num] = MesAtual)
)
```

#### `Meta Anual (Total)`
```dax
SUM('Metas Diárias Mensais'[Meta Mensal])
```

#### `Meta Anual (Por Dia - 365)`
```dax
DIVIDE([Meta Anual (Total)], 365)
```

#### `Meta Anual (Deveria estar hoje - 365)`
```dax
VAR DiaDoAno = TODAY() - DATE(YEAR(TODAY()), 1, 1) + 1
RETURN [Meta Anual (Por Dia - 365)] * DiaDoAno
```
> Meta acumulada linear até o dia de hoje no ano.

#### `Dias Úteis Mês Atual (até hoje)`
```dax
VAR InicioMes = DATE(YEAR(TODAY()), MONTH(TODAY()), 1)
VAR Hoje = TODAY()
RETURN
CALCULATE(
    COUNTROWS('Calendario'),
    FILTER(ALL('Calendario'),
        'Calendario'[Date] >= InicioMes &&
        'Calendario'[Date] <= Hoje &&
        'Calendario'[EhFimDeSemana] = FALSE()
    )
)
```

#### `Meta Mensal (Deveríamos estar hoje)`
```dax
[Meta Diária (Mês Atual)] * [Dias Úteis Mês Atual (até hoje)]
```
> Meta proporcional aos dias úteis decorridos no mês.

#### `Meta Esperada Até Hoje (Canais)`
```dax
VAR Hoje = TODAY()
VAR DiasNoMes = DAY(EOMONTH(Hoje, 0))
VAR DiaAtual  = DAY(Hoje)
RETURN [Meta Mensal Canal] * DIVIDE(DiaAtual, DiasNoMes)
```
> Meta proporcional ao calendário corrido (não dias úteis).

---

### 📁 Medidas Globais

#### `Valor Mês Anterior (dinâmico)`
```dax
VAR RefDate = COALESCE(MAX('Calendario'[Data]), TODAY())
VAR RefMonthStart = DATE(YEAR(RefDate), MONTH(RefDate), 1)
VAR PrevStart     = EDATE(RefMonthStart, -1)
VAR PrevEnd       = EOMONTH(RefMonthStart, -1)
RETURN
CALCULATE(
    'AA Medidas'[Valor Total + Faturar],
    DATESBETWEEN('Calendario'[Data], PrevStart, PrevEnd)
)
```
> Valor do mês anterior ao contexto de data selecionado (dinâmico).

#### `Dias Úteis no Mês (Sel)`
> Conta dias úteis do mês selecionado no contexto do calendário.

#### `Dias Úteis Decorridos (Até Hoje)`
> Conta dias úteis do mês selecionado até hoje. Para meses passados, retorna o total do mês; para futuros, retorna o total do início do mês.

#### `% Mês Útil Decorrido (Até Hoje)`
```dax
DIVIDE([Dias Úteis Decorridos (Até Hoje)], [Dias Úteis no Mês (Sel)], 0)
```
> Percentual do mês útil decorrido. Base para cálculo proporcional de metas.

#### `Meta Geral (MesNome Selecionado)`
> Soma total das metas de todos os canais para o mês selecionado via `Calendario[MesNome]`.

#### `Meta Geral Philozon (MesNome Selecionado)`
> Soma das metas Philozon para o mês selecionado.

---

### 📁 Medidas por Canal (Dim CanalEixo)

Estas medidas usam `TREATAS` para aplicar filtros virtuais sem relacionamentos físicos, permitindo visuais dinâmicos por canal.

#### `Valor Total (por CanalEixo)`
```dax
VAR GrupoSel = SELECTEDVALUE('Dim CanalEixo'[Grupo])
VAR CodSel   = SELECTEDVALUE('Dim CanalEixo'[CodCanal])
RETURN
IF(
    ISBLANK(CodSel),
    CALCULATE([Valor Total + Faturar], TREATAS({GrupoSel}, 'Agentes de Vendas'[Canal de Venda])),
    CALCULATE([Valor Total + Faturar],
        TREATAS({GrupoSel}, 'Agentes de Vendas'[Canal de Venda]),
        TREATAS({CodSel}, 'Vendas'[Código Canal de Venda])
    )
)
```

#### `Vendas Mês Atual (CanalEixo)` / `Vendas Mês Passado (CanalEixo)`
> Variantes de `Valor Total (por CanalEixo)` filtradas pelo mês atual/anterior.

#### `Meta Mensal (CanalEixo)`
> Lógica complexa que mapeia sub-canais (códigos 100, 133, 145, 148) para as linhas corretas na tabela de distribuição de metas. Suporta Outside Sales (Canal Representante, Distribuição, Exportação) e Revenda Digital.

#### `Meta Esperada Até Hoje (CanalEixo)`
```dax
VAR Hoje = TODAY()
VAR DiasNoMes = DAY(EOMONTH(Hoje, 0))
VAR DiaAtual  = DAY(Hoje)
RETURN [Meta Mensal (CanalEixo)] * DIVIDE(DiaAtual, DiasNoMes)
```

---

### 📁 Medidas de Metas por Vendedor (por Canal)

Padrão comum: usam `SELECTEDVALUE(Calendario[MesNome])` e `SWITCH` para selecionar a coluna de meta correta da tabela de metas do canal.

| Medida | Tabela de Origem | Canal |
|---|---|---|
| `Meta VD Ozoncare(Mês Selecionado)` | Metas Vendedores V D Ozoncare | VD Ozoncare |
| `Meta Philozon BC (Mês Selecionado)` | Metas Vendedores Philozon BC | Inside Sales BC |
| `Meta Philozon SP (Mês Selecionado)` | Metas Vendedores Philozon SP | Inside Sales SP |
| `Meta Outside Sales (Mês Selecionado)` | Metas Vendedores Outside Sales BC | Outside Sales |

#### Medidas Proporcionais (Até Hoje)
Calculam a meta proporcional usando `% Mês Útil Decorrido (Até Hoje)`:

```dax
VAR MesNomeCtx = CALCULATE(SELECTEDVALUE('Calendario'[MesNome]), KEEPFILTERS('Calendario'[Data]))
VAR MetaMes    = CALCULATE([Meta <Canal>(Mês Selecionado)], TREATAS({MesNomeCtx}, 'Calendario'[MesNome]))
RETURN MetaMes * [% Mês Útil Decorrido (Até Hoje)]
```

| Medida | Canal |
|---|---|
| `Meta Proporcional (Até Hoje) VD Ozoncare` | VD Ozoncare |
| `Meta Proporcional (Até Hoje) Inside Sales BC` | Inside Sales Philozon BC |
| `Meta Proporcional (Até Hoje) Inside Sales SP` | Inside Sales Philozon SP |
| `Meta Proporcional (Até Hoje) Outside Sales BC` | Outside Sales BC |

---

## 🔗 Dependências entre Medidas

```
Valor Total de Venda
  └── Vlr. Vendas Diretas
  └── Vlr. Vendas Indiretas
        └── Valor Total
              └── Valor Total + Faturar ──────────────────────────┐
                    ├── Vendas Dia Mes Atual                       │
                    ├── Vendas Mes Atual                           │
                    ├── Vendas Mês Passado                         │
                    ├── Valor Mês Anterior (dinâmico)              │
                    └── Valor Total (por CanalEixo) ──────────────┘
                          ├── Vendas Mês Atual (CanalEixo)
                          └── Vendas Mês Passado (CanalEixo)

Meta Anual (Total)
  └── Meta Anual (Por Dia - 365)
        └── Meta Anual (Deveria estar hoje - 365)

Dias Úteis no Mês (Sel)
Dias Úteis Decorridos (Até Hoje)
  └── % Mês Útil Decorrido (Até Hoje)
        └── Meta Proporcional (Até Hoje) VD Ozoncare
        └── Meta Proporcional (Até Hoje) Inside Sales BC
        └── Meta Proporcional (Até Hoje) Inside Sales SP
        └── Meta Proporcional (Até Hoje) Outside Sales BC

Meta Diária (Mês Atual) + Dias Úteis Mês Atual (até hoje)
  └── Meta Mensal (Deveríamos estar hoje)

Meta Mensal Canal
  └── Meta Esperada Até Hoje (Canais)

Meta Mensal (CanalEixo)
  └── Meta Esperada Até Hoje (CanalEixo)
```

---

## 🏗️ Convenções e Padrões do Modelo

- **Tabela de medidas centralizada:** Todas as 40 medidas estão na tabela `AA Medidas` (prefixo "AA" garante que apareça no topo da lista de campos).
- **Pastas de exibição (Display Folders):** Medidas organizadas por origem de dados (ex.: `Medidas Tabela Vendas`, `Medidas Globais`).
- **Nomenclatura de colunas chave:** Padrão `Chave <Entidade>` para PKs/FKs.
- **Tabela `Dim CanalEixo`:** Dimensão virtual para visuais de canal sem relacionamento físico direto — requer uso de `TREATAS`.
- **Metas por mês:** Estrutura desnormalizada (uma coluna por mês) nas tabelas de metas de vendedores; a seleção do mês é feita via `SWITCH` + `SELECTEDVALUE(Calendario[MesNome])`.
- **`COALESCE` em medidas de valor:** Evita propagação de BLANK nas somas compostas.
- **Dias úteis:** Calculados via coluna `EhDiaUtil` do Calendário, não apenas por exclusão de fins de semana (feriados podem estar mapeados).
