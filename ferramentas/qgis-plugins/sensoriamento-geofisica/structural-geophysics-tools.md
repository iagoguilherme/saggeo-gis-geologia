# "Structural Geophysics Tools" → SGTool (Structural Geophysics Tool)

> **Aviso:** não existe plugin chamado "Structural Geophysics Tools" na plugins.qgis.org nem no GitHub (verificado em 2026-09-08 nas tags `geophysics` e `structural-geology` e por busca). O nome do post quase certamente se refere ao **SGTool**, cujo README se intitula "Structural Geophysics Tool" (Mark Jessell, feito para o curso WAXI/Agate de Geofísica Estrutural). Esta ficha documenta o SGTool e, no fim, lista os outros nomes candidatos, separando plugin QGIS de software/biblioteca externa.

> SGTool: filtros de campo potencial (RTP, 1VD, sinal analítico, tilt, continuação, Euler, worms) sobre grids aeromagnetométricos e gravimétricos dentro do QGIS; serve para realçar lineamentos magnéticos e unidades gamaespectrométricas nos dados abertos do SGB.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS (há versão irmã para ArcGIS Pro) |
| **Categoria** | sensoriamento-geofisica |
| **Autor(es)** | Mark Jessell (University of Western Australia / Loop3D); contribuições de Gordon Cooper, Felipe F. Melo & Valéria C. F. Barbosa, Frank Horowitz; rotinas convertidas do SAGA-GIS |
| **Licença** | MIT |
| **Página oficial** | https://plugins.qgis.org/plugins/sgtool/ |
| **Código-fonte** | https://github.com/swaxi/SGTool |
| **Documentação** | README do repositório (não há manual separado) |
| **Versão verificada** | 0.3.7 (2026-09-02) |
| **QGIS mínimo** | 3.24 (compatível até 4.x) |
| **Status** | ativo (repo criado em nov/2024; 491 commits; último em 2026-09-02) |
| **Verificado em** | 2026-09-08 |

## O que faz

- Filtros de campo potencial por FFT: redução ao polo (RTP) e ao equador (RTE), RTP variável, continuação para cima/baixo, integração vertical, passa-alta/baixa/banda (inclusive direcional e Butterworth), remoção de regional de 1ª/2ª ordem, controle automático de ganho, espectro de potência radial.
- Derivadas e realces: derivada vertical (1VD), derivadas direcionais, gradiente horizontal total, sinal analítico, tilt angle.
- Filtros espaciais e estatística em janela (média, mediana, gaussiano, direcional; desvio, variância, curtose), anisotropia local, MRRTF/MRVBF (do SAGA).
- Deconvolução de Euler, PCA/ICA de vários grids, gridding B-spline de XYZ/CSV, extração de "worms" multiescala (bsdwormer).
- Lê GeoTIFF, GRD do Oasis montaj (com CRS do `.xml`), ERS, Noddy, CSV/XYZ; grava GeoTIFF. Calcula IGRF pelo centróide do grid e pela data do voo.

## Para que serve em geologia

- Realçar lineamentos magnéticos (diques, falhas, zonas de cisalhamento) nos grids aeromagnetométricos que o SGB distribui pelo GeoSGB. Em terreno cristalino, cruzados com lineamentos de relevo/imagem, orientam a locação de poços em aquífero fissural.
- Aplicar 1VD, tilt e gradiente horizontal na gamaespectrometria (K, eTh, eU, contagem total) para separar unidades litológicas e manto de intemperismo (potencial de aquífero raso).
- Continuação para cima para separar anomalia rasa de profunda; Euler para estimar profundidade de fontes (embasamento sob cobertura sedimentar).
- O QGIS puro não tem RTP, continuação, sinal analítico nem leitura direta de GRD do Oasis montaj.

## Instalação

- Complementos → Gerenciar e Instalar → "SGTool".
- Dependências obrigatórias: scipy, pyproj ≥ 3.7.2, networkx, shapely, geopandas. Opcionais: matplotlib (espectro radial), scikit-learn (PCA, ICA, worms). Instalar com o pip do QGIS (OSGeo4W Shell no Windows).

## Como usar (roteiro mínimo) — lineamentos magnéticos a partir do GeoSGB

1. Baixar do GeoSGB (aerogeofísica) o grid de campo magnético anômalo do projeto que cobre a área, em GRD ou XYZ; conferir datum/projeção antes de sobrepor.
2. SGTool → carregar o grid; informar inclinação/declinação ou usar IGRF com a data do levantamento; aplicar RTP.
3. Sobre o RTP: 1VD e tilt angle; opcionalmente continuação para cima (500–1000 m) para ver o regional.
4. Estilizar o tilt (−90° a +90°) com paleta divergente; digitalizar lineamentos ou rodar "worms" e exportar como linhas.
5. Cruzar com lineamentos de relevo/NDVI e com poços do SIAGAS; plotar direções em estereograma/roseta.

