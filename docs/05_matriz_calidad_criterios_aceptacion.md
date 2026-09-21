# 05. Matriz Maestra de Estándares de Calidad y Criterios de Aceptación (CA)

> **Asignatura:** Gestión de Proyectos de Software  
> **Institución:** Instituto Tecnológico de Tlaxiaco (Sistemas Tec Tlaxiaco)  
> **Equipo:** Grupo 4  
> **Propósito:** Especificación formal, técnica y verificable de todos los Criterios de Aceptación correspondientes a las Historias de Usuario US-01 a US-05, calibrados bajo la norma ISO/IEC 25010 y el contexto territorial de la Región Mixteca.

---

## 1. Estructura de la Matriz de Calidad

Para asegurar la máxima rigurosidad y trazabilidad técnica en un proyecto universitario de recursos acotados, cada Criterio de Aceptación (CA) se analiza bajo un estándar uniforme de ingeniería de software que contempla:
* **Código y Escenario Operativo:** Contextualización del caso de uso.
* **Resultado Esperado:** Comportamiento determinista del sistema.
* **Métrica ISO/IEC 25010:** Dimensión de calidad de software evaluada.
* **Mecanismo de Implementación Técnica:** Tecnologías y algoritmos concretos (Soroban, IPFS, SQLCipher, DID, EIP-712).
* **Condición de Frontera (Mixteca):** Resiliencia frente a intermitencia, hardware austero o riesgos a la seguridad.
* **Procedimiento de Verificación / Testing:** Estrategia reproducible y automatizada de pruebas para el equipo.

---

## 2. Matriz Detallada por Historia de Usuario

### Historia de Usuario: US-01 — Registro e inmutabilidad de recursos jurídicos exitosos
* **Rol:** Defensor comunitario de tierras en Oaxaca.
* **Objetivo:** Subir recursos legales exitosos (amparos, demandas agrarias, peritajes) con registro inmutable en blockchain.
* **Valor:** Garantizar que la documentación no sea alterada, censurada ni eliminada por intereses gubernamentales o corporativos.

| Código CA | Escenario Operativo | Resultado Esperado | Estándar ISO/IEC 25010 | Mecanismo Tecnológico | Restricción Regional Mixteca | Procedimiento de Verificación |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **CA-1.1** | Carga local y generación de hashes | El sistema procesa el archivo PDF/A en el nodo IPFS local, calculando su hash SHA-256 e identificador CID v1 sin bloquear la UI ni depender de RPC externos. | **Eficiencia de Desempeño** (Tiempo y Recursos) | Daemon IPFS embebido + Web Workers en hilo secundario + Cripto nativa SHA-256 | Procesadores móviles de 4 núcleos (Android Go); RAM < 3 GB; cero tráfico saliente a Internet. | Prueba automatizada midiendo tiempo de CPU (< 800 ms para 15 MB) y validación de cero paquetes de red emitidos (Wireshark/Fiddler). |
| **CA-1.2** | Registro en contrato inteligente | El contrato almacena permanentemente el CID, hash SHA-256, categoría legal y timestamp de bloque, emitiendo el evento indexado `LegalResourceRegistered`. | **Adecuación Funcional** (Completitud) e **Integridad** | Contrato en Soroban (Rust) con `env.storage().persistent()` y emisión de tópicos de eventos | Tarifa base de 100 stroops (0.00001 XLM); no saturación de gas ante cortes de red. | Test unitario con `soroban-sdk::testutils` verificando almacenamiento persistente y captura de evento emitido. |
| **CA-1.3** | Confirmación y persistencia de recibo | El sistema genera y guarda en almacenamiento local seguro un recibo criptográfico con el `txHash`, número de bloque y estado confirmado. | **Fiabilidad** (Tolerancia a fallos) | Almacenamiento local estructurado en SQLite con recibo JSON firmado | Si la red cae inmediatamente tras la confirmación, el recibo queda persistido en almacenamiento no volátil. | Simulación de desconexión abrupta (*kill process*) tras confirmación; verificación de integridad en reinicio. |
| **CA-1.4** | Inmutabilidad y bloqueo de sobreescritura | El contrato inteligente revierte con error cualquier intento de modificación, sobreescritura o eliminación de un CID registrado previamente. | **Seguridad** (No repudio e Integridad de datos) | Clave única primaria en el almacenamiento del contrato: `require(!storage.has(&cid))` | Impide la cooptación legal por parte de nuevas autoridades que pretendan anular amparos históricos. | Prueba de fuzzing e invariantes: intento deliberado de invocar `register_resource` con el mismo CID retornando error `ContractError::AlreadyExists`. |

