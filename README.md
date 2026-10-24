# Radar Cripto Longs

Escáner de contratos perpetuos USDT de Binance que detecta activos con RSI de 4 horas en sobreventa y los marca para vigilancia o como posible entrada long cuando el RSI empieza a girar hacia arriba.

## Versiones

- **Versión 1 (actual): solo longs.**
- **Versión 2 (planeada): shorts**, con criterios propios y su backtest.
- **Después:** revisar los criterios con los resultados del backtest y decidir si se agregan más filtros.

## Criterios

Solo se usa el RSI de 14 periodos en velas cerradas de 4 horas.

- **Vigilancia:** el RSI es menor o igual a 35.
- **Posible entrada long:** el RSI es menor o igual a 35 y el valor actual es mayor que el de la vela anterior (giro hacia arriba).

Cada posible entrada muestra entrada (cierre de la vela), stop (mínimo reciente menos 1 ATR de 14 periodos) y objetivo a 2R. La distancia al stop y el nivel de volumen de 24 horas se muestran como información, no como filtros.

## Uso

### Página

Abre `index.html` en el navegador. La página consulta directamente la API pública de Binance (sin API key), calcula el RSI en el navegador y se actualiza sola cada 5 minutos si la casilla está activa.

Si el navegador no puede conectarse a Binance, la página muestra un mensaje de error. Esto puede pasar por restricciones de red o de seguridad del navegador.

### Escaneo desde la terminal

```
pip install -r requirements.txt
python run.py
```

### Backtest

```
python backtest.py --days 365 --top 30
```

Imprime una tabla con el rendimiento del conjunto de criterios, en el periodo de ajuste y en el de validación, y guarda `backtest_results.json`. Incluye comisiones de 0,04 % por lado y deslizamiento de 0,05 % por lado.

## Advertencias

- Esto no es una recomendación de inversión.
- Los futuros con apalancamiento pueden liquidar la posición.
- Un activo que cumple la regla no garantiza una operación rentable. Revisa siempre la gráfica y el riesgo antes de operar.
- El backtest usa solo contratos que existen hoy, por lo que puede sobreestimar los resultados.

## Estructura

- `index.html`: página con el escáner en vivo.
- `fetch.py`: descarga de velas y volúmenes desde Binance.
- `signals.py`: indicadores y reglas del RSI de 4 horas.
- `run.py`: escaneo desde la terminal.
- `backtest.py`: prueba histórica de la regla.
- `requirements.txt`: dependencias de Python.
