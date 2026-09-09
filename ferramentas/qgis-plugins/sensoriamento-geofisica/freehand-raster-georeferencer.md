# Freehand Raster Georeferencer

> Coloca uma imagem escaneada (mapa geológico antigo, croqui de relatório, figura de artigo) sobre a base do QGIS arrastando, girando e escalando com o mouse, sem pontos de controle; exporta com world file.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | sensoriamento-geofisica |
| **Autor(es)** | Guilhem Vellut (independente) |
| **Licença** | GPL-2.0 |
| **Página oficial** | https://plugins.qgis.org/plugins/FreehandRasterGeoreferencer/ |
| **Código-fonte** | https://github.com/gvellut/FreehandRasterGeoreferencer |
| **Documentação** | https://gvellut.github.io/FreehandRasterGeoreferencer/ |
| **Versão verificada** | 0.8.3 (2021-02-15) |
| **QGIS mínimo** | 3.0 (máximo declarado 3.99 — não aparece no gerenciador do QGIS 4) |
| **Status** | manutenção esporádica (última release 2021; branch `qgis4` com commits do autor em jun–jul/2026, ainda sem release) |
| **Verificado em** | 2026-09-08 |

## O que faz

- Adiciona uma imagem JPEG, PNG, TIFF ou BMP como camada especial centralizada na vista atual; se existir world file (`.jgw`, `.tfw`, `.pgw`) ou `.aux.xml`, usa.
- Ferramentas de mouse: mover; rotacionar (Ctrl/⌘ = em torno do ponto clicado); escalar em X e Y (Ctrl/⌘ = uniforme; aceita valor numérico e DPI); ajustar cada lado; "georreferenciar com 2 pontos" (1º ponto translada, 2º rotaciona e escala).
- Transparência em passos de 10 % e desfazer.
- Exporta de dois jeitos: (a) imagem reamostrada com rotação/escala aplicadas + world file só com deslocamento (padrão, compatível com tudo); (b) imagem original + world file contendo rotação/escala ("only export world file"; nem todo software lê).

## Para que serve em geologia

- Encaixar rápido um mapa geológico escaneado (folhas SGB/CPRM, DNPM, RADAMBRASIL, mapas de relatórios de outorga) sobre imagem e relevo atuais para ler contatos e estruturas perto de um poço.
- Posicionar croquis sem coordenadas: planta de locação desenhada à mão, figura de artigo, mapa de acesso, traço de seção hidrogeológica.
- Sobrepor imagem de mapa aeromagnetométrico/gamaespectrométrico (PNG/JPG de atlas ou relatório) quando o grid original não está disponível — preferir sempre o GRD/GeoTIFF do GeoSGB quando existir.
- Pré-alinhar antes de refinar no Georreferenciador nativo: o world file exportado serve de chute inicial.
- O QGIS puro exige pontos de controle com coordenadas conhecidas; aqui basta o olho, o que resolve figuras sem grade nem cantos cotados.

## Instalação

- Complementos → Gerenciar e Instalar → "Freehand raster georeferencer". Sem dependências extras (lê a imagem via Qt, não via GDAL).
- Só QGIS 3.x (3.0 a 3.99). No QGIS 4 o plugin não é listado; o porte está em andamento no branch `qgis4` do GitHub (commits de 2026-06-30 a 2026-07-05, "gdal for raster reading"), sem versão publicada.

## Como usar (roteiro mínimo) — mapa geológico escaneado sobre a base atual

1. Obter o mapa em PNG ou JPEG (300 dpi basta); se for PDF, exportar a página como imagem e recortar as margens.
2. Projeto em SIRGAS 2000 / UTM 22S ou 23S com base (OSM, IBGE) e hillshade carregados; dar zoom na área do mapa.
3. Barra do plugin → "Add raster for interactive georeferencing" → escolher a imagem; ela cai centralizada na vista.
4. Reduzir a opacidade; usar "Georeference with 2 points" clicando em duas feições reconhecíveis (confluência de drenagem, cruzamento de estrada, sede de município) primeiro na imagem, depois na base.
5. Ajustar com mover/rotacionar/escalar; conferir em várias feições espalhadas pelo mapa, não só no centro.
6. "Export raster with world file" → imagem + world file; carregar como raster comum. Se precisar de precisão, usar o resultado como entrada do Georreferenciador nativo com pontos de controle.

## Entradas e saídas

- **Entrada:** imagem JPEG/PNG/TIFF/BMP (TIFF de 16 bits ou multibanda pode exigir conversão para RGB 8 bits antes); opcionalmente world file/`.aux.xml`.
- **Saída:** imagem + world file no CRS do projeto. A camada interativa não é um raster comum: só visualização e edição pelas ferramentas do plugin, até exportar.

## Limitações e armadilhas

- Só transformação afim (translação, rotação, escala X/Y): não corrige distorção de escaneamento, dobra de papel nem diferença de projeção. Mapa em SAD69/Córrego Alegre ou em policônica fica "quase" certo no centro e desalinhado nas bordas — para isso usar o Georreferenciador nativo com polinomial/TPS.
- Lê a imagem via Qt: quase nenhum formato GDAL, e rasters grandes travam — escanear em resolução moderada.
- Sem histograma, calculadora ou estilização de raster até exportar.
- World file com rotação (modo alternativo) não é lido por todo software; o modo padrão reamostra a imagem (perde nitidez).
- Suporte a CRS diferente do projeto é limitado; trabalhar no CRS final desde o início.
- Manutenção: 0.8.3 é de 2021; ~36 issues abertas e pull requests de terceiros (2021–2026) sem resposta; relatos de `TypeError` em `metadata()` (#72, 2024) e "para de funcionar" (#70, 2024) em QGIS 3.3x. Testar na sua versão antes de depender.
- Precisão é visual: em produto de outorga registrar que o mapa foi ajustado à mão e não medir distâncias/áreas sobre ele.

## Alternativas

- **Georreferenciador nativo do QGIS** (Raster → Georreferenciador, GDAL): pontos de controle, transformações polinomiais e TPS, resíduos por ponto. Preferir sempre que o mapa tiver grade de coordenadas ou cantos cotados.
- **gdal_translate -gcp + gdalwarp**: mesmo fluxo por linha de comando, para lote.
- **Combinação**: Freehand para o chute inicial, Georreferenciador nativo para o ajuste fino.
- **ArcGIS Pro** (pago): georreferenciamento interativo semelhante (mover/rotacionar/escalar) integrado ao fluxo de GCPs.

## Ver também

- Fluxo "Georreferenciar mapa geológico escaneado" (`fluxos/README.md`, a documentar).
- `dados/README.md` → GeoSGB (mapas geológicos) e regra de conferir datum de dados antigos.
