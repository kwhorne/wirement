# Wirement Development Guidelines

## Stack

### Laravel v12
- **Documentation:** https://laravel.com/docs
- **Focus:** Modern PHP development with clean architecture
- **Requirements:** PHP 8.3+ for optimal performance

### FluxUI v2
- **Documentation:** https://fluxui.dev/docs
- **License:** Requires commercial license for business use
- **Focus:** Apple-inspired design and modern UI components
- **Usage:** Primary frontend component library

### Filament v4
- **Documentation:** https://filamentphp.com/docs/4.x
- **Focus:** Modern admin panels with TALL stack
- **Requirements:** Laravel 11+, Livewire 3+
- **Usage:** Admin interface and rapid prototyping

### Tailwind CSS v4
- **Documentation:** https://tailwindcss.com/docs
- **Focus:** Utility-first CSS with OKLCH colors
- **Usage:** Styling foundation for all components

## Wirement Development Principles

### Starter Pack Philosophy
- **Minimal & Clean:** Only essential components included
- **Scalable:** Built to grow with your project
- **Best Practices:** Follows Laravel and modern web standards
- **Developer Experience:** Focus on rapid development and easy maintenance

### Code Quality
- **Automated Testing:** Pest framework for unit and feature tests
- **Code Style:** Laravel Pint for consistent formatting
- **Static Analysis:** PHPStan for type safety
- **Performance:** Optimized for production from day one

### Development Workflow
```bash
# Run all quality checks
composer review

# Individual testing
composer test     # Pest tests
composer format   # Laravel Pint
composer analyse  # PHPStan
```

### Configuration Management
- **Environment:** Separate configs for dev/staging/prod
- **Secrets:** Use .env for sensitive data
- **Caching:** Optimized cache strategy
- **Logging:** Structured logging for debugging

## Icons

### Heroicons (Standard)
- **Usage:** Filament admin panels (only Heroicons supported)
- **Availability:** Limited set, but well integrated
- **Documentation:** https://heroicons.com/

### Lucide Icons (Extended)
- **Usage:** FluxUI components and frontend
- **Availability:** Comprehensive icon set with 1000+ icons
- **Import:** Use FluxUI's Artisan command

```bash
# Import specific icons
php artisan flux:icon crown grip-vertical github

# Interactive import
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

Wirement follows a modern, minimalist design philosophy inspired by Apple's Human Interface Guidelines.

### Design Principles
- **Clarity:** Content and functionality prioritized over decoration
- **Consistency:** Same patterns and components used throughout
- **Hierarchy:** Clear visual hierarchy with spacing and typography
- **Accessibility:** WCAG 2.1 AA compliance as minimum

### Visual Style
- **Aesthetics:** Calm, Apple-like with subtle gradients
- **Layout:** 2-column structure where appropriate
- **Rhythm:** Consistent vertical rhythm and spacing
- **Touch Targets:** Large, accessible interactive elements

### Color Palette
```css
/* Neutral colors */
--background: #F9FAFB;    /* Light background */
--text-primary: #1A1A1A;  /* Dark text */
--text-secondary: #6B7280; /* Secondary text */

