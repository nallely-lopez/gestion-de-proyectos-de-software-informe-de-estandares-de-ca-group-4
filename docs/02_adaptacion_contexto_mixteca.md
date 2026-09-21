# 02. Adaptación de Métricas de Calidad de Software al Ecosistema Descentralizado y a la Región Mixteca

> **Asignatura:** Gestión de Proyectos de Software  
> **Institución:** Instituto Tecnológico de Tlaxiaco (Sistemas Tec Tlaxiaco)  
> **Equipo:** Grupo 4  
> **Criterio Evaluado:** Indicador A: Adaptación a situaciones y contextos complejos (Valor: 5.0 pts — Rango Excelente: 4.8 – 5.0)

---

## 1. El Desafío de Calidad en Contextos Extremos

La ingeniería de software convencional asume condiciones operativas ideales: conectividad persistente de banda ancha, servidores centralizados de alta disponibilidad en la nube (AWS, Azure), hardware de última generación y modelos jurídicos corporativos estandarizados. 

En el desarrollo de una infraestructura tecnológica descentralizada para la **defensa territorial y jurídica en la Región Mixteca de Oaxaca**, estas premisas resultan completamente inválidas e inoperantes. La calidad de software no puede medirse con métricas abstractas concebidas para entornos corporativos; debe redefinirse y calibrarse frente a la **geografía accidentada, la precariedad de infraestructura, los marcos de derecho consuetudinario indígena y la situación de riesgo físico** que enfrentan las comunidades agrarias.

```
+---------------------------------------------------------------------------------------+
|                 CONDICIONES DEL ENTORNO EN LA REGIÓN MIXTECA                          |
|                                                                                       |
|  [Geografía & Red]           [Energía & Hardware]         [Comunidad & Ley]           |
|  - Sierra Mixteca (cañones)  - Apagones por tormentas     - Sistemas Normativos       |
|  - Conectividad 2G / EDGE    - Teléfonos Android Go         Indígenas (Usos/Costumbres|
|  - Pérdida de paquetes >40%  - RAM: 2-3 GB / 32GB Storage - Asambleas Agrarias        |
|  - Desconexión prolongada    - Batería como recurso vital - Defensa de tierra y agua  |
+---------------------------------------------------------------------------------------+
                                           |
                                           v
+---------------------------------------------------------------------------------------+
|               ADAPTACIÓN DE MÉTRICAS DE CALIDAD (ISO/IEC 25010)                       |
|                                                                                       |
|  * Tolerancia a fallos: Operación 100% Offline-First sin degradación funcional        |
|  * Eficiencia: Cargas útiles ultraligeras (<25 KB por sincronización incremental)     |
|  * Seguridad: Cifrado en reposo AES-256 (SQLCipher) + Despojo de PII en origen        |
|  * Usabilidad: Interfaces resilientes para contextos de alta tensión y bilingüismo   |
+---------------------------------------------------------------------------------------+
```

---

## 2. Diagnóstico Socio-Técnico y Geográfico de la Región Mixteca

La Región Mixteca (abarcando la Mixteca Alta —con cabecera en la Heroica Ciudad de Tlaxiaco—, la Mixteca Baja y la Mixteca de la Costa) presenta particularidades determinantes para la arquitectura de software:

### 2.1. Infraestructura de Telecomunicaciones y Topología de Red
* **Intermitencia Extrema y Desconexión Prolongada:** Fuera de las cabeceras municipales, la cobertura celular se degrada a redes **2G/EDGE** o señal satelital comunitaria precaria. Es habitual experimentar periodos de desconexión total de 48 a 96 horas continuas debido a cortes de fibra óptica en carreteras de montaña o caída de enlaces de microondas.
* **Tasa de Descarte de Paquetes (*Packet Loss*):** Las pruebas en campo registran pérdidas de paquetes de entre 35% y 65% en transferencias móviles, provocando el colapso sistemático de conexiones HTTPS/RPC convencionales que dependen de *handshakes* TCP prolongados y transferencias monolíticas.
* **Latencia Desproporcionada:** Los accesos satelitales comunitarios presentan latencias de ida y vuelta (*RTT*) superiores a 800 ms – 1,400 ms, haciendo inviables los mecanismos de sincronización basados en sondeo (*polling*) continuo.

