Tags: #frontend #angular 
Created: 2022-09-03 14:09
References: 

# Angular Interceptor
[[Angular]] interceptors are providers that implement the `HttpInterceptor` interface, that are used to transform an ongoing request before passing it to the next interceptor in the chain.

Interceptors may also modify the stream of the response by applying additional [[RxJS Operator]]s with to `next.handle()` observable.

It is also possible for interceptors to handle a request entirely, which is a more rare use case.

The order in which the interceptors are chained is the order in which they are provided. This **can not** be changed after it was specified.

```ts
export const interceptorProviders =
[
	{ provide: HTTP_INTERCEPTORS, useClass: RequestTimestampService, multi: true },
	{ provide: HTTP_INTERCEPTORS, useClass: AjaxBusyIdentifierInterceptorService, multi: true },
	{ provide: HTTP_INTERCEPTORS, useClass: AuthInterceptorService, multi: true },
	{ provide: HTTP_INTERCEPTORS, useClass: XML2JsonInterceptorService, multi: true },
	{ provide: HTTP_INTERCEPTORS, useClass: ErrorNotifierService, multi: true },
	{ provide: HTTP_INTERCEPTORS, useClass: RetryInterceptorService, multi: true }
];
```

The `multi: true` property must be set if multiple interceptors are being provided.