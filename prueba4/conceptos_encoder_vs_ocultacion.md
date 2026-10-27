# Encoder vs. técnicas que "ocultan" (prueba 4 — Fase 3)

"Encoder" es solo **una** pieza de un abanico de técnicas, y suelen confundirse.
Ordenadas de menos a más "ocultadoras". Lo importante: **ninguna lo vuelve
invisible de verdad**; cada una cae por un motivo distinto.

| Técnica | Qué hace | Por qué cae igual |
|---|---|---|
| **Encoder** | Reescribe los bytes (p.ej. XOR) y pega un *decoder stub* delante. Sin clave secreta, reversible. | El stub es casi siempre el mismo → **es una firma en sí mismo**. Y el comportamiento al ejecutar sigue igual. |
| **Packer** | Comprime/empaqueta el .exe entero y lo descomprime en memoria al arrancar. Nació legítimo (reducir tamaño, p.ej. UPX). | El AV reconoce los packers comunes, y "estar empaquetado + conectar hacia fuera" ya es sospechoso. |
| **Crypter** | **Cifra de verdad** el payload con una clave; un stub lo descifra en memoria al ejecutar. Esto sí "oculta" el contenido estático mejor. | El stub descifrador deja rastro, la **entropía** alta (todo cifrado) es una señal, y sobre todo el **comportamiento** en RAM. |
| **Obfuscación** | Enreda la lógica/código (renombrar, código basura) para dificultar el análisis. | Dificulta leerlo, no esconde lo que *hace* al correr. |

## La idea clave

- Un **encoder solo disfraza los bytes** (como escribir con otra letra) → no hay secreto real.
- Un **crypter/packer sí "ocultan"** el contenido real hasta que el programa arranca (lo cifran/comprimen).

## La lección de la prueba

Los cuatro tienen el mismo talón de Aquiles: para funcionar, en algún momento el
payload real **tiene que descifrarse y ejecutarse en memoria**. Ahí es donde
Defender/EDR lo caza — igual que pasó en la Tarea 3.1: lo mató **al ejecutar**,
no en disco. Por eso el disfraz estático no basta contra un AV moderno.

> El entregable de valor de esta prueba es **entender por qué se detectan** y qué
> rastro dejan, no fabricar algo indetectable. Documentamos detección, no evasión.
