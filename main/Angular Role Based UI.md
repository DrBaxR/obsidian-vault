Tags: #angular 
Created: 2022-09-28 16:09
References: 

# Angular Role Permission Based UI
## Description
This note describes how you can implement hiding certain parts of the UI of an [[Angular]] web app based on what role the logged user has and what the permissions for that role are.

This approach uses a [[Structural Directive]] that gets applied to all the elements that should have restricted access to them. This structural directive receives the *resource* that is to be accessed and the *action* (`create`, `read`, `update`, `delete`) that will be executed for the resource by the element to which the directive is applied.

## Usage
Here's an example of how the directive can be used for a button that should only be shown *if the logged in user has permissions to **delete** a **work group***:

```html
<button *appIfUserHasAccessTo="{ action: ResourceAction.Delete, resource: Resource.WorkGroup }" (click)="delete()">
  Delete Wrok Group
</button>
```

In this example, `ResourceAction` and `Resource` are both *enum*s that contain all the possible values for the action/resource.

Besides hiding certain parts of the UI based on the permissions the user has, you might also want to restrict access to certain routes based on those permissions, which can be done using the `PermissionGuard`.

Here's how you can restrict the access to a route to only those users that have the permission to read work groups ():

*app-routing.module.ts*
```ts
[
	// ...
	{path: 'work-group', component: WorkGroupComponent, canActivate: [PermissionGuard], data: { permission: { action: ResourceAction.Read, resource: Resource.WorkGroup } }}
	// ...
]
```

## Implementation
### Directive
The implementation for the directive looks something like this:

```ts
@Directive({
  selector: '[appIfUserHasAccessTo]'
})
export class IfUserHasAccessToDirective implements OnChanges {
  @Input('appIfUserHasAccessTo') resourceToAccess: UserPermission | undefined;

  private hasView = false;

  constructor(
    private auth: AuthService,
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef,
  ) {
    auth.userPermissions$.subscribe(p => {
      this.checkPermission();
    })
  }

  ngOnChanges(changes: SimpleChanges): void {
    if (changes.resourceToAccess) {
      this.checkPermission();
    }
  }

  checkPermission() {
    if (!this.resourceToAccess) {
      if (!this.hasView) {
        this.viewContainer.createEmbeddedView(this.templateRef);
        this.hasView = true;
      }

      return;
    }

    const hasPermission = this.auth.isResourceAccessible(this.resourceToAccess);

	// v--- the important part ---v
    if (hasPermission && !this.hasView) {
      this.viewContainer.createEmbeddedView(this.templateRef);
      this.hasView = true;
    } else if (!hasPermission && this.hasView) {
      this.viewContainer.clear();
      this.hasView = false;
    }
    // ^--- the important part ---^
  }
}
```

This directive uses an `AuthService` to determine whether the resource is accessible to the user (this part is not that relevant, since it may differ from project to project). Based on the result of `isResourceAccessible`, the directive decides *whether the element to which it is applied should be rendered or not*.

This check is done in two cases:
- the `userPermissions` (data that contains what the user has access to) were fetched
- the value of the directive's input has changed
This is because the check depends on the two values.

### Guard
The guard uses the auth service's `userPermissions$` observable to determine whether it should let the user access the route or to redirect to the root route. The wait this is done is by mapping the attributes to a `boolean | UrlTree`: a `boolean` that has the value *true* when the user can access the route and a `UrlTree` when the user should be redirected.

```ts
@Injectable({
  providedIn: 'root'
})
export class PermissionGuard implements CanActivate {

  constructor(
    private auth: AuthService,
    private router: Router,
  ) {}

  canActivate(
    route: ActivatedRouteSnapshot,
    _state: RouterStateSnapshot
  ): Observable<boolean | UrlTree> | Promise<boolean | UrlTree> | boolean | UrlTree {
    const resourceToAccess: UserPermission = route.data?.permission;

    if (!resourceToAccess) return false;

	// v--- the important part ---v
    return this.auth.userPermissions$.pipe(
      filter(attributesNullable => attributesNullable != null),
      switchMap(_attributes => {
        const hasAccess = this.auth.isResourceAccessible(resourceToAccess);

        if (hasAccess) {
          return of(true);
        } else {
          return of(this.router.parseUrl('/'));
        }
      })
    ) as Observable<boolean | UrlTree>;
    // ^--- the impoortant part ---^
  }
}
```

### Auth Service
Here are the relevant parts of the `AuthService`. *Note* that the `initUser` method always gets called in the application so there is always data in the `userPermissions$` observable:

```ts
@Injectable({
  providedIn: 'root'
})
export class AuthService {

  private userPermissionsSubject$ = new BehaviorSubject<RoleAttributes | undefined>(undefined);
  public userPermissions: RoleAttributes | undefined;
  public userPermissions$ = this.userPermissionsSubject$.asObservable();

  constructor(
    private http: HttpClient,
    private roleService: RoleService,
  ) {
	// ...
	
    this.userPermissions$.subscribe(p => this.userPermissions = p);
  }

  initUser() {
	// ...

    this.roleService.getLoggedUserRoleAttributes(this.accessToken!).subscribe(attributes => {
      this.userPermissionsSubject$.next(attributes);
      this.userPermissions = attributes;
    });
  }

  isResourceAccessible(resourceToAccess: UserPermission): boolean {
    if (!this.userPermissions) return false;

    const currentPermissions = this.userPermissions[resourceToAccess.action] as Resource[];

    if (!currentPermissions) {
      throw new Error(`No permissions configured for action '${resourceToAccess.action}' in logged in user's assigned role.`);
    }

    return currentPermissions.some(r => r === resourceToAccess?.resource);
  }
}
```