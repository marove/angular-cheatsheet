# Angular Cheatsheet

## 💻 Qué es Angular

- Angular es un framework web Front-End.

- Mantenido por un equipo dedicado de Google, Angular proporciona un amplio conjunto de herramientas, API y bibliotecas para simplificar y optimizar el flujo de trabajo de desarrollo.

- Angular ofrece una plataforma sólida sobre la que crear aplicaciones rápidas y fiables que se adaptan tanto al tamaño del equipo como al tamaño de la base de código.


**Diferencia entre Angular y AngularJS**:

* Angular 1.x  == AngularJS
* Angular 2+   == Angular


## 🚀 Instalación

1.  **Instalar Node.js LTS** junto con NPM.

2.  **Instalar el cliente de Angular (Angular CLI)**:

    Angular CLI es una herramienta de interfaz de línea de comandos que permite crear, desarrollar, probar, implementar y mantener aplicaciones Angular directamente desde un shell de comandos.

    Angular CLI se publica en npm como el paquete @angular/cli e incluye un binario llamado `ng`. Los comandos que invocan `ng` utilizan Angular CLI.

    * **Linux/macOS** (con permisos de superusuario):
        ```bash
        sudo npm install -g @angular/cli
        ```

    * **Windows** (en un terminal **sin** permisos de administrador):
        ```powershell
        npm install -g @angular/cli
        ```

3.  **Crear un nuevo proyecto**:
    Navega al directorio donde quieras crear tu proyecto y ejecuta:
    ```bash
    ng new <nombre-proyecto>
    ```



## 📂 Estructura de Ficheros

### Directorios Principales

* `src/app`: Componente central que contiene el core de la aplicación.
* `src/assets`: Almacena activos como imágenes.
* `src/environments`: Ficheros de configuración para los distintos entornos de la aplicación.
* `e2e`: Aloja los ficheros para realizar testing end-to-end.
* `node_modules`: Donde se alojan las librerías descargadas con npm.

### Ficheros de Configuración (Root)

* `package.json`: Contiene información, scripts NPM y configuración de las dependencias.
* `tsconfig.json`: Fichero de configuración de TypeScript.
* `angular.json` (o `angular-cli.json`): Fichero de configuración del framework y del CLI de Angular.

### Ficheros Clave en `src`

* `main.ts`: El punto de entrada y fichero de arranque de la aplicación.
* `index.html`: El HTML central que contiene el componente principal de la app.
* `app.component.ts`: El componente raíz (principal) de la aplicación.
* `app.module.ts`: Donde se declara todo lo que se crea en la aplicación: componentes, servicios, otros módulos, etc.



## 🏛️ Arquitectura de Alto Nivel

* **Módulo**: Un bloque de construcción que contiene componentes, rutas, servicios, etc.. Puede haber múltiples módulos y módulos que dependen de otros. Análogo a un paquete de Java o namespace de .NET.
* **Componente**: Un concepto de vista que contiene un template con datos y lógica. Son reutilizables y forman un árbol DOM.
* **Directiva**: Adjunta comportamiento, extiende o transforma un elemento del DOM y sus hijos.
* **Servicio**: Capa de datos no relacionada con los componentes, como una petición a una API.
* **Routing**: Renderiza un componente basado en el estado de la URL, conduciendo la navegación de la aplicación.



## 🧩 Componentes

* Definen áreas de responsabilidad en la UI, promoviendo la reusabilidad.
* Un componente consiste en tres partes:
    * **Una clase de componente (TypeScript)** que maneja datos y funcionalidad.
    * **Una plantilla HTML** que determina la UI.
    * **Estilos específicos (CSS)** que definen el aspecto.
* Una aplicación Angular es un árbol de componentes, lo que permite un código mejor organizado.



## 🔄 Flujo de Datos

### Dentro de un mismo Componente

#### De la Clase al Template (HTML)

* **Interpolación `{{ }}`**: Bindea unidireccionalmente propiedades o expresiones de la clase en el template.
    ```html
    <p> {{ title + '!' }} </p>
    <div> {{ numberOne + numberTwo }} </div>
    <span> {{ isHappy ? ':)' : ':(' }} </span>
    ```
* **Data-Binding `[ ]`**: Bindea unidireccionalmente propiedades de la clase en los atributos de un elemento HTML.
    ```html
    <img [src]="logo">
    <input type="text" [value]="user.name">
    <h1 [innerHTML]="title"></h1>
    ```

#### Del Template (HTML) a la Clase

* **Event-Binding `( )`**: Ejecuta acciones en la clase cuando sucede un evento en un elemento HTML.
    ```html
    <button (click)="handleClick()"></button>
    <input type="text" (input)="handleInput($event)">
    ```

#### Flujo Bidireccional

* **Two-Way-Binding `[(ngModel)]`**: Flujo bidireccional que combina data-binding y event-binding. Angular lo proporciona con `ngModel`, útil para inputs.
    ```html
    <input type="text" [(ngModel)]="name">
    ```

#### Entre Elementos del Template

* **Template Reference `#`**: Se asigna un nombre de referencia a un elemento HTML para extraer sus propiedades y pasarlas a otro elemento.
    ```html
    <input type="text" #username>
    <button (click)="handleClick(username.value)">Get Value</button>
    ```



## ✨ Directivas

* ***ngFor***: Para iterar sobre una colección.
    ```html
    <ul>
      <li *ngFor="let passenger of passengers; let i = index;">
        {{ i }}: {{ passenger.fullname }}
      </li>
    </ul>
    ```
* ***ngIf***: Para mostrar un elemento condicionalmente.
    ```html
    <div *ngIf="name.length > 2">
      Searching for... {{ name }}
    </div>
    ```
