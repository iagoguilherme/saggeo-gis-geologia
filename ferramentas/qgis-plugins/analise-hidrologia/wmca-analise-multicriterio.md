# Weighted Multi-Criteria Analysis — WMCA

> Análise multicritério ponderada em raster: mostra as classes de cada
> raster, você dá um peso por raster e uma nota por classe, e o plugin soma
> peso × nota pixel a pixel — base de um mapa de favorabilidade
> hidrogeológica (ou de fragilidade) para locação de poços.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | analise-hidrologia |
| **Autor(es)** | Carvalho Neto, R.M. (UFSM; hoje PlugGIS) e Benedetti, A.C.P. (UFSM) |
| **Licença** | não declarada (sem LICENSE no repositório; loja exige GPL-compatível, mas não está explícito); só o cabeçalho padrão do Plugin Builder nos `.py` cita "GPL v2 or later" |
| **Página oficial** | https://plugins.qgis.org/plugins/multi_criteria/ |
| **Código-fonte** | https://github.com/romariocarvalhoneto/Weighted-Multi-Criteria-Analysis---WMCA |
| **Documentação** | README curto; vídeo em português (canal Beta Analitica, "Análise Multicritério Ponderada com QGIS", https://www.youtube.com/watch?v=puboFOrqsks); algoritmo no TCC de Especialização em Geomática, UFSM, 2021-12-17 (https://repositorio.ufsm.br/handle/1/28666); rasters de exemplo em https://github.com/romariocarvalhoneto/Raster_WMCA_sample |
| **Versão verificada** | 0.4.2 (2023-02-07); 7.633 downloads somando as 5 versões (0.1 a 0.4.1 entre 2020-05 e 2020-07) |
| **QGIS mínimo** | 3.0 (máximo 3.99; a 0.4.2 é experimental, a 0.4.1 consta como estável) |
| **Status** | abandonado (sem commit desde 2023-04; funciona no QGIS 3.x; máx. 3.99, não aparece no QGIS 4); último commit de código 2023-02-07 ("Bug fix for QGIS 3.22 — from osgeo import gdal, osr"); o autor segue ativo na PlugGIS (pluggis.com.br), então pode voltar a atualizar |
| **Verificado em** | 2026-09-08 |

## O que faz

- Lista os rasters carregados no projeto; para cada um lê a banda 1 com
  GDAL/NumPy e abre uma aba com as classes (`np.unique`) em três colunas:
  *Original Value*, *Grade* e *Disregard*.
- O usuário digita um **peso por raster** e uma **nota por classe**;
  *Disregard* tira a classe do cálculo (código interno -9998).
- Calcula pixel a pixel `Σ (peso_i × nota_i)` — soma ponderada simples,
  sem normalizar pela soma dos pesos — e grava GeoTIFF Float32 com a
  georreferência do primeiro raster (em `QgsTask`, com barra de progresso).
- Recusa raster com mais de 100 classes ("Use the 'Reclassify by table'…")
  e avisa que fica lento entre 50 e 100.

## Para que serve em geologia

- **Favorabilidade hidrogeológica / potencial de água subterrânea** para
  locar poço em aquífero fissural (DF e Goiás): critérios reclassificados
  em faixas — densidade de lineamentos, litologia (SGB/SEMAD), declividade
  (MDE), uso e cobertura (MapBiomas), distância à drenagem, espessura do
  manto de alteração — cada um com nota 1–5 por classe e peso por critério.
- **Fragilidade/vulnerabilidade** de aquífero e área de proteção de poço, com notas invertidas.
- **Cenários**: trocar um peso e reprocessar sem reescrever expressão.

## Instalação

- Marcar "Mostrar complementos experimentais"; buscar "Weighted
  Multi-Criteria Analysis" (id `multi_criteria`). Só GDAL/NumPy do QGIS.
- Instale a **0.4.2**. A 0.4.1 (2020, "estável" na loja) quebra no QGIS
  ≥ 3.22 por `import osr` sem `from osgeo`; a correção só está na 0.4.2,
  que a loja classifica como experimental — quem não habilita experimentais
  recebe a versão quebrada. Não é ofertado no QGIS 4.

## Como usar (roteiro mínimo)

1. Prepare cada critério como raster **categórico** (inteiro, ≤ 100
   classes) e **na mesma grade** (extensão, resolução, linhas × colunas e
   SRC): *Reclassify by table* para contínuos (declividade em 5 faixas) e
   *Warp/Clip* para alinhar. Não use 0 como classe (o autor reserva 0 para
   nodata na amostra); defina nodata explícito.
2. Carregue todos no projeto; Raster → **WMCA**. Adicione raster por
   raster; em cada aba, digite a nota por classe (1 = desfavorável … 5 =
   muito favorável) e marque *Disregard* em classes sem sentido (água, urbano).
3. Na tabela principal, digite o peso de cada raster (ex.: lineamentos
   0,30; litologia 0,25; declividade 0,15; manto 0,15; drenagem 0,10; uso
   0,05). Fazer a soma dar 1 é responsabilidade sua.
4. Escolha o `.tif` de saída e confirme; o resultado entra no mapa.
5. Classifique o índice (quebras naturais/quantis) em muito baixa → muito
   alta e valide contra vazões de poços existentes (SIAGAS ou cadastro).

## Entradas e saídas

- **Entrada:** N rasters (banda 1), categóricos, mesma contagem de células;
  pesos e notas digitados no diálogo.
- **Saída:** um GeoTIFF Float32 com o índice `Σ peso × nota`; nodata
  herdado do raster (ou -9999).

## Limitações e armadilhas

- **Só confere linhas × colunas** (`assert`), não geotransform nem SRC:
  rasters de mesma dimensão e extensão diferente saem deslocados sem aviso.
- **Sem normalização**: mudar a escala das notas ou dos pesos muda a escala
  do índice; compare cenários só com a mesma escala.
- **Nada é salvo**: pesos e notas não ficam em arquivo — registre numa
  planilha para rastreabilidade no laudo.
- Nodata: usa o do raster ou -9999; a classe 0 **não** é tratada como nodata
  pelo código, apesar da recomendação; saída do *Disregard* (-9998) não verificada.
- Sem Processing (sem lote nem modelo) e sem AHP: os pesos vêm de fora.
- PR #11 "fix import osr" (gisma, 2022-07-05) segue aberto embora o autor
  tenha corrigido o mesmo bug em 2023-02; 0 issues, 5 estrelas, 5 forks.
- Licença não declarada: para redistribuir ou modificar (ex.: portar para
  QGIS 4) só o cabeçalho GPL-2+ dos `.py` serve de base — confirmar com o autor.

## Alternativas

- **Raster Calculator** nativo: reclassifique cada critério com as notas e
  some `0.30*"lin@1" + 0.25*"lito@1" + …`. Mesmo resultado, aceita critério
  contínuo (fuzzy), entra em modelo e lote e fica documentado — para laudo.
- **AHP manual**: matriz de Saaty em planilha (autovetor, razão de
  consistência < 0,10) → pesos → Raster Calculator ou WMCA.
- **Python** (`rasterio` + `numpy`) ou GRASS `r.mapcalc`: reprodutível de
  ponta a ponta quando há muitos cenários.

## Ver também

- `fluxos/README.md` — mapa de favorabilidade hidrogeológica — a criar.
- `dados/README.md` — geologia SGB/SEMAD, MDE, MapBiomas, lineamentos.
- `ferramentas/qgis-plugins/analise-hidrologia/bhcgeo-balanco-hidrico.md` —
  excedente hídrico anual como critério de recarga.