---

### Historia de Usuario: US-02 — Consulta y filtrado seguro de precedentes por tipo de amenaza
* **Rol:** Asesor legal de una comunidad agraria o ejidal.
* **Objetivo:** Buscar y filtrar estrategias legales registradas según el tipo de amenaza (minería, megaproyectos, agua, tala) y tenencia de la tierra.
* **Valor:** Identificar con rapidez precedentes jurídicos viables y aplicables a la defensa territorial de la comunidad.

| Código CA | Escenario Operativo | Resultado Esperado | Estándar ISO/IEC 25010 | Mecanismo Tecnológico | Restricción Regional Mixteca | Procedimiento de Verificación |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **CA-2.1** | Indexación en almacenamiento local | La búsqueda filtra contra el índice local optimizado (SQLite/PouchDB) en menos de 1.5 segundos, sin saturar nodos RPC públicos. | **Eficiencia de Rendimiento** (Latencia) | Índices B-Tree en SQLite local sobre columnas `threat_type`, `land_tenure`, `state` | Búsquedas fluidas en juzgados de distrito o campo sin depender de llamadas HTTP externas. | Benchmark automatizado ejecutando 50 consultas complejas sobre un catálogo de 2,000 precedentes; límite máximo tolerado: 1,500 ms. |
| **CA-2.2** | Descarga descentralizada por cercanía | El cliente solicita los fragmentos del documento directamente a nodos IPFS locales de la red comunitaria antes de consultar pasarelas externas. | **Compatibilidad** (Interoperabilidad P2P) | Descubrimiento local mDNS y Bitswap en red de área local comunitaria (LAN/WiFi comunitario) | Transferencia de amparos en asambleas sin internet mediante routers comunitarios o enlaces Ad-Hoc. | Aislamiento de pasarela WAN: transferencia de archivo de 5 MB entre dos dispositivos en la misma subred local con éxito 100%. |
| **CA-2.3** | Estado de verificación del precedente | Los resultados despliegan el bloque de confirmación, validez del hash SHA-256 y cantidad de respaldos comunitarios vigentes. | **Usabilidad** (Comprensibilidad y Transparencia) | Componentes visuales sobrios con distintivos de estado criptográfico (Verificado, Huérfano, Revocado) | Visualización rápida para autoridades comunitarias de edad avanzada en asambleas. | Prueba de UI con renderizado de precedentes verificando visibilidad obligatoria del bloque, validez de hash y conteo de firmas. |
| **CA-2.4** | Resiliencia de consulta | La interfaz permite búsquedas y lectura ininterrumpidas incluso con caída de conexión o nodos remotos fuera de servicio. | **Fiabilidad** (Disponibilidad y Madurez) | Modo Offline-First nativo con persistencia en caché local y desacoplamiento de red | Resiliencia total ante apagones y cortes de fibra óptica en la Sierra Mixteca. | Ejecución con la interfaz de red apagada (`netsh interface set interface disable`); verificación de búsqueda funcional sin diálogos de error bloqueantes. |

---

### Historia de Usuario: US-03 — Publicación anónima protegida mediante identidad autosoberana (DID)
* **Rol:** Defensor territorial en situación de alto riesgo.
* **Objetivo:** Compartir estrategias y alertas técnicas utilizando una identidad descentralizada (DID) o firma criptográfica.
* **Valor:** Aportar conocimiento a la red comunitaria sin revelar identidad personal ni exponer a la comunidad a represalias armadas o judiciales.

