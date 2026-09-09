# Coord. AttribuTable

> Acrescenta à tabela de atributos de uma camada de pontos as coordenadas
> no SRC que você escolher — graus decimais, graus-minutos-segundos ou
> coordenadas planas (UTM) — reprojetando na hora; preenche em lote as
> coordenadas que formulários de outorga e o SIAGAS pedem.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | dados-campo |
| **Autor(es)** | Romário Moraes Carvalho Neto (PlugGIS; ex-UFSM) |
| **Licença** | não declarada (sem LICENSE no repositório; loja exige GPL-compatível, mas não está explícito); só o cabeçalho padrão do Plugin Builder nos `.py` cita "GPL v2 or later" |
| **Página oficial** | https://plugins.qgis.org/plugins/coord_attributable/ |
| **Código-fonte** | https://github.com/romariocarvalhoneto/Coord.-AttribuTable |
| **Documentação** | README de três linhas no repositório; sem manual |
| **Versão verificada** | 0.1 (2020-07-22) — única versão publicada; 7.655 downloads |
| **QGIS mínimo** | 3.0 (máximo 3.99; experimental) |
| **Status** | abandonado (sem commit desde 2020-07; funciona no QGIS 3.x; máx. 3.99, não aparece no QGIS 4); o autor segue ativo na PlugGIS (pluggis.com.br), então pode voltar a atualizar |
| **Verificado em** | 2026-09-08 |

## O que faz

- Copia a camada de pontos para um Shapefile novo (`selectAll` +
  `native:saveselectedfeatures`) — a original não é alterada — e acrescenta
  um par de colunas X/Y por SRC escolhido no seletor padrão do QGIS; pode
  adicionar vários, um de cada vez ("This CRS was already chosen" se repetir).
- Para SRC geográfico pergunta `Do you want D° M' S"?`: **Yes** grava texto
  `-15° 46' 48.123"` (segundos com 3 casas, sinal negativo no grau, sem
  letra N/S/E/W); **No** grava graus decimais em Double. SRC projetado
  grava X/Y em metros (Double) sem perguntar.
- Reprojeta com `QgsCoordinateTransform` quando o SRC da camada difere do
  escolhido, usando o contexto de transformação do projeto.
- Colunas nomeadas pelo código EPSG: `Y° 4674`/`X° 4674` (GMS),
  `Y 4674`/`X 4674` (decimal), `Y 31983`/`X 31983` (UTM). Carrega o
  Shapefile no projeto ao terminar.

## Para que serve em geologia

- **Outorga e cadastro**: os formulários da ADASA (DF) e da SEMAD (GO) e o
  cadastro no SIAGAS pedem coordenadas em GMS e/ou UTM SIRGAS 2000. Com
  dezenas de poços numa camada, gera as colunas numa passada — num diálogo
  só, sem escrever expressão — e copia para a planilha do requerimento ou
  para o cabeçalho do relatório de perfuração.
- **Conferência de fuso**: a mesma tabela com lat/long (EPSG:4674), UTM 22S
  (31982) e 23S (31983) — o DF está no 23S, o oeste de Goiás no 22S —
  evita transcrever coordenada no fuso errado.

## Instalação

- Complementos → Configurações → marcar "Mostrar complementos
  experimentais"; buscar "Coord. AttribuTable".
- Sem dependências externas (só PyQGIS e Processing).
- QGIS 4: não é ofertado (máximo 3.99); instalação forçada não testada.

## Como usar (roteiro mínimo)

1. Confira o SRC da camada de poços (Propriedades → Fonte): coordenada
   errada aqui sai errada em todas as colunas.
2. Complementos → **Coord. AttribuTable**. Escolha a camada de pontos e o
   arquivo de saída (`.shp`).
3. No seletor, escolha EPSG:4674 (SIRGAS 2000 geográfico) e adicione;
   responda **Yes** para GMS (ou **No** para decimal).
4. Repita com EPSG:31983 (SIRGAS 2000 / UTM 23S) para as planas e clique
   OK: o plugin copia todas as feições, cria as colunas e carrega o Shapefile.
5. Confira um poço de coordenada conhecida (marco, GPS) antes de copiar
   para o formulário; exporte para CSV/GPKG se for anexar.

## Entradas e saídas

- **Entrada:** camada de pontos simples em qualquer SRC (usa `asPoint()`:
  MultiPoint não funciona).
- **Saída:** Shapefile novo com as colunas de coordenada — GMS como texto,
  decimal e UTM como Double; carregado no projeto.

## Limitações e armadilhas

- **Só Shapefile.** Os nomes `Y° 4674` têm símbolo de grau e espaço; o
  driver DBF pode alterá-los ou truncá-los (não verificado em teste).
  Exporte para GPKG/CSV logo em seguida.
- **GMS sem hemisfério em letra**: usa sinal negativo no grau. Formulário
  que pede "S"/"W" exige ajuste manual ou a expressão `to_dms(…, 'suffix')`.
- **Datum**: SAD69/Córrego Alegre → SIRGAS 2000 depende da transformação
  configurada no QGIS (Configurações → Transformações de datum); erro de
  dezenas de metros se estiver errada. O plugin não avisa.
- Sem tratamento de erro (caminho inválido ou provedor sem `AddAttributes`
  falha em silêncio ou com exceção Python) e sem algoritmo de Processing:
  não entra em modelo nem em lote.
- Sem changelog nem issues (3 commits, 0 estrelas): sem bug relatado, mas sem sinal de uso.
- Licença não declarada: para redistribuir ou modificar (ex.: portar para
  QGIS 4) só o cabeçalho GPL-2+ dos `.py` serve de base — confirmar com o autor.

## Alternativas

- **Add X/Y fields to layer** (`native:addxyfields`): nativo, aceita SRC
  diferente da camada e prefixo; campos `x`/`y` decimais; roda em modelo e
  lote. Cobre o caso UTM e o decimal.
- **Calculadora de campo**: `$x`/`$y` no SRC da camada;
  `x(transform($geometry, @layer_crs, 'EPSG:31983'))` para UTM;
  `to_dms(y(transform($geometry, @layer_crs, 'EPSG:4674')), 'y', 2, 'suffix')`
  dá `15°46′48.12″ S` — com hemisfério em letra.
- **Exportar camada → CSV** com opção `GEOMETRY=AS_XY` e SRC de saída:
  coordenadas sem plugin nem campo novo.

## Ver também

- `fluxos/README.md` — cadastro de poços e requerimento de outorga — a criar.
- `dados/README.md` — SRC oficiais (SIRGAS 2000; fusos 22S/23S no DF e GO).
- `ferramentas/qgis-plugins/dados-campo/point-sampling-tool.md` — enriquece
  o mesmo cadastro com cota, geologia e aquífero.
