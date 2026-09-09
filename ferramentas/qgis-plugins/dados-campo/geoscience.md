# Geoscience

> Plugin de furos de sondagem para o QGIS: faz o desurvey (collar + survey),
> gera o traço 3D de cada furo, posiciona dados de intervalo/pontuais ao longo
> do furo e corta seções verticais — o caminho mais direto para montar perfil
> litológico de poços tubulares e furos de pesquisa dentro do QGIS.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | dados-campo |
| **Autor(es)** | Roland Hill (Spatial Integration / Four Winds Technology) |
| **Licença** | GPL-3.0 (arquivo LICENSE do repositório; o `metadata.txt` não declara licença) |
| **Página oficial** | https://plugins.qgis.org/plugins/geoscience/ |
| **Código-fonte** | https://github.com/rolandhill/geoscience |
| **Documentação** | https://spatialintegration.com/docs/ (Drill Manager, Display, Sections) + README |
| **Versão verificada** | 2.0 (2026-08-16) para QGIS 4; última para QGIS 3: 1.17 (2025-04-08) |
| **QGIS mínimo** | 4.0 (v1.20 em diante); 3.2 (série 1.x até 1.17) |
| **Status** | ativo (5 releases em 2026, último commit 2026-08-16); README diz o contrário — ver "Limitações" |
| **Verificado em** | 2026-09-08 |

## O que faz

- **Desurvey**: calcula o traço 3D de cada furo a partir da tabela de collar
  e, opcionalmente, da de survey; sem survey usa azimute/dip do collar.
- **Dados de furo em planta**: projeta tabelas de intervalo (From/To) ou
  pontuais (Depth) sobre o traço como segmentos, pontos ou "discos"
  coloridos/dimensionados por atributo (litologia, teor, condutividade).
- **Seções verticais**: linha desenhada com o mouse ou ortogonal (W-E/S-N),
  com largura de janela; inclui collars, traços, dados de furo, vetores 3D
  e raster de elevação (MDE) como linha de superfície.
- **Estrutural**: converte alfa/beta de testemunho orientado em dip/dip direction.
- **Utilitários**: inverter sentido de linhas (simbologia de falhas),
  transparência em lote de rasters, grade local (WKT afim 2D/3D, inclusive
  em pés). v2.0 adiciona "Live map link" com o software Spatial Integration.

## Para que serve em geologia

- Perfil litológico/construtivo de poços tubulares: collar (ID, E, N, cota)
  + intervalos (ID, De, Até, litologia) → traço vertical e seção colorida.
  Para poço vertical o survey é dispensável (dip -90).
- Correlação entre poços numa seção: aquífero, topo do embasamento, alteração.
- Furos inclinados de pesquisa: desurvey com survey real e discos por teor.
- O QGIS puro não transforma (ID, From, To) em geometria 3D nem gera seção
  com furos + MDE em coordenadas (distância ao longo da seção × cota).

## Instalação

- Complementos → Gerenciar e Instalar Complementos → buscar "Geoscience".
  Sem dependências Python extras.
- **QGIS 4.x**: 2.0 (ou ≥ 1.20). **QGIS 3.x**: o gerenciador só oferece até
  a 1.17 (2025-04-08); as versões ≥ 1.20 não funcionam no QGIS 3.
- 1.20/1.21 no QGIS 4.0.1 não carregavam (módulo `desurveyhole_dialog`
  faltando no pacote, issue #26); corrigido na 1.22 (2026-04-16).

## Como usar (roteiro mínimo)

1. Reprojete tudo para **CRS projetado** (SIRGAS 2000 / UTM); lat/long é
   recusado no desurvey.
2. Carregue as tabelas como camadas (CSV, GPKG, SHP, TAB):
   - collar: obrigatório `ID`, `E`, `N`; opcional `Cota`, `Azimute`, `Dip`;
   - survey (opcional): `ID`, `Profundidade`, `Azimute`, `Dip`;
   - dados de furo: `ID`, `De`, `Até` (+ atributos) ou `ID`, `Profundidade`.
3. Geoscience → Drill → **Drill Manager**: aponte as camadas, mapeie os
   campos, confirme o sinal do dip e o comprimento de segmento. Sem cota no
   collar, use a opção de drapear a boca sobre um MDE.
4. **Desurvey** (ou "Force Desurvey" após editar collar/survey): gera traço
   (linha 3D) e dados de furo (segmentos/pontos com ponto médio) em disco.
5. **Create Section**: defina a largura, desenhe a linha ou informe
   coordenadas ortogonais, escolha as camadas (traços, dados, MDE de banda
   única). Sai num grupo próprio, em (distância na seção, cota real), com
   grades de E/N/Z e borda.
6. Estilize por categoria (litologia) ou graduado (teor).

## Entradas e saídas

- **Entrada:** camadas de collar, survey e dados de furo em CRS projetado;
  opcionalmente MDE (banda única) e vetores 3D para as seções.
- **Saída:** traços (LineStringZ), dados de furo (segmentos/pontos 3D),
  discos/estruturas em planta; seções em camadas de **memória**; WKT de CRS
  para grade local.

## Limitações e armadilhas

- Só coordenadas projetadas; azimutes devem ser de grade, coerentes com o CRS.
- **Seções são camadas de memória**: somem ao fechar o QGIS (regeneradas ao
  reabrir o projeto); exporte para GPKG se quiser arquivar.
- Collar vindo de banco (MSSQL/PostGIS) falha ao gravar saídas, pois o
  plugin grava ao lado do arquivo-fonte (issue #24, 2025-08).
- Issues abertas de 2026: `TypeError` quando o azimute vem como texto (#29),
  `AttributeError` com camada de desurvey nula (#28), "Unavailable Layer" ao
  reabrir projeto (#25). Use campos numéricos e não mova a pasta do collar.
- **README desatualizado**: diz "no longer in development due to funded
  OpenLog development by Oslandia", mas houve releases 1.20→2.0 entre
  2026-03 e 2026-08. Tratar como ativo, mantido por uma pessoa só.
- A documentação em spatialintegration.com mistura o plugin livre com o
  produto comercial ("Graph — Professional Edition only" não é o plugin).
- Sem algoritmos de Processing: tudo via diálogo, sem uso em modelos.

## Alternativas

- **OpenLog** (Apeiron/Oslandia, v1.9.2 de 2026-08-03, QGIS ≥ 3.40):
  striplog, seção e 3D sincronizados, conecta a bancos de furos; mais
  completo, mas com versão Premium paga e dependências próprias.
- **Midvatten** (v1.8.3, 2026-02-19, QGIS ≥ 3.34.6): hidrogeologia (níveis,
  estratigrafia, Piper) sobre Spatialite; melhor para monitoramento do que
  para desurvey.
- **Geoscience Section Vertical Exaggeration** (Nicola Kern, v0.1): exagero
  vertical em seções já criadas pelo Geoscience.
- Leapfrog, Micromine, Datamine (comerciais): para modelo 3D implícito.
- "GeoPackage drillhole" e "Drill Hole Viewer": não verificado (ausentes das
  tags drillhole/borehole da plugins.qgis.org em 2026-09-08).

## Ver também

- `fluxos/README.md` — fluxos de perfil litológico e seção entre poços (a criar).
- `dados/README.md` — fontes de MDE para cota da boca do poço.
- `ferramentas/qgis-plugins/dados-campo/point-sampling-tool.md` — cota do MDE para o collar.
- `ferramentas/qgis-plugins/dados-campo/data-plotly.md` — gráficos dos atributos dos intervalos.
