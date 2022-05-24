# Patrón de diseño "Smart and Dumb Components".
## ¿Qué es un patrón de diseño?
Un [patrón de diseño](https://www.patterns.dev/posts/introduction/) es un conjunto de técnicas para resolver problemas comunes que se presentan en la etapa de desarrollo de software.
Los patrones de diseño son la herramienta creada para los problemas tipicos que se presentan en el desarrollo de vistas o interfaces.
## Patrón de componentes tontos e inteligentes.
### Componente inteligente.
Un componente inteligente o *componente contenedor* es aquel componente donde realizaremos la logica de los datos a utilizar. 
Este componente es encargado de realizar los servicios de obtención de los datos _(a travez de un fetch a una API, una llamada a una DB, etc)_, el filtado de los datos y el envío al componente tonto.
Este tipo de componentes **no utilizan ni editan estilos visuales**.
Dentro del arbol de componentes, un componente puede ser catalogado como inteligente cuando: 
- Puede ser ruteado en la aplicación.
- Es un modal.
- Es un componente que podemos agregar dentro el componente principal `AppComponent`.

### Componente tonto.
Un componente tonto o _componente de presentación_ es un componente que su función es renderizar y presentar la información al usuario final.
Este componente se encarga de presentar la información a traves de tablas, graficas, parrafos, etc. __sin alterar el set de datos que el componente inteligente le compartió__.
Un componente de presentación puede ser utilizado por _varios componentes inteligentes_, por lo tanto la utilidad de los componentes de presentación es ser reutilizados en diferentes partes de una web app.
![Arbol de componentes donde se utiliza el patrón de componentes tontos e inteligentes.](https://raw.githubusercontent.com/wiki/devonfw/devon4ng/images/component-tree.svg).

| Smart                                   | Dumb                                                            |
|-----------------------------------------|-----------------------------------------------------------------|
| Contienen el estado de la vista actual  | Muestra los datos compartidos sin alterar el estado de la vista |
| Maneja los eventos emitidos por el dumb | Envía los eventos de la vista al smart |
| Invoca servicios para obtener datos | No tiene acceso a los servicios de la aplicación |
| Consiste en _n_ dumbs components | Es independiente de los numeros de smart components |

### Interacción entre smart y dumbs.
En [Angular](https://angular.io/) existen decoradores para indicar a un componente cuando debe emitir un evento y cuando debe recibir un dato o variable.
Para Angular, se utilizan los decoradores `@Input()` para indicar entrada de datos y `@Output()` para indicar una salida de eventos.

``` javascript 
  export class ValuePickerComponent {

  @Input() columns: string[] = [];
  @Input() items: any[] = [];
  @Input() selected: any = {};
  @Input() filter: string = '';
  @Input() isChunked: boolean = false;
  @Input() showInput: boolean = true;
  @Input() showDropdownHeader: boolean = true;

  @Output() elementSelected = new EventEmitter<{}>();
  @Output() filterChanged = new EventEmitter<string>();
  @Output() loadNextChunk = new EventEmitter();
  @Output() escapeKeyPressed = new EventEmitter();
}
```
![Diagrama simple de la aplicación de smart y dumbs components.](https://raw.githubusercontent.com/wiki/devonfw/devon4ng/images/smart-dumb-components-interaction.svg)