| Código CA | Escenario Operativo | Resultado Esperado | Estándar ISO/IEC 25010 | Mecanismo Tecnológico | Restricción Regional Mixteca | Procedimiento de Verificación |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **CA-3.1** | Depuración de PII y metadatos | El cliente purga en local metadatos ocultos (geolocalización EXIF, datos del software creador y autoría) antes de generar el hash. | **Seguridad** (Confidencialidad y Privacidad) | Filtro local de sanitización de binarios PDF/imágenes (`qpdf`, `exiftool` o módulo nativo de purga de streams de metadatos) | Erradicación de marcas de tiempo y modelos de escáner que permitan rastrear el municipio o la oficina del defensor. | Prueba de inspección forense: inyección deliberada de coordenadas GPS y autoría en PDF; verificación de que el archivo final carece 100% de dichos campos. |
| **CA-3.2** | Autenticación basada en DID efímero | El acceso y autorización se realizan mediante llaves criptográficas compatibles con el estándar W3C DID, sin contraseñas ni registros personales. | **Seguridad** (Autenticación Descentralizada) | Método `did:key` derivado de curvas ed25519 generadas localmente en memoria segura | Cero formularios con nombres, números celulares, correos electrónicos o números telefónicos. | Prueba de ciclo de vida de llave: generación de DID efímero, firma de payload y verificación matemática sin solicitar credenciales al usuario. |
| **CA-3.3** | Ofuscación de red y origen | El enrutamiento de la transacción oculta la dirección IP de origen y geolocalización mediante relés o nodos intermediarios seguros. | **Seguridad** (Anonimato de Red) | Red comunitaria de relevo (*Relay Nodes*) con encriptación cebolla o difusión por intermediarios autorizados | Impide la triangulación por parte de corporaciones telefónicas en antenas celulares rurales 2G. | Inspección de paquetes en el nodo validador Stellar: la dirección IP receptora corresponde al relé y no al dispositivo del defensor. |
| **CA-3.4** | Validación previa de firma digital | El usuario inspecciona el resumen del mensaje firmado localmente antes de autorizar la emisión de la transacción definitiva a la red. | **Usabilidad** (Protección contra errores de usuario) | Cuadro de diálogo modal bilingüe con resumen en texto plano del documento y hash antes de comprometer la firma | Previene firmas accidentales bajo coacción o estrés en retenes y operativos de campo. | Prueba de interacción: verificación de bloqueo de emisión de transacción hasta confirmación explícita del resumen presentado. |

---

### Historia de Usuario: US-04 — Respaldo y validación comunitaria de estrategias (Web of Trust)
* **Rol:** Autoridad comunitaria (Comisariado de Bienes Comunales / Consejo Indígena).
* **Objetivo:** Validar y firmar digitalmente las estrategias legales y técnicas aportadas por otras comunidades u organizaciones aliadas.
* **Valor:** Construir una red de confianza que certifique la legitimidad y efectividad comunitaria de los recursos compartidos.

| Código CA | Escenario Operativo | Resultado Esperado | Estándar ISO/IEC 25010 | Mecanismo Tecnológico | Restricción Regional Mixteca | Procedimiento de Verificación |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **CA-4.1** | Emisión bajo esquema multifirma | Cuentas comunitarias autorizadas generan firmas estructuradas (estándar EIP-712 / Soroban Auth) que respaldan expedientes mediante quórum acreditado. | **Seguridad** (No repudio y Autorización colegiada) | Firmas tipadas EIP-712 o marco de autorizaciones multifirma nativo de Soroban (`soroban_sdk::auth`) | Refleja la colegialidad obligatoria de la asamblea agraria (Comisariado + Consejo de Vigilancia). | Test de quórum: contrato rechaza registro de respaldo con \( k-1 \) firmas y lo acepta deterministamente al recibir la firma \( k \). |
| **CA-4.2** | Registro on-chain de validación | El contrato inteligente enlaza de forma persistente la firma comunitaria al hash del documento sin permitir duplicidades. | **Integridad** y **Adecuación Funcional** | Estructura en contrato: `Map<(BytesN<32>, Address), SignatureData>` | Certificación de validez histórica e invulnerabilidad a manipulación posterior. | Verificación de estado on-chain: intento de enviar un respaldo duplicado desde la misma autoridad revierte con `Error::AlreadyEndorsed`. |
| **CA-4.3** | Trazabilidad del respaldo | La interfaz expone la lista de autoridades que firmaron, fecha de la asamblea comunitaria y hash de la transacción de validación. | **Usabilidad** (Transparencia e Información) | Lectura estructurada del log de eventos y metadatos de validación en la vista del expediente | Claridad absoluta para comuneros en asambleas respecto a qué pueblos avalan la estrategia jurídica. | Validación de interfaz con expediente avalado: renderizado explícito de los nombres de los núcleos agrarios firmantes y fecha de asamblea. |
| **CA-4.4** | Inviolabilidad del respaldo legítimo | Ningún rol administrador central tiene facultades técnicas para revocar, censurar o sobrescribir un respaldo emitido conforme a las reglas del contrato. | **Seguridad** (Descentralización estricta) | Arquitectura sin llave de administración maestra (*governance-free / ownerless smart contract*) | Respeto a la libre determinación indígena (Art. 2 Constitucional); ninguna ONG o gobierno puede censurar un acuerdo comunal. | Auditoría de código estática y pruebas de penetración: verificación de ausencia absoluta del modificador `onlyOwner` o métodos privilegiados de revocación arbitraria. |

