# Dental LC — proyecto

App de gestión para un laboratorio dental. Le permite al laboratorio llevar
trabajos, pagos, créditos y cuenta corriente de los odontólogos/clínicas
que le encargan trabajos.

Este archivo existe para no perder el hilo entre sesiones (igual que se hizo
con el proyecto Andes OO&SS): acá queda el estado, la estructura y las
decisiones tomadas, para que cualquier sesión nueva pueda retomar sin repreguntar.

## Estado actual

- Single-file `index.html` (HTML + CSS + JS todo junto, sin build step).
- Backend: Supabase (REST directo con `fetch`, sin SDK). URL hardcodeada
  en el archivo (`SB_URL`, línea ~238). Revisar que `SB_KEY` sea una anon key
  pública y no una service key antes de compartir el archivo.
- Datos semilla (`SEED_P`, línea ~241) con odontólogos y pacientes de prueba
  (algunos nombres son placeholders tipo "dddddd" — reemplazar por reales
  o vaciar antes de producción).
- Sin build system, sin git remoto todavía (repo local únicamente).

## Estructura funcional (tabs del nav)

- **+ Nuevo**: cargar trabajo — odontólogo, paciente (alta rápida inline),
  fechas de ingreso/entrega, uno o más ítems (tipo de trabajo, cantidad,
  color/tono, piezas dentales sup/inf), precio por ítem, forma de pago,
  adelanto.
- **Pendientes / Todos**: listado de trabajos, filtro por odontólogo y
  buscador, badges de estado.
- **Pagos**: registro y listado de pagos aplicados a trabajos.
- **Creditos**: alta de créditos/adelantos por paciente u odontólogo,
  aplicables después a un trabajo puntual.
- **Cuentas**: cuenta corriente por odontólogo — saldo, detalle de
  trabajos entregados, "pago global" que cancela varios trabajos a la vez.
- **Pacientes**: ficha de paciente con historial de trabajos/pagos/créditos.
- **Historial**: tabla de movimientos (ingresos, pagos, entregas, créditos),
  ordenable por columna, filtrable por odontólogo/paciente.
- **Preciario**: precios por tipo de trabajo, con versiones históricas por
  fecha de vigencia (permite subir precios sin perder el precio vigente de
  trabajos viejos — ver `precioEnFecha()` / `versionVigente()`).
- Exportar/Importar JSON como backup manual (`exportarDatos` / `importarDatos`).

## Funciones clave (para ubicarse rápido en el código)

- `sbPost` / `sbGet` / `sbDel`: capa de acceso a Supabase.
- `loadDB` / `seedSiVacio`: carga inicial y siembra si está vacío.
- `guardarTrabajo`, `editarTrabajo`, `confirmarEliminarTrabajo`: CRUD de trabajos.
- `guardarCredito`, `aplicarCredito`, `eliminarCredito`: CRUD de créditos.
- `renderCuenta`, `pagoGlobal`: cuenta corriente por odontólogo.
- `renderPreciario`, `guardarNuevoPreciario`, `editarPrecio`: versionado de precios.
- `renderHistorial`, `sortHist`: tabla de movimientos.

## Pendiente / a decidir

- Confirmar si el proyecto se queda como single-file o se migra a algo
  modular más adelante (por ahora: **queda single-file**, decisión tomada
  el 25/07/2026).
- Sin remoto git configurado — definir si se sube a GitHub cuando haga falta.
- Revisar exposición de `SB_KEY` en el cliente.

## Historial de cambios

- 2026-07-25: primer commit, se sube el `index.html` tal como estaba
  (sin modificaciones), se arma el repo git local y este `CLAUDE.md`.
