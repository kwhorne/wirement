# Wirement Development Guidelines

## Stack

### Laravel v12
- **Dokumentasjon:** https://laravel.com/docs
- **Fokus:** Moderne PHP-utvikling med clean architecture
- **Krav:** PHP 8.3+ for optimal ytelse

### FluxUI v2
- **Dokumentasjon:** https://fluxui.dev/docs
- **Lisens:** Krever kommersiell lisens for forretningsbruk
- **Fokus:** Apple-inspirert design og moderne UI-komponenter
- **Bruk:** Primær frontend-komponentbibliotek

### Filament v4
- **Dokumentasjon:** https://filamentphp.com/docs/4.x
- **Fokus:** Moderne admin panels med TALL stack
- **Krav:** Laravel 11+, Livewire 3+
- **Bruk:** Admin interface og rapid prototyping

### Tailwind CSS v4
- **Dokumentasjon:** https://tailwindcss.com/docs
- **Fokus:** Utility-first CSS med OKLCH farger
- **Bruk:** Styling foundation for alle komponenter

## Wirement Development Principles

### Starter Pack Philosophy
- **Minimal & Clean:** Bare essensielle komponenter inkludert
- **Skalerbar:** Bygget for å vokse med prosjektet
- **Best Practices:** Følger Laravel og moderne web-standarder
- **Developer Experience:** Fokus på rask utvikling og enkelt vedlikehold

### Code Quality
- **Automated Testing:** Pest framework for unit og feature tests
- **Code Style:** Laravel Pint for konsistent formatering
- **Static Analysis:** PHPStan for type-sikkerhet
- **Performance:** Optimalisert for produksjon fra dag én

### Development Workflow
```bash
# Kjør alle kvalitetskontroller
composer review

# Individuell testing
composer test     # Pest tests
composer format   # Laravel Pint
composer analyse  # PHPStan
```

### Configuration Management
- **Environment:** Separate configs for dev/staging/prod
- **Secrets:** Bruk .env for sensitive data
- **Caching:** Optimalisert cache-strategi
- **Logging:** Strukturert logging for debugging

## Ikoner

### Heroicons (Standard)
- **Bruk:** Filament admin panels (kun Heroicons støttes)
- **Tilgjengelighet:** Begrenset sett, men godt integrert
- **Dokumentasjon:** https://heroicons.com/

### Lucide Icons (Utvidet)
- **Bruk:** FluxUI komponenter og frontend
- **Tilgjengelighet:** Omfattende ikonsett med 1000+ ikoner
- **Import:** Bruk FluxUI sin Artisan-kommando

```bash
# Importer spesifikke ikoner
php artisan flux:icon crown grip-vertical github

# Interaktiv import
php artisan flux:icon
```

### Filament Lucide Integration
```php
use CodeWithDennis\FilamentLucideIcons\Enums\LucideIcon;

public static function configure(Schema $schema): Schema
{
    return $schema
        ->components([
            Forms\Components\TextInput::make('email')
                ->prefixIcon(LucideIcon::Mail)
                ->email()
                ->required()
        ]);
}
```

## Design Philosophy

Wirement følger en moderne, minimalistisk designfilosofi inspirert av Apple's Human Interface Guidelines.

### Designprinsipper
- **Klarhet:** Innhold og funksjonalitet prioriteres over dekorasjon
- **Konsistens:** Samme mønstre og komponenter brukes gjennomgående
- **Hierarki:** Tydelig visuell hierarki med spacing og typografi
- **Tilgjengelighet:** WCAG 2.1 AA compliance som minimum

### Visuell Stil
- **Estetikk:** Rolig, Apple-lignende med subtile graderinger
- **Layout:** 2-kolonne struktur der hensiktsmessig
- **Rytme:** Konsekvent vertikal rytme og spacing
- **Berøringsflater:** Store, tilgjengelige interactive elementer

### Fargepalett
```css
/* Nøytrale farger */
--background: #F9FAFB;    /* Lys bakgrunn */
--text-primary: #1A1A1A;  /* Mørk tekst */
--text-secondary: #6B7280; /* Sekundær tekst */

/* Aksentfarger */
--accent-primary: #4F46E5;   /* Dyp indigo */
--accent-secondary: #059669; /* Skogsgrønn */
```

### Typografi
- **Font:** Inter eller SF Pro
- **Overskrifter:** Bold, 24-32px
- **Brødtekst:** Medium weight, 16px
- **Sekundærtekst:** Regular, 14px
- **Line-height:** 1.5-1.6 for optimal lesbarhet

