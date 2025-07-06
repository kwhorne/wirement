# Wirement - TALL Starter Pack 🚀

> **Modern Laravel starter pack with FluxUI v2 and Filament 4**

Wirement is a bloat-free starter kit for quickly launching **Laravel 12** projects. It comes with **FluxUI v2** and **Filament 4** pre-configured, plus essential tools to speed up your development—nothing more, nothing unnecessary.

## ✨ Features

### 🎨 Frontend Stack
- **FluxUI v2** - Modern, Apple-inspired component library *(requires license)*
- **Tailwind CSS 4.0** - Utility-first CSS framework
- **Hero / Lucide Icons** - Beautiful, customizable icons
- **Alpine.js** - Lightweight JavaScript framework

### 🛠️ Backend Stack
- **Laravel 12** - Latest PHP framework
- **Filament 4** - Modern admin panel builder
- **MySQL** - Reliable database solution
- **Livewire** - Dynamic frontend components

### 🚀 Pre-configured Features
- **Authentication** - Laravel Breeze with modern styling
- **Admin Panel** - Filament 4 ready to go
- **FluxUI Components** - Beautiful UI components
- **Database Seeding** - Basic user seeding
- **Asset Pipeline** - Vite with Tailwind CSS
- **Code Quality** - Laravel Pint, PHPStan, Pest testing

## 🚀 Quick Start

### Requirements
- PHP 8.3+
- Composer
- Node.js & npm
- MySQL 8.0+
- Laravel Herd (recommended)

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/kwhorne/wirement.git
cd wirement
```

2. **Install dependencies**
```bash
composer install
npm install
```

3. **Environment setup**
```bash
cp .env.example .env
php artisan key:generate
```

4. **Database setup**
```bash
php artisan migrate --seed
```

5. **Build assets**
```bash
npm run build
```

6. **Start development server**
```bash
php artisan serve
# or use Laravel Herd
```

## 📁 Project Structure

```
wirement/
├── app/
│   ├── Filament/           # Filament admin panel
│   │   └── Resources/      # Filament resources
│   ├── Models/             # Eloquent models
│   └── Http/               # Controllers, middleware
├── resources/
│   ├── views/              # Blade templates with FluxUI
│   ├── js/                 # Alpine.js components
│   └── css/                # Tailwind CSS
├── database/
│   ├── migrations/         # Database migrations
│   └── seeders/            # Database seeders
└── routes/                 # Application routes
```

## 🎯 Access Points

- **Public Site**: `http://localhost:8000`
- **Admin Panel**: `http://localhost:8000/admin`

## 🔧 Configuration

### FluxUI Icons
Import Lucide icons using the built-in Artisan command:
```bash
php artisan flux:icon crown grip-vertical github
```

### Filament Admin Panel
The admin panel is configured in `app/Providers/Filament/AdminPanelProvider.php`:
- SPA Mode enabled for better performance
- Custom theme with FluxUI integration
- Authentication with Laravel Breeze
- Profile management included

### Development Commands
Wirement includes convenient composer commands:
```bash
composer review  # Run all code quality tools
composer test    # Run Pest test suite
composer format  # Fix code style with Pint
```

## 🎨 Design System

### Color Palette
- **Background**: `#F9FAFB` (neutral-50)
- **Text**: `#1A1A1A` (gray-900)
- **Accent**: Deep indigo or forest green
- **Cards**: Rounded corners (2xl), subtle shadows

### Typography
- **Font**: Inter or SF Pro
- **Headers**: Bold, 24-32px
- **Body**: Medium weight, readable sizes
- **Spacing**: Generous padding (24-40px)

## 🛡️ Security Features

- **User Authentication** - Multi-level access control
- **Role-based Permissions** - Employee, admin, patient roles
- **Data Encryption** - Sensitive data protection
- **GDPR Compliance** - Privacy by design
- **Audit Logging** - Activity tracking

## 🔄 Development Workflow

### Database Seeding
```bash
php artisan migrate:fresh --seed
```

### Asset Development
```bash
npm run dev    # Development mode
npm run build  # Production build
npm run watch  # Watch for changes
```

### Testing
```bash
php artisan test
```

## 📦 Key Dependencies

- **laravel/framework**: `^12.0`
- **filament/filament**: `^4.0`
- **flux-ui/flux**: `^2.0` *(requires license)*
- **tailwindcss**: `^4.0`
- **alpinejs**: `^3.0`
- **livewire/livewire**: `^3.0`

