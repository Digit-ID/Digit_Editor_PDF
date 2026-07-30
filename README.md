# DigiPDF

Ferramenta de PDF do departamento de Investigação e Desenvolvimento da **Digit**.
Feita para substituir os sites gratuitos de PDF onde, hoje, os ficheiros dos clientes são carregados para servidores desconhecidos.

**Os teus ficheiros nunca saem deste dispositivo.**

---

## Privacidade — a regra que manda em tudo o resto

Todo o processamento acontece dentro do separador do navegador. Nenhum ficheiro, byte
ou metadado é enviado para lado nenhum:

* sem servidores e sem contas;
* sem APIs externas;
* sem analítica nem contadores de utilização;
* sem carregamentos, sem cópias, sem histórico;
* nada é gravado em disco a não ser aquilo que o utilizador manda descarregar.

As únicas duas coisas guardadas no navegador são a preferência de tema
(`digipdf_tema`) e a de idioma (`digipdf_lang`). Nada mais.

Depois de a página carregar, o DigiPDF funciona **sem ligação à internet** — todas as
bibliotecas estão dentro da pasta `vendor/`, com versões fixas, e nada é carregado de
CDN em tempo de execução. Ao fechar o separador, tudo desaparece da memória.

Isto é verificável: o ficheiro `index.html` não contém uma única chamada a `fetch`,
`XMLHttpRequest`, `WebSocket`, `EventSource` ou `navigator.sendBeacon`, nem um único
endereço `http://` ou `https://`. Numa sessão completa (abrir vários PDF, editar,
converter para Excel e Word, converter Word para PDF) o navegador faz exactamente 8
pedidos, todos ao próprio site: o `index.html` e os 7 ficheiros de `vendor/`.

---

## O que faz

### Editor de páginas
* Abrir vários PDF ao mesmo tempo — as páginas de todos aparecem numa grelha única,
  cada uma marcada com a cor do ficheiro de origem. É assim que se juntam documentos.
* Rodar −90°, +90° e 180°.
* Reordenar por arrasto, incluindo grupos de páginas selecionadas.
* Eliminar, duplicar, mandar para o início ou para o fim.
* Extrair a seleção para um PDF novo.
* Dividir por intervalos (`1-3, 7, 9-12`), a cada N páginas, ou uma página por
  ficheiro — vários resultados saem dentro de um ZIP.
* Selecção por clique, `Shift` (intervalo) e `Ctrl`/`Cmd` (uma a uma).
* Desfazer e refazer **sem limite de passos**.
* Atalhos: `Ctrl+A`, `Delete`, `[`, `]`, `Ctrl+S`, `Ctrl+Z`, `Ctrl+Shift+Z`, `Esc`.

### Conversões
* **PDF → Excel** — extrai o texto com as posições reais e deteta as colunas pelas
  coordenadas. Antes de exportar mostra uma pré-visualização **editável**: dá para
  corrigir células e excluir linhas. Converte números e datas para valores a sério.
* **PDF → Word** — o mesmo texto, exportado como `.docx` editável; as páginas
  reconhecidas como tabela saem como tabela.
* **Excel ou CSV → PDF** — tabelas paginadas com o cabeçalho repetido em todas as
  páginas, orientação e tamanho de folha à escolha, números alinhados à direita.
  Os CSV são descodificados explicitamente (UTF-8 com recurso a Windows-1252), para
  os acentos não saírem trocados.
* **Word → PDF** — parágrafos, títulos, listas, negrito, itálico e tabelas simples.

### Utilidades
* **Comprimir (forte)** — converte cada página em imagem JPEG, com controlo de
  qualidade e resolução, e mostra o tamanho antes e depois. Avisa em letra bem visível
  que o resultado deixa de ter texto; se o ficheiro ficar maior do que o original,
  diz-o e recomenda ficar com o original.
* **Marca de água** — texto em diagonal, com tamanho, opacidade, inclinação e cor.
* **Numerar páginas** — seis posições, três formatos, número inicial à escolha.
* **Dados do ficheiro** — mostra título, autor, assunto, palavras-chave, programa
  criador e datas, e grava uma cópia sem nada disso.

---

## O que **não** faz, e porquê

Nada de funções a meio gás. Estas ficaram deliberadamente de fora:

* **Não abre PDF protegidos por palavra-passe e não põe nem retira palavras-passe.**
  Fazer isto bem exige criptografia que não é seguro implementar deste lado; um
  ficheiro "protegido" mal feito dá uma falsa sensação de segurança. Quando aparece um
  PDF protegido, o DigiPDF explica-o em vez de falhar sem dizer nada.
* **Não faz reconhecimento de texto (OCR).** Num PDF digitalizado as páginas são
  imagens: não há texto para extrair. Nesse caso o DigiPDF avisa que não há nada para
  converter, em vez de devolver uma folha vazia. Se apenas algumas páginas forem
  imagens, diz quantas.
* **Não reproduz um `.docx` ao pixel.** A conversão Word → PDF é assumidamente
  aproximada e o aviso está no ecrã antes de converter: colunas, caixas de texto,
  cabeçalhos e rodapés, imagens posicionadas e tipos de letra do original não são
  reproduzidos.
* **Não abre `.doc` antigo** (só `.docx`).

Ficheiros acima de 50 MB podem esgotar a memória do navegador; o aviso aparece, mas a
tentativa é permitida.

---

## Estrutura

```
index.html                     aplicação completa, num único ficheiro
vendor/pdf.min.js              pdfjs-dist 3.11.174   (ler e desenhar PDF)
vendor/pdf.worker.min.js       pdfjs-dist 3.11.174   (Web Worker real)
vendor/pdf-lib.min.js          pdf-lib   1.17.1      (construir e alterar PDF)
vendor/jszip.min.js            jszip     3.10.1      (ZIP das divisões)
vendor/xlsx.full.min.js        xlsx      0.18.5      (ler e escrever Excel/CSV)
vendor/mammoth.browser.min.js  mammoth   1.6.0       (ler .docx)
vendor/docx.iife.js            docx      8.5.0       (escrever .docx)
```

Versões fixas, servidas localmente. Nenhuma é carregada de CDN.

---

## Publicar

O site é estático. Em GitHub Pages basta escolher o *branch* `main` e a pasta raiz.
Tem de ser servido por HTTP(S) — abrir o `index.html` diretamente do disco
(`file://`) também funciona, mas o navegador bloqueia o Web Worker do pdf.js e tudo
corre mais devagar, na linha principal.

---

## Interface

Três idiomas: português, inglês e espanhol, com tudo traduzido — incluindo as
mensagens de erro. Dois temas, claro e escuro. Desenho pensado também para telemóvel,
com barra de ações fixa em baixo e alvos de toque de 40 a 46 px.

---

Digit · Investigação e Desenvolvimento