### Spacing & Layout
- **Polstring:** 24-40px for rause mellomrom
- **Komponenter:** 2xl border-radius (16px)
- **Skygger:** Subtile hover-effects
- **Skillelinjer:** Lette, diskrete separatorer

### Komponenter
- **Kort:** Avrundede hjørner med subtile skygger
- **Knapper:** Tydelige call-to-action med hover-states
- **Ikoner:** Lucide line-style for konsistens
- **Inputs:** Rene former med fokus på brukervennlighet


# Filament PHP v3 til v4 - Viktigste forskjeller

## 🏗️ Arkitekturendringer

### Schema-pakke (Unified Architecture)
Filament v4 introduserer en ny unified arkitektur hvor alle Form- og Infolist-komponenter er migrert til Schema-navnerommet. Dette betyr:
- Færre navnerom å håndtere
- Du kan nå blande og matche Form- og Infolist-komponenter i samme Schema-område
- I v3 var lignende UI-elementer implementert separat; i v4 blir disse konsolidert til enkle klasser for bedre konsistens og vedlikeholdbarhet

### Actions refaktorering
- I v3 var Actions ofte en utfordring for utviklere på alle nivåer
- I v4 er Actions oppdatert til å (nesten) alle utvide fra samme base Action-klasse
- Du vil praktisk talt aldri importere feil Action-klasse igjen
- Actions kan nå gjenbrukes på tvers av flere forskjellige Filament-pakker

## ⚡ Ytelsesforbedringar

### Tabellytelse
- Rendering- og interaksjonsytelsen har forbedret seg betydelig, spesielt for store tabeller
- Mange Blade-maler har blitt optimalisert for å redusere antallet visninger som rendres
- Bruker eksisterende PHP-objekter til å rendre HTML i stedet for å inkludere nye filer
- Reduserer antallet filer som må lastes
- **Testing viser at tabeller nå renderer omtrent 2,38 ganger raskere enn i v3**

### Partial Rendering
- Filament v4 drar nytte av "partial component rendering"
- Bare deler av siden oppdateres når noe endres, i stedet for full re-rendering
- Gjør interaksjoner raskere, spesielt på sider med mange felt

## 🆕 Nye funksjoner

### Nested Resources
- En av de mest etterspurte funksjonene er nå bygget direkte inn i Filament
- Lar deg operere på en gitt Filament-ressurs innenfor konteksten til en overordnet ressurs
- **Syntaks:** `php artisan make:filament-resource Product --nested`
- Eksempel: Redigere Lesson-poster i kontekst av deres relaterte Course

### Static Data Tables
- Filament v4 kan nå ta inn statiske, ikke-Model data og vise dem
- Alle de samme funksjonene som eksisterende Filament tables
- **Bruk:** Pass inn en array av dataene i `records()`-metoden
- Perfekt for API-data eller cached data

### Multi-Factor Authentication (MFA)
- Multi-factor authentication er inkludert som et alternativ direkte i boksen
- Brukere må ta et ekstra steg når de registrerer seg og logger inn
- Flere metoder kan settes opp, systemet krever minst én for pålogging

### Email Change Verification
- For økt sikkerhet sendes kanselleringslenke til brukerens gamle e-post
- Blokkerer uautoriserte endringer
- Gir brukerne kontroll over kontoendringer
- Ingen ekstra database-migreringer nødvendig

### Forbedret Error Handling
- Kan tilpasse hvordan feilmeldinger vises i Filament-panelet
- Når Laravel's debug-modus er av, erstatter Filament Livewire's fullskjerm-feilmodaler med flash-notifikasjoner
- Full kontroll over brukeropplevelsen når noe går galt

## 🎨 Tailwind CSS v4 Integration

### Moderne fargesystem
- Tailwind CSS v4 er en stor oppdatering fokusert på ytelse og fleksibilitet
- Moderniserer fargesystemet ved å bytte fra RGB til OKLCH
- Bruker det bredere P3-fargespekteret for mer livlige, nøyaktige farger
- Filament v4 adopterer også OKLCH for sitt tema-system

### Forbedret tilgjengelighet
- Heading-nivåer genereres nå dynamisk for å opprettholde riktig semantisk HTML-struktur
- Fargepaletter genereres mer nøyaktig fra en enkelt basisf arge

## 🔧 Endringer i filbehandling

### Nye standardinnstillinger
- Standard disk for Filament er nå satt til `local` (var `public` i v3)
- Synligheten til filopplastinger er satt til `private` som standard
- Filer er ikke offentlig tilgjengelige som standard
- Du må generere midlertidige signerte URL-er for å få tilgang til dem

