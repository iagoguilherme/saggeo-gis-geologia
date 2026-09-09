# QuickMapServices (NextGIS QuickMapServices)

> Adiciona mapas-base e serviços web (OSM, satélite, relevo, WMS/WFS) ao projeto
> em um clique, a partir de um catálogo aberto — resolve o "fundo" do mapa de
> localização de poço sem digitar URL de tile.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | mapeamento-relevo |
| **Autor(es)** | NextGIS |
| **Licença** | GPL-2.0 ou posterior |
| **Página oficial** | https://plugins.qgis.org/plugins/quick_map_services/ |
| **Código-fonte** | https://github.com/nextgis/quickmapservices |
| **Documentação** | https://nextgis.com/blog/quickmapservices/ · catálogo: https://qms.nextgis.com |
| **Versão verificada** | 1.5.1 (2026-09-03) |
| **QGIS mínimo** | 3.32 (máximo declarado 4.99; suporte a Qt6 desde a 0.20.0) |
| **Status** | ativo (5 releases em 2026; último commit 2026-09-03) |
| **Verificado em** | 2026-09-08 |

## O que faz

- Painel de busca conectado ao catálogo QMS (centenas de serviços públicos
  contribuídos pela comunidade): OSM, imagens de satélite, relevo sombreado,
  mapas temáticos, WMS/WFS governamentais.
- Adiciona a camada com um clique; suporta XYZ/TMS, WMS, WFS, GeoJSON e,
  desde a 1.5.0, *vector tiles*.
- Filtra serviços pela extensão atual da tela; guarda favoritos e recentes.
- "Contributed Pack" (65 fontes: `google_sat`, `google_hybrid`, `bing_sat`,
  `esri_satellite`, `esri_topo`, etc.) já vem integrado desde a 1.2.0 — não
  existe mais o passo "Settings → More services → Get contributed pack".
- Permite cadastrar serviços próprios (TMS/WMS) pelo editor interno.

## Para que serve em geologia

- Mapa de localização do poço/área de estudo com fundo de satélite ou OSM,
  sem configurar conexão XYZ manualmente.
- Conferência rápida de acesso (estradas, propriedades, drenagens) antes do
  campo, sobrepondo o ponto do poço e a bacia de contribuição.
- Fundo para mapa de situação em pranchas, relatórios de outorga e propostas.
- Relevo sombreado/topográfico (ESRI Terrain, OpenTopoMap) como contexto
  geomorfológico para interpretar lineamentos e vales.
- O que resolve que o QGIS puro não resolve: a descoberta do serviço (nome,
  URL, zoom máximo, atribuição) — o QGIS nativo exige que você já saiba a URL.

## Instalação

- Complementos → Gerenciar e Instalar Complementos → buscar "QuickMapServices"
  ou "NextGIS" (o plugin foi renomeado para **NextGIS QuickMapServices** na
  série 1.x, por isso aparece na letra N da lista).
- Sem dependências Python extras. Precisa de internet para o catálogo e para
  os tiles.
- Exige QGIS ≥ 3.32 desde a 1.3.2; em QGIS mais antigo o gerenciador mostra
  apenas versões 0.19.x/0.21.x.

## Como usar (roteiro mínimo)

1. Web → QuickMapServices → Search QMS (ou o ícone da lupa na barra Web).
2. Digite "satellite", "OSM" ou "topo"; a lista mostra serviços filtrados pela
   extensão da tela. Clique em *Add* no serviço desejado.
3. Ajuste o CRS do projeto (tiles nascem em EPSG:3857; o QGIS reprojeta na
   hora, mas fica mais rápido se o projeto estiver em 3857 ou em SIRGAS/UTM
   só com camadas leves por cima).
4. Coloque a camada do poço por cima, ajuste opacidade do fundo e use no
   Layout de impressão. Confira a atribuição exigida pelo serviço.

## Entradas e saídas

- **Entrada:** nenhuma camada; apenas a extensão da tela e a busca no catálogo.
- **Saída:** camada raster (XYZ/WMS) ou vetorial (WFS/GeoJSON/vector tiles)
  online, referenciada por URL no projeto — não baixa arquivo local.

## Limitações e armadilhas

- **Termos de uso:** os serviços do pack vêm "como estão". O README do
  `quickmapservices_contrib` avisa que as fontes "não são validadas e podem
  conter erros, serviços fora do ar e violações de licença — use por sua
  conta e risco". Google e Bing têm restrições para uso comercial, impressão
  e publicação; verifique antes de usar em relatório entregue a cliente.
  Para produto final, prefira OSM/ESRI com atribuição ou WMS oficiais.
- O catálogo mostra metadados e licença por serviço — leia antes de adicionar.
- A 1.5.1 removeu do pack os serviços que exigem chave de API (issue #278,
  Carto). Serviços com token continuam sem suporte nativo (issue #6, aberta
  desde 2015).
- Atribuição automática no mapa ainda é pedido em aberto (issue #58);
  o crédito da fonte precisa ser inserido manualmente no layout.
- Ao desinstalar, a pasta do contributed pack não é removida (issue #243).
- Dependência total de internet: sem conexão o projeto abre com as camadas
  em branco. Para trabalho offline, gere um MBTiles/GeoPackage com
  Processing → "Gerar tiles XYZ".
- O próprio NextGIS declara que o pacote `contrib` "deve se aposentar em
  breve" em favor do catálogo web; fontes só do pack podem sumir.

## Alternativas

- **XYZ Tiles nativo do QGIS** (painel Navegador → XYZ Tiles → Nova conexão):
  sem plugin, mas você precisa da URL e dos parâmetros; ideal para fixar uma
  fonte oficial no template de projeto.
- **HCMGIS** (26.8.27, 2026-08-27; QGIS ≥ 4.0; CC BY-SA 3.0): também traz
  basemaps Google/Carto/ESRI e conversores; mesmas ressalvas de licença.
- **WMS oficiais** (IBGE, SGB, ANA, órgãos estaduais) cadastrados diretamente
  na conexão WMS do QGIS: sem risco de licença para publicação.

## Ver também

- `fluxos/` — mapa de localização de poço (fundo QMS + ponto + bacia).
- `dados/` — fontes WMS oficiais brasileiras para substituir Google/Bing.
- Ficha `srtm-downloader.md` (relevo) e `qgis2threejs.md` (3D com fundo QMS
  como textura do terreno).