/* Accent colors */
--accent-primary: #4F46E5;   /* Deep indigo */
--accent-secondary: #059669; /* Forest green */
```

### Typography
- **Font:** Inter or SF Pro
- **Headings:** Bold, 24-32px
- **Body text:** Medium weight, 16px
- **Secondary text:** Regular, 14px
- **Line-height:** 1.5-1.6 for optimal readability

### Spacing & Layout
- **Padding:** 24-40px for generous spacing
- **Components:** 2xl border-radius (16px)
- **Shadows:** Subtle hover effects
- **Dividers:** Light, discrete separators

### Components
- **Cards:** Rounded corners with subtle shadows
- **Buttons:** Clear call-to-action with hover states
- **Icons:** Lucide line-style for consistency
- **Inputs:** Clean forms with focus on usability

## Filament PHP v3 to v4 - Key Differences

### Architecture Changes

#### Schema Package (Unified Architecture)
Filament v4 introduces a new unified architecture where all Form and Infolist components are migrated to the Schema namespace. This means:
- Fewer namespaces to handle
- You can now mix and match Form and Infolist components in the same Schema area
- In v3, similar UI elements were implemented separately; in v4, these are consolidated into single classes for better consistency and maintainability

#### Actions Refactoring
- In v3, Actions were often a challenge for developers at all levels
- In v4, Actions are updated so (almost) all extend from the same base Action class
- You will practically never import the wrong Action class again
- Actions can now be reused across multiple different Filament packages

## Performance Improvements

### Table Performance
- Rendering and interaction performance has improved significantly, especially for large tables
- Many Blade templates have been optimized to reduce the number of views that are rendered
- Uses existing PHP objects to render HTML instead of including new files
- Reduces the number of files that need to be loaded
- **Testing shows tables now render approximately 2.38 times faster than in v3**

### Partial Rendering
- Filament v4 takes advantage of "partial component rendering"
- Only parts of the page are updated when something changes, instead of full re-rendering
- Makes interactions faster, especially on pages with many fields

## New Features

### Nested Resources
- One of the most requested features is now built directly into Filament
- Allows you to operate on a given Filament resource within the context of a parent resource
- **Syntax:** `php artisan make:filament-resource Product --nested`
- Example: Edit Lesson records in the context of their related Course

### Static Data Tables
- Filament v4 can now take in static, non-Model data and display it
- All the same features as existing Filament tables
- **Usage:** Pass in an array of data in the `records()` method
- Perfect for API data or cached data

### Multi-Factor Authentication (MFA)
- Multi-factor authentication is included as an option directly out of the box
- Users must take an extra step when registering and logging in
- Multiple methods can be set up, system requires at least one for login

### Email Change Verification
- For increased security, cancellation link is sent to user's old email
- Blocks unauthorized changes
- Gives users control over account changes
- No additional database migrations needed

### Improved Error Handling
- Can customize how error messages are displayed in the Filament panel
- When Laravel's debug mode is off, Filament replaces Livewire's fullscreen error modals with flash notifications
- Full control over user experience when something goes wrong

## Tailwind CSS v4 Integration

### Modern Color System
- Tailwind CSS v4 is a major update focused on performance and flexibility
- Modernizes the color system by switching from RGB to OKLCH
- Uses the broader P3 color spectrum for more vivid, accurate colors
- Filament v4 also adopts OKLCH for its theme system

### Improved Accessibility
- Heading levels are now generated dynamically to maintain proper semantic HTML structure
- Color palettes are generated more accurately from a single base color

## File Handling Changes

### New Defaults
- Default disk for Filament is now set to `local` (was `public` in v3)
- File upload visibility is set to `private` by default
- Files are not publicly accessible by default
- You must generate temporary signed URLs to access them

### Laravel 11 Compatibility
- Takes advantage of Laravel 11's new "Local Temporary URLs" feature
- Enabled by default in Laravel 11

## Enhanced Tenancy

### Automatic Scoping
- In v3, developers had to manually scope queries to the current tenant
- In v4, Filament automatically scopes all queries in a panel to the current tenant
- Automatically associates new records with the current tenant using model events
- Significantly less manual work for developers

## Other Improvements

### Page Layouts
- Each page in Filament now has its own schema that defines structure and content
- You can override the default page schema by using the `content()` method
- Full control over layout by adding, removing, or reorganizing schema components

### Global Search
- New `$shouldSplitGlobalSearchTerms` property lets you disable splitting of global search terms
- Improves search performance on large datasets

### Timezone Support
- New `FilamentTimezone` facade lets you set default timezone globally
- Controls multiple components simultaneously: DateTimePicker, TextColumn, and TextEntry

## Upgrade Details

### Minimal Breaking Changes
- Philosophy has been "minimal breaking changes" where possible
- Your Filament v3 app should mostly work on v4
- Especially if you haven't published and edited filament core blade views
- There will be an upgrade script, just like from v2 to v3

### Automatic Upgrade Script
```bash
# Install the upgrade script
composer require filament/upgrade:"^4.0" -W --dev

# Run the upgrade
vendor/bin/filament-v4

# Remove the upgrade script when done
composer remove filament/upgrade
```

### Requirements
- **PHP:** 8.2+
- **Laravel:** 11.0+
- **Livewire:** 3.0+
- **Tailwind CSS:** v4.0+ (if using custom theme CSS file)

## Important Notes

### Production Status
- **Filament v4 is now stable** and production-ready
- Regular updates and security improvements
- Recommended for new projects and production applications

### Plugin Compatibility
- Some plugins you use may not be available in v4 yet
- You can temporarily remove them from `composer.json` until they are upgraded
- Replace them with similar plugins that are v4-compatible
- Wait for plugins to be upgraded before upgrading your app

### Deprecated Features
- Spatie Translatable Plugin will not receive v4 support and is now deprecated
- You can use Lara Zeus Translatable Plugin as a direct replacement
- Tables configured by overriding methods on the Livewire component class have been removed (was deprecated in v3)

## Conclusion

Filament v4 is more than just an upgrade - it's a complete evolution of how we build Laravel admin panels and internal tools. With focus on:

- **Performance:** Significantly faster tables and partial rendering
- **Flexibility:** Unified schema system and nested resources
- **Security:** MFA and email change verification
- **Developer Experience:** Fewer namespaces, better organization
- **Future-proofing:** Modern web standards with Tailwind v4