### Laravel 11 kompatibilitet
- Drar nytte av Laravel 11's nye "Local Temporary URLs"-funksjon
- Aktivert som standard i Laravel 11

## 🔒 Forbedret Tenancy

### Automatisk scoping
- I v3 måtte utviklere manuelt scope spørringer til gjeldende tenant
- I v4 scoper Filament automatisk alle spørringer i et panel til gjeldende tenant
- Assosierer automatisk nye poster med gjeldende tenant ved hjelp av modellhendelser
- Betydelig mindre manuelt arbeid for utviklere

## 📋 Andre forbedringer

### Page Layouts
- Hver side i Filament har nå sitt eget schema som definerer struktur og innhold
- Du kan overstyre standard side-schema ved å bruke `content()`-metoden
- Full kontroll over layout ved å legge til, fjerne eller omorganisere schema-komponenter

### Global Search
- Ny `$shouldSplitGlobalSearchTerms`-egenskap lar deg deaktivere splitting av globale søketermer
- Forbedrer søkeytelse på store datasett

### Timezone Support
- Ny `FilamentTimezone` facade lar deg sette standard tidssone globalt
- Kontrollerer flere komponenter samtidig: DateTimePicker, TextColumn, og TextEntry

## 📅 Oppgraderingsdetaljer

### Minimal Breaking Changes
- Filosofien har vært "minimale breaking changes" der det er mulig
- Din Filament v3-app bør stort sett fungere på v4
- Spesielt hvis du ikke har publisert og redigert filament core blade views
- Det vil være et oppgraderingsskript, akkurat som fra v2 til v3

### Automatisk oppgraderingsskript
```bash
# Installer oppgraderingsskriptet
composer require filament/upgrade:"^4.0" -W --dev

# Kjør oppgraderingen
vendor/bin/filament-v4

# Fjern oppgraderingsskriptet når ferdig
composer remove filament/upgrade
```

### Krav
- **PHP:** 8.2+
- **Laravel:** 11.0+
- **Livewire:** 3.0+
- **Tailwind CSS:** v4.0+ (hvis du bruker custom theme CSS fil)

## ⚠️ Viktige merknader

### Produksjonsstatus
- **Filament v4 er nå stabil** og produksjonsklar
- Regelmessige oppdateringer og sikkerhetsforbedringer
- Anbefales for nye prosjekter og produksjonsapplikasjoner

### Plugin-kompatibilitet
- Noen plugins du bruker er kanskje ikke tilgjengelige i v4 ennå
- Du kan midlertidig fjerne dem fra `composer.json` til de er oppgradert
- Erstatte dem med lignende plugins som er v4-kompatible
- Vente på at plugins blir oppgradert før du oppgraderer appen din

### Deprecated funksjoner
- Spatie Translatable Plugin får ikke v4-støtte og er nå deprecated
- Du kan bruke Lara Zeus Translatable Plugin som direkte erstatning
- Tabeller konfigurert ved å overstyre metoder på Livewire-komponentklassen er fjernet (var deprecated i v3)

## 🎯 Konklusjon

Filament v4 er mer enn bare en oppgradering - det er en komplett evolusjon av hvordan vi bygger Laravel admin panels og interne verktøy. Med fokus på:

- **Ytelse:** Betydelig raskere tabeller og partial rendering
- **Fleksibilitet:** Unified schema-system og nested resources
- **Sikkerhet:** MFA og email change verification
- **Utvikleropplevelse:** Færre navnerom, bedre organisering
- **Fremtidssikring:** Moderne web-standarder med Tailwind v4

Dette gjør v4 til den mest kraftfulle versjonen av Filament til nå, samtidig som den beholder kompatibilitet med eksisterende v3-applikasjoner så mye som mulig.

===

<laravel-boost-guidelines>
=== foundation rules ===

# Laravel Boost Guidelines

The Laravel Boost guidelines are specifically curated by Laravel maintainers for this application. These guidelines should be followed closely to enhance the user's satisfaction building Laravel applications.

## Foundational Context
This application is a Laravel application and its main Laravel ecosystems package & versions are below. You are an expert with them all. Ensure you abide by these specific packages & versions.

- php - 8.3.20
- filament/filament (FILAMENT) - v4
- laravel/framework (LARAVEL) - v12
- laravel/prompts (PROMPTS) - v0
- livewire/flux (FLUXUI_FREE) - v2
- livewire/flux-pro (FLUXUI_PRO) - v2
- livewire/livewire (LIVEWIRE) - v3
- larastan/larastan (LARASTAN) - v3
- laravel/pint (PINT) - v1
- pestphp/pest (PEST) - v3
- rector/rector (RECTOR) - v2
- tailwindcss (TAILWINDCSS) - v4


