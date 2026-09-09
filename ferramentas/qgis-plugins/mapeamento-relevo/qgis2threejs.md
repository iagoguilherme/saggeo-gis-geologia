# Qgis2threejs

> Transforma MDE + camadas vetoriais em cena 3D interativa (three.js) que roda
> no navegador, e exporta glTF — serve para mostrar ao cliente o terreno com
> os poços, a bacia e a seção sem precisar de software 3D.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | mapeamento-relevo |
| **Autor(es)** | Minoru Akagi (minorua / akaginch) |
| **Licença** | GPL-3.0 |
| **Página oficial** | https://plugins.qgis.org/plugins/Qgis2threejs/ |
| **Código-fonte** | https://github.com/minorua/Qgis2threejs |
| **Documentação** | https://minorua.github.io/Qgis2threejs/docs/ (fonte na branch `docs`) |
| **Versão verificada** | 3.2.1 (2026-09-07) para QGIS 4; 2.10.4 (2026-09-07) para QGIS 3 |
| **QGIS mínimo** | 4.0 na série 3.x; 3.4 na série 2.10.x (máximo 4.99) |
| **Status** | ativo (2.632 commits; releases 3.2/3.2.1 e 2.10.3/2.10.4 em ago–set/2026) |
| **Verificado em** | 2026-09-08 |

## O que faz

- Gera terreno 3D a partir de raster monobanda (GDAL) ou plano horizontal,
  com textura do canvas do QGIS, de camadas escolhidas ou de imagem própria.
- Cria objetos 3D de vetores por atributo: pontos → esfera, cilindro, cone,
  caixa, disco, plano, billboard ou **modelo 3D (COLLADA .dae, glTF/.glb)**;
  linhas → linha, tubo (*pipe*), cone, caixa, **parede (*wall*)**;
  polígonos → polígono, **extrusão**, sobreposição (*overlay*) sobre o MDE.
- Exporta para web (HTML + JS autocontidos, publicáveis em GitHub Pages ou
  Netlify), para glTF/glb (Blender, impressão 3D) e, desde a 3.2, para JSON
  do three.js.
- Animações por keyframe de câmera, transição de opacidade e troca de textura;
  modo narrativo e marcador AR.
- Salva as configurações em `.qto3settings` ao lado do projeto `.qgz`.

## Para que serve em geologia

- Maquete virtual da área: MDE (SRTM/Copernicus) + poços como cilindros com
  altura proporcional à profundidade (valor negativo = abaixo do terreno).
- Traços de furos/sondagens como *pipe* em linhas 3D (Z por vértice) —
  ver issue #348 para as armadilhas de CRS e Z.
- Traço de seção hidrogeológica como *wall* descendo até uma cota base;
  unidades geológicas como polígonos extrudados ou *overlay* colorido.
- Bacia de contribuição e área de recarga em *overlay* semitransparente
  sobre o relevo, para reunião com cliente ou órgão de outorga.
- glTF para levar o terreno com os poços ao Blender ou imprimir em 3D.
- O que resolve que o QGIS puro não resolve: a vista 3D nativa não exporta
  para web nem para glTF (só `.obj` e imagem).

## Instalação

- Complementos → Gerenciar e Instalar Complementos → buscar "Qgis2threejs".
  Em QGIS 4 instala-se a 3.2.x; em QGIS 3.4–3.44 a 2.10.x.
- **Pré-visualização dentro do plugin exige Qt WebEngine:**
  - QGIS 4: `python3-pyqt6-webengine` vem no OSGeo4W e nos pacotes Ubuntu;
    se faltar, o menu mostra apenas "Qgis2threejs Exporter (No preview)".
  - QGIS 3 (série 2.10): WebEngine funciona a partir do QGIS 3.36 com
    PyQt5 WebEngine ≥ 5.15.6 (o do Ubuntu 22.04 é antigo demais); senão
    cai no QWebView/WebKit legado.
  - macOS: builds oficiais podem trazer PyQt5 desatualizado; o WebEngine
    não aparecia em Mac M4 (issue #376, fechada no marco 3.2).
- Desde a 3.2 a cena pode ser pré-visualizada em navegador externo mesmo sem
  WebEngineView — contorno oficial para Mac e Linux problemáticos.
- Navegador com WebGL/GPU para ver o resultado (three.js); GPUs sem a
  extensão `ANGLE_instanced_arrays` falham (issue #352).

## Como usar (roteiro mínimo)

1. Carregue o MDE (ex.: `SRTMGL1.tiff` do SRTM-Downloader) e mude o **CRS do
   projeto para um sistema projetado** (UTM/SIRGAS ou EPSG:3857): o plugin
   usa a unidade horizontal do projeto e o Z em metros — em graus o relevo
   fica achatado.
2. Estilize o MDE e as camadas (poços, bacia, seção) como quer que apareçam
   na textura; dê zoom para que o canvas cubra a área.
3. Web → Qgis2threejs → Qgis2threejs Exporter. Marque o MDE no grupo *DEM*;
   marque os poços e escolha *Cylinder* com altura/raio por expressão.
4. Scene → Scene Settings: extensão base, exagero vertical, fundo, luz.
5. File → Export to Web… (marque *Enable the Viewer to Run Locally* para
   abrir o HTML sem servidor) ou File → Save Scene As → glTF.

## Entradas e saídas

- **Entrada:** raster monobanda via provedor GDAL (MDE); pontos/linhas/
  polígonos (2D com Z por atributo/expressão, ou 3D); imagens de textura;
  modelos .dae/.glb para símbolos.
- **Saída:** pasta com `index.html` + JS/dados; arquivo `.gltf`/`.glb`;
  JSON three.js; `.qto3settings` com a configuração.

## Limitações e armadilhas

- Raster multibanda e provedores não-GDAL (WMS, XYZ) não servem como MDE —
  use-os apenas como textura.
- "Do not clip DEM" carrega o raster inteiro na memória; MDE grande trava o
  navegador. Prefira recortar pela extensão base ou por polígono.
- Textura TIFF não abre em vários navegadores; use PNG/JPEG/WebP.
- Relatos de eixos Y/Z trocados na exportação glTF/glb (#360, #428) —
  confira o modelo no Blender antes de entregar.
- Suporte a nuvem de pontos é legado (Potree 1.6, sem COPC); o changelog da
  3.1 diz que foi removido, mas a documentação 3.2 ainda descreve o menu —
  estado exato não verificado.
- Modelos `.dae/.glb` locais são copiados na exportação, mas as texturas
  associadas precisam ser copiadas à mão; `.qto3settings` de versão mais
  nova pode não abrir em versão antiga (File → Clear export settings).
- 188 issues abertas; problemas de preview em Mac/Linux são recorrentes.

## Alternativas

- **Vista 3D nativa do QGIS** (View → New 3D Map View): terreno do MDE,
  extrusão, animação por keyframe e export `.obj`; sem export web/glTF.
  Suficiente para captura de tela em relatório.
- **BlenderGIS** (GPL-3.0; Blender ≥ 2.83; último commit 2025-12-20): importa
  MDE/SRTM, shapefile e basemaps direto no Blender para render fotorrealista
  ou animação; curva de aprendizado maior. Para 3D Tiles/Cesium não há
  plugin equivalente verificado nesta ficha.

## Ver também

- `fluxos/` — maquete 3D de área de poços (MDE + poços + bacia + seção).
- Fichas `srtm-downloader.md` (MDE de entrada) e `quickmapservices.md`
  (satélite como textura); `dados/` — MDE (TOPODATA, ANADEM, Copernicus).
