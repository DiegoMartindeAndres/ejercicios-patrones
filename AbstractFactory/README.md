# Ejercicio guiado: Abstract Factory 🏭🏠

> Patrón creacional (POO). En este ejercicio vas a aplicar **Abstract Factory** para construir *familias de objetos compatibles* sin acoplar tu código a clases concretas.

## Enunciado / Introducción 🧩

Estás trabajando en **SmartHome Installer**, una mini-app de consola que prepara kits de domótica para viviendas.

Cada kit debe incluir una **familia completa de dispositivos compatibles** entre sí:

- Un **Sensor de movimiento** (detecta presencia).
- Un **Enchufe inteligente** (permite encender/apagar un aparato).
- Una **Central domótica** (empareja dispositivos y coordina acciones).

La empresa vende el kit en varias **variantes de protocolo** (familias):

- `ZIGBEE`
- `ZWAVE`
- `WIFI`

El problema: si en tu código haces `new SensorZigbee()`, `new CentralWifi()`… acabarás mezclando protocolos (incompatibles) o llenándolo todo de `if/else` y dependencias directas. 😵‍💫

Tu objetivo es diseñar el sistema para que:

- El **cliente** (la instalación) trabaje con **interfaces** (`SensorMovimiento`, `EnchufeInteligente`, `CentralDomotica`).
- La creación de objetos se centralice en una **fábrica abstracta**.
- Cambiar de protocolo sea tan simple como seleccionar **otra fábrica concreta**.

Este ejercicio está inspirado en:
- Las transparencias: `./Transpas/3-AbstractFactory.md`
- El ejemplo base (misma estructura, distinta temática): `./code/es/uva/poo/abstractfactory/`

---

## Qué vas a construir 🧱

Una mini-aplicación con:

1. **Productos abstractos**: `SensorMovimiento`, `EnchufeInteligente`, `CentralDomotica`
2. **Productos concretos** (por protocolo): `...Zigbee`, `...ZWave`, `...Wifi`
3. **Fábrica abstracta**: `FabricaDomotica` (crea los 3 productos)
4. **Fábricas concretas**: `FabricaDomoticaZigbee`, `FabricaDomoticaZWave`, `FabricaDomoticaWifi`
5. **Cliente**: `InstaladorKitDomotico` (usa la fábrica y “prueba” el kit)
6. **Demo (main)**: `Demo` (selecciona la fábrica y ejecuta el cliente)

---

## Pasos para la implementación (guiados) 🧭

> Recomendación: usa el paquete `es.uva.poo.abstractfactory` o uno equivalente en tu proyecto.

### 1) Crea los **productos abstractos** (interfaces) 🔌

Crea estas interfaces (una por cada producto de la familia):

- `SensorMovimiento` con `String detectarMovimiento();`
- `EnchufeInteligente` con `void encender();` y `void apagar();`
- `CentralDomotica` con `void emparejar();` y `void registrarDispositivos(SensorMovimiento sensor, EnchufeInteligente enchufe);`

Plantillas (rellena los TODO):

```java
public interface SensorMovimiento {
    // TODO: devuelve una descripción del evento (por ejemplo, "Movimiento detectado")
    String detectarMovimiento();
}
```

```java
public interface EnchufeInteligente {
    void encender();
    void apagar();
}
```

```java
public interface CentralDomotica {
    void emparejar();

    // La central registra dispositivos de la MISMA familia (compatibles)
    void registrarDispositivos(SensorMovimiento sensor, EnchufeInteligente enchufe);
}
```

### 2) Crea la **fábrica abstracta** (`FabricaDomotica`) 🏗️

Define una interfaz `FabricaDomotica` con 3 métodos de creación:

- `SensorMovimiento crearSensorMovimiento();`
- `EnchufeInteligente crearEnchufeInteligente();`
- `CentralDomotica crearCentralDomotica();`

Plantilla:

```java
public interface FabricaDomotica {
    SensorMovimiento crearSensorMovimiento();
    EnchufeInteligente crearEnchufeInteligente();
    CentralDomotica crearCentralDomotica();
}
```

