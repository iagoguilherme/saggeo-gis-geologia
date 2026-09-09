# Software

Programas de desktop e servidor que complementam o QGIS. Lista comentada;
cada item ganha ficha própria (`software/<nome>.md`) quando for testado
num fluxo real.

## Base GIS

| Software | Para quê | Licença | Ficha |
|---|---|---|---|
| **QGIS** (LTR) | plataforma principal de tudo neste repositório; usar sempre a versão LTR (Long Term Release) em produção | GPL-2.0+ | a fazer |
| **GDAL/OGR** | conversão, reprojeção, recorte e mosaico de raster/vetor em linha de comando; é o motor por baixo do QGIS | MIT | a fazer |
| **GRASS GIS** | hidrologia de MDE (bacia, drenagem, direção de fluxo), análise de terreno; acessível pelo Processing do QGIS | GPL-2.0+ | a fazer |
| **SAGA GIS** | morfometria, índices de terreno (TWI, curvatura), interpolação; acessível pelo Processing do QGIS | GPL-2.0+ / LGPL | a fazer |
| **PostGIS** | banco espacial para cadastro de poços com histórico e consultas por raio/bacia | GPL-2.0 | a fazer |
| **QField** | coleta em campo no Android/iOS sincronizada com o projeto QGIS (fichas de poço, fotos, GPS) | GPL-2.0+ | a fazer |
| **Mergin Maps** | sincronização de projetos QGIS ↔ campo, alternativa ao QFieldCloud | GPL-3.0 (cliente) / serviço pago | a fazer |

## Geologia estrutural e estereogramas

| Software | Para quê | Licença | Ficha |
|---|---|---|---|
| **Stereonet (Allmendinger)** | referência em estereogramas; análise de fraturas e cinemática | gratuito, fechado | a fazer |
| **OpenStereo** | estereogramas em Python, código aberto | GPL | a fazer |

## 3D e modelagem

| Software | Para quê | Licença | Ficha |
|---|---|---|---|
| **Blender + BlenderGIS** | terreno 3D com drapeamento de mapa geológico e poços; render para apresentação | GPL | a fazer |
| **CloudCompare** | nuvens de pontos (drone, LiDAR), MDS/MDT | GPL-2.0 | a fazer |
| **GemPy** | modelagem geológica 3D implícita em Python | EUPL / LGPL (ver versão) | a fazer |

## Pagos (só como referência de alternativa)

| Software | Para quê |
|---|---|
| **ArcGIS Pro** | equivalente comercial do QGIS; alguns clientes exigem formatos .aprx/.lyrx |
| **Surfer (Golden Software)** | gradeamento e mapas de contorno; muito usado em relatórios de geofísica |
| **Leapfrog Geo** | modelagem geológica 3D implícita; referência em mineração |
| **Oasis montaj (Seequent)** | processamento de dados geofísicos aéreos e terrestres |
| **Global Mapper** | conversão e visualização rápida de formatos |