## Conventions
- You must follow all existing code conventions used in this application. When creating or editing a file, check sibling files for the correct structure, approach, naming.
- Use descriptive names for variables and methods. For example, `isRegisteredForDiscounts`, not `discount()`.
- Check for existing components to reuse before writing a new one.

## Verification Scripts
- Do not create verification scripts or tinker when tests cover that functionality and prove it works. Unit and feature tests are more important.

## Application Structure & Architecture
- Stick to existing directory structure - don't create new base folders without approval.
- Do not change the application's dependencies without approval.

## Frontend Bundling
- If the user doesn't see a frontend change reflected in the UI, it could mean they need to run `npm run build`, `npm run dev`, or `composer run dev`. Ask them.

## Replies
- Be concise in your explanations - focus on what's important rather than explaining obvious details.

## Documentation Files
- You must only create documentation files if explicitly requested by the user.


=== boost rules ===

## Laravel Boost
- Laravel Boost is an MCP server that comes with powerful tools designed specifically for this application. Use them.

## Artisan
- Use the `list-artisan-commands` tool when you need to call an Artisan command to double check the available parameters.

## URLs
- Whenever you share a project URL with the user you should use the `get-absolute-url` tool to ensure you're using the correct scheme, domain / IP, and port.

## Tinker / Debugging
- You should use the `tinker` tool when you need to execute PHP to debug code or query Eloquent models directly.
- Use the `database-query` tool when you only need to read from the database.

## Reading Browser Logs With the `browser-logs` Tool
- You can read browser logs, errors, and exceptions using the `browser-logs` tool from Boost.
- Only recent browser logs will be useful - ignore old logs.

## Searching Documentation (Critically Important)
- Boost comes with a powerful `search-docs` tool you should use before any other approaches. This tool automatically passes a list of installed packages and their versions to the remote Boost API, so it returns only version-specific documentation specific for the user's circumstance. You should pass an array of packages to filter on if you know you need docs for particular packages.
- The 'search-docs' tool is perfect for all Laravel related packages, including Laravel, Inertia, Livewire, Filament, Tailwind, Pest, Nova, Nightwatch, etc.
- You must use this tool to search for Laravel-ecosystem documentation before falling back to other approaches.
- Search the documentation before making code changes to ensure we are taking the correct approach.
- Use multiple, broad, simple, topic based queries to start. For example: `['rate limiting', 'routing rate limiting', 'routing']`.
- Do not add package names to queries - package information is already shared. For example, use `test resource table`, not `filament 4 test resource table`.

### Available Search Syntax
- You can and should pass multiple queries at once. The most relevant results will be returned first.

1. Simple Word Searches with auto-stemming - query=authentication - finds 'authenticate' and 'auth'
2. Multiple Words (AND Logic) - query=rate limit - finds knowledge containing both "rate" AND "limit"
3. Quoted Phrases (Exact Position) - query="infinite scroll" - Words must be adjacent and in that order
4. Mixed Queries - query=middleware "rate limit" - "middleware" AND exact phrase "rate limit"
5. Multiple Queries - queries=["authentication", "middleware"] - ANY of these terms


=== php rules ===

## PHP

- Always use strict typing at the head of a `.php` file: `declare(strict_types=1);`.
- Always use curly braces for control structures, even if it has one line.

### Constructors
- Use PHP 8 constructor property promotion in `__construct()`.
    - <code-snippet>public function __construct(public GitHub $github) { }</code-snippet>
- Do not allow empty `__construct()` methods with zero parameters.

### Type Declarations
- Always use explicit return type declarations for methods and functions.
- Use appropriate PHP type hints for method parameters.

<code-snippet name="Explicit Return Types and Method Params" lang="php">
protected function isAccessible(User $user, ?string $path = null): bool
{
    ...
}
</code-snippet>

## Comments
- Prefer PHPDoc blocks over comments. Never use comments within the code itself unless there is something _very_ complex going on.

## PHPDoc Blocks
- Add useful array shape type definitions for arrays when appropriate.

## Enums
- Typically, keys in an Enum should be TitleCase. For example: `FavoritePerson`, `BestLake`, `Monthly`.


=== herd rules ===

## Laravel Herd

- The application is served by Laravel Herd and will be available at: https?://[kebab-case-project-dir].test. Use the `get-absolute-url` tool to generate URLs for the user to ensure valid URLs.
- You must not run any commands to make the site available via HTTP(s). It is _always_ available through Laravel Herd.