### 3) Implementa **una primera familia completa** (Zigbee) 📡

Crea las clases concretas para Zigbee:

- `SensorMovimientoZigbee`
- `EnchufeInteligenteZigbee`
- `CentralDomoticaZigbee`

Sugerencias:
- Los métodos pueden imprimir por consola mensajes del tipo `[ZIGBEE] ...`.
- `SensorMovimientoZigbee.detectarMovimiento()` puede devolver un `String` que luego imprimas.

Ejemplo mínimo (solo guía):

```java
public class EnchufeInteligenteZigbee implements EnchufeInteligente {
    @Override
    public void encender() {
        // TODO: imprimir "[ZIGBEE] Enchufe encendido"
    }

    @Override
    public void apagar() {
        // TODO: imprimir "[ZIGBEE] Enchufe apagado"
    }
}
```

### 4) Implementa la **fábrica concreta** de Zigbee 🧰

Crea `FabricaDomoticaZigbee` que implemente `FabricaDomotica` y devuelva:

- `new SensorMovimientoZigbee()`
- `new EnchufeInteligenteZigbee()`
- `new CentralDomoticaZigbee()`

Plantilla:

```java
public class FabricaDomoticaZigbee implements FabricaDomotica {

    @Override
    public SensorMovimiento crearSensorMovimiento() {
        // TODO
        return null;
    }

    @Override
    public EnchufeInteligente crearEnchufeInteligente() {
        // TODO
        return null;
    }

    @Override
    public CentralDomotica crearCentralDomotica() {
        // TODO
        return null;
    }
}
```

### 5) Implementa el **cliente** (`InstaladorKitDomotico`) 👷

El cliente debe:

1. Recibir una `FabricaDomotica` en el constructor.
2. Crear los productos a través de la fábrica (no con `new ...`).
3. Probar que son compatibles haciendo:
   - `central.emparejar();`
   - `central.registrarDispositivos(sensor, enchufe);`
   - imprimir el resultado de `sensor.detectarMovimiento();`
   - `enchufe.encender();` y `enchufe.apagar();`

Plantilla:

```java
public class InstaladorKitDomotico {

    private final SensorMovimiento sensor;
    private final EnchufeInteligente enchufe;
    private final CentralDomotica central;

    public InstaladorKitDomotico(FabricaDomotica fabrica) {
        // TODO: construir la familia completa usando la fabrica
        this.sensor = null;
        this.enchufe = null;
        this.central = null;
    }

    public void instalarYProbar() {
        // TODO: emparejar, registrar dispositivos, simular detección y encender/apagar
    }
}
```

### 6) Añade una segunda familia (Z-Wave o WiFi) ✅

Repite el proceso del paso 3 y 4 creando:

- Productos concretos (por ejemplo `SensorMovimientoZWave`, etc.)
- Su fábrica concreta (`FabricaDomoticaZWave`)

🎯 Objetivo: que el cliente `InstaladorKitDomotico` NO cambie.

---

## Código cliente (Demo / main) 🧪

> Este es el código de prueba que debe funcionar cuando termines la implementación.

```java
package es.uva.poo.abstractfactory;

public class Demo {

    private static InstaladorKitDomotico configurarAplicacion() {
        FabricaDomotica fabrica;

        // Simulación de configuración. Cambia el valor para probar distintas familias.
        // Opciones: "ZIGBEE", "ZWAVE", "WIFI"
        String protocolo = "ZIGBEE";

        if (protocolo.equalsIgnoreCase("ZIGBEE")) {
            fabrica = new FabricaDomoticaZigbee();
        } else if (protocolo.equalsIgnoreCase("ZWAVE")) {
            fabrica = new FabricaDomoticaZWave();
        } else {
            fabrica = new FabricaDomoticaWifi();
        }

        return new InstaladorKitDomotico(fabrica);
    }

    public static void main(String[] args) {
        InstaladorKitDomotico instalador = configurarAplicacion();
        instalador.instalarYProbar();
    }
}
```

