# Guidelines
## Stack
Laravel v12 https://laravel.com/docs

Flux UI v2 https://fluxui.dev/docs
- Vi skal i all hovedsak benytte komponenter med Flux UI.

Filament v4 https://filamentphp.com
Dokumentasjon: https://filamentphp.com/docs/4.x
- Vi skal holde oss til stanarder for Filamentphp der funksjonalitet ligger innenfor et Filament panel.

Lucide-ikoner
Vi elsker Heroicons, men vi erkjenner at det er et ganske begrenset ikonsett. Hvis du trenger flere ikoner, anbefaler vi å bruke Lucide i stedet.
For å gjøre det enkelt å bruke Lucide-ikoner, tilbyr Flux en praktisk Artisan-kommando for å importere dem til prosjektet ditt:

`
php artisan flux:icon
`

Denne kommandoen vil be deg velge hvilke ikoner du ønsker å importere. Du kan også manuelt spesifisere ikonene du vil importere ved å oppgi navnene deres som argumenter til kommandoen:

`
php artisan flux:icon crown grip-vertical github
`
Lucide-ikoner Filament v4 example:
```php

use CodeWithDennis\FilamentLucideIcons\Enums\LucideIcon;

public static function configure(Schema $schema): Schema
{
    return $schema
        ->components([
            Forms\Components\TextInput::make('email')
                ->prefixIcon(LucideIcon::Mail)
                ->email()
                ->required();
        ]);
}
```

## Design
Fokuser på layout, luft (spacing), typografi og estetikk — ikke plassholderinnhold. Bruk en 2-kolonne-struktur der det er hensiktsmessig, med konsekvent visuell rytme, myke skygger og store berøringsflater. Hver skjerm skal føles gjennomarbeidet og intensjonell, med en følelse av å være en native app.

UI-stil:
• Rolig, Apple-lignende estetikk med subtile graderinger eller skygger
• Fargepalett: nøytral bakgrunn (#F9FAFB eller lignende), mørk tekst (#1A1A1A), elegant aksentfarge (dyp indigo eller skogsgrønn)
• Typografi: Bruk Inter eller SF Pro, fete overskrifter (24–32 px), mellomstore etiketter, fin sekundærtekst
• Luft: Raus polstring (24–40 px), jevn vertikal rytme
• Kortkomponenter: Avrundede hjørner (2xl), hover-skygger, lette skillelinjer innvendig
• Ikoner: Lucide- eller Feather-stil linjeikoner, rene og konsistente


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

### Beta-status
- **Filament v4 er for øyeblikket i beta og er ikke stabil**
- Breaking changes kan bli introdusert i utgivelser under beta-perioden
- Anbefales å vente til stabil utgivelse for produksjonsapplikasjoner

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