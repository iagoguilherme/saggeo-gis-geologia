# Point Sampling Tool

> Extrai, num só passo, valores de vários rasters e atributos de vários
> polígonos para uma camada de pontos — cota do MDE, unidade geológica,
> aquífero, anomalia geofísica — gerando uma nova camada de pontos com tudo
> junto.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | dados-campo |
| **Autor(es)** | Borys Jurgiel |
| **Licença** | GPL-3.0 (arquivo LICENSE, 2025-08-25); o README ainda diz "GPL v2 or later"; `metadata.txt` não declara |
| **Página oficial** | https://plugins.qgis.org/plugins/pointsamplingtool/ |
| **Código-fonte** | https://github.com/borysiasty/pointsamplingtool |
| **Documentação** | README curto no repositório; sem manual |
| **Versão verificada** | 0.5.6 (2026-03-03) |
| **QGIS mínimo** | 3.0 (máximo 4.99; Qt6 desde a 0.5.5, 2025-08-25) |
| **Status** | manutenção esporádica (commits só de compatibilidade: 2022-11, 2025-08, 2026-03) |
| **Verificado em** | 2026-09-08 |

## O que faz

- Recebe uma camada de pontos e uma lista de fontes: bandas de raster e
  campos de polígonos, todos de uma vez, com seleção múltipla.
- Para cada ponto, lê o valor da célula de cada banda (`identify` do
  provedor) e o atributo do polígono que contém o ponto.
- Grava nova camada de pontos (GeoPackage por padrão; também CSV e
  Shapefile) com as colunas amostradas, nomes editáveis; pode acrescentar a
  um GPKG existente e adiciona ao projeto. Aceita MultiPoint (0.5.3+).

## Para que serve em geologia

- **Cota da boca do poço** a partir do MDE (Copernicus 30 m, TOPODATA,
  ALOS) quando o cadastro não tem nível ou o GPS deu cota ruim — insumo
  para o desurvey no Geoscience e para o nível estático em cota absoluta.
- Atribuir a cada poço **unidade litoestratigráfica**, **domínio
  hidrogeológico** e **bacia** a partir dos polígonos do SGB/CPRM e SEMAD.
- Ler rasters de **geofísica** (magnetometria, gama K/eU/eTh) e de espessura
  de regolito nos pontos de poço, para cruzar com vazão.
- Amostrar declividade, densidade de lineamentos e distância a drenagem numa
  única operação para montar a tabela de uma análise de favorabilidade.
- Nativamente é preciso encadear "Sample raster values" por raster e "Join
  attributes by location" por polígono; aqui é um diálogo só.

## Instalação

- Complementos → Gerenciar e Instalar Complementos → buscar "Point sampling tool".
- Sem dependências extras. QGIS 4/Qt6: usar 0.5.6 (a 0.5.5 trouxe Qt6, a
  0.5.6 corrigiu a compatibilidade com QGIS 4).

## Como usar (roteiro mínimo)

1. Coloque **todas** as camadas no mesmo CRS (ex.: SIRGAS 2000 / UTM 22S ou
   23S): reprojete MDE e polígonos para o CRS dos poços. O plugin não reprojeta.
2. Complementos → Analyses → **Point sampling tool**.
3. Escolha a camada de pontos (poços). Na lista, selecione com Ctrl/Shift os
   campos que quer manter dos pontos, as bandas dos rasters (`MDE: Band 1`)
   e os campos dos polígonos (`geologia: SIGLA_UNID`).
4. Na aba de campos, renomeie as colunas de saída (ex.: `cota_mde`,
   `unid_geo`); para Shapefile mantenha ≤ 10 caracteres.
5. Defina o arquivo de saída (GPKG recomendado) e confirme "adicionar ao mapa".
6. Confira nulos: pontos fora do raster ou de qualquer polígono ficam vazios;
   pontos em nodata podem vir com o valor de nodata em vez de nulo.

## Entradas e saídas

- **Entrada:** camada de pontos (ou multipontos); N rasters (uma entrada por
  banda); N camadas de polígonos (uma entrada por campo). Mesmo CRS.
- **Saída:** nova camada de pontos em GeoPackage, CSV ou Shapefile com as
  colunas amostradas; sem camada temporária (issue #28).

## Limitações e armadilhas

- **Um polígono por camada**: com polígonos sobrepostos só o último
  encontrado na varredura é usado (comentário no código: "only last one if
  more polygons overlaps"; issue #8, aberta desde 2018). Separe coberturas
  e embasamento em camadas distintas.
- **Sem reprojeção**: compara os CRS e avisa; se o usuário continuar,
  amostra coordenadas erradas em silêncio.
- Valores de raster convertidos para **float** ("I HAVE TO IMPLEMENT RASTER
  TYPE HANDLING" no código): categóricos inteiros viram 1.0, 2.0; conversão
  de nodata em NULL não verificada.
- Busca por bounding box de ±0,001 unidade do mapa: 1 mm em UTM, mas ~100 m
  em graus — mais um motivo para usar CRS projetado.
- Shapefile trunca nomes a 10 caracteres; desde a 0.5.4 não há trava, então
  o driver trunca sem avisar. Prefira GPKG.
- Sem algoritmo de Processing: não entra em modelos nem em lote. Não amostra
  linhas e não faz estatística zonal.

## Alternativas

- **Sample raster values** (`native:samplerasterlayer`): nativo, um raster
  por vez (todas as bandas), usável em modelos e em lote; reprojeta na hora.
- **Drape (set Z value from raster)** (`native:setzfromraster`): grava a
  cota do MDE no Z da geometria, com banda, nodata, escala e offset — melhor
  para levar o collar com Z real ao Geoscience.
- **Join attributes by location** (`native:joinattributesbylocation`):
  polígono → ponto com um-para-um ou um-para-vários (resolve a sobreposição).
- **Zonal statistics** (`native:zonalstatistics`): média/min/max num raio
  de influência do poço em vez do valor de uma célula.
- Fora do QGIS: `rasterio.sample` + `geopandas.sjoin` em Python quando o
  fluxo for repetitivo (cadastro com milhares de poços).

## Ver também

- `fluxos/README.md` — enriquecimento do cadastro de poços (cota, geologia, aquífero) — a criar.
- `dados/README.md` — MDEs, geologia SGB/SEMAD e rasters geofísicos.
- `ferramentas/qgis-plugins/dados-campo/geoscience.md` — consome a cota extraída aqui no collar.
- `ferramentas/qgis-plugins/dados-campo/data-plotly.md` — plotar os valores amostrados.
