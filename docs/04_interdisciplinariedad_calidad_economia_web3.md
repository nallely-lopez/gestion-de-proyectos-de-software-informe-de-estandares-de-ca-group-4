# 04. Convergencia Interdisciplinaria: Ingeniería de Calidad de Software y Economía Descentralizada (Web3)

> **Asignatura:** Gestión de Proyectos de Software  
> **Institución:** Instituto Tecnológico de Tlaxiaco (Sistemas Tec Tlaxiaco)  
> **Equipo:** Grupo 4  
> **Criterio Evaluado:** Indicador E: Actividades y conocimientos interdisciplinarios (Valor: 5.0 pts — Rango Excelente: 4.8 – 5.0)

---

## 1. Introducción y Marco Interdisciplinario

El éxito de una infraestructura de software orientada a bienes públicos no depende únicamente de la elegancia de su arquitectura técnica, sino de su **viabilidad económica y sostenibilidad operativa en el tiempo**. Históricamente, innumerables proyectos universitarios y de impacto social con excelente diseño mueren pocos meses después de su entrega académica debido a la falta de financiamiento para infraestructura, abandono del mantenimiento por parte de los egresados y asfixia burocrática en los canales tradicionales de donación.

Este informe articula de manera interdisciplinaria dos campos del conocimiento:
1. **La Ingeniería de Calidad de Software:** Enfoques formales de aseguramiento de calidad (QA), matrices de verificación basadas en la norma ISO/IEC 25010 y cumplimiento estricto de criterios de aceptación (*Acceptance Criteria*).
2. **La Economía Web3 y Criptoeconomía de Bienes Públicos:** Mecanismos descentralizados de asignación de capital mediante subvenciones por hitos verificables (**Stellar Community Fund - SCF**) y transmisión continua de fondos segundo a segundo (**Drips Protocol v2**).

```
+-----------------------------------------------------------------------------------------------+
|                    CONVERGENCIA: CALIDAD DE SOFTWARE + CRIPTOECONOMÍA                         |
|                                                                                               |
|  [Ingeniería de Software]                                      [Economía Descentralizada]     |
|  - Criterios de Aceptación (CA-1.1 al CA-5.4)                 - Desembolsos por Hitos (SCF)   |
|  - Suites de pruebas automáticas (Rust/Foundry) ------------> - Cero intermediación bancaria  |
|  - Tolerancia a desconexión y bajo costo                      - Transmisión continua (Drips)  |
|                                                                                               |
|                                           |                                                   |
|                                           v                                                   |
|  +-----------------------------------------------------------------------------------------+  |
|  |                     SOSTENIBILIDAD REAL PARA LA REGIÓN MIXTECA                          |  |
|  |  * La calidad del código desbloquea los fondos de desarrollo.                            |  |
|  |  * Los fondos en streaming mantienen la red sin depender de presupuestos de gobierno.   |  |
|  |  * Los costes de transacción microscópicos de Stellar viabilizan el uso en pobreza.    |  |
|  +-----------------------------------------------------------------------------------------+  |
+-----------------------------------------------------------------------------------------------+
```

---

## 2. La Fragilidad de los Modelos de Financiamiento Tradicionales

Las organizaciones comunitarias y los colectivos de derechos humanos en México se enfrentan a barreras estructurales en el sistema financiero convencional:
* **Asfixia Burocrática y Retención de Fondos:** La gestión de fondos a través de donatarias autorizadas o subsidios estatales toma entre 6 y 14 meses de trámites burocráticos. En múltiples ocasiones, activistas territoriales sufren el congelamiento arbitrario de sus cuentas bancarias mediante requerimientos hacendarios o judiciales impulsados por intereses corporativos.
* **Comisiones Devoradoras:** Las transferencias bancarias internacionales pierden entre un 5% y un 12% en tarifas de intermediación cambiaria (SWIFT, corresponsales bancarios y bancos receptores).
* **Condicionamiento Político:** El financiamiento gubernamental suele exigir la subordinación de las demandas comunitarias o la entrega de padrones de comuneros, vulnerando la seguridad de los defensores.

Frente a esta realidad, la economía Web3 provee **rieles financieros inmunes a la censura**, programables y transparentes, donde cada transferencia se rige por contratos matemáticos y no por discrecionalidades políticas.

---