---

### Historia de Usuario: US-05 — Sincronización local y acceso sin conexión para zonas remotas
* **Rol:** Defensor de tierras que trabaja en comunidades con conectividad nula o intermitente.
* **Objetivo:** Descargar un repositorio local cifrado y verificar la autenticidad de los documentos sin conexión constante a internet.
* **Valor:** Utilizar las herramientas legales en asambleas comunitarias, juzgados de distrito o audiencias en campo.

| Código CA | Escenario Operativo | Resultado Esperado | Estándar ISO/IEC 25010 | Mecanismo Tecnológico | Restricción Regional Mixteca | Procedimiento de Verificación |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **CA-5.1** | Persistencia cifrada en dispositivo | El software descarga colecciones y metadatos en una base de datos local cifrada en reposo mediante AES-256 (SQLCipher). | **Seguridad** (Confidencialidad en Reposo) | Motor SQLCipher con derivación de clave por PBKDF2 (256,000 iteraciones) | Protección crítica ante robo, extravío o incautación física del teléfono en retenes policiales/militares. | Inspección de bajo nivel del archivo `.db`: lectura con editor hexadecimal verificando entropía total y ausencia de texto legible sin la clave de descifrado. |
| **CA-5.2** | Verificación estricta offline | El cliente recalcula y compara el hash SHA-256 de los documentos locales contra el registro criptográfico sin emitir llamadas a la red. | **Fiabilidad** (Autonomía de Operación) | Rutina criptográfica autónoma en WebAssembly / Rust local | Certeza jurídica en asambleas en zonas serranas sin cobertura celular. | Ejecución offline forzada: alteración voluntaria de un byte en un PDF local; el sistema detecta de inmediato el desacople de hash y bloquea su uso. |
| **CA-5.3** | Sincronización diferencial ligera | Al detectar conectividad móvil mínima (2G/EDGE), el sistema descarga solo las cabeceras y bloques faltantes en segundo plano sin reiniciar la descarga. | **Eficiencia de Rendimiento** (Ahorro de Ancho de Banda) | Sincronización basada en *Merkle Trees* y protocolo diferencial por rangos (*HTTP Range / IPFS CAR files*) | Transferencia exitosa con señal celular inestable y paquetes de datos prepago sumamente limitados. | Simulación de canal de red degradado (NetLimiter / Chrome DevTools: 50 kbps, 50% packet drop); sincronización exitosa de lote diferencial sin pérdidas de estado. |
| **CA-5.4** | Autonomía de validación criptográfica | La comprobación de validez de firmas y sellos de tiempo opera íntegramente en local con el último estado sincronizado del cliente ligero. | **Portabilidad** y **Fiabilidad** | Validador local de firmas de consenso y certificados en cliente ligero | Independencia absoluta de la disponibilidad de nodos RPC externos o servidores cloud. | Prueba de desconexión por 30 días: verificación exitosa de recibos y firmas previas utilizando el snapshot local persistido. |

---

## 3. Matriz Resumen de Cobertura de Calidad e Indicadores

| Historia de Usuario | Criterios de Aceptación | Dimensión ISO/IEC 25010 Predominante | Indicador Académico Satisfecho |
| :--- | :--- | :--- | :--- |
| **US-01** (Inmutabilidad) | CA-1.1, CA-1.2, CA-1.3, CA-1.4 | Adecuación Funcional, Seguridad, Desempeño | **Desempeño Técnico Stellar/Drips** (Contratos Soroban, Storage TTL) |
| **US-02** (Búsqueda Segura) | CA-2.1, CA-2.2, CA-2.3, CA-2.4 | Eficiencia de Rendimiento, Fiabilidad, Usabilidad | **Indicador A: Contexto Mixteca** (Consultas < 1.5s, redes locales) |
| **US-03** (Anonimato DID) | CA-3.1, CA-3.2, CA-3.3, CA-3.4 | Seguridad, Usabilidad, Confidencialidad | **Indicador D: Pensamiento Crítico** (Despojo de PII, privacidad de vida) |
| **US-04** (Respaldo Comunitario) | CA-4.1, CA-4.2, CA-4.3, CA-4.4 | Seguridad, Integridad, Usabilidad | **Indicador A & D** (Gobernanza de asamblea, descentralización sin backdoor) |
| **US-05** (Modo Offline-First) | CA-5.1, CA-5.2, CA-5.3, CA-5.4 | Fiabilidad, Seguridad, Portabilidad | **Indicadores A & E** (SQLCipher, sincronización 2G, resiliencia económica) |
