# TrilhaPT — Landing Page

Landing page estática do TrilhaPT, com:

- Página multilíngue (PT/EN/ES)
- CTA para WhatsApp
- Banner de consentimento de cookies
- Integração com Google Analytics 4 (após consentimento)
- Modal simples de Política de Privacidade

## Estrutura

- `index.html`: página única (HTML, CSS e JS)
- `favicon.svg`: favicon do site
- `netlify.toml`: build/deploy no Netlify

## Variáveis de ambiente (Netlify)

O deploy injeta os parâmetros abaixo no `index.html` durante o build:

- `WHATS_NUMBER`: número do WhatsApp sem `+` e sem espaços (ex.: `351928498178`)
- `GA_MEASUREMENT_ID`: ID do GA4 (ex.: `G-XXXXXXXXXX`)
- `MAINTENANCE_MODE`: `true` ou `false` para ativar/desativar a tela de manutenção
- `LAST_UPDATED`: data da última atualização dos dados no formato `YYYY-MM-DD` (ex.: `2026-08-28`)
- `API_URL`: URL base da API para buscar último update (ex.: `https://api.trilhapt.com`) — **opcional**

### Onde configurar no Netlify

No painel do site no Netlify:

1. `Site configuration`
2. `Environment variables`
3. Adicionar:
	- `WHATS_NUMBER`
	- `GA_MEASUREMENT_ID`
	- `MAINTENANCE_MODE`
	- `LAST_UPDATED`
	- `API_URL` (opcional)

## Como funciona a injeção no deploy

No `netlify.toml`, o comando de build substitui placeholders no `index.html`:

- `__WHATS_NUMBER__`
- `__GA_MEASUREMENT_ID__`
- `__MAINTENANCE_MODE__`
- `__LAST_UPDATED__`
- `__API_URL__`

Se `API_URL` estiver definida, o site faz um fetch GET para `${API_URL}/faq/last-update` no carregamento e tenta usar a data retornada no lugar da data estática.

Depois publica a pasta `dist`.

## Deploy

Cada push na branch conectada ao Netlify dispara build/deploy automático.

## Desenvolvimento local

Como o projeto é estático, podes abrir o `index.html` diretamente no navegador para revisar layout e textos.

Se quiseres simular a injeção de variáveis localmente, executa:

```bash
mkdir -p dist && sed -e "s|__WHATS_NUMBER__|351928498178|g" -e "s|__GA_MEASUREMENT_ID__|G-XXXXXXXXXX|g" -e "s|__MAINTENANCE_MODE__|false|g" -e "s|__LAST_UPDATED__|2026-08-28|g" -e "s|__API_URL__|https://api.trilhapt.com|g" index.html > dist/index.html && cp support.js favicon.svg robots.txt sitemap.xml dist/
```

E abre `dist/index.html`.

## Observações

- O GA4 só é carregado após aceite no banner de cookies.
- Se `GA_MEASUREMENT_ID` não estiver definido, o analytics não é carregado.
- O modo manutenção é controlado pela variável de ambiente `MAINTENANCE_MODE` no deploy.
