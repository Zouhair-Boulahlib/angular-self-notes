# angular-self-notes
angular note to refresh in mind angular features and quircks

## Property binding databinding :


### 1- Target Directive :
**Builtin directives:**
 - [ngClass] = "expression ".
 - [ngStyle] = " expression".
 - ngIf.
 - ngSwitch ngSwitchCase ngSwitchDefault.

   
ngTemplateOutlet.
   
### 2- Target Property :
**Properties:**
  

 - [property] standard binding.
 - [attr.name] attribute binding.
 - [class.name] This is the special class property binding.
 - [style.name] This is the special style property binding.
 
Class binding :
   **differnet class bindings :**
   
   - [class] = "exp"  -> replace all classes.

   - [class.myClass] = "exp" -> setting to myClass memebership adding/remove no replacement.

   - [ngClass] = "exp" -> expr in [string, Arry , Map (key=String one more class, value = boolean).


**Style binding :**
   * differnet Style bindings :
   
   - [style.myStyle] = "expr"  exemple : [style.fontSize]="fontSizeWithUnits",    where :  fontSizeWithUnits: string = "30px".

   - [style.myStyle.units]= "expr" exemple: [style.fontSize.px]="fontSizeWithoutUnits", where : fontSizeWithoutUnits: string= "30".
   
   - [ngStyle]= "map"  exemple : map = { fontSize: "30px", "margin.px": 100, color:  "red" : "green"}.
   
### Built-in directives:

**If satetement:**

    <div *ngIf="expr"></div>


#### Switch satetement:

    <div [ngSwitch]="expr">                      ->  swich
    	<span *ngSwitchCase="expr"></span>       ->  case
    	 <span *ngSwitchDefault></span>	         ->  default
    </div>

### For Loop:


    <div *ngFor="#item of expr"></div>

template Variables of ngFor :
		

 - last (boolean)
 - first (boolean)
 - index (number)
 - odd (boolean)
 - even (boolean)
 - trackBy => syntax TrackBy:keyFunction where keyFunction has the
   following shape => function (key, object)

### Template outlet :


	<ng-template #titleTemplate>
		<h4 class="p-2 bg-success text-white">Repeated Content</h4>
	</ng-template>

	<ng-template [ngTemplateOutlet]="titleTemplate"></ng-template>


**Exemple using ContextData :**

	<ng-template #titleTemplate let-text="title">
		<h4 class="p-2 bg-success text-white">{{text}}</h4>
	</ng-template>

	<ng-template [ngTemplateOutlet]="titleTemplate" [ngTemplateOutletContext]="{title: 'Header'}"></ng-template>

	<div class="bg-info p-2 m-2 text-white">
		There are {{getProductCount()}} products.
	</div>

	<ng-template [ngTemplateOutlet]="titleTemplate" [ngTemplateOutletContext]="{title: 'Footer'}"></ng-template>

**Seen before:**
 **ngClass** and **ngStyle**

Note : a component's template can't access the globale namespace it only can access the component's context
Exemple Template can't use Math.floor as Math is only acessible in globale context (or namespace)


###### Understanding Dynamically Defined PropertiesUnderstanding Dynamically Defined Properties

you can add extrat component field from a template's expression ( that's because a template use a component's context and plain javascript) not recommanded but possible

###### Event Binding :

**(** *target*  **)**  **=**  **"** *expression*  **"** .

when browser trigger an event it provide un **$event** object containing the following attributes :

1. type :  string that identify the type of the event.
2. target:  the object that triggered the event (a DOM Element in general) .
3. timeStamp: timestamp when the event triggered.

Exemple : (input) = "selectedItem=**$event.target.value**" .

###### Key capturing or filtering Parcticale Exemple :
(**keyup.enter**) = "selectedProduct=product.value"

###### Two-way dataBinding :
Banana in the box syntax :
**[(ngModel)]** both event and property binding are used. The ngModel is a builtin angular directive.

### Working with Forms :

#### Template-Driven forms :

Exemple:

```html
<form novalidate (ngSubmit)="addProduct(newProduct)">
	<div>
		<label>Name</label>
		<input name="name" [(ngModel)]="newProduct.name" required minlength="5" pattern="^[A-Za-z ]+$" />
	</div>
	<button type="submit">Create</button>
</form>
```
1. **form** **novalidate** (**ngSubmit**)="**addProduct**(newProduct)" :
  - **form :**  the host element.
  - **novalidate :**  ,do not perfom browser builtin validation.
  - **ngSubmit** : angularDirective fo binding to form submit event.

2. ** input** name="name" [(**ngModel**)]="newProduct.name" **required** **minlength**="5" **pattern**="^[A-Za-z ]+$".
 - **input :**  the host html element.
 - **ngModel**: angular's directive for the two-way databinding
 -  **required**, **minlength** and **pattern** :  builtin angular validators attributes.

 List of builtin Angular Validator attributes : 

|  attribute | Description  |
| -- | -- |
| required  | mandatory field . |
| minlength | min allowed length . |
| maxlength | max allowed length . |
| pattern | input most match the Regex . |

Validators change the angular form validation classes of an element.
for instance :

```html
<input class="form-control ng-pristine ng-invalid ng-touched" minlength="5"
name="name" pattern="^[A-Za-z ]+$" required="" ng-reflect-name="name">
```
the following part : class="form-control **ng-pristine** **ng-invalid** **ng-touched**", means that :
1. **ng-touched** : the input element been visited  (using tab key for exemple)
2. **ng-invalid** the value of the input do not match one of the validators (**required**, **pattern** or **minlength**)
3. **ng-pristine** not been changed by the user ( not that is a user not a script in the browser)

###### Exhaustive list :
 1.  **ng-untouched** vs **ng-touched** : element been visited by the user or not.
 2.  **ng-pristine** vs **ng-dirty** : content been edited or not.
 3. **ng-valid** vs **ng-invalid** : content valid or not.
 
*Exemple of use in css* :
 ```css
input.ng-dirty.ng-invalid { border: 2px solid #ff0000 }
input.ng-dirty.ng-valid { border: 2px solid #6bc502 }
```
Ps: if an element use a template variable : **#name="ngModel"**,
you can access to Validation object Properties using that variable

for instance : *ngIf="name.errors.required" , name.errors.pattern,  name.errors.minlength.requiredLength .....

List: **path**, **valid**, **invalid**, **pristine**, **dirty**, **touched**, **untouched**, **errors** and **value**.

##### **The NgForm Object :**
that object is usefull to perfom an entier form validation .

*in the Component class :*

```javascript
import { NgForm } from "@angular/forms";
//...

submitForm(form: NgForm) {
	/...
}
```
*in the template :*
```html
<form novalidate #form="ngForm" (ngSubmit)="submitForm(form)">
```

###### NgForm object has the following properties :
 form.valid (boolean):
 form.controls (map ( key: name of the control, value: control object aka validator ))


#### Model-Driven forms :

to use model forms , you must **import** the **ReactiveFormsModule**  into your main angular **module** :

```javascript
import { ReactiveFormsModule, FormsModule } from "@angular/forms";
//..
@NgModule({
	//..
	imports: [ FormsModule, ReactiveFormsModule, .......],
	//..
})
export class AppModule {}
```

#### Customize basic element of a Form Model
*usefull imports :*

```javascript
import { FormControl, FormGroup, Validators } from "@angular/forms";
```
###### Form Control :

```javascript
export class ProductFormControl extends FormControl {
	label: string;
	modelProperty: string;
	constructor(label:string, property:string, value: any, validator: any) {
		super(value, validator);
		this.label = label;
		this.modelProperty = property;
	}
}
```
###### Form Group :

```javascript
export class ProductFormGroup extends FormGroup {
	constructor() {
	super({
		name: new ProductFormControl("Name", "name", "", Validators.required),

		category: new ProductFormControl("Category", "category", "",
			Validators.compose([Validators.required,
			Validators.pattern("^[A-Za-z ]+$"),
			Validators.minLength(3),
			Validators.maxLength(10)])),
			
		price: new ProductFormControl("Price", "price", "",
			Validators.compose([Validators.required,
			Validators.pattern("^[0-9\.]+$")]))
	});
		}
		get productControls(): ProductFormControl[] {
			return Object.keys(this.controls)
			.map(k => this.controls[k] as ProductFormControl);
		}
}
```
assuming that a component has the following property :

```javascript
form: ProductFormGroup = new ProductFormGroup();
```

it should be accessible throw the template using : 

```html
<form class="m-2" novalidate [formGroup]="form" (ngSubmit)="submitForm(form)">
```

where submitForm(form) is as : 
```javascript
submitForm(form: NgForm)
```
###### Creating Custom Form Validators :

*Exemple :*
```javascript
export class LimitValidator {
	static Limit(limit:number) {
		return (control:FormControl) : {[key: string]: any} => {
			let val = Number(control.value);
			if (val != NaN && val > limit) {
				return {"limit": {"limit": limit, "actualValue": val}};
			} else {
				return null;
			}
		}
	}
}
```

*Applying a Custom Validator :*

```javascript
price: new ProductFormControl("Price", "price", "",
	Validators.compose([
		Validators.required,
		LimitValidator.Limit(100),
		Validators.pattern("^[0-9\.]+$")
	])
);
```

*The errors property of a FormControl :*

```javascript
this.errors['limit'].limit
```
### Directives :

TS code :

```javascript
@Directive({
    selector: "[demo-attr]"
})
export class PaAttrDirective {
    constructor(private element: ElementRef) {
		this.element.nativeElement.classList.add("green-text")
	}
}
```

use in Template :

```html
<div demo-attr> </div>
```
**demo-attr**  :  the selector of the directive.
**div** : hosst element in  (note that in TS file is provided as ElementRef object).


| Name | Description |
| ------------ | ------------ |
| ngOnInit | This method is called after Angular has set the initial value for all the input properties that the directive has declared. |
| ngOnChanges | This method is called when the value of an input property has changed and also just before the ngOnInit method is called.|
| ngDoCheck | This method is called when Angular runs its change detection process so that directives have an opportunity to update any state that isn’t directly associated with an input property. |
|ngAfterContentInit | This method is called when the directive’s content has been initialized|
|ngAfterContentChecked | This method is called after the directive’s content has been inspected as part of the change detection process.|
|ngOnDestroy | This method is called immediately before Angular destroys a directive.|



Directives support both input bound properties ( use of @Input() decorators) and custom event (use of @Output() decorators).

while angular support many plateforms is recommanded to avoid relaying on the constructor to get the ElementRef object, a better approache is to use :

@HostBinding to map the host-element's attributes (class, id, textContent) to a bounded property (property decorated by @Input decorator)

```javascript
@Input("pa-attr")
@HostBinding("class")
 bgClass: string;
```
also you can use @HostListener to listen to the host event and  exemple :

```javascript
 @Output("pa-category")
    click = new EventEmitter<string>();

  @HostListener("click")
    triggerCustomEvent() {
        if (this.product != null) {
            this.click.emit(this.product.category);
        }
    }
```
using that technique is no longer needed to use the constructor to get the host element's reference wiches a plateform independent approache.

##### the ngOnChanges() anglar lifecycle hook :

```javascript
ngOnChanges(changes: {[property: string]: SimpleChange }) { ...
```

This method use the SimpleChange Class to track changes on a directive's property .

Main properties and method of such SimpleChange class :

| Name  | Description  |
| ------------ | ------------ |
| previousValue  | This property returns the previous value of the input property.  |
| currentValue  | This property returns the current value of the input property.  |
| isFirstChange()  | This method returns true if this is the call to the ngOnChanges method that occurs before the ngOnInit method. |

Exemple :

```javascript
export class PaModel {
	
	@Input("selfNoteModel")
	modelProperty: string;

	@HostBinding("value")
	fieldValue: string = "";

	ngOnChanges(changes: { [property: string]: SimpleChange }) {
		let change = changes["modelProperty"];
		if (change.currentValue != this.fieldValue) {
		this.fieldValue = changes["modelProperty"].currentValue || "";
	}
}
```

### Structural Directives :

let's create a personnal If statement :

```javascript
import {
	Directive, SimpleChange, ViewContainerRef, TemplateRef, Input
} from "@angular/core";
@Directive({
	selector: "[psIf]"
})
export class PsStructureDirective {

	constructor(private container: ViewContainerRef,
			private template: TemplateRef<Object>) { }

	@Input("psIf")
	expressionResult: boolean;

	ngOnChanges(changes: { [property: string]: SimpleChange }) {
			let change = changes["expressionResult"];
			if (!change.isFirstChange() && !change.currentValue) {
				this.container.clear();
			} else if (change.currentValue) {
				this.container.createEmbeddedView(this.template);
			}
	}
}
```
now the Template usage of the psIf directive :

```html
<ng-template [psIf]="variable > 5  ?  true : false">
	<div id="mainDiv">
	 content ....
	</div>
</ng-template>
```
when the Angular evalute the expression : **variable > 5  ?  true : false** then the result is assigned to psIf inpout-bounded property so the ngOnChanges method get triggred (cuz a property get changed whiches the psIf redudent but it is)

the **if (!change.isFirstChange() && !change.currentValue) {** code is to check if it the first change or not (remember that the *ngOnChanges()* get called before *ngOnInit()* ) .

Let's not forget the constructor :

...
constructor(private container: **ViewContainerRef**, private template: **TemplateRef**< Object >) {}
...

**ViewContainerRef Methods and Properties :**
1. **element** :  This property returns an ElementRef object that represents the
container element.

2. **createEmbeddedView(template)** : This method uses a template to create a new view. See the text after.the table for details. This method also accepts optional arguments for context data.

3.  **clear()** : This method removes all the views from the container.

4. **length** : This property returns the number of views in the container.

5. **get(index)** : This method returns the ViewRef object representing the view at the
specified index.

6. **indexOf(view)**: This method returns the index of the specified ViewRef object.
7. **insert(view, index)** : This method inserts a view at the specified index.
8. ** remove(Index)** : This method removes and destroys the view at the specified index.
9.  **detach(index)** :  This method detaches the view from the specified index without
destroying it so that it can be repositioned with the insert method.

Better is for the better :  **Using the Concise Structural Directive Syntax** 

instead of writing :
```html
<ng-template [psIf]="variable > 5  ?  true : false">
	<div id="mainDiv">
	 content ....
	</div>
</ng-template>
```

wee can write :

```html

	<div id="mainDiv" *psIf="variable > 5  ?  true : false">
	 content ....
	</div>

```
no use of [ ] nor ng-template just a *  (called an asterisk).

###### Iterating Structural Directives :

let's creat a personnel For loop directive :

```javascript
import { Directive, ViewContainerRef, TemplateRef,
Input, SimpleChange } from "@angular/core";
@Directive({
selector: "[psForOf]"
})
export class PsIteratorDirective {

	constructor(private container: ViewContainerRef,
	private template: TemplateRef<Object>) {}

	@Input("psForOf")
	dataSource: any;

	ngOnInit() {
		this.container.clear();
		for (let i = 0; i < this.dataSource.length; i++) {
			this.container.createEmbeddedView(this.template,
					new PaIteratorContext(this.dataSource[i], i, this.dataSource.length));
		}
	}
}
class PsIteratorContext {

	odd: boolean; even: boolean;
	first: boolean; last: boolean;

	constructor(public $implicit: any, public index: number, total: number ) {
			this.odd = index % 2 == 1;
			this.even = !this.odd;
			this.first = index == 0;
			this.last = index == total - 1;
	}
}
```
Note that the **xxOf** is mandatory for concise syntax  ( it becoms ***xx** instead of **[xxOf]**) .

template without concise syntax :
```html
<ng-template [psForOf]="arrayOfObjects" let-item let-i="index" let-odd="odd" let-even="even">
	<tr [class.bg-info]="odd" [class.bg-warning]="even">
		<td>{{i + 1}}</td>
		<td>{{item.name}}</td>
		<td>{{item.category}}</td>
		<td>{{item.price}}</td>
	</tr>
</ng-template>
```
concise Syntax :

```html
<tr  *psFor="let item of getProducts(); let i = index; let odd = odd;
let even = even" [class.bg-info]="odd" [class.bg-warning]="even">
		<td>{{i + 1}}</td>
		<td>{{item.name}}</td>
		<td>{{item.category}}</td>
		<td>{{item.price}}</td>
</tr>
```

going crazy with Convention naming using an expressive DSL :

```html
<div *psFor="let item from IteriableSource limit 10 offset 5">
  content
</div>
```
Angular is going to bind those values to @Input bounded properties such as :

```javascript
@Input("psForFrom") 
fromExpr : Array  // contains IteriableSource value

@Input("psForLimit") 
limitExpr : Number  // contains 10 value

@Input("psForOffset") 
OffsetExpr : Number  // contains5 value
```
so angular use the following convention :  DireictiveSelector + Capitlized(Keyword) where keyword in that exemple keywords are :  from , limit and offset

Note that the let variable (in that exemple named **item** ) is bound to context object's **$implicit** property . 
in that case the **ViewContainerRef.createEmbeddedView()** method expect both a template reference as first argument and an object that contains $implicit property and the already mentionned properties (from, limit offset) . (hypothes to demonstrat  later) .

#### Dealing with Collection-Level Data Changes :
**Problem :**
  each element of a collection's property change is reflected by angular's change detection process. not the same for collection level change (remove, replace, add element ...) are not auto handled by angular.

**Solution :** 


```javascript
ngDoCheck() {
	console.log("ngDoCheck Called");
	this.updateContent();
}
private updateContent() {
	this.container.clear();
	for (let i = 0; i < this.dataSource.length; i++) {
	this.container.createEmbeddedView(this.template,
	new PaIteratorContext(this.dataSource[i],
	i, this.dataSource.length));
	}
}
```
The problem with the ngDoCheck method is that it is invoked every time Angular detects a change anywhere in the application (exemple : focus, click, input change ...etc)  that's a true performance bottleneck.

**Better Solution :**
Angular provides some tools for managing updates more efficiently and updating content only when it is required.

**import :**
```javascript
import { .....,
	 IterableDiffer, IterableDiffers, ChangeDetectorRef, 
	 CollectionChangeRecord, DefaultIterableDiffer
} from "@angular/core";
```

**in the Directive class :**
```javascript
private differ: DefaultIterableDiffer<any>;
  //.....
constructor(....
private differs: IterableDiffers,
private changeDetector: ChangeDetectorRef){

}
//....
ngOnInit() {
this.differ =
<DefaultIterableDiffer<any>> this.differs.find(this.dataSource).create();
}

ngDoCheck() {
	let changes = this.differ.diff(this.dataSource);
	if (changes != null) {
		console.log("ngDoCheck called, changes detected");
		changes.forEachAddedItem(addition => {
			this.container.createEmbeddedView(this.template,
			new PaIteratorContext(addition.item, addition.currentIndex, changes.length));
		});
	}
}
```

##### Directive and Querying the Host Element Content :
decorator :

```
@ContentChild(Class| String, option? )
child :ClassOfTheChild;
```
1. first argument is : 
the **class** of child directive of the element or a **string represention of a template variable**. 

2. second argument :
option (**object { descendants: true }** f you want to include the descendants of children in the results) .

Note :the  @ContentChild decorator match onley the first child . to get all childeren we can use :
```
@ContentChildren(ClassOFChildern)
contentChildren: QueryList<ClassOFChildern>;  // QueryList defined in @angular/core
```
Main properties and methods of QueryList class :
**properties** : length, first, last , changes.
**methods**: reduce(function), map(function), filter(function), some(function) and forEach(function) .

###### Recieveing Query Change Notification :

```
ngAfterContentInit() {
	this.contentChildren.changes.subscribe(() => {
	setTimeout(() => this.updateContentChildren(this.modelProperty), 0);
	});
}
private updateContentChildren(dark: Boolean) {
	if (this.contentChildren != null && dark != undefined) {
		this.contentChildren.forEach((child, index) => {
			child.setColor(index % 2 ? dark : !dark);
		});
	}
}
```
so **changes** is an observable object that used to subscribe to it using an observer .

Ok we Deal with Directives so It's time to rock with  Components

### Components :
Component = Directives + HTML content [ + CSS style ]

###### The Component Decorator Properties :

1. **animations** : This property is used to configuration animations.
2. **encapsulation** : This property is used to change the view encapsulation settings, which control how component styles are isolated from the rest of the HTML document.
3.  **selector ** :  This property is used to specify the CSS selector used to match host elements.
3. **providers** : This property is used to create local providers for services.
4. **viewProviders** : This property is used to create local providers for services that are available only to view children.
5. classic properties : **templateUrl**, **template**, **styleUrls** and **style**.

###### Data Flow in components :

1. parent to child component : property data-bound (use properties decorated with @Input decorator , the [ property ] = "expr" syntax) .

2. Child to parent component :  event data-bound (use properties decorated by @Output decorator, the (event) = " expr", expr = mehtod($event) in most cases  syntax).

###### Projecting Host Element Content :
** Hello future-me this is the best part .**
###### ng-content :

template of a component (selector : exemple-component)
```html
<div>
 .....
  <ng-content></ng-content>
.....
</div>
```
when using this template the ng-content is replaced by the content inside the host element of the exempleComponent
```html
<exemple-component>
		<div>
		 custom content to replace ng-content
		</div>
</exemple-component>

```
###### Querying Template Content :

```
@ViewChildren(ClassOfDirectiveOrComponent)
viewChildren: QueryList<ClassOfDirectiveOrComponent>;
```

associated lifecycle hook :

```
 ngAfterViewInit() {
	this.viewChildren.changes.subscribe(() => {
			this.updateViewChildren();
	});
	this.updateViewChildren();
}

private updateViewChildren() { ... }
```

see also : @ViewChild(class) , ngAfterViewChecked


You may need to combine view child and content child queries if you have used the ng-content element. The content defined in the template is queried , but the
project content—which replaces the ng-content element—is queried using the child queries


### Using and Creating pipes :
##### Creating a Custom Pipe :

```
import { Pipe } from "@angular/core";
@Pipe({
name: "addTax"
})
export class Riba {
	defaultRate: number = 10;
	transform(value: any, rate?: any): number {
		let valueNumber = Number.parseFloat(value);
		let rateNumber = rate == undefined ?
		this.defaultRate : Number.parseInt(rate);
		return valueNumber + (valueNumber * (rateNumber / 100));
	}
}
```
Pipes are classes to which the **@Pipe decorator** has been applied and that implement a **method called transform**.

**Pipe Decorator properties :**
1. **name** : This property specifies the name by which the pipe is applied in templates.

2. **pure** : When true, this pipe is reevaluated only when its input value or its arguments are changed. This is the default value.

* usage :*
```
{{item.price | addTax:(taxRate || 0) }}
```
arguements are separated  with the ":" while target datasource and pipe are seperated with "|".
exemple :
```
{{item.price | addTax:(taxRate || 0) | currency:"USD":"symbol" }}
```
### Services and DI :

declaring dependency on a Service using constructor :

```
constructor(private service: AClassService) { }

```
Pipe , Directives and Component all can declare dependencies on a service .

a Service Class is decorated with @Injectable() decorator to relay on angular DI System.

Registring a Service at Module level :  put Service Class in the providers array of the Module decorator.

### The Angular Providers

1. **Class provider** : This provider is configured using a class. Dependencies on the service are resolved by an instance of the class, which Angular creates.

2. **Value provider** : This provider is configured using an object, which is used to resolve dependencies on the service.

3. **Factory provider** : This provider is configured using a function. Dependencies on the service are resolved using an object that is created by invoking the function.

4. **Existing service provider** : This provider is configured using the name of another service and allows aliases for services to be created.

##### Usage :

###### Class Provider :

```
{
provide: LogService,
useClass: LogService
}
```
