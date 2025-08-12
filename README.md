# ESTRUCTURA DEL PROYECTO
<img width="1136" height="1886" alt="image" src="https://github.com/user-attachments/assets/b3b94dce-aaab-48da-9521-4f04e585c2dd" />

Qué hace cada capa (resumen):
- app: interacción con usuario (CLI ahora; REST o UI después).
- domain: entidades y reglas invariantes (sin dependencias externas).
- service: orquesta casos de uso (usa repositorios, valida, aplica negocio).
- repository: interfaces (puertos) que definen cómo accedemos a datos.
- persistence: implementaciones concretas (memoria/archivo/JDBC).
- validation/exception/util: soporte transversal.
- config: creación “manual” de dependencias (sin framework aún).
- test: espejo de las capas para tests unitarios.

Reparto de trabajo (para no pisarse):
/Sugerencia: crear ramas feat/<area>-<breve> y PRs hacia dev. main siempre estable.

-Equipo Dominio & Validación

---domain/*, validation/*, exception/*, value/*

---Entregable: entidades con invariantes (p.ej., un Turno no puede estar en el pasado; un Profesional no atiende dos turnos a la misma hora).

-Equipo Servicios (casos de uso)

---service/*, coordina con repos.

---Entregable: TurnoService con crear/listar/cancelar/reprogramar + tests.

-Equipo Persistencia (Iteración 1: memoria)

---persistence/memory/* implementa interfaces repo.

---Entregable: repos en memoria con datos de ejemplo.

-Equipo CLI (app)

---app/* y config/*

---Entregable: menú de consola: crear/listar/cancelar/reprogramar, selección por clínica/sucursal.

-QA/Tests/Build

---test/*, scripts/*, docs/*

---Entregable: cobertura básica JUnit5, scripts de verificación, README vivo.

Convenciones rápidas (acuerdos de equipo)
-Ramas: main estable; dev integración; feat/<area>-<breve>, fix/*, test/*.
-Commits (prefijo): feat:, fix:, docs:, test:, refactor:, chore:.
-Paquetes: com.github.comechingones.turnos.<capa>.
-Clases: PascalCase, tests como XxxTest.
-Mensajes y validaciones: nunca System.out en servicios/repos; usar excepciones + devolver resultados claros.
-README vivo: cómo correr, cómo testear, qué módulos hay, tareas abiertas.

.gitignore (Gradle/IntelliJ):
- Gradle
.gradle/
build/

- IntelliJ
.idea/
*.iml
out/
