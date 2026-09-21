# 01. Análisis Técnico Riguroso: Requerimientos de Calidad y Gestión Open-Source en Stellar y Drips Protocol

> **Asignatura:** Gestión de Proyectos de Software  
> **Institución:** Instituto Tecnológico de Tlaxiaco (Sistemas Tec Tlaxiaco)  
> **Equipo:** Grupo 4  
> **Criterio Evaluado:** Desempeño Técnico: Análisis de Stellar/Drips (Valor: 5.0 pts — Rango Excelente: 4.8 – 5.0)

---

## 1. Introducción y Marco de Referencia

El presente documento expone un análisis técnico de alta rigurosidad sobre los estándares de calidad de software, arquitectura de ejecución y modelos de gobernanza de código abierto de dos ecosistemas fundamentales de la Web3: **Stellar Network** (con su entorno de contratos inteligentes **Soroban**) y **Drips Protocol v2** (desarrollado sobre Ethereum Virtual Machine en estrecha convergencia con el ecosistema de colaboración distribuida **Radicle**).

Este análisis se fundamenta en la necesidad de diseñar un sistema descentralizado para el **registro, verificación y consulta de recursos jurídicos para la defensa territorial y comunitaria en Oaxaca (Región Mixteca)**, evaluando cómo las garantías de determinismo, consumo eficiente de recursos, inmutabilidad y auditoría abierta de ambas tecnologías satisfacen los requerimientos críticos expresados en las historias de usuario **US-01** a **US-05**.

---

## 2. Análisis Técnico de la Red Stellar y Soroban

### 2.1. Arquitectura de Ejecución y Máquina Virtual (WASM Sandboxing)
Stellar evolucionó de ser una red especializada en pagos transfronterizos a una plataforma integral de cómputo verificable mediante la incorporación de **Soroban**, su plataforma de contratos inteligentes basada en **WebAssembly (WASM)**.

```
+-----------------------------------------------------------------------+
|                           STELLAR NETWORK                             |
|                                                                       |
|  +---------------------------+       +-----------------------------+  |
|  |   Stellar Core (SCP)      | <---> |   Soroban Environment       |  |
|  | - FBA Consensus           |       | - WebAssembly (WASM) VM     |  |
|  | - Quorum Slices           |       | - Deterministic Rust SDK    |  |
|  | - Finality in 3-5 sec     |       | - Gas Metering (CPU/RAM)    |  |
|  +---------------------------+       +-----------------------------+  |
|                 ^                                  ^                  |
|                 |                                  |                  |
|                 v                                  v                  |
|  +---------------------------+       +-----------------------------+  |
|  |   State Storage Layer     |       |   State Archival System     |  |
|  | - Temporary TTL           |       | - Rent-based Expiration     |  |
|  | - Persistent TTL          |       | - Off-chain Restoration     |  |
|  | - Instance Storage        |       |   via Merkle Proofs         |  |
|  +---------------------------+       +-----------------------------+  |
+-----------------------------------------------------------------------+
```

1. **Entorno de Ejecución Determinista:** Soroban ejecuta código compilado a WebAssembly (`wasm32-unknown-unknown`). El motor de ejecución impone determinismo absoluto: elimina cualquier comportamiento indefinido dependiente del hardware anfitrión (como operaciones de punto flotante no canónicas) y utiliza aritmética de enteros de longitud fija (`i128`, `u128`, `i256`, `u256`).
2. **Medición de Recursos (Gas Metering):** A diferencia de modelos simples basados exclusivamente en instrucciones ejecutadas, Soroban desglosa el costo de ejecución en dos dimensiones estrictas:
   * **Instrucciones CPU:** Ciclos de procesamiento consumidos dentro del intérprete WASM.
   * **Memoria RAM y Entrada/Salida:** Cuantificación exacta del tamaño del código WASM cargado y los bytes leídos/escritos en el almacenamiento del libro mayor (*ledger*).
3. **Rust SDK como Estándar de Tipado Seguro:** El desarrollo de contratos en Soroban se realiza primariamente en **Rust**. La biblioteca `soroban-sdk` aprovecha el sistema de tipos de Rust, la gestión de memoria sin recolector de basura (*borrow checker*) y macros de compilación para garantizar que no existan desbordamientos de búfer, accesos a memoria no inicializada ni condiciones de carrera a nivel de código de contrato.