## 3. Stellar Community Fund (SCF): Financiamiento Vinculado a Calidad de Software

El programa **Stellar Community Fund (SCF)** es una iniciativa global de la Stellar Development Foundation (SDF) que otorga subvenciones no dilutivas (*grants*) para proyectos de código abierto que aporten valor al ecosistema Stellar/Soroban.

### 3.1. Modelo de Desembolso Basado en Hitos Verificables (Milestone-Gated Releases)
A diferencia de los subsidios tradicionales donde el capital se entrega por adelantado sin garantías de entrega, el modelo de financiamiento de SCF vincula la entrega de tramos de capital (*tranches*) directamente a la **evidencia técnica del aseguramiento de calidad**:

```
+-----------------------------------------------------------------------------------------------+
|                       MAPEO: CALIDAD DE SOFTWARE <---> TRAMOS SCF                             |
+-----------+-----------------------------------+--------------------+--------------------------+
| Fase SCF  | Hito de Calidad Verificable       | Criterios Cubiertos| Desembolso Estimado      |
+-----------+-----------------------------------+--------------------+--------------------------+
| Fase 1    | Contrato Soroban en testnet con   | CA-1.1, CA-1.2,    | $10,000 USD              |
| (Kickoff) | cobertura unitaria >90% en Rust.  | CA-1.4             | (en tokens XLM)          |
+-----------+-----------------------------------+--------------------+--------------------------+
| Fase 2    | Cliente local Offline-First con   | CA-2.1, CA-5.1,    | $15,000 USD              |
| (Build)   | SQLite/SQLCipher y pruebas de red.| CA-5.2             | (en tokens XLM)          |
+-----------+-----------------------------------+--------------------+--------------------------+
| Fase 3    | Módulo DID con purga de metadatos | CA-3.1, CA-3.2,    | $15,000 USD              |
| (Hardening| y multifirma comunitaria EIP-712. | CA-4.1, CA-4.2     | (en tokens XLM)          |
+-----------+-----------------------------------+--------------------+--------------------------+
| Fase 4    | Pruebas de campo en Mixteca con   | CA-2.4, CA-5.3,    | $10,000 USD              |
| (Launch)  | sincronización 2G/EDGE validada.  | CA-5.4             | (en tokens XLM)          |
+-----------+-----------------------------------+--------------------+--------------------------+
```

### 3.2. Criptoeconomía de Microcostos en Stellar
El aspecto económico fundamental de Stellar para la Región Mixteca radica en su estructura de tarifas:
* **Tarifa Base Fija:** 100 stroops (0.00001 XLM). Con un precio promedio estimado de 0.12 USD por XLM, cada transacción cuesta aproximadamente **$0.0000012 USD**.
* **Impacto Comunitario:** Con tan solo $1.00 USD de fondo comunitario, la plataforma puede ejecutar más de **800,000 registros e indexaciones de recursos jurídicos** en la red. Esto elimina la barrera económica que impide a comunidades indígenas utilizar tecnologías como Ethereum L1, donde una sola transacción en momentos de congestión puede superar los $15 o $40 USD.

---

## 4. Drips Protocol v2: Transmisión Continua de Capital y Splits Solidarios

Mientras que los fondos de SCF impulsan la fase inicial de desarrollo y despliegue del software, **Drips Protocol v2** proporciona el motor de **sostenibilidad económica a perpetuidad**.

### 4.1. Streaming Continuo de Fondos (Real-Time Money Streaming)
En lugar de depender de campañas esporádicas de donación, las organizaciones de derechos humanos, fundaciones internacionales de justicia ambiental y simpatizantes de la causa indígena configuran **flujos continuos de capital (streams)** en USDC o DAI que transfieren micro-fracciones de dólar por segundo hacia la identidad del repositorio en Drips.

* **Flujo Predecible:** Permite al equipo de soporte y a los custodios de nodos comunitarios contar con un flujo de ingresos predecible segundo a segundo, facilitando la planeación del mantenimiento técnico sin incertidumbre mensual.
* **Resiliencia ante la Desconexión:** Dado que Drips almacena la tasa de flujo de manera matemática en el contrato `DripsHub`, los fondos continúan acumulándose para el proyecto aun si los nodos en la Mixteca están desconectados por tormentas durante semanas. Al restablecerse la conectividad, el balance acumulado se reclama en una sola transacción eficiente.