### 2.2. Restricciones Energéticas y Parque de Dispositivos (Hardware Tier)
* **Vulnerabilidad de la Red Eléctrica:** Durante la temporada de lluvias (junio a octubre), las descargas atmosféricas y deslaves dañan transformadores rurales de CFE, provocando cortes prolongados del suministro eléctrico. El consumo energético del software (uso de CPU/GPU para hashing o cifrado) debe ser mínimo para evitar drenar las baterías de los teléfonos inteligentes de los defensores en campo.
* **Dispositivos Móviles de Gama de Entrada:** El 85% de los defensores comunitarios utiliza teléfonos inteligentes Android de gama baja (frecuentemente ediciones **Android Go** con procesadores quad-core básicos, 2 GB a 3 GB de memoria RAM y almacenamiento interno compartido saturado). El software no puede exigir máquinas virtuales pesadas, navegadores Chromium integrados en segundo plano ni almacenamiento local voluminoso.
* **Equipos Comunitarios Compartidos:** En las oficinas de los Comisariados de Bienes Comunales o bibliotecas comunitarias operan computadoras de escritorio recicladas (Intel Core 2 Duo o Celeron, 4 GB de RAM, discos duros mecánicos HDD con alta fragmentación).

### 2.3. Estructura Jurídica Agraria y Sistemas Normativos Indígenas
* **Marco Constitucional y Legal:** La propiedad en la región es mayoritariamente social: **Comunidades Agrarias** y **Ejidos** regidos por la *Ley Agraria mexicana* y el *Artículo 27 de la Constitución Política de los Estados Unidos Mexicanos*.
* **Gobernanza Comunal (*Usos y Costumbres*):** La máxima autoridad decisoria no es un individuo, sino la **Asamblea General de Comuneros / Ejidatarios**. Las funciones operativas recaen en el **Comisariado de Bienes Comunales** (Presidente, Secretario, Tesorero) y el **Consejo de Vigilancia**.
* **Amenazas Territoriales:** Las comunidades enfrentan litigios de alto impacto contra concesiones mineras a cielo abierto otorgadas sin Consulta Previa, Libre e Informada (conforme al Convenio 169 de la OIT), megaproyectos energéticos, desvío de cuencas de agua y tala clandestina organizada.
* **Criminalización y Amenaza a la Vida:** Los defensores de la tierra sufren hostigamiento judicial, vigilancia estatal ilegal y agresiones armadas. Cualquier fuga de datos personales o metadatos de autoría puede costar la libertad o la vida del defensor.

---

## 3. Reconfiguración del Estándar ISO/IEC 25010 para la Mixteca

El estándar internacional **ISO/IEC 25010** establece ocho características de calidad de software. A continuación, se detalla la reinterpretación y adaptación técnica rigurosa de cada una para este contexto:

```
                  +-------------------------------------------------------------+
                  |            CALIDAD ISO/IEC 25010 ADAPTADA                   |
                  +-------------------------------------------------------------+
                  |                                                             |
                  |  1. Adecuación Funcional  ---> Precisión legal agraria      |
                  |  2. Eficiencia de Rend.  ---> Bajo CPU / Red 2G optimizada  |
                  |  3. Compatibilidad        ---> P2P local sin internet       |
                  |  4. Usabilidad            ---> Interfaz bilingüe y sobria   |
                  |  5. Fiabilidad            ---> Offline-First estricto       |
                  |  6. Seguridad             ---> PII Cero + Cifrado AES-256   |
                  |  7. Mantenibilidad        ---> Código modular, stack ligero |
                  |  8. Portabilidad          ---> Android Go + Linux/Windows   |
                  +-------------------------------------------------------------+
```

### 3.1. Adecuación Funcional (Functional Suitability)
* **Criterio Estándar:** Grado en que el conjunto de funciones cubre todas las tareas declaradas.
* **Adaptación a la Mixteca:** El sistema no debe comportarse como un simple gestor documental genérico, sino como un **bastión de fe pública comunitaria**. La función de inmutabilidad (US-01) debe garantizar que un amparo indirecto o una resolución del Tribunal Unitario Agrario (TUA) permanezca inalterable y pueda presentarse como prueba técnica vinculante ante juzgados federales sin depender de servidores que puedan ser intervenidos o clausurados.

