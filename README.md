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
