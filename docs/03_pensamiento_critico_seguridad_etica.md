# 03. Pensamiento Crítico: Dilemas Éticos, Seguridad de la Información Blockchain y Mitigación de Riesgos en Código Abierto

> **Asignatura:** Gestión de Proyectos de Software  
> **Institución:** Instituto Tecnológico de Tlaxiaco (Sistemas Tec Tlaxiaco)  
> **Equipo:** Grupo 4  
> **Criterio Evaluado:** Indicador D: Pensamiento crítico mediante tecnologías (Valor: 5.0 pts — Rango Excelente: 4.8 – 5.0)

---

## 1. Introducción: Más Allá del Tecnofetichismo

La adopción de tecnologías descentralizadas en proyectos sociales suele verse nublada por discursos tecnofetichistas que promueven que *"la blockchain resolverá automáticamente la corrupción y la injusticia"*. Desde la perspectiva crítica de la ingeniería de software y el compromiso social del Instituto Tecnológico de Tlaxiaco, rechazamos esta visión simplista.

La tecnología no es neutral; encarna supuestos políticos, económicos y operativos. Aplicar contratos inteligentes y almacenamiento inmutable en el contexto de la **defensa comunitaria de la tierra en la Región Mixteca** exige un ejercicio riguroso de pensamiento crítico para identificar los dilemas éticos, los vectores de ataque sobre la seguridad de las personas y los riesgos intrínsecos del desarrollo de código abierto cuando es ejecutado por equipos universitarios pequeños con recursos limitados.

```
+-----------------------------------------------------------------------------------------+
|                  DILEMAS ÉTICOS Y DE SEGURIDAD EN BLOCKCHAIN RURAL                      |
|                                                                                         |
|  [Dilema 1: Inmutabilidad vs Vida]    [Dilema 2: Autonomía vs Colonialismo Cripto]     |
|  - El dato on-chain no se borra.      - La tecnología no puede suplantar la asamblea.  |
|  - ¿Qué ocurre si se filtra un PII    - "Code is NOT law": el código es subordinado   |
|    o un manantial sagrado?             a los Sistemas Normativos Indígenas.             |
|  Solución: Lapidación criptográfica   Solución: Contratos como archivo de fe pública,  |
|  (Cryptographic Tombstoning).          no como árbitros autónomos de decisiones.        |
|                                                                                         |
|  [Dilema 3: Anonimato vs Calumnia]   [Seguridad: Modelo de Amenazas STRIDE]            |
|  - Protección de la vida (DID) vs      - Espionaje en antenas 2G y nodos RPC públicos.  |
|    envenenamiento con precedentes falsos.- Ausencia de backdoors ("onlyOwner" eliminado).|
|  Solución: Redes de Confianza (WoT)    Solución: Purga local previa + Redes de relevo.  |
|  con quórum progresivo de asamblea.                                                     |
+-----------------------------------------------------------------------------------------+
```

---

## 2. Cuestionamientos Éticos Fundamentales en Blockchain

### 2.1. La Paradoja de la Inmutabilidad frente a la Seguridad Humana
El principio fundamental de las redes blockchain e IPFS es la **inmutabilidad**: una vez que un registro se sella en el libro mayor o un bloque se añade a la red distribuida, este no puede ser alterado ni purgado por ninguna entidad.
* **El Peligro Crítico:** En litigios de defensa territorial contra cárteles madereros o corporaciones mineras, la revelación involuntaria de información sensible (nombres de testigos protegidos, firmas manuscritas, geolocalización precisa de tomas de agua o cuevas ceremoniales) pone en riesgo inmediato la integridad física de las familias.
* **Evaluación Crítica:** Asumir que la inmutabilidad es siempre una virtud es un error de diseño fatal. Si un archivo con datos sensibles es subido a IPFS y anclado a Stellar, ningún juez ni desarrollador podrá "eliminarlo" de la cadena global.
* **Mecanismo de Mitigación Propuesto (Lapidación Criptográfica / *Cryptographic Tombstoning*):**
  1. **Purga Previa Obligatoria (CA-3.1):** El software no permite el cálculo del hash ni la subida sin antes ejecutar un pipeline automatizado de despojo de metadatos (limpieza de etiquetas XMP, datos EXIF, marcas de agua digitales de escáner y OCR de firmas personales).
  2. **Anulación Lógica en Contrato (Tombstone):** Si un recurso debe ser invalidado por solicitud de la asamblea comunal, el contrato en Soroban incluye una función de *tombstoning* que no borra la historia previa (lo cual violaría las reglas de la VM), pero marca el hash como `REVOKED_BY_COMMUNITY`, instruyendo a todos los clientes ligeros locales a purgar el contenido de su base de datos SQLite y bloquear su despliegue en la interfaz.

