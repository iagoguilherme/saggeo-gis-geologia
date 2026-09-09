# BHCgeo

> Balanço hídrico climático de Thornthwaite & Mather (1955) pixel a pixel:
> de 12 rasters de chuva, 12 de evapotranspiração potencial e um de CAD
> gera armazenamento, ETR e o balanço mensal (excedente/deficiência) — o
> excedente é a estimativa de primeira ordem da recarga potencial.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | analise-hidrologia |
| **Autor(es)** | Carvalho Neto, R.M. (UFSM; hoje PlugGIS); Cruz, J.C. (UFSM); Cruz, R.C. (UNIPAMPA) |
| **Licença** | não declarada (sem LICENSE no repositório; loja exige GPL-compatível, mas não está explícito); só o cabeçalho padrão do Plugin Builder nos `.py` cita "GPL v2 or later" |
| **Página oficial** | https://plugins.qgis.org/plugins/bhcgeoqgis/ |
| **Código-fonte** | https://github.com/romariocarvalhoneto/BHCgeo |
| **Documentação** | README curto; regras de nome de arquivo na própria janela; sem manual. Método: Thornthwaite & Mather (1955), *The water balance* |
| **Versão verificada** | 0.3 (2023-04-12); 4.151 downloads somando as 3 versões (0.1 2020-05-04; 0.2 2020-07-09) |
| **QGIS mínimo** | 3.0 (máximo 3.99; experimental) |
| **Status** | abandonado (sem commit desde 2023-04; funciona no QGIS 3.x; máx. 3.99, não aparece no QGIS 4); último commit "corrigir erro de biblioteca" (changelog "fix bug from library."); o autor segue ativo na PlugGIS (pluggis.com.br), então pode voltar a atualizar |
| **Verificado em** | 2026-09-08 |

## O que faz

- Lê de uma pasta 25 GeoTIFFs de **nome fixo**: `pJan.tif … pDez.tif`,
  `etpJan.tif … etpDez.tif` e `cad.tif` (mm; nodata -9999).
- Começa no mês escolhido com o solo cheio (ARM = CAD) e percorre os 12
  meses em cada pixel: se P ≥ ETP, enche o ARM até a CAD e o resto vira
  excedente; se P < ETP, `ARM = ARM_ant × exp((P − ETP)/CAD)`,
  `ETR = P + (ARM_ant − ARM)` e a diferença para a ETP é deficiência.
- Grava séries mensais `bMês.tif` (B: excedente quando > 0, −deficiência
  quando < 0, 0 enquanto o solo reenche), `etrMês.tif` e `armMês.tif`,
  conforme as caixas marcadas.
- Gera `Report.txt` com a "prova real" por pixel: Σ ETP = Σ ETR + Σ DEF,
  Σ P = Σ ETR + Σ EXC e Σ ALT = 0 (tolerância 1 mm); se falhar, sugere
  trocar o mês inicial. Roda em `QgsTask` com barra de progresso.

## Para que serve em geologia

- **Recarga potencial**: o excedente (B > 0) é a água que sobra depois de
  o solo saturar — teto da recarga do aquífero antes de descontar o
  escoamento superficial. Na região de Brasília/Goiás (chuva de outubro a
  abril) o excedente aparece de dezembro a março; o resto é deficiência.
- **Mapa de recarga** por unidade hidrogeológica para estudos de
  disponibilidade e outorga (ADASA, SEMAD) e como critério "recarga" de
  um mapa de favorabilidade (WMCA).
- **Época seca**: os meses de deficiência (maio–setembro) indicam quando o
  nível estático tende ao mínimo — referência para teste de bombeamento.

## Instalação

- Marcar "Mostrar complementos experimentais"; buscar "BHCgeo". Só usa
  GDAL/NumPy do QGIS. Não é ofertado no QGIS 4.
- **Windows apenas, pelo código**: monta o caminho como `pasta + "\\" +
  "cad.tif"` (barra invertida fixa); no macOS/Linux o arquivo não é achado
  (não testado, mas evidente). Rodar em VM Windows ou trocar por `os.path.join`.

## Como usar (roteiro mínimo)

1. Monte 12 rasters mensais de **P** (normal climatológica: CHIRPS,
   WorldClim ou estações ANA/Hidroweb e INMET interpoladas), 12 de **ETP**
   (Thornthwaite pela temperatura do WorldClim ou Penman-Monteith do INMET)
   e um de **CAD** (100 mm é o padrão clássico; melhor por classe de solo).
2. Alinhe tudo na mesma grade e SRC; exporte GeoTIFF com nodata -9999 e os
   nomes exatos (`pJan`, `etpJan`, `cad`, em português) numa pasta só.
3. Raster → **BHCgeo**: selecione a pasta, o mês inicial (fim da estação
   chuvosa, solo cheio — no Centro-Oeste, março ou abril), marque B, ETR,
   ARM e o Report. OK: as 36 saídas e o `Report.txt` ficam na mesma pasta.
4. Leia o `Report.txt`: se a prova real acusar erro, troque o mês inicial.
5. Excedente anual: some os `bMês.tif` só onde positivos (Raster
   Calculator `("bJan@1">0)*"bJan@1" + …` ou `r.series`) — recarga potencial;
   multiplique por coeficiente de infiltração por unidade para a recarga efetiva.

## Entradas e saídas

- **Entrada:** 25 GeoTIFFs de nome fixo, mesma dimensão (verificada por
  `assert`), valores em mm/mês, nodata -9999; mês inicial.
- **Saída:** até 36 GeoTIFFs Float32 mensais (B, ETR, ARM) + `Report.txt`.
  Sem DEF/EXC/ALT em raster separado e sem totais anuais.

## Limitações e armadilhas

- Caminho com `\\` fixo (Windows); nomes de arquivo e nodata obrigatórios;
  só compara nº de células, não SRC nem extensão — raster desalinhado passa.
- **ARM inicial = CAD sem iteração de convergência**: mês inicial sem solo
  cheio enviesa o primeiro ciclo; o relatório detecta, mas a escolha é manual.
- **Excedente ≠ recarga**: parte escoa em superfície; em aquífero fissural
  sob Latossolo espesso, parte fica no manto poroso. Trate como teto.
- CAD uniforme (100 mm) simplifica demais; usar mapa de solos. Não valida
  unidades (tudo em mm/mês); o uso previsto é a normal climatológica.
- Sem Processing: sem lote (ex.: um ano por vez para série histórica); sem
  totais anuais nem DEF/EXC separados — pós-processar. 0 issues, 1 estrela.
- Licença não declarada: para redistribuir ou modificar (ex.: corrigir o
  caminho, portar para QGIS 4) só o cabeçalho GPL-2+ dos `.py` serve — confirmar com o autor.

## Alternativas

- **Raster Calculator / Modelador**: reprodutível, mas 12 passos com
  condicional e exponencial encadeados; vale só para um pixel de teste.
- **Planilha clássica** (ex.: BHnorm da ESALQ) para uma estação: use para
  validar o pixel da estação contra o raster.
- **Python** (`numpy` + `rasterio`): Thornthwaite-Mather em ~40 linhas,
  com iteração até o ARM convergir e sem restrição de nome/pasta.

## Ver também

- `dados/README.md` — CHIRPS, WorldClim, ANA/Hidroweb, INMET, solos Embrapa.
- `ferramentas/qgis-plugins/analise-hidrologia/wmca-analise-multicriterio.md`
  — excedente anual reclassificado como critério de favorabilidade.
- `fluxos/README.md` — estimativa de recarga para outorga — a criar.
