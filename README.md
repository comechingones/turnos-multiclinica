# ESTRUCTURA DEL PROYECTO

turnos-multiclinica/

├─ build.gradle

├─ settings.gradle

├─ gradlew

├─ gradlew.bat

├─ .gitignore

├─ README.md

├─ CHANGELOG.md

├─ docs/

│  ├─ arquitectura.md

│  ├─ decisiones-ADR/

│  └─ diagramas/

├─ scripts/

│  ├─ verificar-entorno.sh        # java/gradle/junit ok

│  └─ cargar-datos-ejemplo.sh     # opcional: semilla de datos

├─ config/

│  └─ app.properties              # configuración simple (ej. ruta de archivo)

└─ src/

   ├─ main/
   
   │  ├─ java/
   
   │  │  └─ com/github/comechingones/turnos/
   
   │  │     ├─ app/                        # Capa de aplicación / CLI
   
   │  │     │  ├─ Main.java                # punto de entrada
   
   │  │     │  ├─ CliMenu.java             # menú de consola
   
   │  │     │  └─ ConsoleIO.java           # utilidades de E/S por consola
   
   │  │     ├─ domain/                     # Modelo de dominio (POJO)
   
   │  │     │  ├─ Clinica.java
   
   │  │     │  ├─ Sucursal.java
   
   │  │     │  ├─ Profesional.java
   
   │  │     │  ├─ Paciente.java
   
   │  │     │  ├─ Especialidad.java
   
   │  │     │  ├─ Turno.java               # fecha/hora, sucursal, profesional, paciente, estado
   
   │  │     │  └─ value/                   # value objects (validación centralizada)
   
   │  │     │     ├─ DNI.java
   
   │  │     │     ├─ Email.java
   
   │  │     │     └─ Id.java
   
   │  │     ├─ service/                    # Reglas de negocio / casos de uso
   
   │  │     │  ├─ TurnoService.java        # crear/listar/cancelar/reprogramar
   
   │  │     │  ├─ PacienteService.java
   
   │  │     │  └─ AgendaService.java
   
   │  │     ├─ repository/                 # Puertos (interfaces)
   
   │  │     │  ├─ TurnoRepository.java
   
   │  │     │  ├─ PacienteRepository.java
   
   │  │     │  └─ ProfesionalRepository.java
   
   │  │     ├─ persistence/                # Adaptadores (implementaciones de repo)
   
   │  │     │  ├─ memory/                  # in-memory para empezar + tests
   
   │  │     │  │  ├─ InMemoryTurnoRepository.java
   
   │  │     │  │  └─ ...
   
   │  │     │  ├─ file/                    # siguiente etapa (CSV/JSON)
   
   │  │     │  │  ├─ CsvTurnoRepository.java
   
   │  │     │  │  └─ JsonTurnoRepository.java
   
   │  │     │  └─ jdbc/                    # etapa posterior (SQLite/H2)
   
   │  │     │     ├─ DataSourceFactory.java
   
   │  │     │     └─ JdbcTurnoRepository.java
   
   │  │     ├─ dto/                        # opcional: objetos de transporte/IO
   
   │  │     │  └─ TurnoDTO.java
   
   │  │     ├─ mapper/                     # mapeos Domain <-> DTO
   
   │  │     │  └─ TurnoMapper.java
   
   │  │     ├─ validation/                 # validaciones y políticas
   
   │  │     │  ├─ TurnoValidator.java
   
   │  │     │  └─ PacienteValidator.java
   
   │  │     ├─ exception/                  # excepciones de dominio/aplicación
   
   │  │     │  ├─ DomainException.java
   
   │  │     │  ├─ NotFoundException.java
   
   │  │     │  └─ ValidationException.java
   
   │  │     ├─ config/
   
   │  │     │  └─ AppConfig.java           # “ensamble” simple de dependencias
   
   │  │     └─ util/
   
   │  │        └─ DateTimeUtils.java
   
   │  └─ resources/
   
   │     ├─ logback.xml                    # (más adelante) logging SLF4J/Logback
   
   │     └─ data-ejemplo/                  # datasets de ejemplo (CSV/JSON)
   
   └─ test/
   
      ├─ java/
      
      │  └─ com/github/comechingones/turnos/
      
      │     ├─ domain/
      
      
      │     │  └─ TurnoTest.java
      
      │     ├─ service/
      
      │     │  └─ TurnoServiceTest.java
      
      │     └─ persistence/
      
      │        └─ memory/
      
      │           └─ InMemoryTurnoRepositoryTest.java
      
      └─ resources/
      
         └─ data-test/

Qué hace cada capa (resumen):
- app: interacción con usuario (CLI ahora; REST o UI después).
-domain: entidades y reglas invariantes (sin dependencias externas).
-service: orquesta casos de uso (usa repositorios, valida, aplica negocio).
-repository: interfaces (puertos) que definen cómo accedemos a datos.
-persistence: implementaciones concretas (memoria/archivo/JDBC).
-validation/exception/util: soporte transversal.
-config: creación “manual” de dependencias (sin framework aún).
-test: espejo de las capas para tests unitarios.

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
