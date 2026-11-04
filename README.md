# Depors — App de campeonatos de microfútbol

**En línea:** [depors.check.snakegym.cloud](https://depors.check.snakegym.cloud)
**301 commits** · proyecto propio, de punta a punta

Sistema para organizar campeonatos de microfútbol de barrio: inscripción de
equipos, sorteo, fase de grupos y cuadro final. El organizador carga los
resultados desde la cancha; cualquiera consulta la tabla desde su celular, sin
registrarse.

---

## Tecnologías

| Capa | Herramientas |
|---|---|
| **Frontend** | Angular 22 (componentes standalone, carga diferida), TypeScript, RxJS |
| **Backend** | Python, FastAPI, Pydantic, psycopg |
| **Base de datos** | PostgreSQL 17 |
| **Móvil** | Capacitor (Android) |
| **Infraestructura** | Docker, Docker Compose, Caddy con TLS automático, VPS Linux |
| **Pruebas** | pytest |

---

## Lo que resuelve

### El sorteo y el calendario se calculan, no se escriben a mano
El organizador elige de 2 a 8 grupos y la aplicación reparte los equipos,
arma el calendario de todos contra todos y genera el cuadro final. Si los
clasificados no cierran un cuadro parejo, lo completa con los mejores terceros
—y cuartos si hace falta—, igual que en el Mundial.

### La tabla de posiciones no se guarda: se calcula
Lo que se almacena es el resultado de cada partido, que es un hecho ocurrido.
La tabla, los goleadores y el cuadro salen de sumarlos en cada consulta. Así, al
corregir un marcador mal cargado la tabla se arregla sola, sin recálculos
manuales que tarde o temprano quedan desfasados.

### Los desempates están a la vista
Puntos, diferencia de gol, goles a favor y enfrentamiento directo, en ese orden
y escritos en la pantalla. En un torneo de barrio el desempate es donde se arma
el problema, y la regla tiene que estar visible antes de que haga falta.

### La lógica difícil son funciones puras
`api/torneo.py` concentra el reparto en grupos, el sorteo, el calendario, la
tabla con sus desempates y el cuadro. Entran datos y salen datos: sin base ni
servidor, se prueba entero con `pytest` en un segundo. Es donde estarían los
errores, así que es lo que está cubierto.

### Tres roles, tres vistas distintas
Un espectador ve la tabla, el calendario, el cuadro, los goleadores, la valla
menos vencida, las tarjetas y el reglamento del torneo. El organizador además
inscribe equipos, corre el sorteo, carga resultados y lleva la contabilidad.
El superadministrador gestiona usuarios y campeonatos archivados. Cada zona
está protegida por su propia guarda de ruta, y el servidor vuelve a comprobar
el permiso: esconder un botón no es seguridad.

### Pensada para cargar resultados de pie en la cancha
La pantalla que más se usa es la de resultados, y se usa con una mano y sin
sentarse. Todo lo demás del diseño se subordinó a eso.

---

## Capturas

<!-- Pendientes: pantalla de inicio y tabla de posiciones -->
