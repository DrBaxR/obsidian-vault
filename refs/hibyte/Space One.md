Tags: #reference 
Created: 2022-08-23 12:08

# Space One
## UI
Workflows = scripts that you can define on the run
SpoQL = query language
Scripts:
- Event Consumer = script care se ruleaza cand se intampla un event pe Backend
- Import Scripts = pentru sincronizare intre instante
[[Cron Job]] = ceva ce se intampla la un interval
Datasource Import Jobs = poti sa folosesti specifici un import script si un cron expression ca sa sti din cand in cand sa ruleze scriptu
Scope = logica e la fel doar ca datele difera (un scope nu are acces la datele altui scope)

**Note**: Users vine by default -> cu ei te si loghezi pe Space One.
Spaces -> Overview => toate functionalitatile (cand creezi o instanta noua)

## Code
Space One e impartit in mai multe module ([[POM]]).
Pentru un modul nou, in `context.xml` adaug in `<Resources>` nou in care specific `base` (path-ul catre target-ul unui *modul* (proiect [[Maven]])).

*Things you need*: Tomcat 9.0.36, Postgres (user: postgres si password: hibyte420), Mongo

### Module
Modulul trebuie sa aiba la `<parent>` Space One parent module.

Pe langa ca le specifici in `context.xml`, mai trebuie sa aiba si o anumita structura. Clasa java `WhateverModule.java`, care e anotata cu `@SpaceOneModule` si care implementeaza `ModuleEntry`.

#### Feature
Mai trebuie si un folder nou `spacefeature` - contine o clasa anotata cu `@SpaceFeature` si implementeaza `SpaceFeatureHandler`, care se executa cand creezi un scope nou si specifici ca vrei si modulul respectiv (**se executa doar can faci socpe-ul**).

Pentru endpointuri anotezi tu `@ScopedResource("scope")` si implementezi `ScopeResource`.

Space Feature = folosesti `SpaceFeatureHelperService` si `ConfigurationService` ca sa specifici ce chestii din modul sa fie accesibile din scope-ul creat.

Pentru data objects, trebuie doar DTO-ul acut in Space One.

`ContentItemService` se ocupa cu gestionare tipurilor de date - interactioneaza direct cu baza de date - orice operatie faci cu acest serviciu, ai nevoie de principal.
`AppScopeService` face ce face si `ContentItemService` doar ca pe scope.

`ContentItem` - obiectul din baza de date, are mai multe metode prin care poti sa setezi proprietati.

Ca sa folosesti sistemul de authentificare facut de Space One, trebuie sa injectezi `Pricipal`-ul si sa il anotezi `@AuthenticatePrincipal` si cu ala ai acces la user-ul care a facut request-ul.

## Resources
Prezentarea lui Darius