=== filament/core rules ===

## Filament
- Filament is used by this application, check how and where to follow existing application conventions.
- Filament is a Server-Driven UI (SDUI) framework for Laravel. It allows developers to define user interfaces in PHP using structured configuration objects. It is built on top of Livewire, Alpine.js, and Tailwind CSS.
- You can use the `search-docs` tool to get information from the official Filament documentation when needed. This is very useful for Artisan command arguments, specific code examples, testing functionality, relationship management, and ensuring you're following idiomatic practices.
- Utilize static `make()` methods for consistent component initialization.

### Artisan
- You must use the Filament specific Artisan commands to create new files or components for Filament. You can find these with the `list-artisan-commands` tool, or with `php artisan` and the `--help` option.
- Inspect the required options, always pass `--no-interaction`, and valid arguments for other options when applicable.

### Filament's Core Features
- Actions: Handle doing something within the application, often with a button or link. Actions encapsulate the UI, the interactive modal window, and the logic that should be executed when the modal window is submitted. They can be used anywhere in the UI and are commonly used to perform one-time actions like deleting a record, sending an email, or updating data in the database based on modal form input.
- Forms: Dynamic forms rendered within other features, such as resources, action modals, table filters, and more.
- Infolists: Read-only lists of data.
- Notifications: Flash notifications displayed to users within the application.
- Panels: The top-level container in Filament that can include all other features like pages, resources, forms, tables, notifications, actions, infolists, and widgets.
- Resources: Static classes that are used to build CRUD interfaces for Eloquent models. Typically live in `app/Filament/Resources`.
- Schemas: Represent components that define the structure and behavior of the UI, such as forms, tables, or lists.
- Tables: Interactive tables with filtering, sorting, pagination, and more.
- Widgets: Small component included within dashboards, often used for displaying data in charts, tables, or as a stat.

### Relationships
- Determine if you can use the `relationship()` method on form components when you need `options` for a select, checkbox, repeater, or when building a `Fieldset`:

<code-snippet name="Relationship example for Form Select" lang="php">
Forms\Components\Select::make('user_id')
    ->label('Author')
    ->relationship('author')
    ->required(),
</code-snippet>


## Testing
- It's important to test Filament functionality for user satisfaction.
- Ensure that you are authenticated to access the application within the test.
- Filament uses Livewire, so start assertions with `livewire()` or `Livewire::test()`.

### Example Tests

<code-snippet name="Filament Table Test" lang="php">
    livewire(ListUsers::class)
        ->assertCanSeeTableRecords($users)
        ->searchTable($users->first()->name)
        ->assertCanSeeTableRecords($users->take(1))
        ->assertCanNotSeeTableRecords($users->skip(1))
        ->searchTable($users->last()->email)
        ->assertCanSeeTableRecords($users->take(-1))
        ->assertCanNotSeeTableRecords($users->take($users->count() - 1));
</code-snippet>

<code-snippet name="Filament Create Resource Test" lang="php">
    livewire(CreateUser::class)
        ->fillForm([
            'name' => 'Howdy',
            'email' => 'howdy@example.com',
        ])
        ->call('create')
        ->assertNotified()
        ->assertRedirect();

    assertDatabaseHas(User::class, [
        'name' => 'Howdy',
        'email' => 'howdy@example.com',
    ]);
</code-snippet>

<code-snippet name="Testing Multiple Panels (setup())" lang="php">
    use Filament\Facades\Filament;

    Filament::setCurrentPanel('app');
</code-snippet>

<code-snippet name="Calling an Action in a Test" lang="php">
    livewire(EditInvoice::class, [
        'invoice' => $invoice,
    ])->callAction('send');

    expect($invoice->refresh())->isSent()->toBeTrue();
</code-snippet>


=== filament/v4 rules ===

## Filament 4

### Important Version 4 Changes
- File visibility is now `private` by default.
- The `deferFilters` method from Filament v3 is now the default behavior in Filament v4, so users must click a button before the filters are applied to the table. To disable this behavior, you can use the `deferFilters(false)` method.
- The `Grid`, `Section`, and `Fieldset` layout components no longer span all columns by default.
- The `all` pagination page method is not available for tables by default.
- All action classes extend `Filament\Actions\Action`. No action classes exist in `Filament\Tables\Actions`.
- The `Form` & `Infolist` layout components have been moved to `Filament\Schemas\Components`, for example `Grid`, `Section`, `Fieldset`, `Tabs`, `Wizard`, etc.
- A new `Repeater` component for Forms has been added.
- Icons now use the `Filament\Support\Icons\Heroicon` Enum by default. Other options are available and documented.

