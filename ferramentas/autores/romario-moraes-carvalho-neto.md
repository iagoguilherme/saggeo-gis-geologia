# Romário Moraes Carvalho Neto (PlugGIS)

> Desenvolvedor de plugins QGIS sob medida para mineração, irrigação e
> análise ambiental. Publica demonstrações semanais; a maior parte do que
> mostra é paga, mas quatro plugins são gratuitos e estão na loja do QGIS.

| Campo | Valor |
|---|---|
| **Formação** | engenheiro florestal; mestre em Eng. Civil (Recursos Hídricos e Saneamento Ambiental); doutor pela UFSM e pela Università degli Studi di Padova (GitHub) |
| **Atuação** | "Desenvolvedor Python e GIS" na PlugGIS, Campinas-SP (LinkedIn); passagens por Embrapa e por consultoria ambiental/mineração (ZoomInfo, não verificado com o próprio) |
| **Não é geólogo** | o foco é automação de geoprocessamento; o conteúdo de geologia é de mineração/geotecnia (sondagem, cava, leito de rio), não de hidrogeologia |
| **LinkedIn** | https://www.linkedin.com/in/romariocarvalhoneto/ — 5.622 seguidores, verificado |
| **GitHub** | https://github.com/romariocarvalhoneto — 11 repositórios públicos |
| **Loja do QGIS** | https://plugins.qgis.org/plugins/author/Rom%C3%A1rio%20Moraes%20Carvalho%20Neto/ (lista só 1; os outros estão sob autoria conjunta com UFSM) |
| **Site / produtos** | https://pluggis.com.br/ — página de plugins: https://pluggis.com.br/plugins-e-scripts-qgis/ |
| **YouTube** | canal "PlugGIS", ~1.000 inscritos em ago/2026; handle não confirmado (os links ficam nos comentários dos posts) |
| **Contato público** | pluggis.tech@gmail.com (no site e no metadata do VRI Pivo) |
| **Avaliado em** | 2026-09-08 |

## O que ele publica

Doze publicações lidas na aba Atividades do LinkedIn (jul–set/2026), mais
duas antigas achadas por busca. Padrão:

- **Frequência:** cerca de uma por semana, quase sempre com vídeo curto do
  plugin rodando e o link completo "no primeiro comentário".
- **Temas (por ordem de frequência):** mineração (perfil de sondagem em
  escala e em 3D, fundo de cava irregular com volumetria, batimetria e
  volumes em leito de rio, pranchas automáticas de perfis); irrigação e
  terraplenagem (VRI Pivo, reservatório escavado com corte/aterro);
  análise ambiental (avaliação multicritério de locais, área de
  influência, relatório ambiental automatizado); opinião sobre IA e
  automação.
- **Modelo de negócio:** os plugins mostrados nos vídeos são vendidos sob
  consulta pela PlugGIS; ele usa os posts como vitrine. Só os quatro
  abaixo são gratuitos e de código aberto.

## Ferramentas que ele desenvolve

### Gratuitas, na loja do QGIS

| Plugin | Versão | QGIS | Licença | O que faz | Entra no catálogo? |
|---|---|---|---|---|---|
| **Coord. AttribuTable** | 0.1 (2020-07-22), experimental | 3.0–3.99 | GPL-2.0+ só no cabeçalho dos .py | coordenadas de pontos na tabela de atributos, inclusive GMS, no SRC escolhido | **sim** → [dados-campo/coord-attributable.md](../qgis-plugins/dados-campo/coord-attributable.md) |
| **WMCA – Weighted Multi-Criteria Analysis** | 0.4.2 (2023-02-07), experimental | 3.0–3.99 | GPL-2.0+ só no cabeçalho dos .py | pesos por raster e notas por classe → raster de favorabilidade/fragilidade | **sim** → [analise-hidrologia/wmca-analise-multicriterio.md](../qgis-plugins/analise-hidrologia/wmca-analise-multicriterio.md) |
| **BHCgeo** | 0.3 (2023-04-12), experimental | 3.0–3.99 | GPL-2.0+ só no cabeçalho dos .py | balanço hídrico climático de Thornthwaite-Mather pixel a pixel | **sim** → [analise-hidrologia/bhcgeo-balanco-hidrico.md](../qgis-plugins/analise-hidrologia/bhcgeo-balanco-hidrico.md) |
| **VRI Pivo** | 0.1, experimental (loja: "disponível no repositório público", ago/2026) | 3.0+ | não declarada | zonas de manejo para irrigação por pivô central | não: irrigação, fora do escopo de geologia |

### Só no GitHub