### 2.2. Tecnosoberanía vs. Criptocolonialismo Digital
En los últimos años, diversos proyectos Web3 han intentado imponer a comunidades indígenas estructuras de "Gobernanza DAO" basadas en votaciones tokenizadas (*token-weighted voting*), donde quien posee más monedas tiene más votos.
* **Cuestionamiento Ético:** La sustitución de los sistemas de cargos y las asambleas comunitarias por esquemas algorítmicos foráneos constituye una forma de **colonialismo digital**. En la cosmovisión comunal mixteca, la autoridad no emana del capital computacional ni de la acumulación de tokens, sino del servicio comunitario (*tequio*), el prestigio moral y el consenso asambleario.
* **Solución de Diseño:** La arquitectura propuesta **subordina el software a la comunidad**, nunca al revés. El contrato inteligente en Soroban y el estándar de firmas no deciden ni juzgan; operan estrictamente como un **notario digital inalterable** que registra y certifica los consensos alcanzados previamente de viva voz en la Asamblea Comunal (CA-4.1).

### 2.3. Dialéctica entre Anonimato Defensivo (DID) y Responsabilidad Comunitaria (WoT)
* **La Tensión:** La historia de usuario **US-03** exige anonimato mediante identidades autosoberanas efímeras para salvaguardar a defensores en riesgo extremo. Sin embargo, un sistema 100% anónimo y sin filtros abre la puerta a que actores maliciosos (apoderados de empresas mineras o esquiroles locales) inyecten jurisprudencia falsa, documentos apócrifos o actas amañadas para sembrar confusión en la comunidad.
* **Resolución Crítica:** Se establece un modelo de **confianza escalonada y progresiva**:
  * Cualquier defensor en riesgo puede cargar un precedente con DID efímero (US-03).
  * El sistema etiqueta inmediatamente el registro como: `Estado: Pendiente de Respaldo Comunitario`. El documento es visible, pero la interfaz advierte explícitamente su naturaleza no acreditada.
  * Únicamente cuando las autoridades comunales acreditadas (US-04) firman digitalmente el expediente mediante multifirma respaldada por asamblea, el recurso adquiere la categoría de `Precedente Certificado en Web of Trust`. Esto neutraliza la desinformación sin comprometer la vida del aportante anónimo.

---

## 3. Modelo de Amenazas a la Seguridad de la Información (Análisis STRIDE)

Para garantizar la rigurosidad en la seguridad del sistema frente a adversarios con recursos estatales o corporativos, se aplicó la metodología de modelado de amenazas **STRIDE** adaptada al entorno rural mixteco:

```
+--------------------------------------------------------------------------------------------+
|                        MATRIZ DE AMENAZAS STRIDE EN LA MIXTECA                             |
+-------------------+------------------------------------+-----------------------------------+
| Amenaza STRIDE    | Vector de Ataque Concreto          | Contramedida y Criterio CA        |
+-------------------+------------------------------------+-----------------------------------+
| Spoofing          | Suplantación del Comisariado       | Firmas EIP-712/Soroban Auth con   |
| (Suplantación)    | mediante llaves privadas robadas.  | esquema multifirma (k de n) CA-4.1|
+-------------------+------------------------------------+-----------------------------------+
| Tampering         | Alteración local de amparos en     | Verificación local contra hash    |
| (Alteración)      | teléfonos o bases SQLite.          | SHA-256 inmutable de la red CA-5.2|
+-------------------+------------------------------------+-----------------------------------+
| Repudiation       | Autoridad coludida desconoce firma | Sello de tiempo on-chain y evento |
| (Repudio)         | legítima otorgada en asamblea.     | LegalResourceRegistered CA-1.2/4.3|
+-------------------+------------------------------------+-----------------------------------+
| Information       | Monitoreo de tráfico celular 2G y  | Purga local de PII (CA-3.1),      |
| Disclosure (Fuga) | nodos RPC públicos para rastrear IP| enrutamiento por relés (CA-3.3).  |
+-------------------+------------------------------------+-----------------------------------+
| Denial of         | Bloqueo selectivo de nodos RPC o   | Arquitectura Offline-First con    |
| Service (DoS)     | corte deliberado de fibra óptica.  | réplicas locales IPFS/SQLite CA-2 |
+-------------------+------------------------------------+-----------------------------------+
| Elevation of      | Backdoor o llave maestra de        | Eliminación absoluta de roles     |
| Privilege         | "administrador" en el contrato.    | 'onlyOwner' en el código CA-4.4   |
+-------------------+------------------------------------+-----------------------------------+
```

### 3.1. Mitigación contra el Rastreo en Nodos RPC Centralizados
La mayoría de las dApps comerciales conectan las billeteras de los usuarios directamente a proveedores centralizados de infraestructura RPC (como Infura o Alchemy). 
* **El Riesgo en Zonas Rurales:** En comunidades con una única celda de telefonía celular, una corporación con acceso a datos de telecomunicaciones puede correlacionar una llamada RPC a una hora exacta con la dirección IP y la celda telefónica del defensor que subió la demanda agraria.
* **Contramedida Técnica (CA-3.3):** El cliente ligero jamás emite transacciones vinculando la IP de origen directamente al contenido. La generación del hash y la firma se realizan de manera 100% aislada en el dispositivo móvil; la transacción puede ser transportada físicamente en una memoria USB cifrada hacia una cabecera con acceso público anónimo o enviarse a través de nodos comunitarios de relevo (*relays*) con mezcla de tráfico.