### Organize Component Classes Structure
- Schema components: `Schemas/Components/`
- Table columns: `Tables/Columns/`
- Table filters: `Tables/Filters/`
- Actions: `Actions/`


=== laravel/core rules ===

## Do Things the Laravel Way

- Use `php artisan make:` commands to create new files (i.e. migrations, controllers, models, etc.). You can list available Artisan commands using the `list-artisan-commands` tool.
- If you're creating a generic PHP class, use `artisan make:class`.
- Pass `--no-interaction` to all Artisan commands to ensure they work without user input. You should also pass the correct `--options` to ensure correct behavior.

### Database
- Always use proper Eloquent relationship methods with return type hints. Prefer relationship methods over raw queries or manual joins.
- Use Eloquent models and relationships before suggesting raw database queries
- Avoid `DB::`; prefer `Model::query()`. Generate code that leverages Laravel's ORM capabilities rather than bypassing them.
- Generate code that prevents N+1 query problems by using eager loading.
- Use Laravel's query builder for very complex database operations.

### Model Creation
- When creating new models, create useful factories and seeders for them too. Ask the user if they need any other things, using `list-artisan-commands` to check the available options to `php artisan make:model`.

### APIs & Eloquent Resources
- For APIs, default to using Eloquent API Resources and API versioning unless existing API routes do not, then you should follow existing application convention.

### Controllers & Validation
- Always create Form Request classes for validation rather than inline validation in controllers. Include both validation rules and custom error messages.
- Check sibling Form Requests to see if the application uses array or string based validation rules.

### Queues
- Use queued jobs for time-consuming operations with the `ShouldQueue` interface.

### Authentication & Authorization
- Use Laravel's built-in authentication and authorization features (gates, policies, Sanctum, etc.).

### URL Generation
- When generating links to other pages, prefer named routes and the `route()` function.

### Configuration
- Use environment variables only in configuration files - never use the `env()` function directly outside of config files. Always use `config('app.name')`, not `env('APP_NAME')`.

### Testing
- When creating models for tests, use the factories for the models. Check if the factory has custom states that can be used before manually setting up the model.
- Faker: Use methods such as `$this->faker->word()` or `fake()->randomDigit()`. Follow existing conventions whether to use `$this->faker` or `fake()`.
- When creating tests, make use of `php artisan make:test [options] <name>` to create a feature test, and pass `--unit` to create a unit test. Most tests should be feature tests.

### Vite Error
- If you receive an "Illuminate\Foundation\ViteException: Unable to locate file in Vite manifest" error, you can run `npm run build` or ask the user to run `npm run dev` or `composer run dev`.


=== laravel/v12 rules ===

## Laravel 12

- Use the `search-docs` tool to get version specific documentation.
- Since Laravel 11, Laravel has a new streamlined file structure which this project uses.

### Laravel 12 Structure
- No middleware files in `app/Http/Middleware/`.
- `bootstrap/app.php` is the file to register middleware, exceptions, and routing files.
- `bootstrap/providers.php` contains application specific service providers.
- **No app\Console\Kernel.php** - use `bootstrap/app.php` or `routes/console.php` for console configuration.
- **Commands auto-register** - files in `app/Console/Commands/` are automatically available and do not require manual registration.

### Database
- When modifying a column, the migration must include all of the attributes that were previously defined on the column. Otherwise, they will be dropped and lost.
- Laravel 11 allows limiting eagerly loaded records natively, without external packages: `$query->latest()->limit(10);`.

### Models
- Casts can and likely should be set in a `casts()` method on a model rather than the `$casts` property. Follow existing conventions from other models.


=== fluxui-free/core rules ===

## Flux UI Free

- This project is using the free edition of Flux UI. It has full access to the free components and variants, but does not have access to the Pro components.
- Flux UI is a component library for Livewire. Flux is a robust, hand-crafted, UI component library for your Livewire applications. It's built using Tailwind CSS and provides a set of components that are easy to use and customize.
- You should use Flux UI components when available.
- Fallback to standard Blade components if Flux is unavailable.
- If available, use Laravel Boost's `search-docs` tool to get the exact documentation and code snippets available for this project.
- Flux UI components look like this:

<code-snippet name="Flux UI Component Usage Example" lang="blade">
    <flux:button variant="primary"/>
</code-snippet>


### Available Components
This is correct as of Boost installation, but there may be additional components within the codebase.

