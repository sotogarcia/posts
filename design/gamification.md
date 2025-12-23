A continuación se describe el contexto técnico existente y el trabajo que debe realizarse para incorporar una capa de gamificación completa.

## 1) Contexto y punto de partida

Actualmente existe una plataforma de test donde el alumnado realiza ejercicios tipo test. Estos test pueden ser creados por tres vías: 
1. Por los propios alumnos
2. Por un "coach" automático (bot)
3. Por docentes o tutores (tiende a desaparecer, pero debe mantenerse la funcionalidad).

Los ejercicios se alimentan de un banco de preguntas que se gestiona desde una segunda plataforma.

A nivel técnico, la solución se apoya en:

- **Base de datos PostgreSQL** que contiene **tablas correspondientes a modelos de Odoo** y está en producción.
- **Instancia Laravel** que:
  - **Lee** principalmente desde PostgreSQL.
  - **Escribe** en tablas concretas (registro de usuarios y algunas entidades adicionales).
  - Se **comunica con Odoo** para solicitar creación o actualización de contenidos.
  - La comunicación con Odoo se realiza mediante **JSON-RPC**.
- Front-end con **Material Design for Bootstrap**, uso de **jQuery** y algún componente JavaScript propio para la interacción durante la realización de test.

Este ecosistema implica que la gamificación no se desarrolla "desde cero", sino que debe **integrarse** con la instancia PostgreSQL/Odoo ya existente.

La lógica de aplicación en Laravel y la UI están definidas, pero estas pueden ser rediseñadas.

## 2) Objetivo: qué se quiere conseguir con la gamificación

Se pretende añadir una capa de gamificación que incremente participación, retención y dinamismo social. Los objetivos funcionales son:

1. **Compartición de ejercicios de test** con componente social: búsqueda de amigos y posibilidad de compartir/descubrir retos.
2. **Retos entre participantes** (usuario vs usuario) con reglas claras (qué se reta, cuánto dura, cómo se gana).
3. **Feedback inmediato** tras la realización de ejercicios/retos (resultado, comparación, progreso, recompensa).
4. **Ligas** organizadas por clasificación con **ascensos y descensos** por temporadas.
5. **Coach** (bot) que guíe y motive, incluyendo:
   - **Rachas** (streaks) y refuerzo conductual.
   - **Mapa de progreso** (visualización del avance: hitos, niveles, rutas, etc.).
6. **Sistema de puntos** por participación y por objetivos alcanzados.
7. **Sistema de premios**: los puntos deben poder **canjearse** por recompensas.

En resumen: se busca un sistema que combine **competición**, **progreso personal** y **recompensa**, con un componente social claro.

## 3) Alcance de trabajo

El trabajo consiste en diseñar e implementar la gamificación como un conjunto coherente de funcionalidades, integradas en el entorno actual. Para que sea ejecutable, conviene plantearlo en cuatro grandes bloques: **modelo de datos**, **lógica de negocio**, **integración** y **UI/UX**.

### A) Modelo de datos (persistencia)

Debe diseñarse cómo se guardan y relacionan los elementos de gamificación en la base de datos existente, evitando romper el modelo productivo actual. Como mínimo, la gamificación requiere almacenar:

- **Relaciones sociales**: solicitudes de amistad, amigos, bloqueos, privacidad.
- **Retos**: emisor, receptor, estado (pendiente/aceptado/en curso/finalizado), ventana de tiempo, ejercicio/s seleccionados, resultado.
- **Puntos y economía**:
  - Transacciones de puntos (por qué se otorgan/quitan, cuándo, trazabilidad).
  - Saldo por usuario.
- **Ligas**:
  - Estructura de liga/divisiones.
  - Temporadas.
  - Clasificación (ranking) por temporada y reglas de ascenso/descenso.
- **Premios**:
  - Catálogo de premios.
  - Coste en puntos.
  - Canjes, estado del canje y auditoría.
- **Otros:**
  - Estado de la racha, último día de actividad, hitos de racha.
  - Hitos/niveles.
  - Reglas para desbloqueo.
  - Registro de avance.

Aquí es clave decidir (y ejecutar) **dónde vive cada cosa**: si se modela como tablas nuevas "lado Laravel", como modelos Odoo, o un enfoque híbrido. Dado que ya existe convivencia con Odoo y PostgreSQL. En cualquier caso el diseño debe:

