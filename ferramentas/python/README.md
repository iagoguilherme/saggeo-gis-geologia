# Bibliotecas Python

Para automatizar fora do QGIS (scripts, APIs, relatórios) ou dentro dele
(console Python / Processing). Lista comentada; fichas em
`python/<nome>.md` quando forem usadas num fluxo real.

## Geoespacial de base

| Biblioteca | Para quê | Ficha |
|---|---|---|
| **geopandas** | vetores como DataFrame; joins espaciais, reprojeção, buffers | a fazer |
| **shapely** | geometria (ponto, linha, polígono); base do geopandas | a fazer |
| **pyproj** | transformação de coordenadas (SIRGAS 2000 ↔ UTM ↔ WGS 84) | a fazer |
| **rasterio** | leitura/escrita de raster, amostragem de valores em pontos, recorte | a fazer |
| **rioxarray / xarray** | rasters multibanda e séries temporais (Sentinel, chuva) com eixos nomeados | a fazer |
| **pyogrio** | leitura/escrita rápida de vetores (substitui fiona no geopandas) | a fazer |
| **owslib** | consumir WMS/WFS/WCS (GeoSGB, INDE, SIEG) por código | a fazer |
| **folium / leafmap** | mapas web interativos para relatório ou portal | a fazer |

## Terreno e hidrologia

| Biblioteca | Para quê | Ficha |
|---|---|---|
| **richdem** | depressões, direção de fluxo, acumulação em MDE | a fazer |
| **whitebox** (WhiteboxTools) | mais de 400 ferramentas de terreno e hidrologia; também plugin QGIS | a fazer |
| **pysheds** | delineação de bacia e rede de drenagem | a fazer |

## Geologia, poços e perfis

| Biblioteca | Para quê | Ficha |
|---|---|---|
| **striplog** | intervalos litológicos (perfil de poço) como objetos; plotagem de colunas | a fazer |
| **lasio / welly** | perfis geofísicos de poço em formato LAS | a fazer |
| **mplstereonet** | estereogramas em Matplotlib (polos, planos, contornos de densidade) | a fazer |
| **gempy** | modelagem geológica 3D implícita | a fazer |
| **pyvista** | visualização 3D (poços, superfícies, blocos) | a fazer |

## Geofísica e potencial

| Biblioteca | Para quê | Ficha |
|---|---|---|
| **harmonica / verde / boule** (Fatiando a Terra) | gravimetria e magnetometria: gradeamento, continuação, derivadas | a fazer |
| **simpeg** | inversão geofísica (ERT, magnetometria, gravimetria) | a fazer |
| **pygimli** | modelagem e inversão, forte em eletrorresistividade | a fazer |

## Sensoriamento remoto

| Biblioteca | Para quê | Ficha |
|---|---|---|
| **earthengine-api / geemap** | Google Earth Engine: NDVI, séries temporais, mosaicos sem baixar imagem | a fazer |
| **sentinelhub / cdsetool** | download de Sentinel via Copernicus Data Space | a fazer |
| **scikit-learn** | classificação supervisionada quando não se usa o SCP | a fazer |

## Convenção de ambiente

- Um `venv` por projeto (regra global da máquina); `pip install` só
  dentro dele.
- Para scripts que rodam **dentro** do QGIS, usar o Python do próprio
  QGIS (`/Applications/QGIS-LTR.app/Contents/MacOS/bin/python3` no macOS)
  e instalar pacotes com ele, senão o plugin não enxerga.
