# Dra. Vera L. Bortoluzzi — Parapsicologia Clínica

Projeto de presença digital para Vera Lúcia Bortoluzzi, parapsicóloga clínica em
Balneário Camboriú/SC.

## Arquivos

| Arquivo | Público | O que é |
|---|---|---|
| `index.html` | Cliente final | Site institucional (~1,8 MB — imagens e vídeo embutidos como data URI) |
| `loja.html` | Cliente final | Loja: 19 itens em 5 segmentos, filtro por segmento, checkout por WhatsApp |
| `marketing.html` | Interno / pitch | Plano de tráfego: funil, canais, verba, calendário de 90 dias, modelo comercial |
| `proposta.html` | Interno / pitch | Proposta comercial: diagnóstico, fases, escada de produtos, projeção |

HTML autocontido — sem build, sem dependência além das fontes do Google.

## ⚠️ Antes de subir em domínio próprio

Os links entre `index.html` e `loja.html` apontam hoje para URLs de artifact,
para a apresentação. Ao hospedar os arquivos lado a lado, converter para
relativo:

```sh
sed -i 's#https://claude.ai/code/artifact/a1e9d29e-d5fd-465f-ab2c-bd2552b9deac#loja.html#g' index.html
sed -i 's#https://claude.ai/code/artifact/c8cb0df3-e405-4958-bc8c-75bc9345f098#index.html#g' loja.html
```

`marketing.html` e `proposta.html` não são páginas públicas — não linkar do site.

## Pendências

- [x] ~~Confirmar o WhatsApp.~~ Confirmado pelo Kaleb: (47) 99935-8040 —
      `5547999358040` nos 26 links.
- [ ] Fotos profissionais da Dra. Vera e do consultório — item de maior impacto
      em conversão e o único que não dá para produzir sem ela.
- [ ] Aval dela sobre os 19 preços da loja (hoje são sugestão de mercado).
- [ ] Horário de atendimento, para o site, o Google Meu Negócio e as respostas
      automáticas do WhatsApp.

## Direção visual

Claro e premium, tema único: alabastro quente `#F8F6F2`, tinta aubergine
`#1B1721`, acento em ouro velho `#9A7739`. Cormorant Garamond nos títulos,
Jost no corpo. O espectro cromoterápico fica reservado às especialidades e à
legenda das cores.

Sistema de design compartilhado entre as três páginas públicas — os tokens e os
componentes de base (nav, botões, rodapé, revelação no scroll) são idênticos,
o que mantém a identidade coesa.

A fotografia do hero e o vídeo de cromoterapia são frames extraídos dos vídeos
que a própria cliente enviou: cristais sob luz cromática, o equipamento das
sessões presenciais.

## Decisões de conteúdo

- **Tratamento "Dra." aplicado**, por decisão do Kaleb. Mitigação adotada: o
  título aparece sempre junto das seis formações reais dela, que é o que o
  sustenta caso alguém questione. O risco registrado é que o título sugere
  registro em conselho de saúde que a parapsicologia não possui.
- **Sem promessa de cura ou tratamento de doença.** Categoria sensível para
  Google Ads, CONAR e CDC — o `marketing.html` traz a seção de risco completa.
  Site e loja têm aviso legal explícito de terapia complementar no rodapé.
- **Física quântica** aparece como modelo de autoconhecimento, não como
  mecanismo de cura — mesma razão.
- **Modelo comercial:** o plano recomenda base fixa + percentual sobre receita
  digital, em vez do valor fechado de R$ 20 mil/mês condicionado a "ficar
  famosa", que não tem critério objetivo e costuma gerar disputa no 3º mês.

## Fontes

- Blog da cliente: https://veraparapsicologa.blogspot.com/ (posts de 2009)
- Perfil do Google Meu Negócio (endereço, telefone fixo, Hotel Sibara)
- Vídeos e áudios enviados por WhatsApp

## Novos arquivos (setembro)

| Arquivo | Público | O que é |
|---|---|---|
| `dossie.html` + PDF | **Dra. Vera** | 12 capítulos, 16 páginas. O documento principal para ela ler |
| `Planilha-...xlsx` | **Dra. Vera** | 6 abas para ela preencher: preços, acessos, ideias, agenda |

### Loja pronta para anúncio

- Open Graph e Twitter Card (preview do link no WhatsApp, Instagram, TikTok)
- JSON-LD: `HealthAndBeautyBusiness` + `ItemList` com os 19 produtos
- Blocos de pixel comentados: Meta, TikTok, GA4 — descomentar e trocar os IDs
- Mapa `CHECKOUT` no JS: colar a URL da Hotmart/Kiwify ao lado do `data-pid`
  do produto e o botão passa a levar ao pagamento em vez do WhatsApp
- Captura de `utm_*`, `fbclid`, `ttclid`, `gclid` em sessionStorage, repassada
  ao checkout e à mensagem do WhatsApp — é o que permite saber qual anúncio
  gerou a venda
- Eventos disparados no clique: `InitiateCheckout` / `Contact` para os três pixels

**Falta:** subir uma imagem 1200x630 e trocar a URL do `og:image`. Sem ela o
link compartilhado aparece sem imagem e converte muito menos.

### Instagram

Perfil existente: https://www.instagram.com/vera.bortoluzzi/ — ligado no site e
na loja. **Não criar outro:** perfil antigo tem histórico e isso conta a favor.

### Geração dos arquivos

O PDF sai do `dossie.html` via Chromium headless. As fontes estão embutidas em
base64 no HTML porque o Chromium do container não alcança o Google Fonts —
sem isso o PDF sai com fonte de sistema.

```sh
/opt/pw-browsers/chromium-1194/chrome-linux/chrome --headless --no-sandbox \
  --virtual-time-budget=15000 --no-pdf-header-footer \
  --print-to-pdf="Dossie-Dra-Vera-Bortoluzzi.pdf" "file://$PWD/dossie.html"
```

## Pasta `deploy/` — pronta para hospedar

`index.html`, `loja.html`, `og.png` e `LEIA-ME.txt`, com os links entre as
páginas já convertidos para relativos. É a pasta que vai para a hospedagem —
os arquivos da raiz continuam apontando para os artifacts, que servem só para
apresentação interna.

Regerar depois de qualquer alteração na raiz:

```sh
rm -rf deploy && mkdir deploy
cp index.html loja.html deploy/ && cp caminho/para/og.png deploy/
# trocar as URLs de artifact por relativas — ver bloco no histórico do projeto
```

**Antes de publicar em domínio próprio**, trocar as 42 ocorrências de
`SEU-DOMINIO.com.br` no `deploy/loja.html`:

```sh
sed -i 's#https://SEU-DOMINIO.com.br#https://oseudominio.com.br#g' deploy/loja.html
```

Sem isso o `og:image` e o `canonical` apontam para um domínio inexistente e o
preview do link sai sem imagem.

A imagem `og.png` (1200x630) foi gerada a partir do frame do cristal. Quando
houver foto profissional da Dra. Vera, vale refazer com o rosto dela — preview
com rosto converte mais.
