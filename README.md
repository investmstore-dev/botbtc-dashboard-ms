# BOT BTC Dashboard — Mining Store
**Dashboard en tiempo real** para el bot de trading BTCUSD en Crypto Fund Trader.

> Repositorio del bot: [botbtc-ms](https://github.com/investmstore-dev/botbtc-ms)

---

## Vista previa

Dashboard estilo terminal oscuro con:
- Estado del bot (activo / inactivo)
- Capital actual, profit %, drawdown en tiempo real
- Progreso hacia el objetivo CFT (+8% Phase 1)
- Historial de trades con P&L
- Gráfico de equity curve

---

## Cómo usar

El dashboard es un archivo HTML estático (`index.html`) que lee datos JSON generados por el bot.

### 1. El bot genera los datos

Cuando `bot.py` está corriendo, actualiza automáticamente estos archivos en `botbtc-ms/data/`:

```
data/
├── state.json    ← estado actual (capital, equity, posición abierta, régimen)
├── trades.json   ← historial de trades cerrados
└── equity.json   ← curva de equity (puntos tiempo/valor)
```

### 2. Servir el dashboard localmente

La forma más simple es abrir el archivo directamente:

```
Doble clic en index.html → abre en el navegador
```

O servir con Python para evitar restricciones CORS al leer los JSON:

```bash
# Desde la carpeta botbtc-dashboard-ms
python -m http.server 8080
```

Luego abrir: `http://localhost:8080`

### 3. Apuntar al directorio `data/` del bot

Editar en `index.html` la ruta a los archivos JSON (por defecto espera `../botbtc-ms/data/`):

```javascript
const DATA_PATH = "../botbtc-ms/data/";   // ajustar según tu estructura
```

---

## Estructura

```
botbtc-dashboard-ms/
└── index.html    # Dashboard completo (HTML + CSS + JS en un solo archivo)
```

No requiere dependencias externas ni compilación. Todo está embebido en `index.html`.

---

## Despliegue opcional (acceso remoto)

Para ver el dashboard desde cualquier dispositivo mientras el bot corre:

**Opción A — GitHub Pages:**
1. En GitHub → Settings → Pages → Source: `main branch / root`
2. Disponible en: `https://investmstore-dev.github.io/botbtc-dashboard-ms`
3. Los archivos JSON deben estar accesibles públicamente (o usar una API propia)

**Opción B — servidor local con ngrok:**
```bash
python -m http.server 8080
ngrok http 8080
```
Ngrok genera una URL pública temporal para acceder desde el móvil.

---

## Datos que muestra

| Campo | Fuente |
|---|---|
| Capital actual | `state.json → balance` |
| Equity en tiempo real | `state.json → equity` |
| Profit % | `state.json → profit_pct` |
| Drawdown actual | `state.json → dd_pct` |
| Posición abierta | `state.json → active_trade` |
| Régimen mercado | `state.json → regime` (TREND / NEUTRAL / CHOPPY) |
| Choppiness Index | `state.json → h4_chop` |
| Supertrend dirección | `state.json → h4_st_dir` |
| Historial trades | `trades.json` |
| Curva de equity | `equity.json` |

---

## Relacionado

- [botbtc-ms](https://github.com/investmstore-dev/botbtc-ms) — código del bot (strategy, config, backtest)