<available-flux-components>
avatar, badge, brand, breadcrumbs, button, callout, checkbox, dropdown, field, heading, icon, input, modal, navbar, profile, radio, select, separator, switch, text, textarea, tooltip
</available-flux-components>


=== fluxui-pro/core rules ===

## Flux UI Pro

- This project is using the Pro version of Flux UI. It has full access to the free components and variants, as well as full access to the Pro components and variants.
- Flux UI is a component library for Livewire. Flux is a robust, hand-crafted, UI component library for your Livewire applications. It's built using Tailwind CSS and provides a set of components that are easy to use and customize.
- You should use Flux UI components when available.
- Fallback to standard Blade components if Flux is unavailable.
- If available, use Laravel Boost's `search-docs` tool to get the exact documentation and code snippets available for this project.
- Flux UI components look like this:

<code-snippet name="Flux UI component usage example" lang="blade">
    <flux:button variant="primary"/>
</code-snippet>


### Available Components
This is correct as of Boost installation, but there may be additional components within the codebase.

<available-flux-components>
accordion, autocomplete, avatar, badge, brand, breadcrumbs, button, calendar, callout, card, chart, checkbox, command, context, date-picker, dropdown, editor, field, heading, icon, input, modal, navbar, pagination, popover, profile, radio, select, separator, switch, table, tabs, text, textarea, toast, tooltip
</available-flux-components>


=== livewire/core rules ===

## Livewire Core
- Use the `search-docs` tool to find exact version specific documentation for how to write Livewire & Livewire tests.
- Use the `php artisan make:livewire [Posts\CreatePost]` artisan command to create new components
- State should live on the server, with the UI reflecting it.
- All Livewire requests hit the Laravel backend, they're like regular HTTP requests. Always validate form data, and run authorization checks in Livewire actions.

## Livewire Best Practices
- Livewire components require a single root element.
- Use `wire:loading` and `wire:dirty` for delightful loading states.
- Add `wire:key` in loops:

    ```blade
    @foreach ($items as $item)
        <div wire:key="item-{{ $item->id }}">
            {{ $item->name }}
        </div>
    @endforeach
    ```

- Prefer lifecycle hooks like `mount()`, `updatedFoo()`) for initialization and reactive side effects:

<code-snippet name="Lifecycle hook examples" lang="php">
    public function mount(User $user) { $this->user = $user; }
    public function updatedSearch() { $this->resetPage(); }
</code-snippet>


## Testing Livewire

<code-snippet name="Example Livewire component test" lang="php">
    Livewire::test(Counter::class)
        ->assertSet('count', 0)
        ->call('increment')
        ->assertSet('count', 1)
        ->assertSee(1)
        ->assertStatus(200);
</code-snippet>


    <code-snippet name="Testing a Livewire component exists within a page" lang="php">
        $this->get('/posts/create')
        ->assertSeeLivewire(CreatePost::class);
    </code-snippet>


=== livewire/v3 rules ===

## Livewire 3

### Key Changes From Livewire 2
- These things changed in Livewire 2, but may not have been updated in this application. Verify this application's setup to ensure you conform with application conventions.
    - Use `wire:model.live` for real-time updates, `wire:model` is now deferred by default.
    - Components now use the `App\Livewire` namespace (not `App\Http\Livewire`).
    - Use `$this->dispatch()` to dispatch events (not `emit` or `dispatchBrowserEvent`).
    - Use the `components.layouts.app` view as the typical layout path (not `layouts.app`).

### New Directives
- `wire:show`, `wire:transition`, `wire:cloak`, `wire:offline`, `wire:target` are available for use. Use the documentation to find usage examples.

### Alpine
- Alpine is now included with Livewire, don't manually include Alpine.js.
- Plugins included with Alpine: persist, intersect, collapse, and focus.

### Lifecycle Hooks
- You can listen for `livewire:init` to hook into Livewire initialization, and `fail.status === 419` for the page expiring:

<code-snippet name="livewire:load example" lang="js">
document.addEventListener('livewire:init', function () {
    Livewire.hook('request', ({ fail }) => {
        if (fail && fail.status === 419) {
            alert('Your session expired');
        }
    });

    Livewire.hook('message.failed', (message, component) => {
        console.error(message);
    });
});
</code-snippet>


=== pint/core rules ===

## Laravel Pint Code Formatter

- You must run `vendor/bin/pint --dirty` before finalizing changes to ensure your code matches the project's expected style.
- Do not run `vendor/bin/pint --test`, simply run `vendor/bin/pint` to fix any formatting issues.


=== pest/core rules ===

## Pest

