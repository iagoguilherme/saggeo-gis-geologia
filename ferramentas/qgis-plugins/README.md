# Plugins QGIS para geologia

Catálogo verificado em **2026-09-08** na [plugins.qgis.org](https://plugins.qgis.org/)
e nos repositórios de origem. Cada plugin tem ficha própria com autor,
licença, versão, dependências, roteiro mínimo, armadilhas e alternativas.

Legenda de status em [../README.md](../README.md#legenda-de-status).

## Mapeamento e relevo

| Plugin | Ficha | Versão | QGIS | Licença | Status | Observação |
|---|---|---|---|---|---|---|
| **QuickMapServices** (agora "NextGIS QuickMapServices") | [quickmapservices.md](mapeamento-relevo/quickmapservices.md) | 1.5.1 (2026-09-03) | 3.32 → 4.x | GPL-2.0+ | ativo | pacote "contributed" já vem embutido; Google/Bing têm restrição de uso em produto entregue |
| **SRTM-Downloader** | [srtm-downloader.md](mapeamento-relevo/srtm-downloader.md) | 4.0.4 (2026-08-03) · 3.3.4 para QGIS 3 | 3.x / 4.x | GPL-3.0 | ativo | baixa via API do OpenTopography (chave gratuita); não usa mais login NASA Earthdata; 12 MDEs, não só SRTM |
| **Qgis2threejs** | [qgis2threejs.md](mapeamento-relevo/qgis2threejs.md) | 3.2.1 (2026-09-07) · 2.10.4 para QGIS 3 | 3.4 / 4.x | GPL-3.0 | ativo | pré-visualização interna instável no macOS; usar "abrir no navegador" |

## Geologia estrutural e perfis

| Plugin | Ficha | Versão | QGIS | Licença | Status | Observação |
|---|---|---|---|---|---|---|
| **qProf** | [qprof.md](estrutural-perfis/qprof.md) | 0.5.0 (2022-11-07) · 0.5.2/0.6.3 experimentais (2026-05) | 3.x / 4.x (exp.) | GPL-3.0 | manutenção esporádica | código vivo está no GitLab; o GitHub apontado na loja parou em 2021 |
| **Profile Tool** | [profile-tool.md](estrutural-perfis/profile-tool.md) | 4.3.4 (2026-03-19) | 3.40 → 4.x | GPL-2.0+ | manutenção esporádica | mantenedor declarou "sem desenvolvimento novo"; o Perfil de Elevação nativo (QGIS ≥ 3.26) cobre o básico |
| **Stereonet** | [stereonet.md](estrutural-perfis/stereonet.md) | 0.3 (2018-05-25) | 3.0 → 3.99 | GPL-2.0 | **abandonado** | não instala no QGIS 4; usar **Stereoplot** (1.0.1, 2026-09-01, MIT) no lugar |
| **sec_interp** | [sec-interp.md](estrutural-perfis/sec-interp.md) | 3.7.1 (2026-09-06) | 3.28 → 4.x | GPL-2.0/3.0 | ativo | projeto novo (nov/2025), muitos releases, issues sem resposta; validar resultado antes de usar em laudo |

## Dados de campo

| Plugin | Ficha | Versão | QGIS | Licença | Status | Observação |
|---|---|---|---|---|---|---|
| **Geoscience** | [geoscience.md](dados-campo/geoscience.md) | 2.0 (2026-08-16) · 1.17 para QGIS 3 | 3.x / 4.x | GPL-3.0 | ativo | autor Roland Hill; README ainda diz "sem desenvolvimento", mas há 5 releases em 2026 |
| **Data Plotly** | [data-plotly.md](dados-campo/data-plotly.md) | 4.5.1 (2026-06-24) | 3.28 → 4.x | GPL-2.0 | ativo | precisa do pacote Python `plotly` e de QtWebKit (QGIS 3) / QtWebEngine (QGIS 4); não faz Piper/Stiff |
| **Point Sampling Tool** | [point-sampling-tool.md](dados-campo/point-sampling-tool.md) | 0.5.6 (2026-03-03) | 3.0 → 4.x | GPL-3.0 | manutenção esporádica | exige mesmo SRC em todas as camadas; o algoritmo nativo "Sample raster values" resolve o caso simples |

## Sensoriamento remoto e geofísica

| Plugin | Ficha | Versão | QGIS | Licença | Status | Observação |
|---|---|---|---|---|---|---|
| **SCP — Semi-Automatic Classification** | [scp-semi-automatic-classification.md](sensoriamento-geofisica/scp-semi-automatic-classification.md) | 9.0.4 (2026-07-11) · 8.5.0 para QGIS 3 | 3.x (8.5) / 4.x (9.x) | GPL-3.0 | ativo | depende do pacote Remotior Sensus; conta gratuita no Copernicus Data Space; crash em loop reportado no macOS Apple Silicon |
| **SGTool** ("Structural Geophysics Tools" no post) | [structural-geophysics-tools.md](sensoriamento-geofisica/structural-geophysics-tools.md) | 0.3.7 (2026-09-02) | 3.24 → 4.x | MIT | ativo | o nome do post **não existe**; corresponde ao SGTool (Mark Jessell, WAXI): filtros, derivadas, continuação e gradeamento de dados potenciais |
| **Freehand Raster Georeferencer** | [freehand-raster-georeferencer.md](sensoriamento-geofisica/freehand-raster-georeferencer.md) | 0.8.3 (2021-02-15) | ≤ 3.99 | GPL-2.0 | manutenção esporádica | não aparece no QGIS 4 (branch `qgis4` sem release); só transformação afim; Georreferenciador nativo quando o mapa tem grade |

## QGIS 3 LTR ou QGIS 4?

Em 2026 vários plugins passaram a lançar versões **só para o QGIS 4**
(Qt6) e congelaram a linha do QGIS 3. Antes de migrar a máquina de
trabalho, conferir:

| Plugin | Última versão para QGIS 3 | Roda no QGIS 4? |
|---|---|---|
| QuickMapServices | 1.5.1 (mesma) | sim |
| SRTM-Downloader | 3.3.4 | sim (4.0.x) |
| Qgis2threejs | 2.10.4 | sim (3.x) |
| qProf | 0.5.0 / 0.5.2 exp. | só experimental (0.6.x) |
| Profile Tool | 4.3.4 (mesma, exige ≥ 3.40) | sim |
| Stereonet | 0.3 | **não** → Stereoplot |
| sec_interp | 3.7.1 (mesma) | sim |
| Geoscience | 1.17 | sim (2.0) |
| Data Plotly | 4.5.1 (mesma) | sim |
| Point Sampling Tool | 0.5.6 (mesma) | sim |
| SCP | 8.5.0 (sem suporte) | sim (9.x) |
| SGTool | 0.3.7 (mesma) | sim |
| Freehand Raster Georeferencer | 0.8.3 | **não** (ainda) |

Recomendação: manter o **QGIS 3.40 LTR** na máquina de produção até que
Freehand Raster Georeferencer e qProf tenham release estável para o 4;
usar o QGIS 4 em paralelo para SCP 9 e Qgis2threejs 3.

## Divergências em relação ao post original

| No post | Verificado |
|---|---|
| "QGIS2ThreeJS" | nome oficial é **Qgis2threejs** |
| "Structural Geophysics Tools" | não existe; o plugin real é **SGTool** (README: "Structural Geophysics Tool") |
| "Stereonet" | o plugin desse nome está abandonado desde 2018 e não roda no QGIS 4; o sucessor é **Stereoplot** |
| "Geoscience → Mining Geoscience" | autor é Roland Hill; "Mining Geoscience" não verificado |
| "SRTM-Downloader → dados da NASA" | desde a 3.3.0 baixa pelo OpenTopography com chave gratuita; a descrição na loja está desatualizada |
| "Todos gratuitos, open-source, QGIS 3.x+" | verdadeiro para os 13, com a ressalva de que 2 não rodam no QGIS 4 e 3 têm a versão atual só no QGIS 4 |

## Candidatos a ficha (vistos de passagem, não documentados)

Apareceram como alternativa durante a verificação e merecem ficha própria
quando forem testados:

- **Stereoplot** — sucessor do Stereonet (UWA-CET, MIT, 2026-09-01).
- **GeoStereonet** (1.2.0, 2026-07) e **Structural Families Mapper** (1.0.1, 2026-08) — estereogramas.
- **OpenLog** (Oslandia, 1.9.2, 2026-08) — furos e perfis de poço; tem versão Premium.
- **Midvatten** (1.8.3, 2026-02) — hidrogeologia: níveis, diagrama de Piper, séries.
- **OpenTopography DEM Downloader** (4.2, 2026-07) — inclui ANADEM 30 m (Brasil).
- **HCMGIS** (QGIS ≥ 4) — mapas base e utilitários, alternativa ao QuickMapServices.
- **Advanced Charts** (1.2.0, QGIS ≥ 4) — gráficos Matplotlib no layout.
- **tomofast_x_q** (0.2.14, 2026-06) — inversão geofísica com Tomofast-x.
- **Geosoft GRD Loader** (1.2.0, 2026-08) — abre grids `.grd` da Geosoft (aerogeofísica do SGB).

## Instalação (regras gerais)

1. Complementos → Gerenciar e Instalar Complementos → aba *Todos* → buscar
   pelo nome. Plugins marcados `experimental` só aparecem com
   *Configurações → Mostrar também os complementos experimentais*.
2. Plugins que precisam de pacote Python (Data Plotly, SCP, SGTool)
   instalam no **Python do QGIS**, não no venv do projeto; ver
   [../python/README.md](../python/README.md#convenção-de-ambiente).
3. Chaves de API (OpenTopography, Copernicus) entram na configuração do
   plugin, nunca em arquivo do repositório.
4. Depois de instalar, registrar na ficha a versão testada e o resultado
   do roteiro mínimo.
