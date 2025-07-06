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