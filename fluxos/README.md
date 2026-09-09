# Fluxos de trabalho

Roteiros reproduzíveis, do dado bruto ao produto. Cada fluxo é um
arquivo `fluxos/<verbo-objeto>.md` com objetivo, entradas, ferramentas,
passos numerados, saída e armadilhas.

Um fluxo só sai de **a documentar** quando foi executado do início ao fim
e o resultado conferido.

| Fluxo | Objetivo | Ferramentas principais | Status |
|---|---|---|---|
| Mapa de localização de poço | mapa A4 com base cartográfica, coordenadas SIRGAS 2000 e grade UTM para relatório/outorga | QuickMapServices, layout de impressão do QGIS | a documentar |
| Extrair cota da boca do poço do MDE | obter altitude de N pontos a partir do SRTM/TOPODATA | Point Sampling Tool ou "Sample raster values", SRTM-Downloader | a documentar |
| Perfil topográfico ao longo de uma linha | perfil para seção hidrogeológica ou linha de sondagem elétrica | qProf, Profile Tool, Elevation Profile nativo | a documentar |
| Seção hidrogeológica com projeção de poços | seção com poços projetados, litologia e nível estático | qProf, sec_interp, dados SIAGAS | a documentar |
| Lineamentos e estereograma para aquífero fissural | mapear lineamentos em imagem Sentinel/MDE sombreado e plotar direções para locação | SCP, sombreamento do QGIS, Stereonet | a documentar |
| Georreferenciar mapa geológico escaneado | colocar mapa antigo (SGB/DNPM, relatório) sobre a base atual | Freehand Raster Georeferencer, Georeferencer nativo | a documentar |
| Bacia de contribuição e área de recarga | delimitar bacia a montante de um ponto a partir do MDE | GRASS r.watershed via Processing, SRTM | a documentar |
| Consultar geologia e aquífero num ponto | obter unidade geológica e domínio hidrogeológico por coordenada | WMS/WFS do GeoSGB, SIEG; repo SEMAD-SGB-GEOLOGIA-REST | a documentar |
| Terreno 3D com poços para apresentação | cena 3D navegável no navegador com MDE, imagem e poços | Qgis2threejs | a documentar |
| Plotar perfil litológico de poços a partir de tabela | gerar colunas litológicas em lote a partir de collar + intervalos | Geoscience, striplog | a documentar |
| Gráficos de hidroquímica e teste de bombeamento | dispersão, histogramas e séries a partir da tabela de atributos | Data Plotly | a documentar |

## Modelo de fluxo

```markdown
# Verbo + objeto

**Objetivo:** uma frase.
**Entradas:** o que precisa ter em mãos (formato, SRC).
**Ferramentas:** links para as fichas.
**Tempo estimado:** minutos.

## Passos
1. ...
2. ...

## Saída
O que se obtém, em que formato, e como conferir se está certo.

## Armadilhas
- ...
```
