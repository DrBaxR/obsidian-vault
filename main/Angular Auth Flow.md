Tags: #frontend #angular 
Created: 2022-09-03 13:09
References: 

# Angular Auth Flow
## Description
[[Authentication]] and [[Authorization]] flow for [[Angular]], using [[Angular Interceptor]]s and [[Angular Guard]]s.

## Flows
All the cases which might be encountered within the auth flow and how they are treated.

### Login
1. User accesses the login page
2. Presses the login button after filling in credentials
3. Request is sent to auth server's `/token` endpoint
4. Client receives an **access token** ([[JWT]]) that it caches in `localStorage`
5. Client navigates to the `returnUrl` query param

### Resource access
1. User wants to navigate to a page
2. Auth guard checks if `localStorage` contains a token

#### Valid token
3. It does so guard lets the user access that route
4. Once page is loaded it requests resources from backend
5. Auth interceptor attaches necessary header that contains the *access token* for authorization
6. Response comes with resource

#### Invalid/Expired token
3. It does so guard lets the user access that route
4. Once page is loaded requests for resources are sent
5. Auth interceptor attaches necessary header that contains the *access token* for authorization
6. Response with error arrives with status `401 Unauthorized`
7. Error interceptor handles response:
	1. Removes token from `localStorage`
	2. Reloads page
	3. Auth guard checks the `localStorage` for token, does not find it so it redirects user to the login page setting the `returnUrl` query param to the current route

#### No token
3. Token not found, so reirects user to the login page setting the `returnUrl` query param

### Logout
1. Token is removed from `localStorage`
2. Page is reloaded
3. Auth guard does not find token in `localStorage`
4. User redirected to login page

## Code
Here are all the required sequences of code for the flow.

### Module
```ts
@NgModule({
  // ...
  providers: [
    ConfirmationService,
    { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true },
    { provide: HTTP_INTERCEPTORS, useClass: ErrorInterceptor, multi: true },
  ],
  // ...
})
export class AppModule {
}
```

### Routing Module
```ts
const routes: Routes = [
  // ...
  {
    path: 'mediQ',
    component: DashboardComponent,
    canActivate: [AuthGuard],
    children: [
      // ...
    ]
  },
  // ...
];
  
@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {
}
```

### Auth Guard
```ts
@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {

  constructor(
    private router: Router,
    private auth: AuthService,
  ) {}
  
  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot,
  ): boolean | UrlTree | Observable<boolean | UrlTree> | Promise<boolean | UrlTree> {
    if (this.auth.isLoggedIn) {
      return true;
    }
  
    this.router.navigate(['/login'], { queryParams: { returnUrl: state.url } });
    return false;
  }
}
```

### Auth Interceptor
```ts
@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  
  constructor(
    private auth: AuthService,
  ) {}
  
  intercept(request: HttpRequest<unknown>, next: HttpHandler): Observable<HttpEvent<unknown>> {
    const tokenType = this.auth.tokenType;
    const accessToken = this.auth.accessToken;
  
    if (accessToken) {
      request = request.clone({
        setHeaders: {
          Authorization: `${tokenType} ${accessToken}`,
        }
      })
    }
  
    return next.handle(request);
  }
}
```

### Error Interceptor
```ts
@Injectable()
export class ErrorInterceptor implements HttpInterceptor {
  
  constructor(
    private router: Router,
    private auth: AuthService,
  ) {}
  
  intercept(request: HttpRequest<unknown>, next: HttpHandler): Observable<HttpEvent<unknown>> {
    return next.handle(request).pipe(
      catchError((err: HttpErrorResponse) => {
        if (err.status === 401) {
          this.auth.logout();
          location.reload();
        }
  
        return EMPTY
      })
    );
  }
}
```

### Auth Service
```ts
export const TOKEN_KEY = 'token'
  
export interface AuthToken {
  access_token: string;
  expires_in: number;
  'not-before-policy': number;
  refresh_expires_in: number;
  refresh_token: string;
  scope: string;
  session_state: string;
  token_type: string;
}
  
@Injectable({
  providedIn: 'root'
})
export class AuthService {
  
  private readonly tokenUrl = 'https://app.auth/token';
  private token: AuthToken;
  
  get accessToken(): string | undefined {
    return this.token?.access_token;
  }
  
  get tokenType(): string | undefined {
    return this.token?.token_type;
  }
  
  get isLoggedIn(): boolean {
    return !!this.token;
  }
  
  constructor(
    private http: HttpClient,
    private router: Router,
  ) {
    const tokenString = localStorage.getItem(TOKEN_KEY);
    this.token = tokenString ? JSON.parse(tokenString) : undefined;
  }
  
  login(username: string, password: string, afterLogin: () => void) {
    const headers = new HttpHeaders({ 'Content-type': 'application/x-www-form-urlencoded' })
    const body = this.createTokenRequestBody(username, password);
  
    this.http.post<AuthToken>(
      this.tokenUrl,
      body,
      { headers }
    ).subscribe(res => {
      this.token = res;
      localStorage.setItem(TOKEN_KEY, JSON.stringify(res));
  
      afterLogin();
    })
  }
  
  private createTokenRequestBody(username: string, password: string): URLSearchParams {
    const body = new URLSearchParams()
    body.set('realm', 'realm_name')
    body.set('grant_type', 'password')
    body.set('client_id', 'client_name')
    body.set('bearer-only', 'true')
    body.set('username', username)
    body.set('password', password)
  
    return body;
  }
  
  logout() {
    localStorage.removeItem(TOKEN_KEY);
    location.reload();
  }
}
```

### Login Component
```ts
export class LoginComponent implements OnInit {
  
  submitted = false;
  isLoading = false;
  
  private returnUrl: string;
  
  constructor(
    private route: ActivatedRoute,
    private router: Router,
    public auth: AuthService,
  ) {
  }
  
  ngOnInit(): void {
    this.returnUrl = this.route.snapshot.queryParams.returnUrl ?? '/mediQ'
  }
  
  restoreNavigation = () => {
    this.router.navigate([this.returnUrl]);
  }

  // template uses `auth.login(email.value, password.value, restoreNavigation)`
}
```