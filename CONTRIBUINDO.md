# Como contribuir

## Adicionar uma ferramenta (plugin, software ou biblioteca)

1. Copie [ferramentas/_TEMPLATE.md](ferramentas/_TEMPLATE.md) para a pasta
   certa:
   - plugin QGIS → `ferramentas/qgis-plugins/<categoria>/<nome>.md`
   - software de desktop/servidor → `ferramentas/software/<nome>.md`
   - biblioteca Python → `ferramentas/python/<nome>.md`
2. Nome do arquivo em `kebab-case`, sem acento, sem maiúscula.
3. Preencha **todos** os campos da tabela. O que não foi conferido recebe
   `não verificado`.
4. Verifique de verdade:
   - página do plugin em `https://plugins.qgis.org/plugins/<slug>/`
     (versão, data, autor, licença, QGIS mínimo, experimental?);
   - repositório de código (último commit, issues abertas relevantes);
   - instale e rode pelo menos um caso de uso antes de marcar `ativo`.
5. Adicione uma linha no índice da categoria
   (`ferramentas/qgis-plugins/README.md` ou o README da pasta) e, se for
   ferramenta nova de outra família, em `ferramentas/README.md`.
6. Commit em português, no formato
   `ferramentas: adiciona ficha do <nome>` ou
   `ferramentas: atualiza <nome> para vX.Y (status)`.

## Avaliar um autor ou curador

Perfil em `ferramentas/autores/<nome>.md` seguindo o roteiro de
[ferramentas/autores/README.md](ferramentas/autores/README.md): fontes,
tabela de ferramentas (gratuita/paga, versão, licença, entra ou não e por
quê), avaliação, como acompanhar, o que não foi verificado. As ferramentas
que entram ganham ficha normal na categoria certa.

## Adicionar uma fonte de dados

Uma linha na tabela de [dados/README.md](dados/README.md) com: nome,
órgão, o que tem, formato/serviço (download, WMS, WFS, API), sistema de
referência, licença/termos e link. Fontes com detalhes de acesso (chave,
filtros, quirks) ganham arquivo próprio em `dados/<fonte>.md`.

## Adicionar um fluxo

Um arquivo `fluxos/<verbo-objeto>.md` (ex.: `extrair-cota-poco-mde.md`)
com: objetivo, entradas, ferramentas usadas (links para as fichas),
passo a passo numerado, saída esperada, tempo estimado e armadilhas.
Um fluxo só sai de "a documentar" quando foi executado do início ao fim
e o resultado conferido.

## Categorias de plugins QGIS

| Pasta | Escopo |
|---|---|
| `mapeamento-relevo` | mapas base, MDE, visualização de terreno e 3D |
| `estrutural-perfis` | perfis topográficos, seções geológicas, estereogramas, projeção de furos |
| `dados-campo` | furos/poços, amostras, gráficos, extração de valores para pontos |
| `sensoriamento-geofisica` | imagens de satélite, classificação, geofísica, georreferenciamento |
| `analise-hidrologia` | análise multicritério, balanço hídrico, recarga, bacias, interpolação |

Se uma ferramenta cabe em duas, fica na que corresponde ao uso principal
e a outra categoria a menciona em "Ver também".

## O que NÃO entra

- Dados brutos (rasters, imagens, shapefiles grandes) — só o como obter.
- Credenciais, chaves de API, tokens — nunca, nem em exemplo.
- Código de produção do ecossistema SAGGEO — fica nos repos dos serviços.
