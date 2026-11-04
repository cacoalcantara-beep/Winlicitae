# Passagens aéreas (referências externas)

Pasta separada do projeto Winlicitae. Os projetos abaixo tratam de busca, reserva e
monitoramento de passagens aéreas e **não têm relação com licitações**. Ficam aqui
apenas como referência para uso futuro.

## 1. mcp-duffel-travels

- Repositório: https://github.com/modellers/mcp-duffel-travels
- README: https://github.com/modellers/mcp-duffel-travels/blob/main/README.md
- Licença: MIT

Servidor MCP (Model Context Protocol) de busca e reserva de voos usando a API da
[Duffel](https://duffel.com). Expõe sete ferramentas que cobrem o fluxo completo:

| Ferramenta | Função |
|---|---|
| `flight_offer_request` | Busca ofertas por origem, destino, datas, passageiros e classe |
| `flight_offers` | Detalhes de uma oferta |
| `flight_booking_validate_or_price_offer` | Valida a oferta e confirma o preço atual |
| `flight_booking_list_services_and_seatmaps` | Serviços adicionais e mapas de assento |
| `flight_booking_create_order` | Cria o pedido com dados dos passageiros |
| `flight_booking_pay_for_order` | Paga um pedido em espera |
| `flight_booking_get_order_status` | Status do pedido e e-tickets |

**Tecnologia:** Node.js 18+ (TypeScript), SDK oficial da Duffel.

**Instalação:**

```bash
git clone https://github.com/modellers/mcp-duffel-travels
cd mcp-duffel-travels
npm install
npm run build
DUFFEL_API_KEY=sua_chave node build/index.js
```

**Configuração no Claude Desktop** (`claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "duffel-travels": {
      "command": "node",
      "args": ["/caminho/absoluto/mcp-duffel-travels/build/index.js"],
      "env": { "DUFFEL_API_KEY": "sua_chave" }
    }
  }
}
```

O arquivo fica em `~/Library/Application Support/Claude/` (macOS) ou
`%APPDATA%/Claude/` (Windows).

## 2. monitor-passagens

- Repositório: https://github.com/ManoelDiniz/monitor-passagens
- Licença: não informada no repositório

Monitor local e automático de preços de passagens aéreas com alertas no Telegram
e notificações do Windows. Inclui módulo de câmbio (DOLA11, USD, EUR, com backtest
contra o CDI) e radar de cripto, tudo em um painel web.

**Funcionalidades:**

- Cadastro de rotas com janela de datas (± até 7 dias), filtros de escalas e bagagem e preço-limite.
- Consultas de hora em hora ou em horários fixos, com recuperação de execuções perdidas.
- Fontes de preço sem chave: Kiwi, MaxMilhas e Google Flights. Com chave: Travelpayouts e Amadeus. Modos `merge` e `failover`.
- Alertas por preço abaixo do limite, queda de X% ou mínimo histórico, com cooldown.
- Backup diário do banco via `mysqldump`, poda de ofertas antigas e início automático com o Windows.

**Tecnologia:** Node.js 20.19+ ou 22.12+ (TypeScript), Fastify, TypeORM, MySQL 8.
Frontend em React 19, Vite e Tailwind CSS 4.

**Instalação:**

```bash
git clone https://github.com/ManoelDiniz/monitor-passagens
cd monitor-passagens
npm install
cp .env.example .env        # Windows: copy .env.example .env
mysql -u root -p -e "CREATE DATABASE monitor CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;"
npm run build
npm start                   # painel em http://localhost:3777
```

Em desenvolvimento: `npm run dev` e `npm run web:dev` em dois terminais.

**Variáveis principais (`.env`):** `DB_USER`, `DB_PASSWORD`, `DB_SYNCHRONIZE`,
`TELEGRAM_BOT_TOKEN`, `PROVIDER_MODE`, `PROVIDER_ORDER`, `SCHEDULE_EVERY_HOURS`,
`SCHEDULE_TIMES`, `DROP_ALERT_PCT`, `NOTIFY_COOLDOWN_HOURS`, `SEARCH_MAX_DATES`,
`OFFERS_RETENTION_DAYS`, `BACKUP_TIME`, `BACKUP_KEEP`, `MYSQLDUMP_PATH`.
Nenhuma chave é obrigatória para começar.

**Comandos úteis:** `npm test`, `npm run typecheck`, `npm run lint`,
`npm run check-now`, `npm run comparar FOR SSA 2026-12-10`, `npm run telegram`,
`npm run backtest-cambio -- USD 500`, `npm run estudo-cripto -- BTC ETH`,
`npm run backup`, `npm run install-startup`.