| Repositório | O que é | Entra? |
|---|---|---|
| `QgsTask_ProgressBar`, `QThread_ProgressBar` (2020) | exemplos de barra de progresso para quem desenvolve plugin | não como ficha; referência útil para o [python/README.md](../python/README.md) |
| `ortomosaicoKMZ` (2025-12) | script que converte ortomosaico KMZ em GeoTIFF | não; caso de uso raro, sem documentação |
| `Manejo` (2020) | manejo florestal sob linhas de transmissão | não; fora do escopo |
| `Evapotranspiration` (2020) | planilhas e docs de evapotranspiração | não; material de apoio do BHCgeo |
| `Raster_WMCA_sample` | 3 rasters de exemplo para o WMCA | citado na ficha do WMCA |
| `GeoAI_Plugin` | **fork** do plugin GeoAI (Segment Anything no QGIS) de Luis Eduardo Pérez Graterol, GPL-3.0; não é dele | o original é candidato para a categoria sensoriamento |

### Pagas ou sob consulta (PlugGIS)

Não entram no catálogo (critério: gratuita), mas ficam registradas como
referência de mercado e de ideias:

| Produto | Relação com o nosso trabalho |
|---|---|
| **Relatório Automatizado de Perfis de Sondagem 3D** | faz para sondagem mineral o que o gerador de prancha da SAGGEO faz para poço: perfil em escala A4 a partir de tabela, "de 40 para 10 minutos"; versão 2026 mostra furos e camadas em 3D e prepara interpolação e volumes. **Benchmark direto.** |
| **Plugin de batimetria** (perfis, volumes, pranchas automáticas, mapa de localização) | a montagem automática de pranchas com vários perfis por folha é a mesma necessidade das seções hidrogeológicas |
| **Projeto Tridimensional de Cava e Pilha**, fundo de cava irregular | volumetria em mineração; fora do escopo |
| **Projeto de Reservatório Escavado** (corte/aterro, 3D) | terraplenagem; fora do escopo |
| **Determinação da Margem de Leito Médio**, **Média do Pacote Sedimentar** (gratuito sob pedido) | mineração em leito de rio; fora do escopo |
| **Pranchas Fotográficas Automáticas** (gratuito sob pedido) | organiza fotos em pranchas de relatório; ideia aplicável ao relatório de campo de poço |
| **Área de Influência**, **Padronizar Shape Files**, **Ocorrências Ambientais com IA**, **QField + Relatório** | licenciamento ambiental; a padronização de shapes para órgão ambiental tem paralelo com os anexos de outorga |
| **Plugin Prefeituras**, **Banco de Dados RI Digital**, **Pomar** | fora do escopo |

## Avaliação

- **Qualidade técnica:** os plugins gratuitos são funcionais e simples,
  com interface própria e vídeo em português; o WMCA tem documento
  metodológico na UFSM. Nenhum tem testes, changelog ou arquivo LICENSE.
- **Manutenção:** os três que entraram não recebem commit desde 2020/2023
  e declaram QGIS máximo 3.99, portanto não aparecem no QGIS 4. Pela
  régua do repositório, status `abandonado`; o autor segue ativo, então
  uma atualização é plausível se houver demanda.
- **Bugs conhecidos (leitura do código, ver fichas):** WMCA tem a versão
  "estável" da loja quebrada no QGIS ≥ 3.22 (a corrigida é a experimental)
  e um PR de terceiro aberto desde 2022; BHCgeo monta caminhos com barra
  do Windows e não roda no macOS/Linux sem editar; Coord. AttribuTable
  grava GMS sem N/S/E/W e só exporta Shapefile.
- **Licença:** não há LICENSE nem campo `license` no metadata; só o
  cabeçalho padrão do Plugin Builder nos `.py` cita "GPL v2 or later".
  Para uso interno não muda nada; para redistribuir ou modificar, pedir
  confirmação ao autor.
- **Para a SAGGEO:** o valor maior não está nos plugins gratuitos, e sim
  no que ele mostra sobre **automação de entregáveis** (prancha de
  sondagem, pranchas de perfis, pranchas fotográficas). Vale assistir aos
  vídeos do plugin de sondagem antes de evoluir o gerador de prancha.

## Como acompanhar

- Seguir no LinkedIn e olhar a aba Atividades a cada mês; os links dos
  vídeos ficam no primeiro comentário de cada post.
- Conferir a loja do QGIS e o GitHub a cada seis meses para ver se saiu
  versão para o QGIS 4 dos três plugins com ficha.

## Não verificado

- Handle e URL do canal no YouTube (a página `@pluggis` não existe;
  a busca pública apontou um canal que é do QGIS Brasil).
- Dados de carreira do ZoomInfo (Embrapa, consultoria).
- Conteúdo dos comentários dos posts (links dos vídeos).