- sea mantenible,
- permitir auditoría (especialmente puntos y canjes),
- no degradar el rendimiento.

### B) Lógica de negocio (reglas de gamificación)

La gamificación requiere reglas que deben definirse y codificarse:

- **Cómo se generan puntos**:
  - Por participación (hacer test, completar sesiones, constancia).
  - Por objetivos (aciertos, mejora respecto a uno mismo, completar un set, etc.).
  - Antifraude básico (evitar farming fácil: límites diarios, cooldowns, etc.).
- **Cómo funcionan los retos**:
  - Tipos de reto (mismo test para ambos, sets equivalentes, contrarreloj...).
  - Resolución y desempates.
  - Recompensa por ganar/perder/participar.
- **Ligas**:
  - Cómo se calcula el ranking (puntos, ELO, ratio aciertos/tiempo, mixto).
  - Duración de temporada.
  - Criterios de ascenso/descenso.
- **Coach/bot**:
  - Mensajes y disparadores (cuando cae la racha, cuando sube de liga, etc.).
  - Recomendación de objetivos (micro-metas).
  - "Mapa de progreso" y orientación: qué tocar a continuación.
- **Premios**:
  - Flujo de canje (validación de saldo, bloqueo de puntos, confirmación).
  - Estados del canje y trazabilidad.

Este bloque debe entregarse con reglas "cerradas" (documentadas) y con implementación.

### C) Integración con lo existente (Laravel ↔ PostgreSQL ↔ Odoo)

La gamificación debe encajar con los flujos actuales:

- La plataforma ya **lee de PostgreSQL** y **se apoya en Odoo** para generar los contenidos.
- Podría tener una lógica completamente independiente siempre que fuese posible enviar:
	1) Test de Odoo a Laravel
	2) Resultados de Laravel a Odoo

### D) UI/UX (Material Design + jQuery + componentes existentes)

La experiencia de gamificación debe sentirse integrada con la plataforma:

- **Búsqueda y gestión de amigos**: encontrar, añadir, aceptar, listado.
- **Retos**: creación, bandeja de entrada, estado, resultados comparativos.
- **Feedback inmediato**: pantalla post-test con:
  - resultado,
  - comparación (si aplica),
  - recompensa (puntos, racha, progreso),
  - siguiente acción sugerida (coach).
- **Ligas**:
  - clasificación,
  - estado de temporada,
  - zona de ascenso/descenso,
  - evolución del usuario.
- **Mapa de progreso**:
  - visual simple, navegable,
  - hitos alcanzados y próximos.
- **Premios**:
  - catálogo,
  - detalle,
  - canje,
  - historial.

En su día se eligió Material Design for Bootstrap; si propone una tecnología alternativa esta debe estar justificada.

## 4) Entregables esperados (lo que debería "quedar hecho")

Para que el trabajo sea verificable, los entregables deberían incluir:

1. **Documento funcional** de gamificación (reglas completas: puntos, retos, ligas, rachas, coach, premios).
2. **Diseño técnico** (arquitectura y modelo de datos, integración con Odoo/Laravel, endpoints).
3. **Implementación**:
   - frontend (pantallas y componentes),
   - lógica del coach.
4. **El backend** puede estar desarrollado o ser conceptual si se reusa la instancia de Odoo en produción y debe:
	- Otorgar capacidad de ajustar parámetros (puntos, temporadas, catálogo de premios, etc.).
	- Facilitar la consulta del progreso de los alumnos y/o grupos.
5. **Pruebas**: al menos pruebas de flujos críticos (puntos, retos, ranking, canjes).
6. **Guía de despliegue** y notas de operación (jobs periódicos de ligas/temporadas, mantenimiento).

## 5) Consideraciones importantes (para evitar problemas)

- **Trazabilidad y auditoría**: el sistema de puntos y canjes debe quedar registrado como "libro mayor" (transacciones), para poder depurar y evitar conflictos.
- **Rendimiento**: rankings y ligas pueden ser costosos si se recalculan mal; conviene definir si se actualizan en tiempo real o por procesos programados.
- **Antifraude básico**: límites diarios, reglas anti-abuso, detección de patrones simples.
- **Privacidad**: búsqueda de amigos y visibilidad en ligas deben contemplar configuraciones mínimas.
- **No romper producción**: cualquier cambio sobre tablas que correspondan a modelos Odoo debe hacerse con extremo cuidado (idealmente extendiendo, no modificando lo crítico).