### 3.2. Eficiencia de Desempeño (Performance Efficiency)
* **Criterio Estándar:** Tiempo de respuesta y uso de recursos bajo condiciones normales de servidor.
* **Adaptación a la Mixteca:**
  * **Tiempo de Respuesta en Consulta Local:** La indexación local en SQLite/SQLCipher debe responder en **menos de 1.5 segundos** (CA-2.1) en procesadores móviles de bajo rendimiento sin realizar llamadas de red.
  * **Consumo de Ancho de Banda:** Los paquetes de sincronización diferencial (CA-5.3) deben compactarse en cargas útiles (*payloads*) inferiores a **25 KB** para permitir el intercambio exitoso en conexiones 2G/EDGE intermitentes sin provocar saturación de búfer (*bufferbloat*).
  * **Consumo Energético en Batería:** El proceso de hashing SHA-256 y descifrado de documentos debe ejecutarse en hilos de baja prioridad (*background workers*) sin exceder el 15% de utilización continua del CPU, salvaguardando la autonomía del dispositivo.

### 3.3. Compatibilidad (Compatibility) e Interoperabilidad P2P
* **Criterio Estándar:** Capacidad de compartir información e interactuar mediante APIs estandarizadas en red.
* **Adaptación a la Mixteca:** El protocolo de transporte debe priorizar el descubrimiento y distribución local mediante **mDNS y Bluetooth Low Energy (BLE) / WiFi Direct** antes de intentar resolver solicitudes hacia Internet. Si dos defensores coinciden en una asamblea en una zona sin cobertura, sus dispositivos deben intercambiar y sincronizar expedientes jurídicos locales mediante nodos IPFS locales de forma transparente (CA-2.2).

