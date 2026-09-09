# Profile Tool

> Traça perfis de terreno a partir de raster (ou pontos com cota) ao longo de
> uma linha e exporta o gráfico e a polilinha 3D — o clássico "perfil rápido"
> do QGIS.

| Campo | Valor |
|---|---|
| **Tipo** | plugin QGIS |
| **Categoria** | estrutural-perfis |
| **Autor(es)** | Borys Jurgiel, Patrice Verchere, Etienne Tourigny, Javier Becerra; mantido por PANOimagen S.L. (colaborador ativo: nicogodet) |
| **Licença** | GPL-2.0-or-later |
| **Página oficial** | https://plugins.qgis.org/plugins/profiletool/ |
| **Código-fonte** | https://github.com/PANOimagen/profiletool |
| **Documentação** | README do repositório (não há manual separado) |
| **Versão verificada** | 4.3.4 (2026-03-19) |
| **QGIS mínimo** | 3.40 (máx. 4.99; `supportsQt6=True`) |
| **Status** | manutenção esporádica — issue #103 "No more development" (2025-02-05): só correções simples e PRs de terceiros |
| **Verificado em** | 2026-09-08 |

## O que faz

- Desenha perfil de elevação ao longo de linha digitalizada no canvas ou de
  feições de uma camada de linhas (uma, várias ou todas).
- Aceita como fonte raster (MDE), camada de pontos com campo de cota e
  camadas de malha (`QgsMeshLayer`, desde a 4.1.6).
- Mostra perfil de altura e perfil de declividade; várias curvas no mesmo
  gráfico; atualização ao vivo pode ser desligada (4.3.0).
- Exporta gráfico em SVG, PDF, PNG; dados em CSV; polilinha 3D em DXF.
- Traz PyQtGraph 0.13.7 embutido; usa Matplotlib como motor opcional se ela
  estiver instalada.

## Para que serve em geologia

- Perfil topográfico rápido da linha de caminhamento elétrico ou da fila de
  SEVs, para corrigir e apresentar a pseudo-seção com o relevo real.
- Cota do terreno na boca de poços ao longo de um alinhamento (camada de
  pontos com cota), para nivelar níveis estáticos de vários poços numa
  mesma seção.
- Verificar o desnível entre o poço e a captação/reservatório para
  dimensionar recalque.
- O QGIS já faz tudo isso nativamente (Perfil de Elevação, ≥ 3.26); o que
  sobra de vantagem no Profile Tool é a exportação direta em DXF e CSV com
  dois cliques e a interface mais simples.

## Instalação

- Complementos → Gerenciar e Instalar Complementos → buscar "Profile tool".
- Exige QGIS 3.40 ou superior (versões antigas do plugin, 4.2.x, cobrem
  QGIS 3.x mais antigos, mas não recebem correção).
- Sem dependência pip: PyQtGraph vem dentro do pacote; Matplotlib é
  opcional (só para quem preferir esse motor de gráfico).
- Funciona no QGIS 4 / Qt6 desde a 4.3.3–4.3.4.

## Como usar (roteiro mínimo)

1. Carregar o MDE (SRC projetado) e, se for o caso, a camada de linhas da
   seção.
2. Abrir o Profile Tool (barra de ferramentas) → "Add Layer" → escolher o
   raster/pontos/malha.
3. Escolher o modo: "Temporary polyline" (clicar no mapa, duplo clique para
   fechar) ou "Selected polyline"/"Selected layer" para usar feições
   existentes.
4. Conferir o gráfico; alternar entre altura e declividade se necessário.
5. Aba "Table"/"Export": salvar CSV (distância × cota), imagem (SVG/PDF/PNG)
   ou DXF 3D para o desenho da seção.

## Entradas e saídas

- **Entrada:** raster de elevação; camada de pontos com campo numérico de
  cota; camada de malha; linha desenhada ou feição de camada de linhas.
- **Saída:** gráfico (SVG, PDF, PNG); CSV com distância e cota; DXF com
  polilinha 2D/3D do perfil.

## Limitações e armadilhas

- **Projeto em modo de sobrevida:** o mantenedor declarou (issue #103) que
  não corrigirá bugs nem adicionará funções além das próprias necessidades,
  porque o QGIS tem perfil nativo.
- Issue #105 (aberta, QGIS 3.40.5): DXF exportado é recusado pelo AutoCAD /
  TrueView ("Improper table entry name RASTER-0.200"). Testar o DXF no
  destino antes de depender dele.
- Issue #95: QGIS 3.28.9/3.32.1 travava ao ativar o plugin; fechada como
  "not planned". Versões antigas do plugin misturadas com QGIS novo dão
  crash — manter ambos atualizados.
- Histórico de fragilidade com bibliotecas: 4.2.1 reverteu o PyQtGraph para
  manter Qt 5.11; 4.2.6 corrigiu NumPy ≥ 1.24; 4.3.0 migrou para Qt6. Em
  Linux com Matplotlib do sistema, o motor Matplotlib pode falhar — usar
  PyQtGraph.
- Não projeta atitudes nem contatos geológicos: é só topografia.

## Alternativas

- **Perfil de Elevação nativo (QGIS ≥ 3.26):** substitui na maior parte dos
  casos; exporta PDF/imagem e, em versões recentes, dados.
- **qProf:** perfil + projeção de dados geológicos; ver `qprof.md`.
- **VoGIS-ProfilTool** (BergWerk GIS, 3.0.2 de 2019-01-07, QGIS 3.4–3.99):
  perfis em lote a partir de geometrias; parado e sem QGIS 4.
- **Sec Interp:** seção interativa com furos; ver `sec-interp.md`.

## Ver também

- `fluxos/` — perfil topográfico de linha de geofísica (a criar).
- `dados/` — MDE de referência.
