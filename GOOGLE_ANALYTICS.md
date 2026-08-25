# Configurar o Google Analytics 4 no trilhapt.com

O código já está preparado no `index.html` — só falta um passo: obter o teu
Measurement ID real e substituir o placeholder.

## Como está montado (para entenderes o que já foi feito)

- O Google Analytics **não carrega automaticamente**. Aparece primeiro um
  banner de consentimento de cookies (obrigatório na UE/Portugal antes de
  correres qualquer script de analytics).
- Só depois de a pessoa clicar em **"Aceitar"** é que o script do GA4 é
  injetado na página.
- Se a pessoa clicar em **"Rejeitar"**, o GA nunca carrega, e a escolha fica
  guardada num cookie por 180 dias (não volta a perguntar nesse período).
- Isto está implementado no bloco de JavaScript perto do fim do `index.html`,
  nas funções `loadAnalytics()` e `initCookieConsent()`.

## Passo 1 — Criar a propriedade GA4

1. Vai a [analytics.google.com](https://analytics.google.com)
2. **Admin** (ícone de engrenagem) → **Create Property**
3. Nome da propriedade: `TrilhaPT`
4. Fuso horário: Portugal, moeda: Euro
5. Categoria do setor: escolhe a que fizer mais sentido (ex: "Community" ou "Other")
6. Cria um **Web data stream** com o URL `https://trilhapt.com`
7. A Google mostra-te um **Measurement ID** com o formato `G-XXXXXXXXXX`

## Passo 2 — Substituir no código

No `index.html`, procura por:

```js
const GA_MEASUREMENT_ID = "G-XXXXXXXXXX"; // TODO: substitui pelo teu Measurement ID real do GA4
```

Substitui `"G-XXXXXXXXXX"` pelo ID real que a Google te deu. Guarda, faz
commit e push — a Netlify atualiza o site sozinha.

## O que vais conseguir medir (útil para o pitch, como falaste)

- Quantas pessoas visitam a página, de que país, em que idioma navegam
- Quantas clicam no botão "Falar agora no WhatsApp" (para medires isto com
  precisão, o ideal é adicionar um evento — ver nota abaixo)
- Taxa de rejeição de cookies (para saberes se o banner está a afastar gente)

### Nota: medir cliques no botão do WhatsApp

Por padrão, o GA4 só sabe que alguém visitou a página, não que clicou no
botão. Se quiseres medir cliques no CTA especificamente (recomendo, porque é
a métrica mais importante para o teu pitch), pede-me depois para adicionar
um evento `gtag('event', 'whatsapp_click')` nos dois botões — é uma alteração
pequena e rápida.

## Sobre privacidade (não sou advogado, mas fica a nota)

Ter um banner de consentimento ajuda a cumprir o RGPD, mas para estares
totalmente em conformidade, o site também deveria ter uma **política de
privacidade** a explicar que dados recolhes e porquê. Se quiseres, posso
ajudar a redigir uma depois — não é complicado para um caso simples como
este, mas não é algo que deva ser feito às pressas.
