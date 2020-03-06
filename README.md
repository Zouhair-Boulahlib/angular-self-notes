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

