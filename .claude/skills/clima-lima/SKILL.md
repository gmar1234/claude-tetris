---
name: clima-lima
description: Consulta la temperatura y el estado del clima actual en Lima, Perú, usando fuentes meteorológicas confiables (SENAMHI primero). Usar cuando el usuario pida el clima, temperatura o pronóstico de Lima, o invoque /clima-lima.
---

# clima-lima

Consulta el clima actual de Lima, Perú y repórtalo de forma breve.

## Pasos

1. Buscar primero datos de SENAMHI (fuente oficial peruana), con una consulta tipo:
   `clima Lima Perú temperatura actual SENAMHI`
2. Si SENAMHI no da datos claros, complementar con otra fuente confiable (clima.com, Meteored, Yahoo Clima).
3. Desconfiar de datos que no encajen con la temporada: en invierno limeño (mayo–septiembre) son normales cielos nublados y máximas de 16–22°C; temperaturas por encima de 26°C o cielos "muy soleados" en esos meses son señal de datos desactualizados o de otra época del año. En verano (diciembre–abril) es al revés: esperar cielos despejados y 24–30°C.
4. Reportar en pocas líneas: estado del cielo, temperatura (máx/mín si están disponibles), viento y humedad si son relevantes.
5. Incluir las fuentes usadas como enlaces markdown al final.

## Notas

- No hay API key ni configuración previa: esto usa búsqueda web general, no un servicio de clima estructurado.
- Si se necesita monitoreo periódico, usar `/loop` o `/schedule` por separado — esta skill solo resuelve una consulta puntual.
