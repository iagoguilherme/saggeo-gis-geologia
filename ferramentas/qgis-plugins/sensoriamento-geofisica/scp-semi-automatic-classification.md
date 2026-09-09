# SCP — Semi-Automatic Classification Plugin

> Baixa, pré-processa e classifica imagens de satélite (Sentinel-2, Landsat, MODIS, ASTER) dentro do QGIS; gera NDVI, composições coloridas e mapas de uso/cobertura do solo usados na locação de poços em aquífero fissural e na caracterização de áreas de recarga.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | sensoriamento-geofisica |
| **Autor(es)** | Luca Congedo (projeto independente; blog "From GIS to Remote Sensing") |
| **Licença** | GPL-3.0 (software); documentação CC BY-SA 4.0 |
| **Página oficial** | https://plugins.qgis.org/plugins/SemiAutomaticClassificationPlugin/ |
| **Código-fonte** | https://github.com/semiautomaticgit/SemiAutomaticClassificationPlugin |
| **Documentação** | https://semiautomaticclassificationmanual.readthedocs.io/ |
| **Versão verificada** | 9.0.4 (2026-07-11) para QGIS 4; 8.5.0 (2024-11-16) é a última da série para QGIS 3.x |
| **QGIS mínimo** | 4.0 (série 9.x); 3.0 a 3.99 (série 8.x, sem suporte) |
| **Status** | ativo (série 9 para QGIS 4; série 8 congelada) |
| **Verificado em** | 2026-09-08 |

## O que faz

- Download de imagens abertas sem sair do QGIS: Sentinel-2 L1C/L2A (Copernicus Data Space), Sentinel-2 L2A, Landsat 5/7/8/9, MODIS (reflectância e temperatura), ASTER L1T e Copernicus DEM GLO-30 (Microsoft Planetary Computer), Harmonized Landsat Sentinel-2 (NASA Earthdata).
- Pré-processamento: conversão para reflectância, correção atmosférica DOS1, recorte, reprojeção, mosaico e gerenciamento de "band sets".
- Classificação supervisionada: Máxima Verossimilhança, Distância Mínima, Spectral Angle Mapping, Random Forest, SVM e Multilayer Perceptron (scikit-learn); clustering (desde 8.3); modelos de deep learning pré-treinados Swin-v2 para Sentinel-2 e Landsat 8/9 (série 9, PyTorch).
- Band calc: calculadora de bandas com expressões (NDVI e outros índices prontos), ROIs com assinatura espectral, edição de raster.
- Pós-processamento: avaliação de acurácia, relatório de classes, classificação para vetor, sieve.
- Motor de processamento: biblioteca Python Remotior Sensus, do mesmo autor — reescrita completa a partir da v8.0 (out/2023); a v9 exige Remotior Sensus ≥ 0.6.

## Para que serve em geologia

- NDVI e composições falsa-cor de Sentinel-2 (10 m) para enxergar vegetação alinhada e umidade ao longo de fraturas — evidência indireta de lineamentos para locação em aquífero fissural (cristalino de GO/DF/MG). O SCP gera o índice; o traçado dos lineamentos é feito à mão sobre hillshade/imagem ou com GeoTrace.
- Classificação de uso e cobertura do solo da área de recarga de um poço ou de uma bacia (mata, pastagem, agricultura, solo exposto, área urbana) para relatório de outorga e estudo de vulnerabilidade.
- Baixar a cena já recortada na área de interesse, com bandas em reflectância de superfície, sem passar pelo navegador.
- Comparar datas (expansão urbana sobre área de recarga, antes/depois de obra).
- O QGIS puro tem calculadora raster e classificação via SAGA/OTB, mas não tem download integrado, band sets, ROIs com assinatura espectral nem fluxo de acurácia num só lugar.

## Instalação

