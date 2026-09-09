# saggeo-gis-geologia

Repositório de **conhecimento GIS aplicado à geologia** — ferramentas,
fontes de dados, fluxos de trabalho e referências, com foco em
hidrogeologia, locação e documentação de poços tubulares no Brasil.

Aqui fica o que foi **verificado e testado**; ideias e rascunhos ficam em
`fluxos/` marcados como "a documentar" até virarem roteiro reproduzível.

## Mapa do repositório

| Seção | Onde | O que contém |
|---|---|---|
| **Ferramentas** | [ferramentas/README.md](ferramentas/README.md) | índice de tudo que é software, plugin ou biblioteca; uma ficha por ferramenta |
| ↳ Plugins QGIS | [ferramentas/qgis-plugins/README.md](ferramentas/qgis-plugins/README.md) | catálogo por categoria (relevo, estrutural/perfis, dados de campo, sensoriamento/geofísica) com status de manutenção |
| ↳ Software | [ferramentas/software/README.md](ferramentas/software/README.md) | QGIS, GRASS, SAGA, GDAL, QField, PostGIS, estereogramas, modelagem 3D |
| ↳ Python | [ferramentas/python/README.md](ferramentas/python/README.md) | bibliotecas geoespaciais e geológicas para automação fora do QGIS |
| **Dados** | [dados/README.md](dados/README.md) | fontes de dados geoespaciais e geocientíficas (SGB, IBGE, ANA, INPE, estaduais, globais) |
| **Fluxos** | [fluxos/README.md](fluxos/README.md) | roteiros passo a passo: do dado bruto ao produto (mapa de locação, seção, cota do poço…) |
| **Referências** | [referencias/README.md](referencias/README.md) | documentação, cursos, livros, comunidades |

## Convenções

- **Idioma:** português brasileiro em tudo (fichas, commits, comentários).
- **Uma ficha por ferramenta**, a partir de
  [ferramentas/_TEMPLATE.md](ferramentas/_TEMPLATE.md). Nome do arquivo em
  `kebab-case` (`point-sampling-tool.md`).
- **Nada sem verificação.** Cada ficha tem "Verificado em" e
  "Versão verificada". Campo que não foi conferido recebe `não verificado`,
  nunca um chute.
- **Status** (na tabela de cada ficha e nos índices):
  `ativo` · `manutenção esporádica` · `abandonado` · `experimental`.
- **Sistema de referência padrão:** SIRGAS 2000 (EPSG:4674 geográfico;
  EPSG:31982/31983 para UTM 22S/23S, que cobrem GO/DF/MG). Fluxos
  dizem explicitamente quando usam outro.
- **Dados brutos não entram no git.** Rasters, imagens de satélite e
  downloads grandes ficam em `dados/_local/` (ignorado). O repositório
  guarda o *como obter*, não o arquivo.

## Como usar

1. Precisa fazer algo? Comece por [fluxos/](fluxos/README.md).
2. Precisa de uma ferramenta? Vá ao índice em
   [ferramentas/](ferramentas/README.md) e leia a ficha antes de instalar
   (dependências, licença, armadilhas).
3. Precisa de dado? [dados/](dados/README.md) diz onde baixar e em que
   sistema de referência vem.
4. Quer adicionar algo? Leia [CONTRIBUINDO.md](CONTRIBUINDO.md).

## Origem

A primeira leva de plugins veio de uma curadoria publicada em rede social
("Plugins de Geologia para QGIS — selecionado por geólogo, para geólogos",
13 plugins em 4 categorias). Cada item foi conferido na
[plugins.qgis.org](https://plugins.qgis.org/) e no repositório de origem
antes de entrar aqui; divergências em relação ao post estão anotadas na
ficha e no índice de plugins.

## Relação com o ecossistema SAGGEO

- Padrões de simbologia e cores do perfil geológico:
  `~/Scripts/saggeo-padroes` (não duplicar aqui).
- Consulta de geologia/aquífero por ponto via serviços do SGB/SEMAD:
  `~/Scripts/SEMAD-SGB-GEOLOGIA-REST` e `saggeo-geobase-docs`.
- Este repositório é **conhecimento e método**; não contém código de
  produção nem credenciais.
