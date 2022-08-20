Tags: #frontend #angular
Created: 2022-08-20 16:08
References: 

# Angular Modules Guidelines
Modules in [[Angular]] are a great way to organize your application into separate blocks of functionality. All application have a main or root module, which gets bootstrapped in order to start the application. Other than that, you can organize you application any way you want.

Each module can fall into one of the following categories:
-   **Domain** - contains a feature, user experience or business domain
-   **Routed** - top component of a module, and acts as the destination of a router route
-   **Routing** - routing configuration of another module
-   **Service** - contains utility service
-   **Widget** - provides a component, pipe or directive
-   **Shared** - provides a set of components, pipes or directives

_Domain_: only export a single top level component, which is the feature that the module was made for. Rarely includes providers (services, pipes)

_Routed_: routed modules are lazy loaded modules. They never export anything, because their components never appear in templates AND they should never be imported in another module, since that makes it so they are eagerly loaded.

_Routing_: serves as companion of domain module, and has the following responsibilities: defines routes; adds router configuration to the module's imports; adds guards and resolvers to the module's providers.

_Service_: provides an utility service, ideally consisting only of providers with no declarations. A good example if HttpClientModule

_Widget_: export a widget that will be used in another modules' templates. Examples are modules of UI libraries.

_Shared_: contains commonly used components, directives, pipes but NO services.

## Resources
https://angular.io/guide/module-types