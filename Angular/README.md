# Angular

### Instalaciones:
- Node.js
- TypeScript
- Angular-cli
- Atom y snippets:
  - Angular 2 Type Script Snippets
  - Atom Bootstrap3
  - Atom Typescript
  - autocomplete-js-import
  - File Icons
  - Platformio Ide Terminal
  - Atom Bootstrap4
  - V Bootstrap4

### Comandos usados:

>Al hacer la instalación en Windows hay veces que da problemas.
Por ejemplo The "@angular/compiler-cli" package was not properly installed.
Para solucionarlo ejecutar cmd en modo administrador y realiza la instalación del angular-cli desde cero:
```shell
npm uninstall -g @angular/cli
npm cache clean
npm install -g @angular/cli@latest
```
>Si no se soluciona, dentro de nuestro proyecto, en la carpeta que contiene a src, hacemos:
```shell
npm i @angular/compiler-cli
```
>Cerramos la consola y creamos un nuevo proyecto con `ng new {nombreProyecto}` o nos vamos a uno ya creado y hacemos `npm install`, `ng build` (nos debería compilar correctamente) y por último `ng serve`

##### Funciones de flecha en TypeScript
```js
promesa = new Promise((resolve, reject) => {
  let condicion:boolean = true;
  if (condicion) {
    resolve(/* valor */);  // Resuelta con éxito
  } else {
    reject(/* motivo */);  // Error, rechazada
  }
});
```

#### 1. HolaMundo
```shell
npm install  # La primera vez
npm start    # Lanzado dentro de la carpeta del proyecto (HolaMundo) compila, se queda en modo escucha y lanza el servidor en http://localhost:3000/
```

#### 2. SPA

##### Creación, configuración inicial y arranque del servidor
```shell
ng new SPA                                  # Crear el proyecto
ng serve                                    # Lanzado dentro de la carpeta del proyecto (SPA) compila, se queda en modo escucha y lanza el servidor en http://localhost:4200/
npm install bootstrap@4.0.0-alpha.6 --save  # Con el --save lo salvamos en las dependencias del proyecto, el package.json
npm install jquery --save
npm install tether --save
```

```shell
"exclude": [
  "../node_modules"  # Añadir esta línea en el tsconfig.json de la raíz del proyecto para evitar que se relentice al compilar esta carpeta cada vez
]
```

##### Components

###### Creación de componentes:
```shell
ng g c components/shared/navbar   # Crea un component para la navbar y lo colocamos en components/shared
ng g c components/home            # Crea un component para la home y lo colocamos en components
ng g c components/about           # Crea un component para la página de about y lo colocamos en components
ng g c components/heroes -is      # Crea un component para la página heroes y lo colocamos en components (con *-is* evitamos que se creen los estilos)`
```

>En un component tenemos el constructor y el método que se crea por defecto `ngOnInit()`.
>El primero se lanza antes; el `ngOnInit()` cuando se renderiza la página

##### Router
>Para configurar las rutas de la aplicación deberemos crear una etiqueta `<router-outlet></router-outlet>` en el `app.component.html` donde queramos que se carguen las páginas que pidamos:
```html
<app-navbar></app-navbar>
<div class="container">
  <router-outlet></router-outlet>
</div>
```
>Además crearemos el app.routes.ts en la raíz de la aplicación y se autoconstruirá con `ng2-routes`. Indicamos tantas líneas del estilo: <br>
`{ path: 'home', component: HomeComponent }` <br>
como rutas deseemos crear. <br>
En este archivo deberá ir la línea: <br>
`export const APP_ROUTING = RouterModule.forRoot(APP_ROUTES, {useHash:true});` <br>
con el atributo `useHash` se indicará si las URLs llevan o no el caracter `#` <br>
Por último, los enlaces que queramos que pasen por el router deberán llevar atributos del estilo: <br>
`[routerLink]="['home']"` <br>
Para gestionar una clase (por ejemplo la clase `active` para el navbar) según la ruta, usaremos con el atributo `routerLinkActive="active"` en la etiqueta HTML correspondiente.
```html
<ul class="navbar-nav mr-auto">
  <li class="nav-item" routerLinkActive="active">
    <a class="nav-link" [routerLink]="['home']">Home</a>
  </li>
  <li class="nav-item" routerLinkActive="active">
    <a class="nav-link" [routerLink]="['buscar']">Buscar</a>
  </li>
</ul>
```

