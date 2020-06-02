Angular:
--------

	Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
	
	ng analytics project off
	
	https://stackoverflow.com/questions/43344600/installing-a-specific-version-of-angular-with-angular-cli
	
	npm install -g @angular/cli@7.0.3
	
	Bootstrap CSS Framework
	-----------------------
	unpkg bootstrap 4
	https://unpkg.com/browse/bootstrap@4.3.1/dist/
	 get min.css url and import it in styles.css file
	 @import url(https://unpkg.com/bootstrap@4.3.1/dist/css/bootstrap.min.css)
	 
	 just by importing this you will lots of difference in appearance.
	 
	 there are other ways we can install bootstrap in command line.Have to check on that.
	  step1:
		npm install --save bootstrap@3
		
	  step2: 
			angular.json file update
			node_modules/bootstrap/dist/css/bootstrap.min.css
	
	Debug Angular App
	-----------------
		1. Chrome dev tool console
		2. Developer tools->Sources->webpack-> .folder->app->somefile.ts
			you can mark break point and check
		3. Augury
	Bootstrapping Angular App
	-------------------------
	RootModule
	RootComponents
	
	main.ts
		platformBrowserDynamic().bootstrapModule(AppModule)  
		AppModule is bootstrapped in main.ts (AppModule is RootModule)
	AppModule.ts
		bootstrap: [AppComponent]
		AppComponent is bootstrapped in AppModule (AppComponent is RootComponents)
		
	index.html
		is having <app-root></app-root> which is selecter tag for app.component
		
	** we can always change the bootstrap module and component it is up to us.but this is how angular app is loaded
	
	
	Create New Project
	------------------
	ng new todo 
	
	Run Project
	-----------
	ng serve  ->inside project folder
	
	Commends
	---------
	ng lint  ->checking for coding standard
	ng build  -> building application (creates dist folder which will be deployed in web server)
	ng test  -> (UNIT TEST) running test (test created in jasmine ->karma runs the test) 
	ng e2e   -> (e2e are created using protracter) entire application test
	
	ng generate component component-name or ng g c component-name -> create new component (ng generate component welcome)
	ng generate service service-name or ng g s service-name -> create new Service (ng generate service hardCodedAuthendication)
	
	Important Files
	---------------
	main.ts and index.html are responsible for bootstrapping angular app
    index.html
    app.component.html
	angular.json  -> all commands defined here
	tsconfig.json  -> type script to java script conversion specification.(we write code in typescript but browser only understands javascript)
	package.json  -> packaged,tools ,frameworks needed for app are specified here
		all the modules defined in package.json will be downloaded(node_modules folder) on running npm install
	ployfils.ts ->browser compatibility maintenance
    styles.css ->global css file
					
	
	Folders
	-------
	app -> all our codes goes here
	
	Terminologies
	-------------
	components -> agular app is build using multiple components.like manu , footer header etc.
				components associated below
				html (template)
				css (style)
				.ts  (code)
				
				Components are the most basic UI building block of an Angular app. An Angular app contains a tree of Angular components.
				Angular components are a subset of directives, always associated with a template. Unlike other directives, only one component can be instantiated per an element in a template.
				A component must belong to an NgModule in order for it to be available to another component or application. To make it a member of an NgModule, list it in the declarations field of the @NgModule metadata.

    Decorator   -> similar to annotation in java
		@Component
		Decorator that marks a class as an Angular component and provides configuration metadata that determines
			how the component should be processed, instantiated, and used at runtime.
			
			
	Data Binding 
	------------
	interpolation- {{property}} (it is from component file)
	ngModel (Banana in a box) - [(ngModel)]="userName"   -userName should be defined in .ts file  
								 Needs to Import FormsModule for this
	method Binding - (click)=handleLogin()
	
	Pass Data from template to TS using Template Referrance variable
	-----------------------------------------------------------------
		Template:
		---------
		<input type="text" class="form-control" #namaTempaleRef (blur)="sendTemplateRefVaraible(namaTempaleRef)" >
		TS
		--
		sendTemplateRefVaraible(nameIpTemplateVar:HTMLInputElement){
			console.log(nameIpTemplateVar.value);
		}
		
		Also you can use ViewChild to read templateRef variable
		@ViewChild('namaTempaleRef')namaTempaleRefViewChid:ElementRef;
		onAddServer() {
			console.log("Value from template ref: "+this.namaTempaleRefViewChid.nativeElement.value);    
		}
		
	Directives
	----------
	ngModel (Banana in a box) - [(ngModel)]="userName"   -userName should be defined in .ts file  
	
	ngIf	*ngIf='invalidLogin'    ->invalidLogin should be defined in .ts file
	*ngFor   *ngFor='let todo of todos' Looping through array
			 <tr *ngFor='let todo of todos'>
				<td>{{todo.id}}</td>
				<td>{{todo.description}}</td>
			 </tr>
	ngSwitch:
		if you wnat to check for multiple if condition for same input, in that scenario we can use ngSwitch
			 
	Custom Directive
	----------------	
		ng g d custom-directive/customStyle
		
		Custom Attribute Directive
		--------------------------
			1. Directive
			import { Directive, Renderer2, ElementRef, OnInit } from '@angular/core';
			@Directive({selector: '[customColorSetter]'})
			export class CustomStyleDirective implements OnInit{
			  constructor(private renderer:Renderer2,private elementRef:ElementRef) { }
			  ngOnInit(): void{
				this.renderer.setStyle(this.elementRef.nativeElement,'background-color','yellow')
			  }
			}
			2.Using directive in template (customColorSetter-selector defined in directive)
			<p customColorSetter>Server with {{serverId}} is {{getServerStatus()}}</p>
			
			Using Host Listener to react to user events in screen
			-----------------------------------------------------
				add below in directive class -- CustomStyleDirective
				@HostListener('mouseenter') doBelowOnMouseEnter(event:Event){
					this.renderer.setStyle(this.elementRef.nativeElement,'background-color','red')
				}
				
				HostBinding - changing style without using Renderer and ElementRef
				----------------------------------------------------
				@HostBinding('style.backgroundColor') myPreferedColor:string = 'green';
				@HostListener('mouseleave') doBelowOnLeave(event:Event){
					this.myPreferedColor='orange'
				}
		
		Custom Structural Directive
		---------------------------
			<p *ngIf="isParagraphEnabled">'Secrer Password=tuna'</p> will be converted as below at the end
			<ng-template [ngIf]="isParagraphEnabled"><p>'Secrer Password=tuna'</p></ng-template>
			
			We can create custom structural directives by using TemplateRef and ViewContainerRef 
				import { Directive, TemplateRef, ViewContainerRef, Input } from '@angular/core';
				@Directive({ selector: '[appIfNot]'})
				export class CustomIfDirective {
				  constructor(private templateRef:TemplateRef<any>,private viewRef:ViewContainerRef) { }
				  // appIfNot should be same as selector
				  @Input() set appIfNot(condition:boolean){
					if(!condition)
					  this.viewRef.createEmbeddedView(this.templateRef)
					else
					  this.viewRef.clear()
				  }   
				}
			template
			--------
				 <p *appIfNot="isParagraphDisabled">'Secrer Password=tuna'</p>
				 
	Parent to Child Data transfer
	-----------------------------
		https://angular.io/guide/component-interaction
		child
		-----
			child.component.ts
			export class HeroChildComponent {
				@Input() hero: Hero;  -->use this in template
 
			}
		Parent Component.ts
		-------------------
			parent.component.html
			<app-hero-child *ngFor="let hero of heroes"  [hero]="hero"  [master]="master"> 	</app-hero-child>
  `
			@Component({
			  selector: 'app-hero-parent',
			  template: `
				<h2>{{master}} controls {{heroes.length}} heroes</h2>
				<app-hero-child *ngFor="let hero of heroes"
				  [hero]="hero"
				  [master]="master">
				</app-hero-child>
  `
			})
			export class HeroParentComponent {
			  heroes = HEROES;
			  master = 'Master';
			}
	Child to parent (Parent listens for child event)
	------------------------------------------------
		https://angular.io/guide/component-interaction#parent-listens-for-child-event
		child:
			emit data
			
		@Component({ selector: 'app-cockpit',templateUrl: './cockpit.component.html'})
		export class CockpitComponent implements OnInit {
		  @Output() serverCreationEvent = new  EventEmitter<{name:String,content:String}>()
		  //Below values come from html
		  newServerName = ""; newServerContent = "";
		  onAddServer() {
			this.serverCreationEvent.emit({name:this.newServerName,content:this.newServerContent}); 
		  }
		}
		
		Parent:
			List to event in html and send value to typescript
			<app-cockpit (serverCreationEvent)="onServerAdded($event)"> </app-cockpit>  --HTML passing data to ts via method call onServerAdded
			
		export class AppComponent {
		  public serverElements = [];
		  onServerAdded(serverData:{name:string,content:string}) {
			this.serverElements.push({name:serverData.name,content:serverData.content,type:'server'})
		  }
		}
		
	Routing
	-------
		app-routing.module.ts file will have routing
		const routes: Routes = [
		  {path:'',component:LoginComponent},
		  {path:'login',component:LoginComponent},
		  {path:'welcome',component:WelcomeComponent},
		  {path:'**',component:ErrorComponent}
	      ];
		  
		app.component.html
			** we need to put <router-outlet></router-outlet> in app.component.html to load the section based on URL
			** Header and footer will remain same, without this routing will not work
			
		<app-menu></app-menu>		
		<router-outlet></router-outlet>
		<app-footer></app-footer>
		  
		Routing from one page to another page
		-------------------------------------
			if login successful we are routing to welcome page
			1. make Router as dependency in constructor
			2. use the Router object to redirect on condition
			
			constructor(private router:Router) { }
			handleLogin(){
				if(this.userName==='raja'&&this.pwd==='raja'){
				  this.invalidLogin=false
				  this.router.navigate(['welcome']);
				}
			}
			
		Routing with relative path
		--------------------------
			*say now you are in 
				localhost:4200/servers/1
				Now you have to go to servers/:id/edit
				
			You can do this using absolute path
				this.route.navigate(['/servers',this.server.id,'edit']);
			Using relative path
				this.route.navigate(['edit'],{ relativeTo: this.activatedRoute});
				
			passing current request params to next request
				this.route.navigate(['/servers',this.server.id,'edit'],{queryParamsHandling:"preserve"});
				//queryParamsHandling:"merge" merge : Merge new with current parameters. preserve : Preserve current parameters.
				//this.route.navigate(['edit'],{ relativeTo: this.activatedRoute,queryParamsHandling:"preserve"});
				
		Routing with parameter
		-----------------------
		Type 1:
		we are going to display login userName in welcome page
		1.Update route with param
			 app-routing.module.ts
			{path:'welcome/:name',component:WelcomeComponent}
		2. pass the userName from login.ts on routing
			 this.router.navigate(['welcome',this.userName]);
		2. welcome.ts
			1. define name field in welcome.ts to us in welcome html
			2. declare ActivatedRoute as dependency in welcome.ts
			3. oninit method get the param and using ActivatedRoute and set it in name field
				export class WelcomeComponent implements OnInit {
				  loggedInUser
				  constructor(private route:ActivatedRoute) { }
				  ngOnInit() {
					this.loggedInUser = this.route.snapshot.params['name'];
				  }
				}
	    4. display userName in html using string interpolation
			welcome{{userName}}
		
		Type 2:
		-------
		********Above step 1,2,3,4 will work only if request comes from different component
		In some scenario we will reload same page with different parameter in that scenario we have to use 	this.route.param to read variable
		**snapshot only work during component initialization
		
		Scenario
			Server-Component -> localhost:4200/servers/1
				server name:XXX
				server id: 1
				Click Here for Next Server(<a [routerLink]="['/servers',server.id+1]">Next Server</a>)
				
				here request is not coming from out side , it is from same page.
				
			TS code to read params 
			----------------------
			ngOnInit() {    
				//FOR REQUEST FROM OUTSIDE
				const serverId = Number(this.activatedRoute.snapshot.params['id'])
				this.server = this.serversService.getServer(serverId);
				
				//FOR REQUEST FROM SAME PAGE(IT WILL NOT HAPPEN FIRST TIME ,SO WE HAVE TO LISTEN FOR CHANGE)
				this.activatedRoute.params.subscribe(
				  (params:Params)=>{
					this.server = this.serversService.getServer(Number(params.id));        
				  }
				)
			}
		
		Query PARAMS IN Routing
		-----------------------
			<a [routerLink]="['/servers',server.id]" [queryParams]="{allowEdit: '1'}" class="list-group-item"
			*ngFor="let server of servers">
			{{ server.name }}
			</a>
			
			TS
			--
			this.activatedRoute.snapshot.queryParams['allowEdit']
			
		Passing Static Data Via Router Link
		-----------------------------------
			 {path:'**',component:ErrorComponent,data:{msg:"The page your looking for not found"}}
			 
			 TS
			 this.message=this.activatedRoute.snapshot.data['msg'];
			 
		Loading Data before loading component in router
		-----------------------------------------------
			{ path: "**", component: ErrorComponent,resolve:{errorMsgResolver:ErrorMessgeResolver} }
			
			ErrorMessgeResolver Service
			---------------------------
				export class ErrorMessage{
					constructor(public errorCode:string,public errorMessage:string) { } 
				}

				@Injectable({
				  providedIn: 'root'
				})
				export class ErrorMessgeResolver implements Resolve<ErrorMessage> {
				  resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<ErrorMessage> | Promise<ErrorMessage> | ErrorMessage{
					//HERE WE CAN LOAD DATA BY CALLING EXTERNAL SERVICE ALSO
					return new ErrorMessage("500","Internal Server Error");
				  }
				  constructor() { }
				}
			Component
			---------
				this.errorObj=this.activatedRoute.snapshot.data['errorMsgResolver'];
			
		
		Routing Via HTML Link
		----------------------
		<a routerLink="/todos">here</a></div>
		
		routerLinkActive
		----------------
		* routerLinkActive is used to add css to currently active route
			<ul class="nav nav-tabs">
				<li role="presentation" class="active"><a routerLink="/">Home</a></li>
				<li role="presentation"><a routerLink="/servers">Servers</a></li>
			</ul>
	
		* if we use routerLinkActive current route and route "/" will mark active we can fix this using
		routerLinkActiveOptions , with this only currently active route is marked active
			<ul class="nav nav-tabs">
				<li role="presentation" routerLinkActive="active" [routerLinkActiveOptions]="{exact:true}"><a routerLink="/">Home</a></li>
				<li role="presentation" routerLinkActive="active"><a routerLink="/servers">Servers</a></li>
			</ul>
			
	Loading Child Next to parent
	----------------------------
		route
		-----
		{path:"servers",component:ServersComponent},
		{path:"servers/:id",component:ServerComponent},
		{path:"servers/:id/edit",component:EditServerComponent},
		
		template
		--------
		<div class="row">
		  <div class="col-xs-12 col-sm-4">
			<div class="list-group">
			  <a [routerLink]="['/servers',server.id]" [queryParams]="{allowEdit: '1'}" class="list-group-item" *ngFor="let server of servers">
				{{ server.name }}
			  </a>
			</div>
		  </div>
		  <div class="col-xs-12 col-sm-4">
			<router-outlet></router-outlet>
		  </div>
		</div>
		
		Above routing config 
		/servers   -> list all servers
		/servers/1 -> loads individual server component -> it replaces serverlist component , whole page will be individual server component
		
		*By using *nested routing we can load server component next to server list component
		{
			path: "servers",component: ServersComponent,children: [
			  { path: ":id", component: ServerComponent },
			  { path: ":id/edit", component: EditServerComponent }
			]
		}
					
	
	Services:
	---------
		ng generate service service-name or ng g s service-name -> create new Service (ng generate service hardCodedAuthendication)
		common logics can be written in separate service file.
		we have define then as dependency in our component construct to make use of them
		
		Service class
		-------------
		HardcodedAuthendicationService:
		@Injectable({
		providedIn: 'root'
		})
		export class HardcodedAuthendicationService {

		  constructor() { }
		  authendicate(userNme,password){
			console.log("userNme: "+userNme+", password:"+password)
			if(userNme==='raja'&&password==='raja'){
			  console.log("in success")
			  return true;
			}
			console.log("failure")
			return false;
		  }

		}
		
		Using Service Class
		-------------------
		LoginComponent:
		constructor(private router:Router,private authService:HardcodedAuthendicationService) { }
		  handleLogin(){
			if(this.authService.authendicate(this.userName,this.pwd)) {
			  this.invalidLogin=false
			  this.router.navigate(['welcome',this.userName]);
			}
		}

	RouteGuard
	----------
		you to restrict the access of pages. like welcome page can only access after loggged in 
		
		1. Create RouteGuardService implements CanaActivate from @angular/router
		2. apply in routing
		
		RouteGuard
		----------
		export class RouteGuardService implements CanActivate {
		  constructor(private authServie:HardcodedAuthendicationService) { }
		  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot){
			if(this.authServie.isUserLoggedIn){
			  return true;
			}
			this.route.navigate(['login']);
		  }
		}
		
		app-routing.module.ts
		---------------------			
		{path:'todos',component:ListTodosComponent,canActivate:[RouteGuardService]},
	
		CanDeactivate - Preventing user from leaving page before saving changes 
		------------------------------------------------------------------------
		{ path: ":id/edit", component: EditServerComponent,canDeactivate:[CanDeactivateGuardService]}
		 
		Deactivate Guard Service
		
			import { Injectable } from "@angular/core";
			import {
			  CanDeactivate,
			  ActivatedRouteSnapshot,
			  RouterStateSnapshot
			} from "@angular/router";
			import { Observable } from "rxjs";

			export interface CanDeactivateComponent {
			  canDeactivate: () => Observable<boolean> | Promise<boolean> | boolean;
			}

			@Injectable({
			  providedIn: "root"
			})
			export class CanDeactivateGuardService
			  implements CanDeactivate<CanDeactivateComponent> {

			  canDeactivate(
				component: CanDeactivateComponent,
				currentRoute: ActivatedRouteSnapshot,
				currentState: RouterStateSnapshot,
				nextState?: RouterStateSnapshot
			  ): Observable<boolean> | Promise<boolean> | boolean {     
				return component.canDeactivate();
			  }

			  constructor() {}
			}
			
		Component
			export class EditServerComponent implements OnInit, CanDeactivateComponent {
			  canDeactivate(): Observable<boolean> | Promise<boolean> | boolean {
				if (this.isFormDataChanged() && !this.isChangesSaved) {
				  confirm("Dont you want discard the changes");
				} else {
				  return true;
				}
			  }  
			}
		
	Calling External Service:
	-------------------------
		1. Create Service class to handle service call
			HttpClient from @angular/common/http	
			
		2. You have to import HttpClientModule in app.module.ts to use it any other component or service.
		    Like FormsModule for *ngIf
			HttpClientModule from @angular/common/http
		
		export class WelcomeDataService {
		  constructor(private httpClient:HttpClient) { }
		  executeHelloWorldBeanService(){
			return  this.httpClient.get("http://localhost:8082/hello-world-bean")  
		  }  
		}
		
		raja:1 Access to XMLHttpRequest at 'http://localhost:8082/hello-world-bean' from origin 'http://localhost:4200' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
		
		3. you have to enable cross site scripting in SpringBoot Resource class
			@CrossOrigin(origins = "http://localhost:4200")
			@RestController
			public class HelloWorldController {
			
	HttpInerceptor:  Assigning auth header to all the request using http interceptor
	--------------------------------------------------------------------------------
	1. Implement HTTP interceptor
		export class HttpBasicAuthInterceptorService implements HttpInterceptor {
		  constructor() {}

			requestWithAuthHeader
			  //OVERRIDED METHOD
			intercept(request: HttpRequest<any>, next: HttpHandler) {
				this.requestWithAuthHeader = request.clone({
					headers: this.getBasicAuthorizationHeader()
				});
				return next.handle(this.requestWithAuthHeader);
			}
		  
			getBasicAuthorizationHeader() {
				return new HttpHeaders({
					Authorization: "Basic " + window.btoa("raja" + ":" + "9580")
				});
			}
		}
		
	2. Define as PROVIDER in app.module.ts
		 providers: [{provide: HTTP_INTERCEPTORS,useClass: HttpBasicAuthInterceptorService,multi: true}],
		 
	Best Practice:
	-------------
	 return  this.recipes.slice()
	 
	Complex Inter Component Communication using Services
	---------------------------------------------------------
		1. recipe-item (child or recipe-list) is grand child of recipe 
		2. onclick recipe-item component calls service method to public selected recipe
		3. recipe component listens to service and reads data
		4. By using service like this we can get rid of chain of emit and listen events.we can directly pass data from grand child 
		Service:  
		--------
		it gives method to emit the data
		import { Injectable, EventEmitter } from '@angular/core';
		import { Recipe } from './recipe.model';

		@Injectable({
		  providedIn: 'root'
		})
		export class RecipeDataService {
		  public selectedRecipeEmitter= new EventEmitter<Recipe>();  
		  emitRecipe(recipe:Recipe){
			this.selectedRecipeEmitter.emit(recipe);
		  }
		}
		
		recipe-item publishing data using service
		-----------------------------------------
		import { Component, OnInit, Input} from '@angular/core';
		import { Recipe } from '../../recipe.model';
		import { RecipeDataService } from '../../recipe-data.service';

		@Component({  selector: 'app-recipe-item',  templateUrl: './recipe-item.component.html',  styleUrls: ['./recipe-item.component.css']})
		export class RecipeItemComponent implements OnInit {
		  @Input('currentRecipeInfo') recipe:Recipe=new Recipe('','','');  
		  constructor(private recipeDataSerice:RecipeDataService) { }  
		  onClicRecipe(){
			this.recipeDataSerice.emitRecipe(this.recipe)
		  }
		}
		
		recipe getting selected data by listening to service
		-----------------------------------------------------
		export class RecipesComponent implements OnInit {
		  recipeToDetailsComponent:Recipe
		  constructor(private recipeDataService:RecipeDataService) { }
		  ngOnInit() {
			this.recipeDataService.selectedRecipeEmitter.subscribe(
			  recipe=>this.recipeToDetailsComponent=recipe
			)
		  }
		}
		
	Observables
	**********************************
	**********************************
	**********************************
	
	Angular Forms
	-------------
		Template Driven Forms
		Reactive Forms
		
		Template Driven Forms
		---------------------
			--- (ngSubmit) ngForm ngModel are few items to notice here
			--- ngModel (need to added to elements which need to be submitted as part of form, if we use the that element should have name attribute)
			--- ngForm #templateRef="ngForm" -- getting form in reffence variable
			---	(ngSubmit) gives us option to call method on submit
			
			<form (ngSubmit)="onSubmit(templateRef)" #templateRef="ngForm">
				<input type="text" id="username" class="form-control" name="username" ngModel>
				<input type="email" id="email" class="form-control" name="email" ngModel>
				<button class="btn btn-primary" type="submit">Submit</button>
			</form>
			
			TS
			--
				  onSubmit(form:NgForm){ console.log(form);}
				  
		Template Form with validations
		------------------------------
		Template
			<form (ngSubmit)="onSubmit(templateRef)" #templateRef="ngForm">
				<input type="email" class="form-control" name="email" ngModel required email #emailTempRef="ngModel">
				<span class="text-danger" *ngIf="emailTempRef.dirty&&emailTempRef.invalid">Eamil is mandatory</span>

				<select name="subscription" class="form-control">
					<option *ngFor="let currentSubscription of subscriptionList" value="{{currentSubscription}}" [selected]="currentSubscription===defaultSubscription">{{currentSubscription}}</option>
				</select>
				<input type="password" class="form-control" name="pwd" ngModel required #passwordTempRef="ngModel">
				<span class="text-danger" *ngIf="passwordTempRef.invalid&&passwordTempRef.dirty">Password is required</span>
				
				<button type="submit" class="btn btn-success" [disabled]="!templateRef.dirty||templateRef.invalid">SUBMIT</button>
			</form>
			
		TS
			export class AppComponent {
				// {static: false} required from angular 8
				@ViewChild('templateRef',{static: false}) viewChildData:NgForm;
				
				  subscriptionList=["Basic","Advanced","Pro"];
				  defaultSubscription="Advanced";

				  data={
					seletedEmail:"",
					seletedPwd:"",
					selectedSubscription:""
				  };
				  onSubmit(ngForm:NgForm){
					console.log(ngForm.form.value.email);
					this.data.seletedEmail=ngForm.form.value.email;
					this.data.seletedPwd=ngForm.form.value.pwd;
					//WE CAN REED THE FORM DATA FROM VIEW CHILD ALSO, ngSbubmit ONLY CALLED ONLY ON FORM SUBMISSION
					//IF YOU WANT TO DO SOME VALIDATION ON BLUR OR ONCHANGE YOU CAN GET DATA USING ViewChild
					this.data.selectedSubscription=this.viewChildData.form.value.pwd;
				  }
			}
		CSS
			.form-control.ng-touched.ng-invalid:not(form)  {
				border: 2px solid #a94442; /* red */
			}
			
		Reactive Forms
		--------------
			ReactiveFormsModule --> import in app.module.ts
					to work with reactive forms we needs , ReactiveFormsModule module
			Steps:
			------
				1. ReactiveFormsModule in app.module.ts
				2. singupForm: FormGroup; create form group in TS file and assign form-control fields in it
					this.singupForm = new FormGroup( {'userName':new FormControl(null))
				3. assign formGroup and formControls created in TS in template
					[formGroup]="singupForm" -> in <form> field
					formControlName="userName"
				
			1. app.module.ts
			----------------
				 ReactiveFormsModule --> import this module				
			TS
			--
				export class AppComponent implements OnInit{
				  genders = ['male', 'female'];
				  singupForm: FormGroup;

				  ngOnInit(): void{
					this.singupForm = new FormGroup(
					  {
						'userName':new FormControl(null,Validators.requiredTrue),
						'email':new FormControl(null,[Validators.requiredTrue,Validators.email]),
						'gender':new FormControl(this.genders[0])        
					  }      
					); 
				  }
				  onSubmit(){
					console.log(this.singupForm);    
				  }
				}
			
			Template
			--------
				<form (ngSubmit)="onSubmit()" [formGroup]="singupForm">
				  <input type="text" id="username" class="form-control" formControlName="userName">
				  <span *ngIf="!singupForm.get('userName').valid && singupForm.get('userName').touched">Please enter valid username</span>
				  <input type="text" id="email" class="form-control" formControlName="email">
				  <input type="radio" [value]="gender" formControlName="gender">{{ gender }}
				  <button class="btn btn-primary" type="submit">Submit</button>
				</form>
				
			Reactive Forms with *Nested FormGroup,Dynamic Form Controls using FormArray,Custom validations and Async Custom VALIDATION
			--------------------------------------------------------------------------------------------------------------------------
				TS
				--
				import { Component, OnInit } from "@angular/core";
				import { FormGroup, FormControl, Validators, FormArray } from "@angular/forms";

				@Component({
				  selector: "app-root",
				  templateUrl: "./app.component.html",
				  styleUrls: ["./app.component.css"]
				})
				export class AppComponent implements OnInit {
				  projectForm: FormGroup;
				  projectStatusList = ["Stable", "Critical", "Finished"];
				  ngOnInit(): void {
					this.projectForm = new FormGroup({
					  projIdEmail: new FormGroup({
						projectName: new FormControl(null, [Validators.required,this.forbiddenProjectName.bind(this)]),
						email: new FormControl(null,[Validators.required, Validators.email],this.forbiddenEmailAyscVal.bind(this))
					  }),
					  projectStatus: new FormControl(null),
					  hobbies:new FormArray([])
					});
				  }

				  //DYNAMIC ELEMENTS ADDING USING FORM ARRAY
				  addHobbie(){
					const hobbie = new FormControl(null,Validators.required);
					(this.projectForm.get('hobbies') as FormArray).push(hobbie);
				  }

				  onSubmit() {
					console.log(this.projectForm.value);
				  }

				  forbiddenProjectName(projectNameControl: FormControl): { [notAllowedName: string]: boolean } | null {
					if (projectNameControl.value === "Test") return { notAllowedName: true };
					return null;
				  }
				  // AYSNC VALIDATION
				  forbiddenEmailAyscVal(emailControl: FormControl): Promise<any> {
					if (emailControl.value === "abc@yz.com") {
					  return new Promise((resolve, reject) => resolve({ blockedEmail: true }));
					}
					return new Promise((resolve, reject) => resolve(null));
				  }
				}
				
				Template
				--------
				<div class="container">
				  <div class="row">
					<div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
					  <form (ngSubmit)="onSubmit()" [formGroup]="projectForm">
						<div class="form-group" formGroupName="projIdEmail">
						  <label for="projectName">Project Name</label>
						  <input type="text" class="form-control" formControlName="projectName">
						  <span
							*ngIf="projectForm.get('projIdEmail.projectName').invalid&&projectForm.get('projIdEmail.projectName').touched">
							<span *ngIf="projectForm.get('projIdEmail.projectName').errors['notAllowedName']">Name Test Not
							  Allowed</span>
							<span *ngIf="projectForm.get('projIdEmail.projectName').errors['required']">Project Name is required</span>
						  </span>
						  <hr>
						  <label for="email">Email</label>
						  <input type="email" class="form-control" formControlName="email">
						  <span *ngIf="projectForm.get('projIdEmail.email').invalid&&projectForm.get('projIdEmail.email').touched">
							<span *ngIf="projectForm.get('projIdEmail.email').errors['blockedEmail']">This Email is in blocked list Try
							  different Email id</span>

						  </span>
						  <hr>
						  <hr>
						</div>
						<div class="form-group">
						  <label for="projectStatus">Project Status</label>
						  <select name="projectStatus" class="form-control" formControlName="projectStatus">
							<option *ngFor="let currentProjectStatus of projectStatusList" value="{{currentProjectStatus}}">
							  {{currentProjectStatus}}</option>
						  </select>
						</div>
						<button class="btn-btn-success" type="submit">Submit</button>

						<hr>
						<hr>
						<div formArrayName="hobbies">
						  <label for="Hobbies"></label>
						  <button class="btn-primary" type="button" (click)="addHobbie()">New Hobbie</button>
						  <div class="form-group" *ngFor="let hobbieControl of projectForm.get('hobbies').controls;let idx=index">
							  <input type="text" class="form-control" [formControlName]="idx">
						  </div>
						</div>
							</form>
						</div>
					</div>
				</div>
