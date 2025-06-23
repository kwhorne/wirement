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
