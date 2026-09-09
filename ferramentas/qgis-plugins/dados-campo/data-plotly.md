# Data Plotly

> Painel de gráficos interativos (Plotly/D3) dentro do QGIS, ligado à tela do
> mapa: dispersão, histograma, box/violin, barras, pizza, ternário, polar,
> contorno — para explorar hidroquímica, vazões, níveis e atributos de poços
> sem sair do projeto.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | dados-campo |
| **Autor(es)** | Matteo Ghetta (Faunalia) |
| **Licença** | GPL-2.0 (arquivo LICENSE do repositório; o `metadata.txt` não declara licença) |
| **Página oficial** | https://plugins.qgis.org/plugins/DataPlotly/ |
| **Código-fonte** | https://github.com/ghtmtt/DataPlotly |
| **Documentação** | https://dataplotly-docs.readthedocs.io/en/latest/ |
| **Versão verificada** | 4.5.1 (2026-06-24) |
| **QGIS mínimo** | 3.28 (QGIS 4 suportado desde a 4.5.0, 2026-04-09) |
| **Status** | ativo (último commit 2026-07-07; releases regulares; 64 issues abertas) |
| **Verificado em** | 2026-09-08 |

## O que faz

- Painel acoplável com scatter (pontos/linhas, interpolação e linhas
  preenchidas desde a 4.4.0), box, violin, barras empilhadas, histograma,
  pizza, histograma 2D, polar, ternário e contorno.
- Gráfico **ligado ao mapa**: selecionar no gráfico seleciona feições na
  camada e vice-versa; opção "usar só feições selecionadas".
- Campos ou **expressões QGIS** em X/Y/cor/tamanho; sobreposição de vários
  gráficos ou subplots em linhas/colunas.
- Exporta PNG, HTML interativo e JSON da configuração; item de gráfico no
  **layout de impressão**.
- Provedor de **Processing** ("Build a generic plot" e variantes) que gera
  HTML + JSON em lote ou em modelos.

## Para que serve em geologia

- Hidroquímica exploratória: STD × condutividade, Cl × Na, pH ×
  alcalinidade, cor por aquífero, seleção cruzada no mapa. Piper e Stiff
  **não** existem aqui; o ternário serve para composição relativa (Ca–Mg–Na+K).
- Distribuição de vazão, capacidade específica e profundidade por município
  ou unidade geológica (histograma, box plot por categoria).
- Rebaixamento × tempo e nível estático × data em poços de monitoramento.
- QA de cadastro: outliers de cota/profundidade/coordenada aparecem no
  gráfico e são localizados no mapa com um clique.
- No QGIS 3.x não há gráfico estatístico interativo ligado à seleção; o item
  nativo do QGIS 4.0 é para layout, não para exploração.

## Instalação

- Complementos → Gerenciar e Instalar Complementos → buscar "Data Plotly".
- **Pacote Python `plotly`** é importado pelo núcleo (`core/plot_factory.py`
  usa `plotly.graph_objs` e `plotly.offline.plot`). Vem no instalador
  Windows/OSGeo4W; no Linux `python3-plotly`; no macOS testar `import plotly`.
- **plotly.js embutido** (1.52.2 e 3.0.1 + polyfill em `jsscripts/`):
  funciona offline, sem chave de API.
- **Motor web**: QGIS 3 exige QtWebKit (`python3-pyqt5.qtwebkit`; ausente em
  alguns builds, ex. Flatpak e macOS 3.44.6, issue #394). QGIS 4/Qt6 usa
  QtWebEngine (PRs #416/#421, desde a 4.5.0).
- **`pandas`** só para os algoritmos de Processing.

## Como usar (roteiro mínimo)

1. Abra o painel (barra de ferramentas ou Complementos → DataPlotly).
2. Escolha o tipo, a camada (ex.: poços com análises) e os campos ou
   expressões de X, Y, cor e tamanho; marque "apenas selecionadas" se quiser.
3. **Create Plot**; ajuste título, eixos (log/linear), cores e marcadores e
   clique **Update Plot**.
4. Clique/arraste no gráfico para selecionar feições no mapa e na tabela.
5. Para comparar aquíferos, crie vários gráficos e use subplots; alguns tipos
   não aceitam sobreposição — o painel avisa.
6. Exporte PNG/HTML, salve o JSON para reutilizar, ou insira o item
   DataPlotly no layout de impressão do relatório.

## Entradas e saídas

- **Entrada:** camada vetorial com atributos numéricos/categóricos (poços,
  tabela sem geometria, polígonos); campos ou expressões.
- **Saída:** gráfico interativo no painel; PNG; HTML autônomo; JSON da
  configuração; item de layout; via Processing, arquivos HTML + JSON.

## Limitações e armadilhas

- Se QtWebKit (QGIS 3) não existir no build, o plugin nem carrega ("No
  module named PyQt5.QtWebKit", issues #47, #171, #394). Instale o pacote
  ou migre para QGIS 4.
- Sem Piper, Stiff, Schoeller ou stereonet: não é ferramenta de
  hidroquímica especializada.
- Renderização via página web: dezenas de milhares de feições ficam lentas.
- Bugs abertos: travamento com `get_symbol_colors()` ao reabrir projeto
  (#335); scatter 3D derruba o QGIS (#28, desde 2017).
- Nulos tratados desde a 4.4.0; em versões antigas quebravam histograma.
- Em QGIS sem tela (servidor) só o Processing funciona, e gera HTML, não PNG.
- A descrição na plugins.qgis.org fala em "interface for QGIS Server": é só
  o sinalizador `server=True` do metadata, sem função relevante em desktop.

## Alternativas

- **Chart Item nativo do layout (QGIS 4.0+)**: barras, linha e pizza por
  campos/expressões, com filtro por atlas — para relatório; não interativo.
- **Advanced Charts** (Faustino Cetraro, v1.2.0 de 2026-08-30, QGIS ≥ 4.0,
  Matplotlib): linha, dispersão, barras, histograma, box, violin, radar,
  área, dois eixos Y, regressão — sem ligação com a seleção do mapa.
- **Midvatten** (v1.8.3): diagrama de Piper e séries de nível para dados
  hidrogeológicos em Spatialite.
- **Python (matplotlib/plotly)** no console ou fora do QGIS: Piper/Stiff e
  figuras de relatório com controle total.

## Ver também

- `fluxos/README.md` — fluxo de QA de cadastro de poços e hidroquímica (a criar).
- `dados/README.md` — fontes de análises químicas e séries de nível.
- `ferramentas/qgis-plugins/dados-campo/point-sampling-tool.md` — valores de raster para os poços antes de plotar.
- `ferramentas/qgis-plugins/dados-campo/geoscience.md` — atributos de intervalos de furo.
