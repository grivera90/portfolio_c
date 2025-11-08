# Portfolio C - Embedded Firmware Projects

Este repositorio contiene una colección de proyectos de firmware embebido desarrollados en C, demostrando buenas prácticas de programación, patrones de diseño y arquitecturas limpias para sistemas embebidos. Siempre serán bien recibidas las sugerencias y aportes. 

## 🎯 Objetivo

Mostrar experiencia práctica en desarrollo de firmware embebido, incluyendo:
- Drivers de bajo nivel para periféricos.
- Patrones de diseño aplicados a sistemas embebidos.
- Arquitecturas desacopladas y testeables.
- Código mantenible y escalable.

## 📁 Estructura del Repositorio

Cada proyecto está contenido en su propio directorio como submódulo Git, permitiendo desarrollo y versionado independiente.

## 🚀 Proyectos

### 1. Multi-Driver System: TCA8418 Keypad + TCA9554 I/O Expander

**Directorio:** [`tca8418_driver_manager/`](./tca8418_driver_manager)

En este proyecto intento dejar expresado la ventaja de trabajar por capas haciendo uso del patrón Hardware Proxy, un mecanismo de IPC (Inter Process Communication) y gestión de múltiples periféricos I2C compartiendo el mismo bus en un ESP32S3. Tenemos un driver que implementa un teclado matricial más un driver expansor de I/O's, mostrando la versatilidad y reutilización del patrón de diseño.

#### Componentes

**TCA8418 Keypad Driver**
- Driver para el IC TCA8418 (Keypad Scan Controller) de Texas Instruments
- Implementa el patrón **Hardware Proxy** para abstracción del hardware
- Configuración de matriz de teclas (hasta 8x10)
- Detección de eventos de tecla (presión y liberación) por polling o interrupción.
- Buffer FIFO interno para hasta 10 eventos
- Comunicación I2C mediante BSP compartido

**TCA9554 I/O Expander Driver**
- Driver completo para el IC TCA9554 (8-bit I/O Expander) de Texas Instruments.
- Implementa el mismo patrón **Hardware Proxy** demostrando reutilización del patrón.
- Control individual de cada uno de los 8 pines GPIO adicionales.
- Configuración de modo input/output por pin.
- Configuración de polaridad por pin (inversión lógica).
- Lectura y escritura de estados (individual o puerto completo).
- Control de salidas: Set, clear y toggle de pines.
- Comparte bus I2C con el TCA8418.
- Soporte multi-instancia con diferentes direcciones I2C (configurable mediante pines A0, A1, A2).
- Comunicación I2C hasta 400 kHz (Fast Mode).

**Keyboard Manager**
- Modulo que consume eventos del TCA8418.
- Gestión mediante cola (queue) inicializada y compartida desde la aplicación/otro módulo.
- Procesamiento asíncrono de eventos de teclas.
- Desacoplamiento total entre hardware y lógica de aplicación.
- Permite integración flexible en diferentes contextos de aplicación.

**BSP (Board Support Package)**
- Abstracción del hardware específico de la placa.
- **I2C compartido:** Interfaz única para ambos drivers (TCA8418 y TCA9554)
- Gestión de bus compartido con múltiples dispositivos.
- GPIO, timers y otros periféricos necesarios.
- Facilita portabilidad entre MCUs y placas diferentes.

#### Arquitectura del Sistema

```
[TBD]
```

#### Características Técnicas

- **Patrón de Diseño:** Hardware Proxy Pattern (aplicado consistentemente en ambos drivers)
- **Comunicación:** I2C compartido entre múltiples dispositivos (hasta 400 kHz Fast Mode)
- **Arquitectura:** 4 capas desacopladas (Hardware → BSP → Driver → Manager/App)
- **Gestión de Eventos:** Cola FIFO compartida desde aplicación para keyboard manager
- **Portabilidad:** BSP abstracto permite cambio de plataforma sin modificar drivers
- **Reutilización:** Mismo patrón aplicado a diferentes periféricos
- **Escalabilidad:** Fácil adición de nuevos dispositivos I2C al sistema
- **Multi-dispositivo:** Gestión de múltiples dispositivos en el mismo bus I2C

#### Casos de Uso del Sistema Completo

**Sistema Integrado**
- Sistema de control con teclado y salidas adicionales
- Panel de control industrial con LEDs, botones y relés
- Dispositivo IoT con interfaz de usuario local
- Sistema de acceso con keypad y control de actuadores
- Prototipo que requiere expansión de GPIO sin cambiar MCU

**TCA9554 Individual**
- Expansión de GPIO en MCUs con pines limitados
- Control de LEDs, relés y cargas de baja corriente
- Lectura de múltiples botones o sensores digitales
- Multiplexación de señales digitales
- Interfaces con múltiples dispositivos que requieren control on/off

---

*💡 **Nota:** Cada proyecto cuenta con su propio README detallado dentro de su directorio, incluyendo instrucciones de uso, ejemplos de código y documentación de API.*

---

## 🛠️ Stack Tecnológico

- **Lenguaje:** C (C99/C11)
- **Arquitectura:** ARM Cortex-M (adaptable)
- **Protocolos:** I2C, UART, SPI
- **Herramientas:** GCC, Make, Git

## 📚 Patrones de Diseño Implementados

- [x] Hardware Proxy Pattern
- [ ] Observer Pattern *(próximamente)*
- [ ] State Machine Pattern *(próximamente)*
- [ ] Factory Pattern *(próximamente)*

## 🎓 Conceptos Demostrados

### Arquitectura de Software
- Separación de responsabilidades
- Inversión de dependencias
- Interfaces abstractas

### Sistemas Embebidos
- Drivers de periféricos
- Gestión de recursos limitados
- Programación orientada a eventos
- Manejo de interrupciones

### Buenas Prácticas
- Código limpio y documentado
- Nomenclatura consistente
- Modularidad y reutilización
- Abstracción de hardware para portabilidad

## 📖 Cómo Usar Este Repositorio

Cada proyecto incluye:
- `README.md` específico con documentación detallada
- Código fuente completamente comentado
- Diagramas de arquitectura (cuando aplica)
- Ejemplos de uso

Descarga:

Por cada repositorio ejecutar:

- git submodule init
- git submodule update

O en el proyecto para inicializar y descargar todos los sub-modulos:

- git clone --recursive git@github.com:grivera90/portfolio_c.git

## 🔄 Actualizaciones Futuras

Este portfolio está en constante evolución. Próximos proyectos incluirán:
- Implementaciones de protocolos de comunicación
- Máquinas de estado para control de dispositivos
- Gestores de memoria y recursos
- Drivers para sensores y actuadores
- Ejemplos de RTOS (Real-Time Operating Systems)

## 👤 Autor

**Gonzalo Rivera**

- GitHub: [https://github.com/grivera90](https://github.com/tu-usuario)
- LinkedIn: [https://www.linkedin.com/in/gonzalo-rivera-7072a262/](https://linkedin.com/in/tu-perfil)
- Email: gonzaloriveras90@gmail.com

---

⭐ Si encuentras útil este repositorio, dale una estrella, comprame un cafecito, un panchito, una coca-cola, un helado (no tengo hambre). 