* ***ngIf: else***: Para mostrar un template alternativo si la condición es falsa.
    ```html
    <ul *ngIf="users.length > 0; else noUsers">
      <li *ngFor="let user of users">
        {{user.firstName}} {{user.lastName}}
      </li>
    </ul>
    <ng-template #noUsers>No users found</ng-template>
    ```
* **[ngClass]**: Añade clases CSS a un elemento de forma dinámica a través de un objeto JavaScript.
    ```html
    <span
        class="status"
        [ngClass]="{
            'checked-in': passenger.checkedIn,
            'checked-out': !passenger.checkedIn
        }">
    </span>
    ```
* **[ngStyle]**: Añade estilos CSS en línea a un elemento de forma dinámica a través de un objeto JavaScript.
    ```html
    <span
        class="status"
        [ngStyle]="{
        backgroundColor: passenger.checkedIn ? '#2ecc71' : '#c0392b',
        border: '2px solid black'
        }">
    </span>
    ```



## 💧 Pipes

* Son un mecanismo para transformar datos directamente en el template HTML.
* Se pueden usar los predefinidos por Angular o crear pipes personalizados.

### Pipes Predefinidos

* **json**: Formatea un objeto JavaScript a string JSON.
    ```html
    <p> {{ passenger | json }} </p>
    ```
* **date**: Formatea una fecha.
    ```html
    <p> {{ passenger.checkInDate | date: 'yMMMMd' }} </p>
    ```
* **uppercase**: Convierte un texto a mayúsculas.
    ```html
    <p> {{ passenger.name | uppercase }} </p>
    ```
* **lowercase**: Convierte un texto a minúsculas.
    ```html
    <p> {{ passenger.name | lowercase }} </p>
    ```
* **number**: Formatea un valor numérico con precisión decimal.
    ```html
    <p> {{ passenger.height | number:"1.2" }} </p>
    ```
* **percent**: Formatea un valor numérico a porcentaje.
    ```html
    <p> {{ 0.5 | percent }} </p>
    ```
* **currency**: Formatea una cantidad numérica a unidades monetarias.
    ```html
    <p> {{ passenger.balance | currency:'GBP' }} </p>
    ```



## 🛡️ Operador Elvis / De Navegación Segura (`?.`)

* Revisa las propiedades de un objeto antes de que se les asignen valores.
* Si una propiedad es nula, no se le asigna valor al objeto, sino que la expresión devuelve `null`.
* Ayuda a evitar el error común de JavaScript de acceso a una propiedad de un valor nulo.
    ```html
    <p> Children: {{ passenger?.children?.length || 0 }} </p>
    ```



## 🏗️ Tipologías de Componentes

### Componente de Presentación (Tonto o sin estado)

* Acepta datos vía inputs y emite cambios vía eventos outputs.
* El flujo de datos es hacia abajo (inputs) y hacia arriba (eventos).

### Componente Contenedor (Inteligente o con estado)

* Se comunica con servicios.
* Renderiza componentes hijos.
* Se encarga del routing imperativo.



## 🚀 Constructores y Ciclo de Vida

* **Constructor**: Es el primer lugar donde entra el código. Como buena práctica, solo se debe usar para inyectar dependencias, no para inicializar acciones como peticiones AJAX.
* **`ngOnChanges`**: Responde cuando cambian una o más propiedades de entrada de datos. Se llama antes de `ngOnInit`. El framework no lo llama si el componente no tiene entradas.
* **`ngOnInit`**: Responde cuando el componente es inicializado. Es el lugar ideal para realizar asignaciones de valores, peticiones AJAX y llamadas a servicios.



## 💉 Servicios e Inyección de Dependencias

* **Servicios**: Realizan la conexión con APIs y se registran en el módulo como `provider`.
* **Inyección de Dependencias**: Angular la realiza automáticamente al introducir el elemento a inyectar como parámetro en un constructor.
    ```typescript
    export class PassengerDashboardComponent implements OnInit {
      passengers: Passenger[];
      constructor(private passengerService: PassengerDashboardService) {}

      ngOnInit() {
        this.passengers = this.passengerService.getPassengers();
      }
      // ...
    }
    ```
* **Anotación `@Injectable()`**: Le indica a Angular que se pueden inyectar otras dependencias en esta clase (normalmente un servicio).
    ```typescript
    @Injectable()
    export class PassengerDashboardService {
      constructor(private http: Http) {} // Se inyecta la librería Http

      getPassengers(): Passenger[] {
        // ...
      }
      // ...
    }
    ```



## 🗺️ Routing

* **A - Por Componente (sobre módulos)**
    ```typescript
    const routes: Routes = [
      { path: '', component: HomeComponent, pathMatch: 'full' },
      // selector wildcard -> rutas que no existen en la aplicacion
      { path: '**', component: NotFoundComponent }
    ];
    ```
* **B - RouterLink (sobre enlaces html)**
    ```html
    <a
      [routerLink]="'/'"
      routerLinkActive="active"
      [routerLinkActiveOptions]="{ exact: true }">
        Home
    </a>
    ```
* **C - Imperativo (sobre método de clase)**
    ```typescript
    goBack() {
      this.router.navigate(['/passengers']);
    }
    ```
* **D - Imperativo Dinámico (sobre método de clase)**
    ```typescript
    handleView(event: Passenger) {
      this.router.navigate(['passengers', event.id]);
    }
    ```


## 💠 Patrones Angular

### Comunicación entre Componentes

* **Composición de Componentes**: Usando componentes "inteligentes" (contenedores) y "tontos" (de presentación) para una clara separación de responsabilidades.