#### 3. Pipes

##### Transformación, recorte y visualización de variables
```shell
{{variable | uppercase}}            # Transforma en mayúsculas la variable
{{variable | lowercase}}            # Transforma en minúsculas la variable
{{variable | slice:3}}              # Recorta la variable desde la posición 3. Ej: Mario -> io
{{variable | slice:0:3}}            # Recorta la variable desde la posición 0 hasta la 3. Ej: Mario -> Mar
{{variable | slice:0:3 | lowercase} # Se pueden combinar. Ej: Mario -> mar
{{array | slice:1:5}}               # Aplicable para arrays. Ej: array = [1,2,3,4,5,6,7,8,9,10]; -> [2,3,4,5]. Se puede usar en variables HTML como <li *ngFor="let item of array | slice:5:20">{{item}}</li>
```
>El formato del slice es `array_or_string_expression | slice:start[:end]`

##### Números
```shell
{{numero | number:'3.0-2'}}         # Se indica la cantidad de dígitos para parte entera, mínimo de dígitos para parte decimal y máximo. Ej: 3.1415926 -> 003.14
{{numero | number:'.1-1'}}          # Ej: 3.1415926 -> 3.1
{{numero | percent}}                # Ej: 0.234 -> 23.4%
{{numero | percent:'2.2-2'}}        # También se puede establecer un digitInfo Ej: 3.1415926 -> 3.1
```
>El formato del digitInfo es siempre un string formado por `'{minIntegerDigits}.{minFractionDigits}-{maxFractionDigits}'` y si no se indica toma su valor por defecto que es minIntegerDigits=1, minFractionDigits=0, maxFractionDigits=3

##### Importes
```shell
{{salario | currency}}                    # Ej: 1234.5 -> USD1,234.50
{{salario | currency:'EUR'}}              # Ej: 1234.5 -> EUR1,234.50
{{salario | currency:'EUR':true}}         # Ej: 1234.5 -> €1,234.50
{{salario | currency:'EUR':true:'4.0-0'}} # Ej: 1234.5 -> €1,235
```
>El formato del currency esta formado por `[:currencyCode[:symbolDisplay[:digitInfo]]]` y si no se indica toma su valor por defecto que es minIntegerDigits=1, minFractionDigits=0, maxFractionDigits=3

##### JSON
```shell
{{heroe | json}}  # Al pasar una variable de tipo JSON, si lo intentamos imprimir únicamente con {{heroe}} nos saldrá [object Object], pero si le pasamos el JsonPipe nos saldrá el contenido y además lo metemos entre etiquetas HTML <pre>, formateado:

{
  nombre: "Logan",
  clave:  "Wolverine",
  edad:   500,
  direccion: {
    calle: "Calle Falsa",
    numero: 123
  }
}
```
##### Variables asíncronas
```shell
{{promesa | async}}  # Al pasar una variable asíncrona con el pipe async, se queda esperando a que se devuelva, para cuandolo haga imprimirse

promesa = new Promise( (resolve, rejected) => {
  setTimeout(
    () => resolve('Llegó el dato!'),
    2300
  );
});
```
##### Fechas
```shell
{{fecha | date:'medium'}}      # Ej: 3 may. 2017 21:48:19
{{fecha | date:'MMMM - dd'}}   # Ej: mayo - 03
```
>Para cambiar el idioma de nuestra aplicación (y que por ejemplo nuestra fecha aparezca en Español), en el app.module.ts importaremos `import { LOCALE_ID } from '@angular/core';` y añadiremos un elemento en el apartado de providers
`
providers: [
    { provide: LOCALE_ID, useValue: 'es' }
  ]
`