### 3.4. Usabilidad (Usability) en Ambientes de Tensión
* **Criterio Estándar:** Facilidad de aprendizaje y satisfacción del usuario en interfaces web/móvil.
* **Adaptación a la Mixteca:** 
  * **Simplicidad Cognitiva y Bilingüismo:** Interfaz sobria, visualmente intuitiva, con iconografía clara y compatibilidad con variantes lingüísticas (Mixteco / Tu'un Sávi y Español), accesible para autoridades comunales de edad avanzada.
  * **Operabilidad bajo Estrés Crítico:** La emisión de alertas y la inspección previa de firmas criptográficas (CA-3.4) deben requerir confirmaciones explícitas con resumen textual inequívoco, evitando transacciones involuntarias en situaciones de emergencia o retenes policiales/militares.

### 3.5. Fiabilidad y Tolerancia a Fallos (Reliability)
* **Criterio Estándar:** Capacidad de un sistema para mantener su nivel de servicio en caso de defectos de software o caídas de infraestructura.
* **Adaptación a la Mixteca:** Principio arquitectónico **Offline-First Absoluto** (CA-5.4). La aplicación debe iniciar, consultar, verificar y permitir la preparación de firmas locales aun si el teléfono tiene el modo avión activado de forma permanente. La sincronización es una operación secundaria asíncrona, jamás un requisito bloqueante de la interfaz.

### 3.6. Seguridad (Security) y Preservación de la Vida
* **Criterio Estándar:** Confidencialidad, integridad, no repudio y autenticidad en sistemas de información.
* **Adaptación a la Mixteca:**
  * **Purga Estricta de Metadatos (CA-3.1):** El software debe destruir metadatos EXIF, firmas de programas de digitalización, marcas de agua de escáner y datos de ubicación GPS antes de calcular el hash y emitir cualquier archivo a la red.
  * **Cifrado en Reposo Cero-Conocimiento (CA-5.1):** La base de datos local SQLite debe cifrarse con **SQLCipher (AES-256)**. Si el dispositivo de un defensor es incautado o extraviado en un retén, los expedientes locales permanecen inaccesibles sin la llave criptográfica local derivada por PBKDF2.

### 3.7. Mantenibilidad y Portabilidad (Maintainability & Portability)
* **Criterio Estándar:** Facilidad para modificar y migrar el software a diferentes plataformas operativas.
* **Adaptación a la Mixteca:** El software de cliente ligero debe compilar de forma cruzada como una aplicación nativa autónoma o binario estático ligero, sin requerir dependencias complejas del sistema operativo anfitrión. Debe ejecutarse con fluidez tanto en teléfonos de bajo costo como en computadoras comunales antiguas con distribuciones Linux ligeras o Windows.

---

## 4. Adaptación Técnica Detallada de las Historias de Usuario

A continuación, se demuestra cómo cada criterio de aceptación de las historias de usuario responde directamente a las particularidades de la Región Mixteca:

### US-01: Registro e Inmutabilidad de Recursos Jurídicos
* **Problema Territorial:** Falsificación recurrente de actas de asamblea y sentencias agrarias por parte de apoderados legales corruptos o empresas extractivas.
* **Solución Técnica Adaptada:** El cliente genera el hash SHA-256 y CID v1 en local (CA-1.1). Cuando hay enlace mínimo, se envía la transacción a Soroban en Stellar. El costo de transacción de fracciones de centavo permite que una comunidad registre decenas de acuerdos comunales sin agotar presupuestos. El bloqueo de sobreescritura (CA-1.4) impide que una nueva administración comunal cooptada o un agente externo revoque amparos históricos legítimos.

### US-02: Consulta y Filtrado Seguro de Precedentes
* **Problema Territorial:** Asesores legales en juzgados de distrito (ej. Juzgado de Distrito en Oaxaca o Huajuapan) necesitan citar jurisprudencia agraria o amparos ganados en comunidades hermanas, pero carecen de conectividad celular en los tribunales o en el trayecto.
* **Solución Técnica Adaptada:** La base de datos local SQLite resuelve la búsqueda por tipo de amenaza (minería, agua, tala) en menos de 1.5s de forma offline (CA-2.1 y CA-2.4). El estado de confirmación muestra cuántos respaldos comunitarios tiene el precedente (CA-2.3), orientando la estrategia legal con certeza comunal.

### US-03: Publicación Anónima Protegida (DID)
* **Problema Territorial:** El asesinato y encarcelamiento de defensores comunitarios se origina frecuentemente en la identificación del denunciante a través de filtraciones o números de IP rastreados por compañías telefónicas cómplices.
* **Solución Técnica Adaptada:** Purga local de PII antes de cualquier procesamiento (CA-3.1). Autenticación basada en llaves W3C DID generadas en el dispositivo sin cuentas de correo ni números telefónicos (CA-3.2). Enrutamiento mediante relés ofuscados (CA-3.3) que rompen la asociación entre la celda celular rural y la transacción emitida.

### US-04: Respaldo y Validación Comunitaria (Web of Trust)
* **Problema Territorial:** En los sistemas indígenas no basta la firma individual; la legitimidad emana de la colegialidad de las autoridades (Comisariado + Consejo de Vigilancia) elegidas en Asamblea.
* **Solución Técnica Adaptada:** Esquema multifirma estructurado EIP-712 / Soroban Auth (CA-4.1) que exige el quórum de firmas de las llaves comunitarias antes de elevar un documento a la categoría de "precedente avalado". La inviolabilidad del respaldo (CA-4.4) anula cualquier figura de "superadministrador de plataforma", respetando la autonomía comunal según el Art. 2 de la Constitución.

### US-05: Sincronización Local y Acceso sin Conexión
* **Problema Territorial:** Asambleas bajo árboles comunales o en cerros sagrados donde no existe ningún tipo de señal electromagnética.
* **Solución Técnica Adaptada:** SQLCipher con AES-256 (CA-5.1) y verificación matemática del hash offline (CA-5.2). Al transitar temporalmente por una carretera con señal 2G, la sincronización diferencial descarga únicamente las cabeceras criptográficas de nuevos precedentes en background (CA-5.3), consumiendo un volumen mínimo de datos celulares.

---

## 5. Resumen de Parámetros de Calidad Calibrados

| Métrica de Calidad | Parámetro Convencional (Web / Cloud) | Parámetro Adaptado (Mixteca Descentralizada) |
| :--- | :--- | :--- |
| **Tiempo de respuesta en búsqueda** | < 200 ms contra API Cloud (ElastiSearch/AWS) | < 1.5 s contra base de datos local embebida (SQLite/SQLCipher) |
| **Disponibilidad del servicio (SLA)** | 99.9% uptime de servidores centrales | 100% de operatividad local del cliente (tolerancia a caída total de red) |
| **Tamaño de sincronización de red** | 5 MB – 50 MB (bundles Webpack / REST JSON) | < 25 KB por bloque diferencial incremental |
| **Esquema de autenticación** | OAuth2, JWT, Google/Facebook Auth, email | W3C DID local efímero (ed25519) sin datos personales vinculados |
| **Gobernanza de autorización** | Rol `admin` en base de datos PostgreSQL | Quórum multifirma on-chain validado por Asamblea Comunal |
| **Consumo de recursos en reposo** | Proceso daemon continuo con WebSockets activos | Inactivo; activación por demanda o conexión esporádica (ahorro de batería) |
