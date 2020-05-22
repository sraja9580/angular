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
		Routing with parameter
		-----------------------
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
			
		Routing Via HTML Link
		----------------------
		<a routerLink="/todos">here</a></div>
		
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
		 
	