## Entradas e saídas

- **Entrada:** raster de campo potencial (GeoTIFF/GRD/ERS/Noddy) ou XYZ/CSV para gridar; parâmetros do campo (inclinação, declinação, data).
- **Saída:** um GeoTIFF por operação; vetores de worms; gráfico de espectro.

## Limitações e armadilhas

- Unidades de comprimento vêm do CRS: grid em lat/long exige comprimentos em graus nos filtros — reprojetar para UTM antes.
- O próprio README marca o espectro de potência radial como "precisa de teste" e diz que os cálculos foram escritos com auxílio de ChatGPT/Claude: conferir resultado contra software de referência em trabalho decisivo.
- Sobrescrever grid aberto em outro programa falha; QGIS no Windows às vezes não salva fora de C:.
- Não faz inversão nem modelagem 2D/3D: para isso, tomofast_x_q + Tomofast-x, SimPEG ou PyGMI.
- Projeto jovem, sem manual: a interface muda entre versões 0.3.x.

## Outros nomes da lista — o que é plugin QGIS e o que não é

| Nome pesquisado | Resultado (2026-09-08) |
|---|---|
| "Structural Geophysics Tools" | não existe; corresponde ao SGTool |
| "Structural Geology Toolbox" | não existe; a tag `structural-geology` tem Dip-Strike Tools, GeolAttitude, LoopStructural, qgSurf, Sec Interp, Stereoplot e Structural Families Mapper |
| "Geophysics" / "GeophysicsTools" | só github.com/arthurHamel/Geophysics-tools: 4 commits, esqueleto do Plugin Builder, nunca publicado — abandonado |
| "Potential Field Toolbox" | não encontrado como plugin QGIS |
| "GravMag" | não é plugin QGIS; existem `gravmag` (biblioteca Python, birocoles) e GravMagSuite (MATLAB) |
| "QGIS Geophysics Plugin" | não encontrado com esse nome |
| **tomofast_x_q** | plugin QGIS real (Jessell & Ogarko), 0.2.14 (2026-06-05), QGIS 3.24–4.x; prepara entrada para o Tomofast-x (inversão grav/mag, instalado à parte) |
| **GRD_Loader** | plugin QGIS experimental (Loop3D), 0.1.4 (2025-11-24); lê GRD do Oasis montaj — o SGTool já faz isso |
| **Geosoft GRD Loader** | plugin QGIS (Shyam Mishra), 1.2.0 (2026-08-23); mesma função |
| **AGT – Archaeological Geophysics Toolbox** | plugin QGIS (INRAP), 3.1.5 (2024-02-15), só QGIS 3; processa dados de magnetômetro/EM/resistivímetro terrestre (Bartington, Geonics EM31, GEM2) — útil em levantamento de pequena área |
| **GeoTrace / GeoTrace2** | plugins QGIS experimentais (Grose/Thiele), 2023, só QGIS 3; extração de traços e orientações em imagem, MDE e geofísica |
| **Geoscience** | plugin QGIS (Roland Hill), 2.0 (2026-08-16), só QGIS 4; furos de sondagem e seções — não processa geofísica |
| Fatiando a Terra / Harmonica | biblioteca Python (v0.7.0, 2024-08); processamento e modelagem grav/mag fora do QGIS |
| SimPEG | biblioteca Python de simulação e inversão (grav, mag, DC/IP, EM); licença não verificada |
| PyGMI | software standalone com GUI (Council for Geoscience, África do Sul; GPL-3.0; docs v3.3.0) para mag, grav e sensoriamento |
| ResIPy | software standalone com GUI para inversão de eletrorresistividade/IP (ERT); licença não verificada |
| Oasis montaj | software comercial (Seequent), referência da indústria; pago |

## Alternativas

- **Oasis montaj** (pago) quando o cliente exige o padrão da indústria.
- **PyGMI** para fluxo completo gratuito com interface gráfica fora do QGIS.
- **Harmonica / SimPEG** para automação e inversão em Python.
- **GeoSGB** já entrega mapas temáticos derivados (imagens/PDF) para muitos projetos aerogeofísicos — antes de processar, ver se o produto pronto resolve.

## Ver também

- Fluxo "Lineamentos e estereograma para aquífero fissural" (`fluxos/README.md`, a documentar).
- `dados/README.md` → GeoSGB (geofísica aérea, regra de conferir datum).
