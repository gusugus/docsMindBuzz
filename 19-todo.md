# 19 - Tareas Pendientes (ToDo General)

Este documento centraliza todas las tareas, ideas y planes de implementación a futuro para la plataforma, organizado como una bitácora y especificación técnica.

## 1. Autenticación de Estudiantes e Historial

- [ ] **Capa de Base de Datos**
  - [ ] Crear nueva tabla `students` y tabla de relación `manager_students` (manager_id, student_id).
  - [ ] Añadir `student_id` como columna referencial (nullable) al esquema de historiales `quiz_runs`.
  - **📍 Archivos Afectados:** `services/database.ts`
  - **⚠️ Impacto:** Modificación del esquema principal de SQLite. Una mala sintaxis corromperá el inicio y causará que otros servicios base fallen.
  - **🧪 Pruebas:** Eliminar el archivo de base de datos local y dejar que arranque desde cero; verificar en un visor SQLite que las nuevas tablas aparezcan.

- [ ] **Configuración e Ingesta Inicial**
  - [ ] Añadir característica de `requireStudentLogin` en el `config.json`.
  - [ ] Script para iterar y cargar el contenido de `./config/students.json` hacia la Base de Datos.
  - **📍 Archivos Afectados:** `services/config.ts`, `services/accountStore.ts` (en `migrateLegacyResources()`).
  - **⚠️ Impacto:** Modifica la validación estricta Zod en el arranque. Tipos erróneos detendrán todo el backend.
  - **🧪 Pruebas:** Modificar `config.json` manualmente a `true` y `false`. Comprobar con la terminal que el servidor no *crashee* e imprima la importación de estudiantes con éxito.

- [ ] **WebSocket y Lógica de Autenticación**
  - [ ] Crear eventos `player:auth` y la emisión para los profesores en `manager:studentList`.
  - [ ] Escudar y bloquear `player:join` revisando si la configuración exige logueo de alumnos.
  - **📍 Archivos Afectados:** `index.ts` (Eventos Websocket), `services/game.ts` (Reglas del objeto Game).
  - **⚠️ Impacto:** Afecta directamente el flujo de unirse a una partida (`14-game-flow.md`). Posible bloqueo de jugadores legítimos.
  - **🧪 Pruebas:** Lanzar una partida de prueba con 2 pestañas. Intentar unirse a un juego forzando la negación sin usar PIN; asegurar que la conexión envíe un `"errorMessage"` en vez de fallar silenciosamente.

- [ ] **Historial y Reportes (CSV)**
  - [ ] Reemplazar la persistencia básica de apodos de UI y atar el `student_id` UUID en las instancias finales del cuestionario.
  - [ ] Modificar `exportCsv()` para añadir `JOIN` SQL y sacar la cédula y nombre del estudiante desde el Payload JSON.
  - **📍 Archivos Afectados:** `services/history.ts`
  - **⚠️ Impacto:** Modifica la lectura y visualización directa de los maestros; la data histórica (Legacy) podría no ser retrocompatible, generándoles CSVs rotos.
  - **🧪 Pruebas:** Descargar el CSV desde el UI de un manager y verificar que las columnas nuevas (`Nombre Real`, `Matrícula`) salgan correctamente tabuladas, y como vacías (`""`) en partidas viejas.

- [ ] **Frontend & UI (Manejo de Interfaces)**
  - [ ] Interfaz prev-ingreso Jugador (Pedir Matrícula antes del nombre/PIN si aplica).
  - [ ] Interfaz *Manager/Admin*: Crear componente de vista del salón y asignación.
  - **📍 Archivos Afectados:** `packages/web/src/pages/PlayerGamePage.tsx`
  - **⚠️ Impacto:** La experiencia central del estudiante. Errores de renders dejarán pantalla blanca (White Screen of Death).
  - **🧪 Pruebas:** Navegar al fronten simluando un estudiante en modo Incógnito; correr ambas vías globales (Config a TRUE vs Config a FALSE) y asegurar la renderización.

---

## 2. (Nueva sección para ideas futuras)
- [ ] ...
