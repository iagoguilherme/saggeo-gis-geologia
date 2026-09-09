# SRTM-Downloader

> Baixa um MDE global (SRTM 30/90 m, Copernicus, ALOS, NASADEM…) recortado
> pela extensão da tela, via API do OpenTopography — o jeito mais rápido de
> ter relevo para bacia de contribuição, perfil e declividade ao redor do poço.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | mapeamento-relevo |
| **Autor(es)** | Dr. Horst Duester (hdus / Kappasys) |
| **Licença** | GPL-3.0 |
| **Página oficial** | https://plugins.qgis.org/plugins/SRTM-Downloader/ |
| **Código-fonte** | https://github.com/hdus/SRTM-Downloader |
| **Documentação** | https://github.com/hdus/SRTM-Downloader/wiki (praticamente vazia; README só com git clone) |
| **Versão verificada** | 4.0.4 (2026-08-03) para QGIS 4; 3.3.4 (2026-07-22) para QGIS 3 |
| **QGIS mínimo** | 4.0 na série 4.x; 3.0 a 3.99 na série 3.3.x (branch `qgis_3`) |
| **Status** | ativo, mantenedor único (último commit 2026-08-03; issues respondidas em 2026) |
| **Verificado em** | 2026-09-08 |

## O que faz

- Lê a extensão atual da tela do mapa e pede ao endpoint
  `portal.opentopography.org/API/globaldem` um GeoTIFF recortado.
- Produtos disponíveis no combo (código OpenTopography): SRTMGL1 (30 m),
  SRTMGL1_E (elipsoidal), SRTMGL3 (90 m), AW3D30 e AW3D30_E (ALOS), NASADEM,
  COP30 e COP90 (Copernicus DSM), EU_DTM, GEDI_L3 (1 km), GEBCOIceTopo e
  GEBCOSubIceTopo (batimetria 500 m).
- Salva `<PRODUTO>.tiff` na pasta escolhida e carrega no projeto.
- Guarda a chave de API e o último produto em `QSettings`
  (`/SRTM-Downloader/api_key`, `/SRTM-Downloader/dem`).
- **Atenção:** apesar do nome e da descrição ("Downloads SRTM Tiles from NASA
  Server"), desde a 3.3.0 (2025-12-07) o plugin **não usa mais o servidor da
  NASA nem login Earthdata**; a fonte é exclusivamente o OpenTopography.

## Para que serve em geologia

- MDE 30 m para delimitar bacia de contribuição de poço/nascente (r.watershed,
  GRASS/SAGA) e área de recarga em estudo de outorga.
- Perfil topográfico ao longo da seção hidrogeológica; cota aproximada da boca
  do poço quando não há nivelamento (com as ressalvas abaixo).
- Declividade, aspecto e relevo sombreado para interpretar lineamentos e
  zonas de fratura em aquíferos fissurais.
- Base de terreno para o Qgis2threejs (visualização 3D com furos).
- O que resolve que o QGIS puro não resolve: o QGIS não tem download de MDE
  embutido; sem plugin é preciso baixar tile a tile no Earthdata/INPE.

## Instalação

- Complementos → Gerenciar e Instalar Complementos → buscar "SRTM-Downloader".
  O gerenciador oferece 4.0.x em QGIS 4 e 3.3.x em QGIS 3.
- **Chave de API do OpenTopography (obrigatória):** criar conta gratuita em
  https://opentopography.org, abrir o painel *MyOpenTopo* → *Request an API
  key*. Sem chave o plugin simplesmente não envia a requisição
  (`if api_key:` no `downloader.py`).
- Em QGIS 4 a 4.0.4 declara `plugin_dependencies=qpip` para instalar
  `defusedxml`; se o QPIP faltar, cai em `xml.etree` (fallback da 4.0.3).
- Não há mais conta NASA Earthdata envolvida (era exigida até a 3.2.x).

## Como usar (roteiro mínimo)

1. Ajuste a tela do mapa para cobrir a área (poço + bacia). Áreas menores
   baixam mais rápido; a API limita cada pedido a 450.000 km² para SRTM GL1.
2. Raster → SRTM-Downloader (ou ícone na barra). Clique em *Set canvas
   extent* para preencher N/S/E/W.
3. Escolha o produto (SRTMGL1 para 30 m ortométrico; COP30 se quiser
   Copernicus), cole a chave de API e defina a pasta de saída.
4. *Download*: o GeoTIFF é gravado e adicionado ao projeto.
5. Reprojete para UTM/SIRGAS 2000 (Warp) antes de calcular declividade ou
   perfil — o dado vem em graus (EPSG:4326) com altitude em metros.

## Entradas e saídas

- **Entrada:** extensão em graus (WGS 84), produto e chave de API; nenhuma
  camada é necessária.
- **Saída:** um GeoTIFF por download (`SRTMGL1.tiff`, `COP30.tiff`…), em
  EPSG:4326, altitude em metros (EGM96 nos produtos ortométricos; variantes
  `_E` no elipsoide WGS 84).

## Limitações e armadilhas

- O arquivo é sobrescrito a cada download do mesmo produto na mesma pasta
  (nome fixo `<PRODUTO>.tiff`) — renomeie antes de baixar outra área.
- SRTM/Copernicus são modelos de superfície (radar): incluem dossel e
  edificações; erro vertical de metros. Não substitui nivelamento da boca
  do poço para cálculo de nível estático em cota absoluta.
- Não herda proxy das configurações do QGIS (issue #25, aberta desde 2022).
- Download que "roda por minutos" e gera TIFF corrompido já foi relatado
  (issue #37, 2025-12; não reproduzido pelo autor, resolveu sozinho).
- Erro "File not found on URL" (issue #38) = área/produto sem cobertura ou
  chave inválida; a mensagem de erro é o texto bruto do XML da API.
- Documentação inexistente: wiki só com "Welcome", README só com `git clone`.
- Versões 4.0.0–4.0.3 tiveram travamentos em QGIS 4 (issues #39, #40) —
  use 4.0.4 ou superior.
- Cotas/limites de uso da chave de API do OpenTopography: não verificado.

## Alternativas

- **OpenTopography DEM Downloader** (4.2, 2026-07-18; QGIS ≥ 3.16): mesma API
  e mesma chave, com produtos extras — inclui **ANADEM 30 m** (ANA, MDT
  brasileiro) — e roda no Processing. Preferível se você já usa modelos.
- **TOPODATA/INPE** (https://data.inpe.br/dados/topodata/): SRTM 90 m
  refinado para 30 m, com derivados (declividade, curvatura) já prontos para
  o Brasil; download manual por folha, sem chave.
- **Copernicus DEM 30 m** direto da ESA/AWS ou via OpenTopography, quando
  quiser um modelo mais recente que o SRTM (2000).
- **NASA Earthdata Search** (SRTMGL1 v3 em `.hgt`): download manual, exige
  conta Earthdata; útil quando a API estiver fora ou para lotes grandes.

## Ver também

- `fluxos/` — bacia de contribuição de poço a partir de MDE 30 m.
- `dados/` — TOPODATA, ANADEM e Copernicus DEM (fontes de relevo do Brasil).
- Ficha `qgis2threejs.md` (usa o MDE baixado aqui como terreno 3D).