### 2.2. Modelo de Almacenamiento y Archivo de Estado (State Archival)
Uno de los mayores desafíos de calidad y escalabilidad en redes descentralizadas es la degradación del rendimiento por explosión del estado (*state bloat*). Soroban introduce un requerimiento de calidad innovador: el **almacenamiento tarifado por tiempo de vida (TTL - Time To Live)**.

| Tipo de Almacenamiento | Duración / Política | Caso de Uso en Defensa Territorial | Impacto en Calidad |
| :--- | :--- | :--- | :--- |
| **Instance Storage** | Vinculado al contrato; expira si no se renueva el TTL | Configuración del contrato de recursos jurídicos y parámetros de quórum | Evita contratos huérfanos que consuman recursos permanentes |
| **Persistent Storage** | Persiste mientras se pague la renta de almacenamiento; puede renovarse | Mapeo de `CID v1` -> `Hash SHA-256` -> `Timestamp` (US-01, US-04) | Inmutabilidad garantizada con previsión de costo económico |
| **Temporary Storage** | Expira automáticamente al llegar al umbral de bloques fijado | Nonces temporales para firmas EIP-712 / DID efímeros (US-03) | Minimiza drásticamente el costo de transacción en Stellar |

Cuando un dato en *Persistent Storage* expira por falta de saldo de renta, este se mueve al estado de **archivo histórico** (*archived state*). El dato no se destruye de la historia criptográfica, pero se retira de la memoria activa de los validadores. Cualquier usuario puede reactivarlo en el futuro presentando una prueba criptográfica de inclusión en el Merkle Tree correspondiente.

### 2.3. Mecanismo de Consenso Stellar (SCP) y Finalidad
Stellar no utiliza Proof of Work (PoW) ni Proof of Stake tradicional (PoS), sino el **Protocolo de Consenso Stellar (SCP)**, una implementación del modelo de *Acuerdo Bizantino Federado (FBA - Federated Byzantine Agreement)*.
* **Rodajas de Quórum (*Quorum Slices*):** Cada validador selecciona un conjunto de pares en los que confía. La unión transitiva de estas rodajas conforma un quórum global sin requerir una autoridad centralizada.
* **Finalidad Determinista:** Los bloques (*ledgers*) se cierran en un promedio de 3 a 5 segundos con finalidad inmediata e irreversible (sin riesgo de reorganizaciones de cadena como ocurre en minería probabilística).
* **Costos Mínimos de Transacción:** La tarifa base es de 100 *stroops* (0.00001 XLM), lo cual equivale a fracciones microscópicas de centavo de dólar estadounidense. Esto resulta vital para proyectos comunitarios donde el costo por registro no puede depender de las volatilidades del gas de redes como Ethereum L1.

---

## 3. Análisis Técnico de Drips Protocol v2

### 3.1. Arquitectura de Transmisión Continua de Capital (Streaming On-Chain)
**Drips Protocol v2** es un conjunto de contratos inteligentes desplegados en redes EVM diseñado para la distribución continua, transparente y programable de fondos tokenizados (ERC-20). Su núcleo matemático permite el flujo continuo de valor segundo a segundo sin requerir transacciones recurrentes en cada intervalo temporal.

```
+--------------------------------------------------------------------------------+
|                             DRIPS PROTOCOL v2                                  |
|                                                                                |
|  +--------------------------------------------------------------------------+  |
|  |                               DripsHub                                   |  |
|  | - Orquestador central de balances e identidades numéricas (UserIDs)     |  |
|  | - Contabilidad interna sin transferencias ERC-20 continuas              |  |
|  | - Algoritmo de amortización temporal O(1)                                |  |
|  +--------------------------------------------------------------------------+  |
|          ^                                  ^                     ^            |
|          |                                  |                     |            |
|  +-------------------+              +----------------+    +-----------------+  |
|  |   AddressDriver   |              |   RepoDriver   |    |    NFTDriver    |  |
|  | Interfaz con      |              | Vincula repos  |    | Representa      |  |
|  | direcciones EOA   |              | git (Radicle/  |    | flujos como     |  |
|  | y contratos       |              | GitHub) a UIDs |    | activos NFT     |  |
|  +-------------------+              +----------------+    +-----------------+  |
|                                                                                |
|  +--------------------------------------------------------------------------+  |
|  |                             Splits Subsystem                             |  |
|  | - Árboles acíclicos dirigidos (DAG) de reparto porcentual                |  |
|  | - Reparto en cascada hacia dependencias y colaboradores                |  |
|  +--------------------------------------------------------------------------+  |
+--------------------------------------------------------------------------------+
```

