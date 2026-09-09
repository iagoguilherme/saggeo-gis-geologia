# Sec Interp (SecInterp)

> Monta uma seção geológica interativa no QGIS: perfil do MDE, afloramentos
> e atitudes projetados, furos de sondagem em 3D lançados na seção e desenho
> da interpretação com snapping.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | estrutural-perfis |
| **Autor(es)** | Juan M. Bernales (geociencio, independente) |
| **Licença** | GPL-2.0 OR GPL-3.0 (arquivo LICENSE dual; README cita GPL-3.0) |
| **Página oficial** | https://plugins.qgis.org/plugins/sec_interp/ |
| **Código-fonte** | https://github.com/geociencio/sec_interp |
| **Documentação** | https://geociencio.github.io/sec_interp_docs/ (Sphinx, inglês; a página inicial ainda mostra "2.9.0") |
| **Versão verificada** | 3.7.1 (2026-09-06) |
| **QGIS mínimo** | 3.0 no metadata; README pede 3.28 LTR+ e Python 3.10+ (máx. 4.99, compatível Qt6 desde a 3.6.0) |
| **Status** | ativo (repositório criado em 2025-11-27; 671 commits; ~30 versões em 9 meses) |
| **Verificado em** | 2026-09-08 |

## O que faz

- Extrai o perfil topográfico ao longo de uma linha de seção (banda do MDE
  escolhida; exagero vertical configurável).
- Projeta na seção os polígonos de geologia (campo de litologia) e os pontos
  estruturais (campos dip e dip direction) contidos num buffer lateral.
- Lança furos de sondagem em 3D (collar + survey + intervals) sobre a seção
  2D, com intervalos litológicos.
- Pré-visualização interativa com nível de detalhe adaptativo (LOD) e
  processamento paralelo; ferramenta de medição com snapping.
- Ferramenta de interpretação: desenha polígonos na seção herdando atributos
  (litologia) das camadas/furos vizinhos.
- Exporta SHP (PolygonZ 3D), CSV, DXF, PDF, SVG, PNG. Interface traduzida
  para 14 idiomas, inclusive português.

## Para que serve em geologia

- Seção hidrogeológica com projeção de poços existentes: collar (boca),
  survey (para poço vertical basta azimute 0 / inclinação −90) e intervals
  (perfil litológico) viram colunas na seção, junto do terreno e dos contatos
  aflorantes — é o desenho que se pede em laudo de locação e em outorga.
- Interpretar a base do manto de alteração / topo do embasamento entre
  poços, desenhando o polígono direto na seção com snapping nos furos.
- Comparar espessura saturada entre poços da mesma seção.
- Nenhum outro plugin verificado nesta categoria junta furos + interpretação
  desenhada; qProf projeta dados mas não permite desenhar a interpretação.

## Instalação

- Complementos → Gerenciar e Instalar Complementos → buscar "Sec Interp"
  (ou instalar o ZIP do GitHub).
- Sem dependência pip: usa só bibliotecas do QGIS/PyQt. Não precisa de
  PyVista, VTK ou OpenGL — o "3D" é projeção de furos e exportação PolygonZ,
  não visualização 3D.
- Recomendado QGIS 3.28 LTR ou superior; testado pelo autor em QGIS 4.x.

## Como usar (roteiro mínimo)

1. Preparar: MDE; camada de linha da seção com **uma única feição
   simples** (não multiparte); polígonos de geologia com campo de litologia;
   pontos estruturais com dip/dip direction; tabelas de furos (collar com ID,
   X, Y, Z; survey com profundidade, azimute, inclinação; intervals com
   de/até e litologia). Todos no mesmo SRC projetado.
2. Abrir Sec Interp → página DEM: raster e banda.
3. Página Seção: camada de linha e distância do buffer (ex.: 50 m).
4. Páginas Geologia / Estrutural / Furos: apontar camadas e campos.
5. "Preview": ajustar exagero vertical e LOD; usar medição e interpretação
   para desenhar os polígonos.
6. "Save" exporta SHP/CSV/DXF/PDF/SVG/PNG; "OK" só grava as configurações
   no projeto.

## Entradas e saídas

- **Entrada:** raster MDE; linha de seção (feição única); polígonos de
  geologia; pontos estruturais; furos em três camadas/tabelas (collar,
  survey, intervals).
- **Saída:** camadas de pré-visualização; shapefiles 3D (PolygonZ) da
  interpretação e dos furos; CSV; DXF; imagens PDF/SVG/PNG; configurações
  persistidas no projeto.

## Limitações e armadilhas

- Projeto muito novo, com um só desenvolvedor: 3 stars, 3 issues abertas
  (fev/2026: erro ao clicar em "interpret", cores, `AttributeError
  'NoneType' ... write` no logger ao ativar a interpretação) **sem resposta
  do autor** até 2026-09-08.
- Cadência de ~30 releases em 9 meses e changelog com métricas de
  "complexidade ciclomática", "cobertura de docstrings" e diretórios de
  ferramentas de IA excluídos do pacote: desenvolvimento fortemente
  assistido por IA. Validar resultados num caso conhecido antes de usar em
  laudo.
- Documentação publicada está defasada (índice em 2.9.0 contra 3.7.1 na
  loja).
- Seção deve ser linha simples; SRC precisa ser o mesmo em todas as camadas;
  desempenho cai com MDE denso e muitos furos (usar LOD).
- Não faz modelagem implícita entre seções nem estereograma.

## Alternativas

- **qProf:** projeção de atitudes por eixo de dobra e interseções, sem furos
  nem desenho; ver `qprof.md`.
- **Perfil de Elevação nativo (QGIS ≥ 3.26):** só topografia e vetores.
- **Geomodelr** (web), **GemPy** e **Loop3D/map2loop + LoopStructural**
  (Python; há plugin LoopStructural experimental, v0.1.12 de 2025-04-02):
  modelagem 3D implícita quando é preciso consistência entre várias seções.
- **Geoscience** (plugin QGIS de furos) — não verificado nesta rodada.

## Ver também

- `fluxos/` — seção hidrogeológica com poços projetados (a criar).
- `dados/` — cadastro de poços SIAGAS e geologia SGB como entrada.
