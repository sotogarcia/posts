# Simplificación de publicación de tests

A continuación se presenta un informe sintético que resume el planteamiento y el debate generado en torno a la publicación de microtests y otros cuestionarios: se expone el objetivo del cambio de procedimiento, se recogen las ventajas y desventajas operativas identificadas y se incluye un glosario para unificar términos y evitar ambigüedades

## Exposición

Se plantea **cambiar el procedimiento de publicación de microtests (y otros tests)** para evitar el ciclo actual de **exportación XML desde Odoo → importación al banco de preguntas de Moodle → configuración del cuestionario en Moodle**.

La propuesta consiste en:

- **Seguir generando y manteniendo los tests en Odoo** (como fuente única).
- **Asignar el test a la formación desde Odoo**, de modo que aparezca automáticamente en la plataforma de tests.
- En Moodle, **publicar un recurso tipo URL (enlace)** que apunte al test en la plataforma de tests.

Objetivo principal: **eliminar doble mantenimiento** (Odoo + Moodle) y asegurar que **las correcciones/actualizaciones** realizadas en Odoo **se reflejen inmediatamente** en lo que ve el alumnado.

## A favor

- **Un único “origen de verdad”**: el test existe solo en Odoo; se evita que Moodle quede desactualizado.
- **Se elimina el trabajo de exportar/importar XML**, con el ahorro de tiempo y errores operativos que eso conlleva.
- **Actualización inmediata**: cambios en preguntas/enunciados en Odoo se reflejan al instante en la plataforma de tests.
- **Menos duplicidades**: se reducen copias repetidas de tests/preguntas en el banco de Moodle.
- **Menor riesgo de que el alumnado haga tests antiguos** por discrepancias entre plataformas.
- **Moodle sigue siendo el “hub” del curso** (estructura, secuenciación, visibilidad, disponibilidad por fechas), pero el contenido del test se centraliza fuera.

## En contra

- **Pérdida de seguimiento detallado en Moodle**:
    - Las **puntuaciones/resultados no vuelcan a Moodle**.
    - En Moodle, como mucho, quedaría el registro de acceso al enlace (si se usa seguimiento de finalización), pero **no el rendimiento**.
- **Impacto crítico en evaluaciones “montadas” en Moodle**:
    - Para tests como los **cuatrimestrales**, donde ya hay un flujo y analítica dentro de Moodle, este cambio se considera **inviable** sin integración.
- **Doble inicio de sesión / experiencia “salto a externo”**:
    - No hay SSO entre plataformas: el alumno **tendría que iniciar sesión también** en la plataforma de tests.
    - No es posible “embeber” el test dentro de Moodle evitando ir a una página externa si esa página requiere login (sin desarrollo adicional).
- **Restricciones de acceso por licencias/permisos**:
    - No todos los cursos/alumnos tendrían acceso a la plataforma de tests; en esos casos **no aplicaría**.
    - Si se intentara publicar sin login, aparece el riesgo de **acceso no autorizado** (“cualquiera con el enlace”).
- **Integración a medida (si se quisiera conservar analítica en Moodle)**:
    - Para volcar notas/resultados desde Odoo a Moodle haría falta **desarrollo específico**, que ahora mismo se considera **no viable por carga de trabajo**.
- **Gestión actual del banco de preguntas en Moodle**:
    - Existe un esfuerzo en marcha para **normalizar categorías y reutilización** dentro de Moodle; este enfoque de enlaces puede hacer que parte de esa gestión deje de aportar valor para estos tests (aunque siga siendo útil para otros casos).

## Terminología

- **Odoo**: sistema donde se **crean y almacenan** las preguntas/tests. Actúa como fuente de datos.
    - [odoo.testsoposiciones.es](https://odoo.testsoposiciones.es)
- **Moodle**: plataforma de aprendizaje donde se **organiza el curso** y se publica el acceso a recursos (incluidos cuestionarios).
    - [oposiciones.tucampusonline.com](https://oposiciones.tucampusonline.com)
    - [oposiciones2.tucampusonline.com](https://oposiciones2.tucampusonline.com)
    - [campus.opostal.es](https://campus.opostal.es)
- **Plataforma de tests**: aplicación web (Laravel) donde parte del alumnado ya **realiza** tests autocreados.
    - [www.testsoposiciones.es](https://www.testsoposiciones.es)
- **XML (export/import)**: formato usado para **mover preguntas** desde Odoo al banco de preguntas de Moodle.
- **Actividad “URL” (Moodle)**: tipo de recurso que publica un **enlace** a un contenido externo (URL ≈ Dirección de Internet).