1. **DripsHub:** Contrato monolítico y altamente optimizado que mantiene los estados de flujo (*drips*) y reparto (*splits*). En lugar de actualizar balances segundo a segundo en el almacenamiento permanente (lo cual saturaría la red de operaciones `SSTORE`), DripsHub almacena la tasa de flujo (monto por segundo) y el instante de inicio. El balance disponible en cualquier momento \( t \) se computa en tiempo de lectura como:
   $$\text{Balance}(t) = \text{Tasa} \times \min(t - t_{\text{inicio}}, \, t_{\text{fin}} - t_{\text{inicio}})$$
2. **Drivers Especializados:**
   * **AddressDriver:** Permite que billeteras externas estándar (EOA) y contratos multifirma gestionen sus flujos y retiren sus fondos.
   * **RepoDriver:** Enlaza identificadores de repositorios de software (incluyendo hashes de proyectos en Radicle o URLs canónicas en GitHub) con identidades numéricas de recepción en Drips.
   * **NFTDriver:** Emite tokens no fungibles (ERC-721) que encapsulan la titularidad y derechos de cobro de un flujo continuo.
3. **Mecanismo de Splits (Grafos de Distribución):** Drips permite configurar matrices de distribución donde un receptor puede reenviar automáticamente porcentajes específicos de sus ingresos hacia otros identificadores, formando un grafo acíclico dirigido (DAG). Esto permite estructurar redes de solidaridad económica entre equipos de desarrollo, investigadores legales y nodos comunitarios.

### 3.2. Estándares de Calidad y Verificación Formal en Drips
Drips v2 implementa los más altos estándares de calidad de ingeniería de software en Solidity:
* **Inmutabilidad y No-Upgradeability:** Los contratos núcleo de Drips no emplean proxies modificables (*non-upgradeable*). Una vez desplegados, ninguna llave privada ni comité puede alterar las reglas de liquidación o desviar fondos en custodia.
* **Pruebas Basadas en Propiedades (Invariants / Fuzzing):** El repositorio oficial de Drips v2 utiliza el framework **Foundry** ejecutando pruebas de propiedades e invariantes matemáticas (garantizando que la suma total de fondos entrantes sea estrictamente idéntica a la suma de fondos salientes más remanentes, eliminando riesgos de fondos bloqueados o creación indebida de valor).
* **Auditorías Criptográficas Públicas:** El código fuente ha sido auditado por firmas líderes de seguridad en Web3 (incluyendo ABDK Consulting), con informes de auditoría accesibles públicamente en su repositorio.

---

## 4. Gestión de Código Abierto (Open-Source Governance)

La calidad del software en infraestructuras críticas descentralizadas depende intrínsecamente del rigor de sus procesos de gobernanza y colaboración abierta.

### 4.1. Ciclo de Vida de Desarrollo en Stellar
```
Idea Comunitaria ---> SEP / CAP ---> Discusión Pública ---> Prototipo en Rust ---> Votación de Validadores ---> Mainnet
```
1. **Propuestas de Mejora (SEPs y CAPs):**
   * **SEPs (Stellar Ecosystem Proposals):** Definen estándares de interoperabilidad a nivel de aplicación (ej. formatos de autenticación SEP-10, estándares de metadatos de tokens SEP-01).
   * **CAPs (Core Advancement Proposals):** Modificaciones a nivel de protocolo y motor de consenso (como CAP-0046 que introdujo las bases de Soroban). Se debaten abiertamente en GitHub y en reuniones técnicas quincenales transmitidas en vivo.
2. **Repositorios Públicos y Licenciamiento:** Stellar Core y el Soroban SDK se gestionan bajo licencias permisivas de código abierto (**Apache 2.0**), garantizando total libertad de inspección, bifurcación e integración sin cobro de regalías ni patentes restrictivas.
3. **Programa de Recompensas por Errores (Bug Bounty):** La Stellar Development Foundation (SDF) mantiene un programa permanente de bug bounty en plataformas reconocidas (**Immunefi**), con incentivos financieros de hasta $100,000 USD para vulnerabilidades críticas en el runtime WASM o en el protocolo SCP.
4. **Integración y Despliegue Continuo (CI/CD):** Cada *Pull Request* en los repositorios de Stellar/Soroban se somete a matrices automatizadas de pruebas en GitHub Actions: chequeos de formato (`cargo fmt`), análisis estático exhaustivo (`cargo clippy`), pruebas de integración cruzada en múltiples arquitecturas (x86_64, aarch64) y compilación reproducible del binario de validación.