---

## 4. Mitigación de Riesgos en Desarrollo y Auditoría de Código Abierto

### 4.1. La Realidad Universitaria frente al Mercado de Auditorías Web3
En la industria cripto corporativa, una auditoría de seguridad formal de contratos inteligentes realizada por firmas transnacionales (como Trail of Bits u OpenZeppelin) oscila entre **$40,000 y $120,000 USD** con listas de espera de meses. 

Para un **equipo universitario pequeño del Instituto Tecnológico de Tlaxiaco**, con recursos económicos nulos y plazos académicos ajustados, pretender una auditoría de esta índole es inviable. Sin embargo, omitir la verificación de seguridad bajo el pretexto de *"somos estudiantes"* resultaría negligente y éticamente inaceptable tratándose de software que resguarda la seguridad de comunidades indígenas.

Por tanto, se diseñó un **Protocolo de Seguridad y Calidad de Bajo Costo y Alta Rigurosidad**:

```
+------------------------------------------------------------------------------------------+
|                 PIPELINE DE SEGURIDAD Y AUDITORÍA PARA EQUIPOS PEQUEÑOS                  |
|                                                                                          |
|  [1. Análisis Estático]    [2. Pruebas Fuzzing]      [3. Invariantes]   [4. Dependencias]|
|  - Cargo Clippy (Rust)     - Soroban Test Fuzzing    - Pruebas formales  - Cargo Audit   |
|  - Slither (Solidity)      - Generación aleatoria      de conservación   - Lockfile fijo |
|  - Semgrep (JavaScript)      de estados límite         de fondos/hashes  - Zero-bloat    |
+------------------------------------------------------------------------------------------+
```

### 4.2. Pipeline Automatizado de Mitigación de Vulnerabilidades

1. **Análisis Estático Exhaustivo y Gratuito:**
   * **Contratos Soroban (Rust):** Ejecución automatizada de `cargo clippy -- -D warnings` configurado con advertencias estrictas para prevenir desreferencias inseguras, conversiones de tipos con pérdida de precisión y rutas de ejecución no controladas.
   * **Contratos EVM / Drips (Solidity):** Análisis continuo con **Slither**, identificando automáticamente riesgos de reentrancy, shadowing de variables de estado y llamadas externas desprotegidas.
   * **Linter de Seguridad en Cliente:** Uso de **Semgrep** con reglas específicas de seguridad OWASP para aplicaciones móviles y clientes locales.

2. **Pruebas Basadas en Propiedades e Invariantes (Property-Based Testing):**
   * En lugar de diseñar únicamente pruebas unitarias manuales para casos felices, se implementan pruebas con el framework `proptest` en Rust:
     * **Invariante 1 (Inmutabilidad Estricta):** *“Dado cualquier CID v1 registrado con éxito, ninguna combinación de parámetros, cuentas llamantes o bloques futuros puede alterar el mapeo original o permitir el registro duplicado del mismo identificador”* (CA-1.4).
     * **Invariante 2 (Consistencia Contable):** En la integración con Drips, *“El balance total reclamable jamás puede superar los fondos reales depositados en el contrato, bajo ninguna secuencia temporal de reclamo”*.

3. **Blindaje de la Cadena de Suministro de Software (Supply Chain Attacks):**
   * El 70% de las brechas en proyectos de código abierto provienen de paquetes maliciosos introducidos en repositorios públicos (`npm`, `crates.io`).
   * **Mitigación Estricta:**
     * Congelamiento absoluto de versiones mediante `Cargo.lock` y `package-lock.json` versionados en Git.
     * Auditoría automatizada de vulnerabilidades conocidas mediante `cargo audit` y `npm audit` ejecutados en cada commit local.
     * Política de dependencias mínimas (*Zero-Bloat Dependency Policy*): rechazo tajante a importar bibliotecas externas voluminosas para funciones criptográficas o utilitarias que puedan ser resueltas con librerías estándar bien probadas.

4. **Auditoría Cruzada Académica y Comunitaria:**
   * Establecimiento de revisiones de código por pares (*peer-reviews*) entre equipos de la carrera de Ingeniería en Sistemas Computacionales del IT Tlaxiaco.
   * Publicación abierta del código bajo licencia libre (MIT/GPL), permitiendo que la comunidad global de desarrolladores de Stellar y activistas de derechos humanos puedan inspeccionar y reportar brechas a través de GitHub Issues antes de cualquier despliegue en redes principales (*Mainnet*).
