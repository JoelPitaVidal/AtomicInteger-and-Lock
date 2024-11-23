# AtomicInteger y Lock en Java

## AtomicInteger

`AtomicInteger` es una clase en Java que proporciona una manera eficiente y segura para manejar variables enteras de forma atómica. Forma parte del paquete `java.util.concurrent.atomic`, que incluye herramientas para manejar operaciones concurrentes sin usar bloqueos explícitos.

### Características:
1. **Operaciones Atómicas**: Todas las operaciones proporcionadas por `AtomicInteger` son atómicas. Esto significa que son indivisibles y no pueden ser interrumpidas por otros hilos durante su ejecución.
2. **Seguridad en Entornos Concurrentes**: Ideal para usar en programas multihilo sin necesidad de sincronización explícita.
3. **Basado en Comparación y Asignación (CAS)**: Utiliza instrucciones a nivel de hardware para garantizar la atomicidad de las operaciones.

### Métodos Comunes:
- `get()`: Devuelve el valor actual.
- `set(int newValue)`: Establece un nuevo valor.
- `incrementAndGet()`: Incrementa el valor en 1 y luego lo devuelve.
- `decrementAndGet()`: Decrementa el valor en 1 y luego lo devuelve.
- `addAndGet(int delta)`: Añade un valor al actual y devuelve el resultado.
- `compareAndSet(int expected, int update)`: Actualiza el valor si coincide con el esperado.

### Ejemplo:
```java
import java.util.concurrent.atomic.AtomicInteger;

public class AtomicExample {
    public static void main(String[] args) {
        AtomicInteger counter = new AtomicInteger(0);

        // Incremento atómico
        int newValue = counter.incrementAndGet();
        System.out.println("Nuevo valor: " + newValue);

        // Comparación y actualización
        boolean updated = counter.compareAndSet(1, 10);
        System.out.println("¿Actualización exitosa? " + updated);
        System.out.println("Valor actual: " + counter.get());
    }
}
```

## Lock

`Lock` es una interfaz en Java que proporciona un mecanismo más flexible que los bloques `synchronized` para controlar el acceso a recursos compartidos entre múltiples hilos. Forma parte del paquete `java.util.concurrent.locks`.

### Características:
1. **Mayor Control**: A diferencia de `synchronized`, los bloqueos `Lock` permiten un mayor control sobre la sincronización.
2. **Condiciones Asociadas**: Los objetos `Lock` pueden tener múltiples condiciones asociadas, lo que permite notificaciones más específicas a los hilos.
3. **Intentos con Tiempo Límite**: Permiten probar el bloqueo con un tiempo límite, evitando bloqueos indefinidos.

### Métodos Principales:
- `lock()`: Adquiere el bloqueo. Si el bloqueo ya está en uso, el hilo actual se detiene hasta que esté disponible.
- `unlock()`: Libera el bloqueo.
- `tryLock()`: Trata de adquirir el bloqueo inmediatamente, devolviendo un booleano.
- `tryLock(long time, TimeUnit unit)`: Intenta adquirir el bloqueo dentro de un período de tiempo especificado.
- `newCondition()`: Devuelve una nueva instancia de `Condition` asociada a este bloqueo.

### Ejemplo:
```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class LockExample {
    private final Lock lock = new ReentrantLock();

    public void safeMethod() {
        lock.lock();
        try {
            // Sección crítica
            System.out.println("Recurso compartido protegido");
        } finally {
            lock.unlock();
        }
    }

    public static void main(String[] args) {
        LockExample example = new LockExample();

        // Ejecución segura en múltiples hilos
        Runnable task = example::safeMethod;
        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);

        t1.start();
        t2.start();
    }
}
```

## Comparación entre AtomicInteger y Lock

- **Uso Principal**:
  - `AtomicInteger`: Ideal para operaciones simples sobre un valor único.
  - `Lock`: Más adecuado para proteger secciones críticas complejas o múltiples recursos.

- **Performance**:
  - `AtomicInteger`: Generalmente más rápido debido a su implementación basada en CAS.
  - `Lock`: Puede ser más lento, pero ofrece flexibilidad.

- **Código Complejo**:
  - `AtomicInteger`: Requiere menos código y es más fácil de usar para operaciones simples.
  - `Lock`: Proporciona un control más detallado y requiere mayor atención al manejo adecuado del bloqueo y desbloqueo.

### Conclusión
Ambas herramientas son fundamentales en la programación concurrente en Java. La elección entre `AtomicInteger` y `Lock` depende del caso de uso específico y la complejidad del programa.
