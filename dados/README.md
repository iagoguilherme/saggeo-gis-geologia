# Fontes de dados

Onde obter dado geoespacial e geocientífico para trabalhos de geologia e
hidrogeologia. O repositório guarda **como obter**; o arquivo baixado fica
em `dados/_local/` (ignorado pelo git).

Sistema de referência oficial no Brasil: **SIRGAS 2000** (EPSG:4674).
Muita fonte antiga ainda vem em SAD69 ou Córrego Alegre — conferir sempre
o `.prj` ou o metadado antes de sobrepor.

## Geologia e hidrogeologia (nacional)

| Fonte | Órgão | O que tem | Acesso | Link |
|---|---|---|---|---|
| **GeoSGB** | SGB/CPRM | mapas geológicos (1:1M a 1:100k), hidrogeologia, recursos minerais, geofísica aérea, geoquímica | download, WMS, WFS | https://geosgb.sgb.gov.br/ |
| **SIAGAS** | SGB/CPRM | cadastro nacional de poços (perfil, NE, vazão, litologia) | consulta web, download por área | https://siagasweb.sgb.gov.br/ |
| **Mapa Hidrogeológico do Brasil ao Milionésimo** | SGB/CPRM | domínios e subdomínios hidrogeológicos, produtividade | download via GeoSGB | ver GeoSGB |
| **RIMAS** | SGB/CPRM | rede de monitoramento de águas subterrâneas (nível, série temporal) | consulta web | http://rimasweb.sgb.gov.br/ |
| **SNIRH / Hidroweb** | ANA | chuva, vazão, cotas de rios; outorgas; bacias hidrográficas (ottobacias) | download, API | https://www.snirh.gov.br/hidroweb/ |
| **Base Hidrográfica Ottocodificada (BHO)** | ANA | trechos de drenagem e ottobacias | download | https://metadados.snirh.gov.br/ |

## Base cartográfica e relevo

| Fonte | Órgão | O que tem | Acesso | Link |
|---|---|---|---|---|
| **IBGE Geociências** | IBGE | malhas municipais, cartas topográficas, base contínua (BC250/BC100), nomes geográficos | download, WMS | https://www.ibge.gov.br/geociencias/ |
| **INDE** | governo federal | catálogo de metadados e serviços de dezenas de órgãos | catálogo, WMS/WFS | https://inde.gov.br/ |
| **TOPODATA** | INPE | SRTM 30 m refinado para o Brasil, com derivados (declividade, orientação, curvaturas, formas de terreno) | COG na plataforma BIG (WMS/STAC) ou download por folha 1:250k; o portal antigo `dsr.inpe.br/topodata` fica fora do ar com frequência | https://data.inpe.br/dados/topodata/ |
| **SRTM / NASADEM** | NASA/USGS | MDE global 30 m | Earthdata Search (login) para download manual; o plugin SRTM-Downloader (≥ 3.3) baixa via API do OpenTopography com chave gratuita, sem conta Earthdata | https://earthdata.nasa.gov/ |
| **Copernicus DEM (GLO-30)** | ESA | MDE global 30 m, mais recente que o SRTM | Copernicus Data Space / OpenTopography | https://dataspace.copernicus.eu/ |
| **OpenTopography** | OpenTopography | agregador de MDEs globais e LiDAR; API com chave gratuita | API, download | https://opentopography.org/ |

## Imagens de satélite e uso do solo

| Fonte | Órgão | O que tem | Acesso | Link |
|---|---|---|---|---|
| **Copernicus Data Space** | ESA | Sentinel-1/2/3; conta gratuita | web, API, plugin SCP | https://dataspace.copernicus.eu/ |
| **USGS EarthExplorer** | USGS | Landsat 4–9, ASTER, declassified | web (login) | https://earthexplorer.usgs.gov/ |
| **MapBiomas** | rede MapBiomas | uso e cobertura do solo anual desde 1985, 30 m | download, GEE | https://brasil.mapbiomas.org/ |
| **INPE Catálogo de Imagens** | INPE | CBERS-4/4A, Amazonia-1, Landsat histórico | web | http://www.dgi.inpe.br/catalogo/ |
| **Google Earth Engine** | Google | acesso a quase tudo acima sem download; conta gratuita para não comercial | API/JS/Python | https://earthengine.google.com/ |

## Estaduais e distritais (Centro-Oeste)

| Fonte | Órgão | O que tem | Acesso | Link |
|---|---|---|---|---|
| **SIEG** | IMB (Instituto Mauro Borges) / Goiás | geologia, hidrogeologia, solos, hidrografia, limites municipais, KML/KMZ, geosserviços do estado de Goiás | download por tema, WMS/WFS; o domínio `sieg.go.gov.br` estava fora do ar em 2026-09-08, usar a página do IMB | https://goias.gov.br/imb/sieg-downloads/ |
| **Geoportal DF** | SEDUH-DF | base cartográfica do DF, lotes, hidrografia, geologia, solos | web, WMS/WFS; certificado TLS autoassinado (navegador avisa) | https://www.geoportal.seduh.df.gov.br/ |
| **SICAR** | governo federal | Cadastro Ambiental Rural: limites de imóveis rurais, APP, reserva legal | download por município | https://www.car.gov.br/ |

Detalhes de acesso a serviços do SGB e da SEMAD por API estão no repo
`~/Scripts/SEMAD-SGB-GEOLOGIA-REST` e em `~/Scripts/saggeo-geobase-docs`.

## Regras de uso

- **Termos de uso de mapas base comerciais** (Google, Bing, ESRI via
  QuickMapServices): servem para visualização; em produto entregue ao
  cliente, preferir OpenStreetMap, IBGE ou imagem licenciada.
- **Citar a fonte** em todo mapa: órgão, produto, escala, ano.
- **Conferir datum** antes de sobrepor qualquer dado antigo.