##### Pipes personalizados
```shell
{{nombre | capitalizado}} # Aplicamos a una variable el pipe que definamos, en este caso capitalizdo.
```
>Habrá que definirlo como un componente, pero con `ng2-pipe` y añadir el Pipe creado en el `app.module.ts` en el apartado de declarations

>La clase que implementa PipeTransform tiene un método
`transform(value: string, todas:boolean = true): string`<br>
Podremos incluir los argumentos individualmente o en un array
`transform(value: string, ...args: any[]): string`

```html
<iframe src="http://www.youtube.com/embed/M7lc1UVf-VE" width="560" height="315"></iframe> <!-- Ningún problema -->
<iframe src="{{video}}" width="560" height="315"></iframe> <!-- De este modo nos daría un error el navegador -->
```

>Para crear un Dom Seguro y que nos permita introducir una varíable con una URL de un vídeo deberemos crear un Pipe como un componente, pero con `ng2-pipe` y añadir el Pipe creado en el `app.module.ts` en el apartado de declarations.

```js
export class DomseguroPipe implements PipeTransform {

  constructor(private domSanitizer:DomSanitizer) { }

  transform(value: string, url: string): any {
    return this.domSanitizer.bypassSecurityTrustResourceUrl(url + value);
  }
}
```

#### 4. SpotiApp

#### 5. Deseos - ionic mobile app
### Instalaciones e inicio:
- `npm install -g cordova ionic`
- `ionic start appName tabs` clonará el proyecto de ionic tabs, lo que nos permitirá tener un HolaMundo sobre el que empezar
- Nos creamos una cuenta en [ionic apps](https://apps.ionic.io/apps/) y podremos enlazar con nuestra cuenta y linkar la aplicación para probarla con cualquier dispositivo:
  - ionic login
  - ionic link
  - ionic upload
- Para lanzar nuestro servidor local `ionic serve`

### Aplicación:
>La aplicación se compone de 2 tabs para los que crearemos 2 plantillas html y 2 componentes TypeScript
Tenemos un servicio que se encarga de consultar y entregar los datos a estos componentes. Para ello, lo importaremos desde los componentes `import { ListaDeseosService } from '../../app/services/lista-deseos.service';` y lo añadimos en el constructor para poder usarlo desde la plantilla HTML `constructor(private listaDeseosService:ListaDeseosService) {}`
De este modo podremos acceder, por ejemplo, con un `*ngFor="let lista of listaDeseosService.listas"`

### Datastorage:
>Usaremos el Datastorage del dispositivo para almacenar os datos de la aplicación
`localStorage.setItem("data", item)`
`localStorage.getItem("data")`
Con `JSON.stringify(objeto)` y `JSON.parse(localStorage.getItem("data")` convertimos a formato JSON y recuperamos nuestro JSON en forma de objeto, para guardar en el localStorage y recuperar de este, respectivamente

### Manejo de clases desde angular
>Para indicar a un elemento HTML que aplique una clase dependiendo de una variable, podremos hacerlo de esta manera `[class.nombreClase]="item.condicion"`

#### 6. Miscelaneos
### Instalaciones e inicio:
- `ng new SPA` crea el proyecto
- `ng serve` lanzado dentro de la carpeta del proyecto compila, se queda en modo escucha y lanza el servidor en http://localhost:4200/

### Aplicación:
>Generamos el primer component ngStyle en la carpeta components de la siguiente forma `ng g c components/ngStyle -it -is`, lo que nos creará un apartado en ese component para el template y los styles, y no cree un archivo específico HTML y otro CSS para ello.

>Como el componente aparece que: `selector: 'app-ng-style'`, nos referiremos a él en cualquier parte del HTML con `<app-ng-style></app-ng-style>`

>Creamos nuevos componentes (en la carpeta components) con `ng g c components/{nombre} -it -is`

>Creamos directivas (en la carpeta components) con `ng g d directives/{nombre} -is`
