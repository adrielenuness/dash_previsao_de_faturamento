# Portal de Previsão de Faturamento — Nova Corrente

Painel financeiro em **um único arquivo** (`index.html`): compara **Faturado × Previsão das áreas** e **Faturado × Meta do mês**, por cliente, escopo e área (Corretiva, Preventiva, Implantação). Não precisa de servidor nem de instalação: basta abrir no **Chrome ou Edge**.

> **Criação: Adriele Nunes**

## ⚠️ Atenção antes de publicar

- Mantenha este repositório **privado**. O arquivo contém valores de metas, faturado histórico e previsões da empresa.
- **Não suba planilhas** (`.xlsx`, `.csv`). O `.gitignore` já bloqueia esses formatos.
- Não ative o GitHub Pages em repositório público: o portal ficaria acessível a qualquer pessoa.

## Como usar

1. Abra `index.html` no Chrome ou Edge (de preferência a partir de uma pasta fixa, de onde sempre será aberto).
2. Em **Configurações > Faturado**, conecte as planilhas (uma vez):
   - **Escolher planilha…** → `Controle de liberações 2026.xlsx` (pasta de rede, Faturamento > Liberação);
   - **Escolher planilha de previsto…** → `ESBOCO DASH PREVISTO X REALIZADO.xlsx` (mesma pasta).
3. O portal atualiza **a cada 4 horas** (enquanto estiver aberto) e pelo botão **↻ Atualizar faturado**.
4. Se o navegador pedir autorização de novo ao reabrir, clique em "Atualizar faturado" (um clique por planilha).

## De onde vêm os dados

| Informação no portal | Fonte | Regra |
|---|---|---|
| **Faturado** do mês vigente e posteriores | `Controle de liberações` (aba `2026`) | Soma da coluna **VALOR** das linhas em que a coluna **S (MOVEC)** é o número do mês, por Cliente + ALINHAMENTO MOVEC |
| **Faturado** dos meses anteriores ao vigente | `ESBOCO DASH PREVISTO X REALIZADO` (aba `TABELA GERAL`) | Coluna **Executado** |
| **Meta do mês** (2026) | `ESBOCO DASH PREVISTO X REALIZADO` (aba `TABELA GERAL`) | Coluna **Previsto** |
| **Previsão das áreas** fev–set/2026 | `TABELA DE PREVISÕES` | Embutida no arquivo (`PREV_ANO`); outubro em diante é digitada pelas áreas |

Os nomes das planilhas são convertidos para os clientes/escopos do portal (por exemplo, COELBA, COSERN, CEB, CELPE, ELEKTRO e "SP" da NEOENERGIA somam em **TORRES**; qualquer SHELLTER vai para **SHELLTER**; "IHS" nas abas antigas é o **VERTIS**). Valores sem linha correspondente no portal aparecem em um aviso amarelo ao lado do botão de atualização.

## Código de edição

O portal abre **bloqueado**: sem o código só é possível consultar. O código libera digitar valores, importar planilhas, restaurar dados e fechar competências, e o portal volta a bloquear sozinho após 30 minutos sem uso.

- O arquivo guarda apenas o **hash** (SHA-256) do código, na constante `EDIT_CODE_HASH`.
- Para trocar: **Configurações > Alterar código de edição** e substitua a constante pela linha mostrada.
- Esta proteção evita edições acidentais, mas **não é segurança forte** (o portal roda no navegador). Deixe a cópia compartilhada numa pasta onde os demais usuários tenham só leitura.

## Pontos de manutenção (início do `<script>`)

| Constante | Para que serve |
|---|---|
| `SYNC_INTERVALO_MS` | Intervalo da atualização automática (hoje 4 horas) |
| `SYNC_ALIAS` / `escopoPrevisto()` | Conversão de nomes de cliente/escopo das planilhas para o portal |
| `SYNC_IGNORAR_CANCELADAS` | `true` para não somar linhas com "Nota cancelada" |
| `META_PLANILHA_MESES` | Meses cuja meta vem da planilha de previsto |
| `FAT_HISTORICO_DA_PLANILHA` | `false` para ler os meses anteriores das liberações em vez do Executado |
| `META_ANO`, `FAT_ANO`, `PREV_ANO` | Valores embutidos (metas, histórico de faturado e previsões antigas), usados quando a planilha não está conectada |
| `EDIT_CODE_HASH`, `EDIT_IDLE_MS` | Código de edição e tempo de bloqueio automático |

## Limitações conhecidas

- O que for digitado na tela fica salvo apenas no navegador de quem digitou.
- A leitura direta de planilhas em pasta de rede funciona em Chrome/Edge (API de acesso a arquivos).
- Meses de 2026 apenas; a aba das liberações precisa se chamar `2026`.