Salida orientativa (puede variar):

```text
[ZIGBEE] Emparejando central...
[ZIGBEE] Registrando sensor y enchufe...
[ZIGBEE] Movimiento detectado en el salón
[ZIGBEE] Enchufe encendido
[ZIGBEE] Enchufe apagado
```

---

## Mini-retos (opcional) 🌟

1) Añade un tercer producto a la familia: `CamaraSeguridad` 📷
- Actualiza `FabricaDomotica` y todas las fábricas concretas.
- Pista: aquí se ve el “coste” del patrón: hay que tocar varias clases.

2) Evita el `if/else` de `Demo` usando:
- Un `switch` (mejora menor) o
- Un `Map<String, FabricaDomotica>` (mejor diseño), o
- Lectura de variable de entorno / argumento de línea de comandos.

---

<details>
  <summary>Necesitas ayuda con el código (solución completa) 🛟</summary>
<br>

#### SensorMovimiento.java

```java
package es.uva.poo.abstractfactory;

public interface SensorMovimiento {
    String detectarMovimiento();
}
```

#### EnchufeInteligente.java

```java
package es.uva.poo.abstractfactory;

public interface EnchufeInteligente {
    void encender();
    void apagar();
}
```

#### CentralDomotica.java

```java
package es.uva.poo.abstractfactory;

public interface CentralDomotica {
    void emparejar();
    void registrarDispositivos(SensorMovimiento sensor, EnchufeInteligente enchufe);
}
```

#### FabricaDomotica.java

```java
package es.uva.poo.abstractfactory;

public interface FabricaDomotica {
    SensorMovimiento crearSensorMovimiento();
    EnchufeInteligente crearEnchufeInteligente();
    CentralDomotica crearCentralDomotica();
}
```

---

### Familia Zigbee

#### SensorMovimientoZigbee.java

```java
package es.uva.poo.abstractfactory;

public class SensorMovimientoZigbee implements SensorMovimiento {

    @Override
    public String detectarMovimiento() {
        return "[ZIGBEE] Movimiento detectado en el salón";
    }
}
```

#### EnchufeInteligenteZigbee.java

```java
package es.uva.poo.abstractfactory;

public class EnchufeInteligenteZigbee implements EnchufeInteligente {

    @Override
    public void encender() {
        System.out.println("[ZIGBEE] Enchufe encendido");
    }

    @Override
    public void apagar() {
        System.out.println("[ZIGBEE] Enchufe apagado");
    }
}
```

#### CentralDomoticaZigbee.java

```java
package es.uva.poo.abstractfactory;

public class CentralDomoticaZigbee implements CentralDomotica {

    @Override
    public void emparejar() {
        System.out.println("[ZIGBEE] Emparejando central...");
    }

    @Override
    public void registrarDispositivos(SensorMovimiento sensor, EnchufeInteligente enchufe) {
        System.out.println("[ZIGBEE] Registrando sensor y enchufe...");

        // En una app real, aquí guardarías referencias, harías handshake, etc.
        if (sensor == null || enchufe == null) {
            throw new IllegalArgumentException("Dispositivos no pueden ser null");
        }
    }
}
```

#### FabricaDomoticaZigbee.java

```java
package es.uva.poo.abstractfactory;

public class FabricaDomoticaZigbee implements FabricaDomotica {

    @Override
    public SensorMovimiento crearSensorMovimiento() {
        return new SensorMovimientoZigbee();
    }

    @Override
    public EnchufeInteligente crearEnchufeInteligente() {
        return new EnchufeInteligenteZigbee();
    }

    @Override
    public CentralDomotica crearCentralDomotica() {
        return new CentralDomoticaZigbee();
    }
}
```

---

### Familia Z-Wave

#### SensorMovimientoZWave.java

```java
package es.uva.poo.abstractfactory;

public class SensorMovimientoZWave implements SensorMovimiento {

    @Override
    public String detectarMovimiento() {
        return "[ZWAVE] Movimiento detectado en el pasillo";
    }
}
```

#### EnchufeInteligenteZWave.java

