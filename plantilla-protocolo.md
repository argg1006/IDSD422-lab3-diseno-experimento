# Plantilla de protocolo — Experimento de ubicación de producto

**Integrantes:** Aitana Gómez · **Fecha:** 19/11/2026

## 1. Pregunta e hipótesis

¿Las ventas semanales del producto estrella son mayores en el pasillo principal que en su ubicación habitual, durante cuatro semanas?

- **H₀:** las ventas semanales promedio en el pasillo principal son iguales o menores que en la ubicación habitual.
- **H₁:** las ventas semanales promedio en el pasillo principal son mayores que en la ubicación habitual.

## 2. Unidad experimental

- **Unidad:** una tienda durante una semana (tienda-semana).
- **Número de unidades y justificación:** 10 tiendas × 4 semanas = 40 tienda-semana. Cada tienda pasa por las dos ubicaciones, así que se compara consigo misma.

## 3. Factor, niveles y tratamiento

- **Factor:** ubicación del producto en la tienda.
- **Niveles:** pasillo principal y ubicación habitual.
- **Tratamiento:** exhibir el producto en el pasillo principal. Cada tienda alterna de ubicación cada semana (2 semanas en cada una).

## 4. Grupo de control

Las tienda-semana con el producto en su ubicación habitual. Sirven de referencia para medir cuánto cambia la venta en el pasillo principal. Como cada tienda participa en ambas condiciones, es su propio control.

## 5. Aleatorización

Se sortea con semilla fija qué ubicación tiene cada tienda en la semana 1; luego alterna (semanas 2 y 4 usan la contraria).

```python
import numpy as np

tiendas = ["Quito Norte", "Quito Sur", "Guayaquil Centro", "Guayaquil Norte",
           "Cuenca Centro", "Cuenca Sur", "Ambato Centro", "Manta", "Loja", "Machala"]

rng = np.random.default_rng(1234597)
primera = rng.choice(["pasillo_principal", "habitual"], size=len(tiendas))

for tienda, ubicacion in zip(tiendas, primera):
    print(tienda, ubicacion)
```

Resultado de una corrida (semilla 1234597):

```
Quito Norte habitual
Quito Sur pasillo_principal
Guayaquil Centro habitual
Guayaquil Norte pasillo_principal
Cuenca Centro pasillo_principal
Cuenca Sur habitual
Ambato Centro habitual
Manta habitual
Loja habitual
Machala pasillo_principal
```

**Justificación:** el azar evita que se favorezca una condición y reparte entre las tiendas las características que no se miden, aunque no garantiza equilibrio perfecto (aquí 4 tiendas empiezan en pasillo y 6 en habitual; igual quedan 20 tienda-semana por condición).

## 6. Respuesta y medición

- **Variable respuesta:** ventas semanales del producto (USD).
- **Período de medición:** cuatro semanas consecutivas.
- **Quién registra y cómo (¿ciego?):** el personal de datos de cada tienda, desde el sistema POS. No hay ciego total (el producto se ve); quien analiza puede recibir las condiciones codificadas (A y B).

## 7. Factores a controlar (mínimo 4)

| Factor a controlar | Cómo se controla o registra |
|---|---|
| 1. Temporada y feriados | Registrar feriados de cada semana; la alternancia reparte su efecto. |
| 2. Promociones | No hacer promociones solo en una ubicación; marcar cualquier promoción. |
| 3. Afluencia de clientes | Registrar eventos cercanos y comparar cada tienda consigo misma. |
| 4. Stock y precio | Mantener inventario y precio iguales; anotar cualquier cambio. |

## 8. Limitaciones del diseño

- Una promoción en una semana de pasillo se confundiría con el efecto de la ubicación.
- Canibalización: el producto podría vender más quitando ventas a otros.
- Efecto de arrastre: clientes que recuerdan la ubicación de la semana anterior.
- Cuatro semanas y 10 tiendas: no se puede generalizar a todas las tiendas ni a largo plazo.
- Las 40 tienda-semana no son independientes (vienen de 10 tiendas).

## 9. Comparación en código (Python/R)

Se ejecutó `scripts/comparacion_experimento.py` sobre `plantillas/registro_experimento.csv`.

```
ubicacion
habitual             1040.0
pasillo_principal    1335.0
Name: ventas_producto, dtype: float64

ubicacion
pasillo_principal    2
habitual             2
Name: count, dtype: int64
```

- **¿Qué grupo muestra mayor venta promedio?** El pasillo principal: 1335 USD frente a 1040 USD (295 USD más, 28,37 %).
- **¿Por qué con 4 filas no se puede concluir nada?** Hay solo 2 registros por grupo y de tiendas distintas: ninguna aparece en ambas ubicaciones, así que no se separa el efecto de la ubicación del de la tienda (una además tenía promoción).
- **¿Qué le faltaría para ser justo?** Las 40 tienda-semana, con cada tienda en ambas condiciones, y una prueba t pareada sobre la diferencia por tienda, registrando promociones, stock y precios.
