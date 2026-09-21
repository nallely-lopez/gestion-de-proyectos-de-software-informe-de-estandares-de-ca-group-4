# 06. Hoja de Ruta Pragmática: Ejecución para Equipos Universitarios de Recursos Limitados

> **Asignatura:** Gestión de Proyectos de Software  
> **Institución:** Instituto Tecnológico de Tlaxiaco (Sistemas Tec Tlaxiaco)  
> **Equipo:** Grupo 4  
> **Propósito:** Plan de desarrollo, pruebas y despliegue ágil, 100% local y de costo cero, optimizado para un equipo de estudiantes universitarios con tiempo acotado y sin infraestructura en la nube comercial.

---

## 1. Filosofía de Desarrollo: Frugalidad Tecnológica y Eficiencia

El desarrollo de software en universidades públicas como el **Instituto Tecnológico de Tlaxiaco** debe guiarse por el principio de **frugalidad tecnológica**: maximizar el valor entregado y el rigor en calidad utilizando herramientas gratuitas, abiertas y ejecutables íntegramente en computadoras portátiles estándar de los estudiantes, sin adquirir deudas de servidores en la nube ni requerir tarjetas de crédito corporativas.

```
+------------------------------------------------------------------------------------------+
|                    ARQUITECTURA DE DESARROLLO 100% LOCAL (COSTO CERO)                    |
|                                                                                          |
|  [Entorno de Contratos]         [Cliente Ligero Offline]        [Validación y Calidad]   |
|  - Rust + Soroban CLI           - SQLite / SQLCipher            - Cargo Test / Clippy    |
|  - Testnet local (Sandbox)      - Frontend Web estático / PWA   - Simulación de red 2G   |
|  - Cero gasto en Gas real       - IPFS Kubo daemon local        - GitHub Actions (Free)  |
+------------------------------------------------------------------------------------------+
```

---

## 2. Stack Tecnológico Seleccionado (Cero Costo de Operación)

| Capa del Sistema | Tecnología Elegida | Justificación Técnica y Universitaria | Alternativa Descartada (Costosa / Pesada) |
| :--- | :--- | :--- | :--- |
| **Lógica Smart Contract** | **Soroban SDK (Rust)** | Pruebas unitarias nativas ultrarrápidas en CPU sin necesidad de minar bloques locales; tipado seguro. | Hardhat / Ethereum L1 (pesado, requiere minería local y gas costoso). |
| **Almacenamiento Descentralizado** | **IPFS Kubo (Nodo Local) + CAR Files** | Ejecución local en localhost; empaquetado de archivos en formato `.car` para transporte en USB. | Pinata / Infura IPFS (requieren suscripción de pago con tarjeta). |
| **Base de Datos Cliente** | **SQLite + SQLCipher** | Motor embebido sin servidor; archivos únicos de base de datos cifrados con AES-256 en reposo. | PostgreSQL / MongoDB (consumen RAM continua y requieren administración). |
| **Interfaz de Usuario** | **PWA (HTML5, Tailwind, Vanilla JS/TS)** | No requiere emuladores pesados de Android Studio; corre en cualquier navegador y se instala en Android Go. | Flutter / React Native pesados (tiempos de compilación largos en laptops de 8GB RAM). |
| **Criptografía de Identidad** | **TweetNaCl / ed25519-dalek** | Librería criptográfica compacta (<50 KB) para generación de DIDs y firmas locales sin dependencias. | SDKs comerciales pesados de proveedores de identidad centralizados. |

---

## 3. Matriz de Roles y Distribución del Trabajo (Equipo de 4 Integrantes)

Para cumplir con los plazos académicos y maximizar la productividad sin sobrecargar a ningún miembro, el trabajo se distribuye de manera balanceada:

| Integrante | Rol en el Proyecto | Responsabilidades Principales | Entregables Clave |
| :--- | :--- | :--- | :--- |
| **Estudiante 1** | **Líder de Smart Contracts & Web3** | Desarrollo del contrato en Soroban (Rust), lógica de registro inmutable, TTL de almacenamiento y emuladores locales. | Código de contrato en Soroban, pruebas unitarias en Rust, scripts de despliegue en Testnet. |
| **Estudiante 2** | **Desarrollador Frontend & Offline-First** | Desarrollo de la PWA, interfaz bilingüe sobria, base de datos SQLite local y persistencia de recibos criptográficos. | Interfaz de usuario responsiva, integración SQLite/SQLCipher, módulo de búsqueda <1.5s. |
| **Estudiante 3** | **Ingeniero de QA, Seguridad y Redes** | Pipeline de análisis estático, despojo de metadatos (PII), pruebas de conectividad 2G/EDGE y pruebas de estrés local. | Suite de tests automatizados, scripts de purga de metadatos, informe de benchmarks de red. |
| **Estudiante 4** | **Documentador, Contexto Agrario & Drips** | Vinculación con autoridades comunales, marco legal agrario, modelado de Splits en Drips y redacción de especificaciones. | Matriz de criterios de aceptación, manual bilingüe, diagrama económico de Drips. |

