# Guía de estilo de codificación Angular

## Introducción

- Esta guía cubre una serie de convenciones de estilo para el código de aplicaciones Angular.

- Estas recomendaciones no son necesarias para que Angular funcione, sino que establecen un conjunto de prácticas de codificación que promueven
la consistencia en todo el ecosistema Angular. Un conjunto consistente de prácticas facilita compartir código y moverse entre proyectos.

- Esta guía _no_ cubre TypeScript ni prácticas generales de codificación no relacionadas con Angular. Para TypeScript, consulta la [guía de estilo de TypeScript de Google] (https://google.github.io/styleguide/tsguide.html).

### En caso de duda, prioriza la consistencia

Siempre que te encuentres con una situación en la que estas reglas contradicen el estilo de un archivo concreto,
da prioridad a mantener la consistencia dentro de ese archivo. Mezclar diferentes convenciones de estilo en un solo
archivo genera más confusión que apartarse de las recomendaciones de esta guía.

## Nomenclatura

### Separa las palabras en los nombres de archivo con guiones

- Separa las palabras dentro de un nombre de archivo con guiones (`-`). Por ejemplo, un componente llamado `UserProfile` tiene como nombre de archivo `user-profile.ts`.

### Usa el mismo nombre para los tests de un archivo con `.spec` al final

- Para las pruebas unitarias, termina los nombres de archivo con `.spec.ts`. Por ejemplo, el archivo de pruebas unitarias para el componente `UserProfile` se llama `user-profile.spec.ts`.

### Haz coincidir los nombres de archivo con el identificador TypeScript interno

- Los nombres de archivo deben describir generalmente el contenido del código dentro del archivo. Cuando el archivo contiene una clase TypeScript, el nombre del archivo debe reflejar el nombre de esa clase. Por ejemplo, un archivo que contiene un componente llamado `UserProfile` debe llamarse `user-profile.ts`.

- Si el archivo contiene más de un identificador principal nombrable, elige un nombre que describa el tema común del código dentro. Si el código de un archivo no encaja en un tema o área de funcionalidad común, considera dividir el código en diferentes archivos. Evita nombres de archivo demasiado genéricos como `helpers.ts`, `utils.ts` o `common.ts`.

### Usa el mismo nombre de archivo para el TypeScript, el template y los estilos de un componente

- Los componentes suelen constar de un archivo TypeScript, un archivo de template y un archivo de estilos. Estos archivos deben compartir el mismo nombre con diferentes extensiones. Por ejemplo, un componente `UserProfile` puede tener los archivos `user-profile.ts`, `user-profile.html` y `user-profile.css`.

- Si un componente tiene más de un archivo de estilos, añade al nombre palabras adicionales que describan los estilos específicos de ese archivo. Por ejemplo, `UserProfile` podría tener los archivos de estilos `user-profile-settings.css` y `user-profile-subscription.css`.

## Estructura del proyecto

### Todo el código de la aplicación va en un directorio llamado `src`

- Todo tu código Angular de UI (TypeScript, HTML y estilos) debe estar dentro de un directorio llamado `src`. El código no relacionado con la UI, como archivos de configuración o scripts, debe estar fuera del directorio `src`.

- Esto mantiene el directorio raíz de la aplicación consistente entre diferentes proyectos Angular y crea una separación clara entre el código de UI y el resto del código de tu proyecto.

### Inicia tu aplicación en un archivo llamado `main.ts` directamente dentro de `src`

- El código para arrancar, o **bootstrap**, una aplicación Angular debe estar siempre en un archivo llamado `main.ts`. Este representa el punto de entrada principal de la aplicación.

### Agrupa los archivos estrechamente relacionados en el mismo directorio

- Los componentes Angular constan de un archivo TypeScript y, opcionalmente, de un template y uno o más archivos de estilos. Debes agruparlos en el mismo directorio.

- Las pruebas unitarias deben estar en el mismo directorio que el código bajo prueba. Evita agrupar pruebas no relacionadas en un solo directorio `tests`.

### Organiza tu proyecto por áreas de funcionalidad

- Organiza tu proyecto en subdirectorios basados en las funcionalidades de tu aplicación o en temas comunes del código en esos directorios. Por ejemplo, la estructura de proyecto para un sitio de cine, MovieReel, podría ser así:

    ```
    src/
    ├─ movie-reel/
    │ ├─ show-times/
    │ │ ├─ film-calendar/
    │ │ ├─ film-details/
    │ ├─ reserve-tickets/
    │ │ ├─ payment-info/
    │ │ ├─ purchase-confirmation/
    ```

- Evita crear subdirectorios basados en el tipo de código que contienen. Por ejemplo, evita crear directorios como `components`, `directives` y `services`.

- Evita meter tantos archivos en un directorio que se vuelva difícil de leer o navegar. A medida que crece el número de archivos en un directorio, considera dividirlo en subdirectorios adicionales.

### Un concepto por archivo

- Prefiere que los archivos fuente se centren en un solo _concepto_. Para clases Angular en concreto, esto suele significar un componente, directiva o servicio por archivo. Sin embargo, está bien que un archivo contenga más de un componente o directiva si tus clases son relativamente pequeñas y están relacionadas como parte de un concepto único.

- En caso de duda, opta por el enfoque que lleve a archivos más pequeños.

## Inyección de dependencias

### Prefiere la función `inject` sobre la inyección de parámetros en el constructor

- Prefiere usar la función `inject` en lugar de inyectar parámetros en el constructor. 
- La función `inject` funciona igual que la inyección por parámetros de constructor, pero ofrece varias ventajas de estilo:

    *   `inject` es generalmente más legible, especialmente cuando una clase inyecta muchas dependencias.
    *   Es sintácticamente más sencillo añadir comentarios a las dependencias inyectadas.
    *   `inject` ofrece mejor inferencia de tipos.
    *   Al compilar a ES2022+ con [`useDefineForClassFields`](https://www.typescriptlang.org/tsconfig/#useDefineForClassFields), puedes evitar separar la declaración e inicialización de campos cuando estos dependen de dependencias inyectadas.

[Puedes refactorizar código existente a `inject` con una herramienta automática](reference/migrations/inject-function).

## Componentes y directivas

### Elección de selectores de componentes

- Consulta la [guía de componentes para más detalles sobre la elección de selectores](guide/components/selectors#choosing-a-selector).

### Nombrado de miembros de componentes y directivas

- Consulta la guía de componentes para más detalles sobre [nombrar propiedades de entrada](guide/components/inputs#choosing-input-names) y [nombrar propiedades de salida](guide/components/outputs#choosing-event-names).

### Elección de selectores de directivas

- Las directivas deben usar el mismo [prefijo específico de aplicación](guide/components/selectors#selector-prefixes) que tus componentes.

- Al usar un selector de atributo para una directiva, usa un nombre de atributo en camelCase. Por ejemplo, si tu aplicación se llama "MovieReel" y creas una directiva que añade un tooltip a un elemento, podrías usar el selector `[mrTooltip]`.

### Agrupa propiedades específicas de Angular antes que los métodos

- Los componentes y directivas deben agrupar sus propiedades específicas de Angular juntas, normalmente al inicio de la declaración de la clase. Esto incluye dependencias inyectadas, entradas, salidas y queries. Define estas y otras propiedades antes que los métodos de la clase. Esta práctica facilita encontrar las APIs de template y dependencias de la clase.

### Mantén los componentes y directivas centrados en la presentación

- El código dentro de tus componentes y directivas debe estar generalmente relacionado con la UI mostrada en la página. 

- Para código que tenga sentido por sí mismo, desacoplado de la UI, es mejor refactorizarlo en otros archivos. Por ejemplo, puedes extraer reglas de validación de formularios o transformaciones de datos en funciones o clases separadas.

### Evita lógica demasiado compleja en los templates

- Los templates de Angular están diseñados para permitir [expresiones similares a JavaScript](guide/templates/expression-syntax). Debes aprovechar estas expresiones para capturar lógica relativamente sencilla directamente en las expresiones del template.

- Cuando el código de una template se vuelve demasiado complejo, refactoriza la lógica al código TypeScript (normalmente con un [computed](guide/signals#computed-signals)). No hay una regla estricta que determine qué constituye "complejo". Usa tu mejor
criterio.

### Usa `protected` en los miembros de clase que solo se usen en el template de un componente

- Los miembros públicos de una clase de componente definen intrínsecamente una API pública accesible mediante inyección de dependencias y [queries](guide/components/queries). Prefiere el acceso `protected` para cualquier miembro que deba ser leído solo desde el template del componente:

    ```ts
    @Component({
    ...,
    template: `<p>{{ fullName() }}</p>`,
    })
    export class UserProfile {
        firstName = input();
        lastName = input();

    // `fullName` no forma parte de la API pública del componente, pero se usa en el template.
    protected fullName = computed(() => `${this.firstName()} ${this.lastName()}`);
    }
    ```

### Usa `readonly` en propiedades inicializadas por Angular

- Marca como `readonly` las propiedades de componentes y directivas que son inicializadas por Angular. Esto incluye las propiedades inicializadas mediante `input`, `model`, `output`, y las consultas (`queries`). El modificador de acceso `readonly` asegura que el valor establecido por Angular no sea sobrescrito:

    ```ts
    @Component({/* ... */})
    export class UserProfile {
        readonly userId = input();
        readonly userSaved = output();
    }
    ```
- Para los componentes y directivas que utilizan las API @Input, @Output y query basadas en decoradores, este consejo se aplica a las propiedades de salida y las consultas (queries), pero no a las propiedades de entrada:

    ```ts
    @Component({/* ... */})
    export class UserProfile {
        @Output() readonly userSaved = new EventEmitter<void>();
        @ViewChildren(PaymentMethod) readonly paymentMethods?: QueryList<PaymentMethod>;
    }
    ```

### Prioriza `class` y `style` en lugar de `ngClass` y `ngStyle`

- Prefiere los enlaces (bindings) de `class` y `style` en lugar de usar las directivas [`NgClass`](/api/common/NgClass) y [`NgStyle`](/api/common/NgStyle):

    ```html
    <!-- PREFER -->
    <div [class.admin]="isAdmin" [class.dense]="density === 'high'">
    <div [class]="{admin: isAdmin, dense: density === 'high'}">

    <!-- AVOID -->
    <div [ngClass]="{admin: isAdmin, dense: density === 'high'}">
    ```

- Tanto los bindings de `class` como los de `style` utilizan una sintaxis más directa que se alinea estrechamente con los atributos HTML estándar. Esto hace que tus templates sean más fáciles de leer y entender, especialmente para los desarrolladores familiarizados con el HTML básico.

- Además, las directivas `NgClass` y `NgStyle` incurren en un costo de rendimiento adicional en comparación con la sintaxis de enlace integrada de `class` y `style`.

- Para más detalles, consulta la [guía de bindings](/guide/templates/binding#css-class-and-style-property-bindings).


### Nombra los manejadores de eventos por lo que _hacen_, no por el evento que los desencadena

- Prefiere nombrar los manejadores de eventos por la acción que realizan en lugar de por el evento que los desencadena:

    ```html
    <!-- PREFER -->
    <button (click)="guardarDatosUsuario()">Guardar</button>

    <!-- AVOID -->
    <button (click)="manejarClick()">Guardar</button>
    ```

    Usar nombres significativos como este hace que sea más fácil saber qué hace un evento al leer el template.

- Para los eventos de teclado, puedes usar los modificadores de eventos de tecla de Angular con nombres de manejadores específicos:

    ```html
    <textarea (keydown.control.enter)="enviarNotas()" (keydown.control.space)="mostrarSugerencias()">
    </textarea>
    ```

- A veces, la lógica de manejo de eventos es especialmente larga o compleja, lo que hace poco práctico declarar un único manejador con un buen nombre. En estos casos, está bien recurrir a un nombre como 'handleKeydown' y luego delegar a comportamientos más específicos basados en los detalles del evento:

    ```ts
    @Component({/* ... */})
    class RichText {
        handleKeydown(event: KeyboardEvent) {
            if (event.ctrlKey) {
                if (event.key === 'B') {
                    this.activarNegrita();
                } else if (event.key === 'I') {
                    this.activarCursiva();
                }
                // ...
            }
        }
    }
    ```


### Mantén los métodos de ciclo de vida simples

- Evita poner lógica larga o compleja dentro de los hooks del ciclo de vida como `ngOnInit`. En su lugar, prefiere crear métodos con nombres descriptivos para contener esa lógica y luego _llamar a esos métodos_ en tus hooks del ciclo de vida. Los nombres de los hooks del ciclo de vida describen _cuándo_ se ejecutan, lo que significa que el código en su interior no tiene un nombre significativo que describa lo que está haciendo.

    ```typescript
    // PREFER
    ngOnInit() {
        this.iniciarRegistro();
        this.ejecutarTareaEnSegundoPlano();
    }

    // AVOID
    ngOnInit() {
        this.logger.setMode('info');
        this.logger.monitorErrors();
        // ...y todo el resto del código que estaría contenido en estos métodos.
    }
    ```


### Usa las interfaces de los hooks de ciclo de vida

- Angular proporciona una interfaz de TypeScript para cada método del ciclo de vida. Al añadir un hook de ciclo de vida a tu clase, importa e `implementa` estas interfaces para asegurar que los métodos tengan el nombre correcto:

    ```ts
    import {Component, OnInit} from '@angular/core';

    @Component({/* ... */})
    export class UserProfile implements OnInit {

        // La interfaz `OnInit` asegura que este método tenga el nombre correcto.
        ngOnInit() { /* ... */ }
    }
    ```