```java
package es.uva.poo.abstractfactory;

public class EnchufeInteligenteZWave implements EnchufeInteligente {

    @Override
    public void encender() {
        System.out.println("[ZWAVE] Enchufe encendido");
    }

    @Override
    public void apagar() {
        System.out.println("[ZWAVE] Enchufe apagado");
    }
}
```

#### CentralDomoticaZWave.java

```java
package es.uva.poo.abstractfactory;

public class CentralDomoticaZWave implements CentralDomotica {

    @Override
    public void emparejar() {
        System.out.println("[ZWAVE] Emparejando central...");
    }

    @Override
    public void registrarDispositivos(SensorMovimiento sensor, EnchufeInteligente enchufe) {
        System.out.println("[ZWAVE] Registrando sensor y enchufe...");
        if (sensor == null || enchufe == null) {
            throw new IllegalArgumentException("Dispositivos no pueden ser null");
        }
    }
}
```

#### FabricaDomoticaZWave.java

```java
package es.uva.poo.abstractfactory;

public class FabricaDomoticaZWave implements FabricaDomotica {

    @Override
    public SensorMovimiento crearSensorMovimiento() {
        return new SensorMovimientoZWave();
    }

    @Override
    public EnchufeInteligente crearEnchufeInteligente() {
        return new EnchufeInteligenteZWave();
    }

    @Override
    public CentralDomotica crearCentralDomotica() {
        return new CentralDomoticaZWave();
    }
}
```

---

### Familia WiFi

#### SensorMovimientoWifi.java

```java
package es.uva.poo.abstractfactory;

public class SensorMovimientoWifi implements SensorMovimiento {

    @Override
    public String detectarMovimiento() {
        return "[WIFI] Movimiento detectado en la cocina";
    }
}
```

#### EnchufeInteligenteWifi.java

```java
package es.uva.poo.abstractfactory;

public class EnchufeInteligenteWifi implements EnchufeInteligente {

    @Override
    public void encender() {
        System.out.println("[WIFI] Enchufe encendido");
    }

    @Override
    public void apagar() {
        System.out.println("[WIFI] Enchufe apagado");
    }
}
```

#### CentralDomoticaWifi.java

```java
package es.uva.poo.abstractfactory;

public class CentralDomoticaWifi implements CentralDomotica {

    @Override
    public void emparejar() {
        System.out.println("[WIFI] Emparejando central...");
    }

    @Override
    public void registrarDispositivos(SensorMovimiento sensor, EnchufeInteligente enchufe) {
        System.out.println("[WIFI] Registrando sensor y enchufe...");
        if (sensor == null || enchufe == null) {
            throw new IllegalArgumentException("Dispositivos no pueden ser null");
        }
    }
}
```

#### FabricaDomoticaWifi.java

```java
package es.uva.poo.abstractfactory;

public class FabricaDomoticaWifi implements FabricaDomotica {

    @Override
    public SensorMovimiento crearSensorMovimiento() {
        return new SensorMovimientoWifi();
    }

    @Override
    public EnchufeInteligente crearEnchufeInteligente() {
        return new EnchufeInteligenteWifi();
    }

    @Override
    public CentralDomotica crearCentralDomotica() {
        return new CentralDomoticaWifi();
    }
}
```

---

#### InstaladorKitDomotico.java

```java
package es.uva.poo.abstractfactory;

public class InstaladorKitDomotico {

    private final SensorMovimiento sensor;
    private final EnchufeInteligente enchufe;
    private final CentralDomotica central;

    public InstaladorKitDomotico(FabricaDomotica fabrica) {
        this.sensor = fabrica.crearSensorMovimiento();
        this.enchufe = fabrica.crearEnchufeInteligente();
        this.central = fabrica.crearCentralDomotica();
    }

    public void instalarYProbar() {
        central.emparejar();
        central.registrarDispositivos(sensor, enchufe);

        System.out.println(sensor.detectarMovimiento());

        enchufe.encender();
        enchufe.apagar();
    }
}
```

</details>
<br>
