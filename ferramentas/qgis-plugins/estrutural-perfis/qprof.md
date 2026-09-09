# qProf

> Gera perfis topográficos a partir de MDE ou GPX e projeta neles atitudes,
> traços e contatos geológicos — a base de uma seção geológica no QGIS.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | estrutural-perfis |
| **Autor(es)** | Mauro Alberti, Marco Zanieri (independentes) |
| **Licença** | GPL-3.0 |
| **Página oficial** | https://plugins.qgis.org/plugins/qProf/ |
| **Código-fonte** | https://gitlab.com/mauroalberti/qProf (ativo); https://github.com/mauroalberti/qProf (parado em 2021, v0.4.4) |
| **Documentação** | ajuda embutida no plugin (`help/help.html`, também em https://github.com/mauroalberti/qProf/blob/master/help/help.html) |
| **Versão verificada** | 0.5.0 estável (2022-11-07); 0.5.2 experimental p/ QGIS 3 (2026-05-31); 0.6.3 experimental p/ QGIS 4 (2026-05-26) |
| **QGIS mínimo** | 3.0 (série 0.5.x, máx. 3.99); 4.0 (série 0.6.x, máx. 4.99) |
| **Status** | manutenção esporádica (última atividade no GitLab em 2026-05-26; issues sem resposta desde 2023) |
| **Verificado em** | 2026-09-08 |

## O que faz

- Extrai o perfil topográfico de um ou mais MDE (raster) ou de arquivo GPX,
  ao longo de linha digitalizada no canvas, de camada de linhas ou de
  coordenadas digitadas; calcula também perfil de declividade.
- Projeta atitudes geológicas (camada de pontos com ID, direção de mergulho
  ou strike RHR, e mergulho) sobre o perfil por três métodos: interseção
  mais próxima (perpendicular), eixo comum ou eixo individual (trend/plunge).
- Projeta traços geológicos (linhas) ao longo de um eixo de dobra definido
  pelo usuário.
- Intersecta o perfil com camadas de linhas (falhas) e polígonos (unidades
  aflorantes), usando campos ID e de classificação para simbologia.
- Exporta o gráfico em PDF/SVG/TIFF e os dados em shapefile/CSV no SRC do
  projeto.

## Para que serve em geologia

- Montar a seção geológica de referência para locação de poço: perfil do
  terreno + contatos aflorantes + atitudes projetadas indicam profundidade
  esperada de um contato ou de uma zona de falha sob o ponto de perfuração.
- Perfil topográfico ao longo de linhas de caminhamento elétrico ou de
  perfil de SEVs, para correção topográfica e apresentação em relatório.
- Em aquífero fissural, projetar lineamentos/falhas mapeados sobre o perfil e
  ver onde interceptam a superfície ao longo da linha de poços.
- O QGIS puro (Perfil de Elevação, ≥ 3.26) faz só a topografia; a projeção de
  atitudes por eixo e a interseção de polígonos/linhas são o diferencial.

## Instalação

- Complementos → Gerenciar e Instalar Complementos → buscar "qProf".
- No QGIS 3.x instala-se a 0.5.0 (estável). As 0.5.2 e 0.6.3 exigem marcar
  "Mostrar complementos experimentais".
- Não há dependência pip: o plugin traz suas bibliotecas geométricas
  embutidas. Usa NumPy/GDAL já presentes no QGIS.
- QGIS 4 (Qt6): somente a série 0.6.x, ainda experimental.

## Como usar (roteiro mínimo)

1. Carregar MDE (SRC projetado, em metros) e camada de linha da seção com
   apenas 2 vértices (A–B).
2. Abrir qProf → aba de perfil topográfico → escolher o MDE e a linha;
   definir a distância de densificação compatível com a resolução do MDE;
   gerar o perfil.
3. Aba de projeção: apontar camada de pontos com campos ID, dip direction
   (ou strike RHR) e dip; escolher o método (perpendicular / eixo comum /
   eixo individual). Selecionar os pontos desejados ou marcar "todos".
4. Aba de interseção: adicionar polígonos da geologia e linhas de falha, com
   os campos ID e de classificação.
5. Exportar gráfico (PDF/SVG) e os pontos projetados (shapefile/CSV) para
   desenhar a seção final em CAD/LaTeX.

## Entradas e saídas

- **Entrada:** raster MDE (um ou mais) ou GPX; linha da seção (canvas,
  camada ou coordenadas); pontos de atitude (ID, dip dir/strike, dip);
  linhas (falhas, traços); polígonos (geologia).
- **Saída:** gráfico do perfil (PDF, SVG, TIFF); shapefile/CSV com os pontos
  projetados e as interseções; perfil de declividade.

## Limitações e armadilhas

- Dados geológicos só são projetados em perfis de **2 pontos**; linhas com
  vários vértices ou GPX geram apenas o perfil topográfico.
- Só os pontos **selecionados** são projetados, a menos que se marque a
  opção de projetar toda a camada.
- Issue #19 (2025-07, QGIS 3.40.8): `AttributeError ... get_elevation_range`
  na 0.5.0, sem resposta do autor. Issue #18: intervalo de amostragem varia
  de forma inesperada; #17: comportamento estranho na interseção de
  polígonos na 0.5. Testar antes de confiar em um relatório.
- O repositório do GitHub (apontado como homepage na plugins.qgis.org) está
  parado em 2021; o código atual está no GitLab.
- O gráfico é estático: não há edição interativa da seção nem desenho de
  interpretação.

## Alternativas

- **Perfil de Elevação nativo do QGIS (≥ 3.26):** topografia, vetores
  extrudados, nuvem de pontos; sem projeção de atitudes.
- **Profile Tool** (`profiletool`): perfil simples com exportação DXF; ver
  `profile-tool.md`.
- **Sec Interp** (`sec_interp`): seção interativa com furos e desenho de
  interpretação; ver `sec-interp.md`.
- **qgSurf** (Alberti): atitude de superfícies a partir do MDE, interseção
  plano × topografia; complementa o qProf.
- **Geomodelr, GemPy, Loop3D/map2loop:** modelagem 3D implícita, fora do
  QGIS, quando várias seções precisam ser consistentes entre si.

## Ver também

- `fluxos/` — seção geológica para locação de poço (a criar).
- `dados/` — MDE (Copernicus/TOPODATA) e geologia SGB usados como entrada.
