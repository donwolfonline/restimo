# Restimo Theme Technical Documentation

## Table of Contents
1. [System Requirements](#system-requirements)
2. [Server Configuration](#server-configuration)
3. [Database Setup](#database-setup)
4. [Installation Process](#installation-process)
5. [Development Environment](#development-environment)
6. [Production Deployment](#production-deployment)
7. [Security Considerations](#security-considerations)
8. [Performance Optimization](#performance-optimization)
9. [Troubleshooting](#troubleshooting)

## System Requirements

### WordPress Requirements
- WordPress Version: 5.0 or higher
- PHP Version: 7.4 or higher (8.0+ recommended)
- MySQL Version: 5.6 or higher / MariaDB 10.1 or higher
- HTTPS support (SSL certificate) for secure transactions

### Server Requirements
- Apache 2.4+ or Nginx 1.18+
- mod_rewrite module (Apache)
- PHP Extensions:
  - curl
  - gd
  - intl
  - mbstring
  - xml
  - zip
  - exif
  - mysqli

### PHP Configuration
```ini
memory_limit = 256M
max_execution_time = 300
post_max_size = 64M
upload_max_filesize = 64M
max_input_vars = 3000
```

### MySQL/MariaDB Configuration
```ini
max_allowed_packet = 64M
innodb_buffer_pool_size = 256M
innodb_file_per_table = 1
```

## Server Configuration

### Apache Configuration
```apache
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
```

### Nginx Configuration
```nginx
location / {
    try_files $uri $uri/ /index.php?$args;
}

location ~ \.php$ {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    fastcgi_index index.php;
    include fastcgi_params;
}
```

## Database Setup

1. **Create Database**
   ```sql
   CREATE DATABASE restimo_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   CREATE USER 'restimo_user'@'localhost' IDENTIFIED BY 'your_strong_password';
   GRANT ALL PRIVILEGES ON restimo_db.* TO 'restimo_user'@'localhost';
   FLUSH PRIVILEGES;
   ```

2. **WordPress Database Configuration**
   - Edit wp-config.php:
   ```php
   define('DB_NAME', 'restimo_db');
   define('DB_USER', 'restimo_user');
   define('DB_PASSWORD', 'your_strong_password');
   define('DB_HOST', 'localhost');
   define('DB_CHARSET', 'utf8mb4');
   define('DB_COLLATE', 'utf8mb4_unicode_ci');
   ```

## Installation Process

### Local Development Installation
1. **Set up Local Environment**
   ```bash
   # Using XAMPP/MAMP/Local
   # Copy WordPress files to htdocs/www directory
   # Import database
   ```

2. **Theme Installation**
   ```bash
   cd wp-content/themes/
   git clone https://github.com/donwolfonline/restimo.git
   cd restimo
   npm install    # If using build tools
   composer install   # If using Composer
   ```

### Production Installation
1. **Server Preparation**
   ```bash
   # Set correct file permissions
   find . -type d -exec chmod 755 {} \;
   find . -type f -exec chmod 644 {} \;
   chmod 600 wp-config.php
   ```

2. **Theme Deployment**
   - Upload via FTP/SFTP
   - Use Git deployment
   - WordPress admin panel upload

## Development Environment

### Required Tools
- Node.js (v14+)
- npm or Yarn
- Composer
- Git
- Local WordPress environment (XAMPP/MAMP/Local)
- Code editor (VS Code recommended)

### Build Process
```bash
# Install dependencies
npm install

# Development build with watch
npm run dev

# Production build
npm run build

# Lint files
npm run lint
```

## Production Deployment

### Pre-deployment Checklist
1. Update wp-config.php:
   ```php
   define('WP_DEBUG', false);
   define('WP_CACHE', true);
   define('FORCE_SSL_ADMIN', true);
   ```

2. Optimize Assets:
   ```bash
   # Minify CSS/JS
   npm run build

   # Optimize images
   npm run optimize-images
   ```

### Deployment Steps
1. Backup existing site
2. Upload new files
3. Update database
4. Clear caches
5. Test functionality

## Security Considerations

### WordPress Security
```php
# wp-config.php security settings
define('DISALLOW_FILE_EDIT', true);
define('WP_AUTO_UPDATE_CORE', true);
define('WP_DISABLE_FATAL_ERROR_HANDLER', false);
```

### File Permissions
```bash
# Set secure file permissions
find . -type d -exec chmod 755 {} \;
find . -type f -exec chmod 644 {} \;
chmod 600 wp-config.php
```

## Performance Optimization

### Caching Configuration
1. **WordPress Object Cache**
   ```php
   define('WP_CACHE', true);
   ```

2. **PHP OpCache**
   ```ini
   opcache.enable=1
   opcache.memory_consumption=128
   opcache.interned_strings_buffer=8
   opcache.max_accelerated_files=4000
   opcache.revalidate_freq=60
   ```

### Image Optimization
```bash
# Optimize images during build
npm run optimize-images
```

## Troubleshooting

### Common Issues
1. **White Screen of Death**
   - Enable WP_DEBUG
   - Check error logs
   - Increase memory limit

2. **Database Connection Issues**
   - Verify credentials
   - Check database server
   - Confirm user privileges

3. **Plugin Conflicts**
   - Disable all plugins
   - Enable one by one
   - Check error logs

### Debug Mode
```php
# wp-config.php debug settings
define('WP_DEBUG', true);
define('WP_DEBUG_LOG', true);
define('WP_DEBUG_DISPLAY', false);
define('SCRIPT_DEBUG', true);
```

For additional support or custom development:
- Email: hi@frederickdineen.com
- Website: www.frederickdineen.com
