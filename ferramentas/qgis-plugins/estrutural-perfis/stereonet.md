# Stereonet (plugin de Daniel Childs)

> Plota polos de planos (strike/dip ou dip direction/dip) em estereograma de
> igual área com contorno de densidade, direto de uma camada de pontos.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | estrutural-perfis |
| **Autor(es)** | Daniel Childs (independente) |
| **Licença** | GPL-2.0 (GitHub: "v2.0 or later") |
| **Página oficial** | https://plugins.qgis.org/plugins/qgis-stereonet/ |
| **Código-fonte** | https://github.com/childsd3/qgis-stereonet (último push 2020-03-16); espelho https://gitlab.com/dchilds/qgis-stereonet |
| **Documentação** | README do repositório |
| **Versão verificada** | 0.3 (2018-05-25) |
| **QGIS mínimo** | 3.0 (máx. 3.99 — **não instala no QGIS 4**) |
| **Status** | abandonado (sem release desde 2018, sem commit desde 2020) |
| **Verificado em** | 2026-09-08 |

## O que faz

- Lê uma camada de pontos (tipicamente CSV via "Texto Delimitado") com
  colunas `Strike` ou `DDR` (dip direction) e `Dip`.
- Plota os polos dos planos em estereograma de igual área (Schmidt,
  hemisfério inferior) com contorno de densidade pelo método de Kamb
  modificado.
- Usa somente as feições selecionadas, permitindo comparar domínios
  estruturais selecionando no mapa.
- Traz embutida a biblioteca `mplstereonet` (Joe Kington); a plotagem é
  feita com Matplotlib.

## Para que serve em geologia

- Em aquífero fissural, ver rapidamente a família dominante de fraturas de
  um afloramento ou de uma área (medidas de campo em CSV) e decidir a
  direção preferencial de lineamentos a cruzar na locação do poço.
- Comparar as fraturas medidas em afloramento com os lineamentos traçados
  em MDE/imagem para a mesma área.
- O QGIS puro não tem estereograma; qualquer estatística de orientação exige
  plugin ou script externo.

## Instalação

- Complementos → Gerenciar e Instalar Complementos → buscar "Stereonet"
  (autor Daniel Childs). Só aparece em QGIS 3.x.
- Dependências: Matplotlib e NumPy do Python do QGIS (OSGeo4W já traz;
  em Linux pode faltar `python3-matplotlib`). `mplstereonet` já vem dentro.
- Não há versão para QGIS 4 / Qt6 e não há sinal de que virá.

## Como usar (roteiro mínimo)

1. Preparar CSV com colunas `ddr` (ou `strike`) e `dip` em graus, mais X/Y.
   Nomes de coluna importam: o README exige `ddr` e `dip`; a página do
   plugin fala em `Strike`/`DDR` e `Dip`.
2. Camada → Adicionar camada de texto delimitado → carregar como pontos.
3. Selecionar no mapa os pontos do domínio de interesse.
4. Clicar no ícone do Stereonet → abre janela Matplotlib com polos e
   contornos.
5. Salvar a figura pelo próprio botão de salvar do Matplotlib (evitar os
   demais botões da barra — ver armadilhas).

## Entradas e saídas

- **Entrada:** camada de pontos com campos de strike ou dip direction e dip
  (graus); seleção ativa.
- **Saída:** janela com o estereograma (imagem salva via Matplotlib). Não
  gera camada nem tabela.

## Limitações e armadilhas

- **Sem QGIS 4:** `qgisMaximumVersion` 3.99; em QGIS 4 não aparece no
  gerenciador.
- Bugs documentados no próprio README: clicar em elementos do menu do
  Matplotlib derruba o QGIS; redimensionar a janela desalinha os marcadores
  de orientação.
- Não plota planos como grandes círculos nem lineações; não faz roseta,
  autovetores, girdle ou Fisher.
- Só polos + contorno; sem exportação de estatística.
- Nome ambíguo: há vários plugins com "stereonet" no nome (ver abaixo).

## Alternativas

Plugins QGIS (tag `stereonet`/`structural-geology`, verificados na
plugins.qgis.org em 2026-09-08):

- **Stereoplot** (`Stereoplot`; Julien Perret & Mark Jessell, UWA-CET; MIT;
  1.0.1 de 2026-09-01; QGIS 3.0–4.99, Qt6): sucessor declarado do plugin de
  Childs — polos, planos, contorno, girdle de melhor ajuste, roseta, filtro
  dinâmico. Exige NumPy < 2.0 e Matplotlib 3.7–< 3.12. **Escolha padrão hoje.**
  O fork intermediário `swaxi/qgis-stereonet` (WAXI/QField) aponta para ele.
- **GeoStereonet** (`geo_stereonet`; Shyam Mishra; 1.2.0 de 2026-07-17;
  QGIS 3.16–4.99): Schmidt/Wulff, Kamb, autovetores, Fisher, roseta, eixo de
  dobra; exporta PNG/SVG/PDF.
- **Structural Families Mapper** (`structural_families_mapper`; RSRC; 1.0.1
  de 2026-08-07): importa CSV/Excel, QA/QC, Rose/Schmidt/Wulff, seleção
  manual de famílias.
- **Dip-Strike Tools** (0.2.2, 2025-08-04): digitalização e gestão de
  atitudes; **qgSurf** (Alberti, 4.4.0 de 2026-08-19; 4.5.0 experimental p/
  QGIS 4): atitude a partir do MDE + estereograma.
- **GeoTrace** (1.34, 2023-01-20, experimental, QGIS ≤ 3.99) e **GeoTrace2**
  (0.1, 2023-04-18, experimental): lineamentos de raster → estereograma e
  roseta. `geocouche` (Alberti) só existe no GitHub — não verificado na
  plugins.qgis.org.

Fora do QGIS: `mplstereonet` (Python), Stereonet de Allmendinger (desktop,
gratuito), OpenStereo (Python/Qt, código aberto), Visible Geology (web,
didático).

## Ver também

- `fluxos/` — fraturas para locação em aquífero fissural (a criar).
- `dados/` — lineamentos SGB / medidas de campo.