### 4.2. Colaboración Abierta y Soberanía Distribuida en Drips / Radicle
1. **Convergencia con Radicle:** A diferencia de proyectos centralizados en plataformas propietarias corporativas, Drips está concebido para operar en conjunto con **Radicle**, una red P2P de colaboración de código construida sobre Git. Esto asegura que el código fuente, los *issues* y las revisiones no puedan ser censurados o dados de baja por decisiones unilaterales de empresas de hosting.
2. **Licenciamiento Libre Estricto:** Los contratos inteligentes de Drips v2 están licenciados bajo **GPL-3.0**, asegurando que cualquier mejora, bifurcación o trabajo derivado permanezca obligatoriamente como software libre y abierto para la comunidad global.
3. **Despliegues Verificables Deterministas:** Los contratos son compilados y desplegados utilizando `CREATE2`, lo que produce direcciones idénticas en todas las cadenas EVM y permite a cualquier usuario verificar byte por byte que el código en ejecución coincide matemáticamente con el código fuente publicado en el repositorio.

---

## 5. Mapeo Técnico: Stellar y Drips en las Historias de Usuario

A continuación, se detalla la convergencia técnica entre las propiedades analizadas de Stellar/Drips y los requerimientos de la solución para la defensa comunitaria:

| Historia de Usuario | Requerimiento Crítico | Mecanismo en Stellar / Soroban | Integración con Drips Protocol |
| :--- | :--- | :--- | :--- |
| **US-01: Registro de recursos jurídicos** | Almacenamiento inmutable del CID v1 y hash SHA-256 sin dependencia de servidores centrales. | `Persistent Storage` de Soroban con extensión de TTL. Emisión de eventos indexados mediante el subsistema de eventos del SDK. | -- |
| **US-02: Consulta y filtrado seguro** | Consulta ultrarrápida (<1.5s) y resiliencia offline. | Nodos locales indexan el estado del contrato en SQLite local; consultas locales sin transacciones de red ni costo. | -- |
| **US-03: Publicación anónima protegida (DID)** | Publicación desvinculada de datos personales e IPs. | Autorizaciones desacopladas de Soroban (`soroban_sdk::auth`), admitiendo firmas ed25519 provenientes de DIDs efímeros. | -- |
| **US-04: Respaldo comunitario (Web of Trust)** | Validación multifirma que refleje los acuerdos de la Asamblea Comunal. | Esquemas de autorización multifirma nativos de Soroban (`require_auth`) sin llaves maestras de administración central. | Fondos comunitarios recibidos por Drips condicionados a la validación de asambleas acreditadas. |
| **US-05: Sincronización offline y bajo ancho de banda** | Verificación criptográfica local en conexiones 2G/EDGE. | Estados compactos y recibos ligeros verificables offline; bajo consumo de bytes por transacción frente a otras cadenas. | Sincronización diferencial de tasas de flujo de financiamiento sin descargar históricos masivos. |

---

## 6. Conclusiones del Análisis Técnico

1. **Eficiencia y Viabilidad Económica:** El modelo de ejecución WASM de Soroban y su protocolo de consenso SCP ofrecen el equilibrio óptimo entre **costo predecible**, **finalidad instantánea** y **seguridad de tipos en Rust**, haciendo viable la implementación de registros legales sin trasladar cargas económicas a comunidades en resistencia.
2. **Sostenibilidad sin Intermediarios:** Drips Protocol v2 complementa el aspecto funcional del software al proporcionar una infraestructura económica soberana, permitiendo que la preservación del repositorio y el soporte técnico sean financiados de forma continua mediante streaming on-chain transparente.
3. **Calidad por Diseño:** Ambas plataformas ejemplifican la ingeniería de software moderna: pruebas de propiedades, inmutabilidad de contratos núcleo, despliegues reproducibles y gobernanza de código abierto sin puntos únicos de fallo.