### 4.2. El Grafo de Distribución de Valor Comunitario (Splits Subsystem)
Drips permite programar una matriz de reparto automático (*splits*) en el contrato inteligente. Cada dólar que ingresa por streaming al proyecto se fragmenta de manera determinista sin que ningún tesorero humano pueda retenerlo o desviar fondos:

```
                                +----------------------------------+
                                |  STREAMING DE ENTRADA (100%)     |
                                |  Fondos de Justicia Ambiental    |
                                +----------------------------------+
                                                 |
                                                 v
                                +----------------------------------+
                                |       DRIPS SPLITS HUB           |
                                +----------------------------------+
                                                 |
         +-----------------------+---------------+-----------------------+
         |                       |                               |       |
         v                       v                               v       v
+------------------+   +-------------------+           +------------------+   +------------------+
|   40% Nodos      |   |  25% Estudiantes  |           |   20% Fondo      |   |  15% Traducción  |
|  Comunitarios    |   |  IT Tlaxiaco      |           |   Jurídico       |   |  Mixteco & Vida  |
|  - Equipos locales|   |  - Becas tequio   |           |  - Peritajes y   |   |  - Intérpretes   |
|  - Energía solar |   |    mantenimiento  |           |    asesoría legal|   |    Tu'un Sávi    |
|  - Conexión 2G   |   |  - QA continuo    |           |    en juzgados   |   |  - Fondo socorro |
+------------------+   +-------------------+           +------------------+   +------------------+
```

1. **40% Operación de Infraestructura y Nodos Comunitarios:** Destinado a sufragar los costos de electricidad, pequeñas baterías solares para los routers comunitarios y tarjetas SIM de respaldo para la sincronización diferencial de los nodos IPFS locales en los Comisariados de Bienes Comunales.
2. **25% Becas de Mantenimiento para Estudiantes del IT Tlaxiaco:** Asignación directa y meritocrática para los estudiantes de Ingeniería en Sistemas Computacionales que asuman la responsabilidad del mantenimiento del código, monitoreo de incidentes de seguridad y actualización de dependencias, instituyendo una figura de *Tequio Tecnológico Profesional Remunerado*.
3. **20% Fondo de Litigio Agrario y Peritajes:** Transferencia directa a los abogados comunitarios y peritos en topografía o antropología que defienden los casos en los Tribunales Unitarios Agrarios (TUA).
4. **15% Traducción Lingüística y Protección de Emergencia:** Destinado a traductores certificados en las variantes lingüísticas de la Mixteca (*Tu'un Sávi*) para adaptar sentencias complejas al lenguaje cotidiano de las comunidades, y un fondo de contingencia para el auxilio legal inmediato de defensores en riesgo.

---

## 5. Estrategia Pragmática para Equipos Universitarios Pequeños

Para un equipo con pocos recursos, poco tiempo y compuesto por estudiantes universitarios, la adopción de este modelo interdisciplinario no agrega complejidad innecesaria, sino que **elimina costos fijos**:

1. **Cero Costos de Alojamiento en Nube:** Al operar de forma descentralizada mediante IPFS local y Stellar/Soroban, el proyecto no paga facturas mensuales de servidores AWS, bases de datos remotas ni licencias de software propietario.
2. **Uso de Redes de Prueba (Testnet) y Simuladores Locales:** Todo el desarrollo y validación de calidad se realiza utilizando `stellar sandbox` y entornos de prueba locales sin gastar dinero real durante el semestre académico.
3. **Alineación con Estándares Internacionales:** La vinculación de los criterios de aceptación (CA) con las métricas de SCF y Drips capacita a los estudiantes en tecnologías de vanguardia global (Rust, Soroban, Solidity, Cryptoeconomics) al tiempo que resuelven una problemática social urgente de su propia región.

---

## 6. Conclusiones Interdisciplinarias

La fusión de la **Ingeniería de Software (control de calidad, rigor en criterios de aceptación)** con la **Economía Descentralizada (subvenciones condicionadas y streaming continuo)** transforma este informe académico en un modelo operativo viable y emancipador:
* La **calidad del software** es el aval técnico que garantiza la solidez del sistema y desbloquea el financiamiento internacional.
* La **economía Web3** provee la autonomía financiera indispensable para que la defensa de la tierra en la Mixteca sea sostenible, incorruptible y verdaderamente soberana.
