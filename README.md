# angular-self-notes
angular note to refresh in mind angular features and quircks

## Property binding databinding :


### 1- Target Directive :
	* builtin directives:

	 - [ngClass] = "expression ".
   
	 - [ngStyle] = " expression".
   
	 - ngIf.
   
	 - ngSwitch ngSwitchCase ngSwitchDefault.
   
	 - ngTemplateOutlet.
   
### 2- Target Property :
	* properties:
  
	 - [property] standard binding.
    
   - [attr.name] attribute binding.
    
   - [class.name] This is the special class property binding.
    
   - [style.name] This is the special style property binding.
 

Class binding :
   * differnet class bindings :
   
   - [class] = "exp"  -> replace all classes.

   - [class.myClass] = "exp" -> setting to myClass memebership adding/remove no replacement.

   - [ngClass] = "exp" -> expr in [string, Arry , Map (key=String one more class, value = boolean).


Style binding :
   * differnet Style bindings :
   
   - [style.myStyle] = "expr"  exemple : [style.fontSize]="fontSizeWithUnits",    where :  fontSizeWithUnits: string = "30px".

   - [style.myStyle.units]= "expr" exemple: [style.fontSize.px]="fontSizeWithoutUnits", where : fontSizeWithoutUnits: string= "30".
   
   - [ngStyle]= "map"  exemple : map = { fontSize: "30px", "margin.px": 100, color:  "red" : "green"}.
   
### Built-in directives:

If satetement:

html ```

<div *ngIf="expr"></div>

```

#### Switch satetement:

html```

<div [ngSwitch]="expr">                          ->  swich
	<span *ngSwitchCase="expr"></span>       ->  case
	 <span *ngSwitchDefault></span>	         ->  default
</div>

```
### For Loop:

html ```
<div *ngFor="#item of expr"></div>
```
template Variables of ngFor :
		last (boolean)
		first (boolean)
		index (number)
		odd (boolean)
		even (boolean)
                 trackBy => syntax TrackBy:keyFunction where keyFunction has the following shape => function (key, object)

###Template outlet:

html```
	<ng-template #titleTemplate>
		<h4 class="p-2 bg-success text-white">Repeated Content</h4>
	</ng-template>

	<ng-template [ngTemplateOutlet]="titleTemplate"></ng-template>
```

Exemple using ContextData:

html```
	<ng-template #titleTemplate let-text="title">
		<h4 class="p-2 bg-success text-white">{{text}}</h4>
	</ng-template>

	<ng-template [ngTemplateOutlet]="titleTemplate" [ngTemplateOutletContext]="{title: 'Header'}"></ng-template>

	<div class="bg-info p-2 m-2 text-white">
		There are {{getProductCount()}} products.
	</div>

	<ng-template [ngTemplateOutlet]="titleTemplate" [ngTemplateOutletContext]="{title: 'Footer'}"></ng-template>
```
Seen before:
 ngClass and ngStyle




Note : a component's template can't access the globale namespace it only can access the component's context
Exemple Template can't use Math.floor as Math is only acessible in globale context (or namespace) 