### Testing
- If you need to verify a feature is working, write or update a Unit / Feature test.

### Pest Tests
- All tests must be written using Pest. Use `php artisan make:test --pest <name>`.
- You must not remove any tests or test files from the tests directory without approval. These are not temporary or helper files - these are core to the application.
- Tests should test all of the happy paths, failure paths, and weird paths.
- Tests live in the `tests/Feature` and `tests/Unit` directories.
- Pest tests look and behave like this:
<code-snippet name="Basic Pest Test Example" lang="php">
it('is true', function () {
    expect(true)->toBeTrue();
});
</code-snippet>

### Running Tests
- Run the minimal number of tests using an appropriate filter before finalizing code edits.
- To run all tests: `php artisan test`.
- To run all tests in a file: `php artisan test tests/Feature/ExampleTest.php`.
- To filter on a particular test name: `php artisan test --filter=testName` (recommended after making a change to a related file).
- When the tests relating to your changes are passing, ask the user if they would like to run the entire test suite to ensure everything is still passing.

### Pest Assertions
- When asserting status codes on a response, use the specific method like `assertForbidden` and `assertNotFound` instead of using `assertStatus(403)` or similar, e.g.:
<code-snippet name="Pest Example Asserting postJson Response" lang="php">
it('returns all', function () {
    $response = $this->postJson('/api/docs', []);

    $response->assertSuccessful();
});
</code-snippet>

### Mocking
- Mocking can be very helpful when appropriate.
- When mocking, you can use the `Pest\Laravel\mock` Pest function, but always import it via `use function Pest\Laravel\mock;` before using it. Alternatively, you can use `$this->mock()` if existing tests do.
- You can also create partial mocks using the same import or self method.

### Datasets
- Use datasets in Pest to simplify tests which have a lot of duplicated data. This is often the case when testing validation rules, so consider going with this solution when writing tests for validation rules.

<code-snippet name="Pest Dataset Example" lang="php">
it('has emails', function (string $email) {
    expect($email)->not->toBeEmpty();
})->with([
    'james' => 'james@laravel.com',
    'taylor' => 'taylor@laravel.com',
]);
</code-snippet>


=== tailwindcss/core rules ===

## Tailwind Core

- Use Tailwind CSS classes to style HTML, check and use existing tailwind conventions within the project before writing your own.
- Offer to extract repeated patterns into components that match the project's conventions (i.e. Blade, JSX, Vue, etc..)
- Think through class placement, order, priority, and defaults - remove redundant classes, add classes to parent or child carefully to limit repetition, group elements logically
- You can use the `search-docs` tool to get exact examples from the official documentation when needed.

### Spacing
- When listing items, use gap utilities for spacing, don't use margins.

    <code-snippet name="Valid Flex Gap Spacing Example" lang="html">
        <div class="flex gap-8">
            <div>Superior</div>
            <div>Michigan</div>
            <div>Erie</div>
        </div>
    </code-snippet>


### Dark Mode
- If existing pages and components support dark mode, new pages and components must support dark mode in a similar way, typically using `dark:`.


=== tailwindcss/v4 rules ===

## Tailwind 4

- Always use Tailwind CSS v4 - do not use the deprecated utilities.
- `corePlugins` is not supported in Tailwind v4.
- In Tailwind v4, you import Tailwind using a regular CSS `@import` statement, not using the `@tailwind` directives used in v3:

<code-snippet name="Tailwind v4 Import Tailwind Diff" lang="diff"
   - @tailwind base;
   - @tailwind components;
   - @tailwind utilities;
   + @import "tailwindcss";
</code-snippet>


### Replaced Utilities
- Tailwind v4 removed deprecated utilities. Do not use the deprecated option - use the replacement.
- Opacity values are still numeric.

| Deprecated |	Replacement |
|------------+--------------|
| bg-opacity-* | bg-black/* |
| text-opacity-* | text-black/* |
| border-opacity-* | border-black/* |
| divide-opacity-* | divide-black/* |
| ring-opacity-* | ring-black/* |
| placeholder-opacity-* | placeholder-black/* |
| flex-shrink-* | shrink-* |
| flex-grow-* | grow-* |
| overflow-ellipsis | text-ellipsis |
| decoration-slice | box-decoration-slice |
| decoration-clone | box-decoration-clone |


=== tests rules ===

## Test Enforcement

- Every change must be programmatically tested. Write a new test or update an existing test, then run the affected tests to make sure they pass.
- Run the minimum number of tests needed to ensure code quality and speed. Use `php artisan test` with a specific filename or filter.
</laravel-boost-guidelines>