## 🤝 Contributing

Contributions are welcome! Please read our contributing guidelines and submit pull requests to help improve Wirement.

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

### 📝 FluxUI License Requirements

**Important:** FluxUI v2 requires a separate commercial license for most use cases:

- **Free for personal/open-source projects** - Limited to non-commercial use
- **Commercial license required** - For any commercial or client work
- **Pricing**: Starting at $199 for single developer
- **Purchase**: Visit [FluxUI.dev](https://fluxui.dev) for licensing options

**Alternative**: You can replace FluxUI components with:
- Tailwind UI components (also requires license)
- Headless UI (free)
- Custom Tailwind components
- Other free component libraries

> **Note**: The starter pack architecture works with any component library - FluxUI is just our recommendation for the best developer experience.

## 👨‍💻 About the Developer

Wirement is developed by **Knut W. Horne** ([kwhorne.com](https://kwhorne.com)) - a passionate developer creating innovative digital solutions with focus on user experience and modern technologies.

---

**Ready to build something amazing?** 🚀

Start your next Laravel project with Wirement and experience the power of modern web development tools working together seamlessly.

## 🔄 Default Credentials

For development purposes, a default admin user is created with the following credentials:

**Admin User:**
- Email: `admin@example.com`
- Password: `password`

> **Security Note:** Remember to change the default password before deploying to production.

## 🚀 Production Deployment

Before deploying to production:

1. **Environment Configuration**
   - Set `APP_ENV=production`
   - Configure secure database credentials
   - Set up proper mail configuration
   - Configure backup and monitoring

2. **Security Checklist**
   - Change default passwords
   - Enable HTTPS/SSL
   - Configure proper file permissions
   - Set up rate limiting
   - Review user access controls

3. **Performance Optimization**
   - Enable caching (`php artisan config:cache`)
   - Optimize autoloader (`composer install --optimize-autoloader`)
   - Configure queue workers for background jobs
   - Set up CDN for static assets

## 🔧 Customization

### Adding New Filament Resources
```bash
php artisan make:filament-resource ModelName --generate
```

### Creating Custom Widgets
```bash
php artisan make:filament-widget WidgetName
```

### FluxUI Components
Wirement is built to work with FluxUI v2 components. **Note:** FluxUI requires a separate license for commercial use. Check the [FluxUI documentation](https://fluxui.dev/docs) for usage examples and licensing information.

### Artisan Commands
The project includes essential Artisan commands:
- `php artisan flux:icon` - Import Lucide icons
- `php artisan make:filament-user` - Create admin users

## 🌟 What's Included

✅ **Laravel 12 Foundation**  
✅ **Filament 4 Admin Panel**  
✅ **FluxUI v2 Ready** *(requires license)*  
✅ **Tailwind CSS 4.0**  
✅ **Alpine.js Integration**  
✅ **Laravel Breeze Authentication**  
✅ **Lucide Icons Support**  
✅ **Vite Asset Pipeline**  
✅ **Database Migrations**  
✅ **User Seeding**  
✅ **Code Quality Tools**  
✅ **Pest Testing Framework**  
✅ **Modern UI Components**  
✅ **Production-Ready Config**  

## 🆘 Support

If you encounter any issues or have questions:

1. Check the [Laravel documentation](https://laravel.com/docs)
2. Review [Filament documentation](https://filamentphp.com/docs)
3. Consult [FluxUI documentation](https://fluxui.dev/docs)
4. Open an issue on GitHub

## 🎯 Next Steps

After installation, you can:

1. **Explore the Admin Panel** - Navigate to `/admin` to see Filament in action
2. **Customize the Design** - Modify colors, fonts, and spacing to match your brand
3. **Add Your Models** - Create Eloquent models and Filament resources
4. **Build Your Frontend** - Use FluxUI components in your Blade templates
5. **Deploy to Production** - Follow the deployment checklist above

## 💡 Tips for Success

- **Start with Models** - Define your data structure first
- **Use Filament Resources** - Leverage the admin panel for quick CRUD operations
- **Embrace FluxUI** - Use the beautiful components for consistent design
- **Follow TALL Stack** - Keep your architecture clean and maintainable
- **Write Tests** - Use the included Pest framework for quality assurance

---

**Built with ❤️ by Knut W. Horne** - *Innovative digital solutions for modern web development*