- Complementos → Gerenciar e Instalar Complementos → buscar "Semi-Automatic Classification Plugin".
- QGIS 4 (Qt 6): instala a série 9.x. QGIS 3.x: o gerenciador oferece a 8.5.0; a documentação atual diz que a versão 8 não é mais suportada.
- Dependências obrigatórias: Remotior Sensus, GDAL, NumPy, SciPy (a v9 baixa e atualiza o Remotior Sensus sozinha). Opcionais: scikit-learn (Random Forest, SVM, MLP) e PyTorch + torchvision (deep learning).
- Windows: instalar scikit-learn/torch pelo OSGeo4W Shell com `pip`. macOS: `/Applications/QGIS.app/Contents/MacOS/bin/pip3 install scikit-learn scipy torch torchvision --extra-index-url https://download.pytorch.org/whl/cpu`. Há também roteiro via conda.
- Contas gratuitas para download: Copernicus Data Space (https://dataspace.copernicus.eu) para Sentinel-2 oficial; NASA Earthdata (https://urs.earthdata.nasa.gov) para HLS. Produtos via Microsoft Planetary Computer não exigem conta.
- Depois de instalar, em SCP → Settings definir "Available RAM (MB)" com metade da RAM da máquina.

## Como usar (roteiro mínimo) — NDVI de Sentinel-2 para área de locação

1. SCP → Download products: desenhar a área, escolher Sentinel-2 L2A, filtrar por data e cobertura de nuvens, "Find", selecionar a cena e baixar só as bandas necessárias (B02, B03, B04, B08) com "preprocess" marcado para gerar reflectância e band set.
2. Band set: conferir que o band set 1 tem as bandas na ordem certa e a definição "Sentinel-2".
3. Band calc: usar a expressão pronta de NDVI (ou `("#NIR#" - "#RED#") / ("#NIR#" + "#RED#")`), rodar e salvar como GeoTIFF.
4. Estilizar o NDVI e sobrepor ao hillshade do SRTM/TOPODATA; digitalizar lineamentos numa camada de linhas.
5. (Opcional) Classificação: criar ROIs por classe no training input, treinar (Random Forest), rodar e avaliar em "Accuracy" contra pontos de verdade de campo.

## Entradas e saídas

- **Entrada:** cenas baixadas pelo plugin ou qualquer raster multibanda já no disco; polígono da área de interesse; ROIs de treinamento (GeoPackage/shape).
- **Saída:** GeoTIFFs (bandas em reflectância, índices, classificação), relatório de classes e acurácia (CSV), assinaturas espectrais, vetor da classificação.

## Limitações e armadilhas

- Série 9.x só roda no QGIS 4; quem está no QGIS 3.x fica na 8.5.0, sem suporte nem correções.
- Dependências ficam fora do gerenciador de plugins: instalar scikit-learn/torch no Python errado (o do sistema em vez do Python do QGIS) é a causa mais comum de "módulo não encontrado". Erro citando `remotior_sensus` = biblioteca ausente ou desatualizada.
- macOS Apple Silicon: issues abertas em 2026 relatam QGIS reiniciando em loop ao ativar o SCP (#432, #449) e falha na instalação de dependências (#430).
- Senha do Copernicus/Earthdata: com "remember" marcado, fica gravada sem criptografia no registro do QGIS.
- Download: não mexer no QGIS durante o download; erro `NoneType has no len()` no "Find" (#433) quando a busca volta vazia.
- Processa cenas inteiras em RAM e disco temporário — vários GB por cena Sentinel-2; recortar antes.
- Classificação pixel a pixel confunde solo exposto, área urbana e rocha aflorante; validar em campo e cruzar com MapBiomas.
- Dados Copernicus/NASA são abertos, mas exigem citação da fonte no mapa.

## Alternativas

- **Google Earth Engine** — mosaicos e séries temporais de Sentinel/Landsat/MapBiomas sem download; exige conta e código; grátis para uso não comercial.
- **Orfeo Toolbox (OTB)** via Processing — classificação e segmentação robustas, sem download integrado.
- **EnMAP-Box** (plugin QGIS) — foco em hiperespectral e machine learning; curva de aprendizado maior.
- **MapBiomas** — se só precisa de uso do solo pronto (30 m, anual desde 1985), baixar direto e pular a classificação.
- **Calculadora raster do QGIS** — basta para NDVI se as bandas já estão em reflectância.

## Ver também

- Fluxo "Lineamentos e estereograma para aquífero fissural" (`fluxos/README.md`, a documentar).
- Fluxo "Bacia de contribuição e área de recarga" (`fluxos/README.md`, a documentar).
- Fontes Copernicus Data Space, USGS EarthExplorer, MapBiomas e Google Earth Engine em `dados/README.md`.
- Citação: Congedo, L. (2021). Semi-Automatic Classification Plugin: A Python tool for the download and processing of remote sensing images in QGIS. *Journal of Open Source Software*, 6(64), 3172.