---

## 4. Plan de Trabajo en 4 Semanas (Sprints Académicos)

```
Semana 1: Núcleo Criptográfico & Contrato Soroban (US-01, CA-1.1 al 1.4)
Semana 2: Base de Datos Local Offline & Búsqueda Rápida (US-02 y US-05, CA-2.1 al 2.4, CA-5.1)
Semana 3: Purga de PII, Identidad DID & Multifirma Comunitaria (US-03 y US-04, CA-3.1 al 4.4)
Semana 4: Pruebas de Resiliencia 2G, Simulación de Cortes y Cierre de Informe
```

### Sprint 1: El Núcleo de Registro Inmutable
* Configuración del entorno local: instalación de `soroban-cli` y Rust toolchain.
* Codificación de la función `register_legal_resource(cid, sha256, category)` con almacenamiento persistente.
* Implementación de la prueba unitaria que valida el bloqueo de sobreescritura (CA-1.4).

### Sprint 2: Almacenamiento Local y Resiliencia Offline
* Estructuración de la base de datos local en SQLite con tabla de precedentes indizada por tipo de amenaza.
* Pruebas de tiempo de respuesta de consulta local garantizando < 1.5 segundos en colecciones de prueba.
* Integración del cifrado en reposo mediante SQLCipher con frase de paso derivada localmente.

### Sprint 3: Privacidad Defensiva y Gobernanza Comunal
* Creación del script local de sanitización de archivos (eliminación de metadatos EXIF y etiquetas de software).
* Módulo de generación de pares de llaves ed25519 para emisión de DIDs efímeros.
* Codificación de la validación multifirma de autoridades comunales basada en el estándar EIP-712 / Soroban Auth.

### Sprint 4: Verificación en Condiciones Extremas y Entrega
* Pruebas con desconexión forzada de red y limitación de ancho de banda a perfil 2G/EDGE (50 kbps).
* Verificación de cálculo autónomo de hashes SHA-256 de documentos sin conexión.
* Consolidación final del repositorio Git, etiquetado de versión y entrega de informe a la coordinación académica.

---

## 5. Pipeline Automatizado de CI/CD (GitHub Actions Gratuito)

Para asegurar la calidad continua sin consumir recursos de las computadoras del equipo, se incluye una plantilla de integración continua para GitHub Actions que se ejecuta de forma gratuita en cada *Push* o *Pull Request*:

```yaml
name: CI Calidad y Seguridad - Grupo 4

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  verificacion-calidad:
    runs-on: ubuntu-latest
    steps:
      - name: Descargar repositorio
        uses: actions/checkout@v4

      - name: Instalar Rust y dependencias
        uses: dtolnay/rust-toolchain@stable
        with:
          components: clippy, rustfmt

      - name: Chequeo de formato de código
        run: cargo fmt -- --check

      - name: Análisis estático con Clippy (Estándar Cero Advertencias)
        run: cargo clippy -- -D warnings

      - name: Auditoría de seguridad de dependencias
        run: |
          cargo install cargo-audit
          cargo audit

      - name: Ejecución de pruebas unitarias y de invariantes
        run: cargo test --verbose
```

---

## 6. Recomendaciones de Éxito para la Defensa del Proyecto

1. **Demostración Práctica Offline:** Durante la evaluación docente, desactivar físicamente la conexión a Internet de la laptop y ejecutar la búsqueda y verificación criptográfica de un amparo agrario. Esto demostrará fehacientemente el cumplimiento de los estándares ISO/IEC 25010 y la adaptación a la Mixteca.
2. **Explicación del Impacto Social:** Destacar que la elección de Stellar y Drips no responde a una moda tecnológica, sino a la búsqueda de microtarifas (0.00001 XLM) y autonomía financiera frente al hostigamiento bancario que sufren los defensores de derechos humanos.
3. **Humildad y Rigor Académico:** Reconocer con transparencia las fronteras técnicas del proyecto (por ejemplo, la necesidad futura de un despliegue de prueba en campo con autoridades comunales reales), demostrando madurez en la gestión de proyectos de software.
