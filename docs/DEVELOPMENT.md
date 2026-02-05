# Development Guide

Complete setup instructions and development workflows for the Habibit project.

---

## Table of Contents

- [Development Environment Setup](#development-environment-setup)
- [Project Structure](#project-structure)
- [Building & Running](#building--running)
- [Development Workflows](#development-workflows)
- [Coding Standards](#coding-standards)
- [Testing](#testing)
- [Debugging](#debugging)
- [Common Tasks](#common-tasks)
- [Troubleshooting](#troubleshooting)

---

## Development Environment Setup

### Prerequisites

Choose your development track:

**Track 1: C++ CLI Development**
- C++ compiler with C++11 support
  - Linux/macOS: GCC 4.8+ or Clang 3.3+
  - Windows: MinGW-w64, MSVC 2015+, or Clang
- Text editor or IDE (VS Code, CLion, Visual Studio)

**Track 2: JavaScript Development**
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Text editor (VS Code, Sublime, Atom)

**Track 3: WordPress Platform Development**
- PHP 7.4 or higher
- MySQL 5.7 or 8.0
- Web server (Nginx, Apache, or PHP built-in server)
- Composer (optional, for dependency management)
- Local WordPress development environment:
  - [Local by Flywheel](https://localwp.com/) (recommended for beginners)
  - [XAMPP](https://www.apachefriends.org/)
  - [MAMP](https://www.mamp.info/)
  - Docker + Docker Compose

---

## Project Structure

```
Habibit/
├── Cpp Impelemtation/              # C++ desktop application
│   ├── Habibit.h                   # Class definitions
│   ├── Habibit.cpp                 # Class implementations
│   ├── mainFile.cpp                # Entry point (modular)
│   ├── Habibit Main.cpp            # Entry point (monolithic)
│   ├── Habibit Main.exe            # Pre-compiled Windows binary
│   ├── .vscode/                    # VS Code configuration
│   └── Javascript implementation/
│       └── Habibit.js              # Browser-based version
│
├── habifarm/                        # WordPress platform
│   ├── app/
│   │   ├── public/                 # WordPress root directory
│   │   │   ├── wp-admin/           # WordPress admin
│   │   │   ├── wp-content/         # Themes, plugins, uploads
│   │   │   │   ├── plugins/
│   │   │   │   │   ├── woocommerce/
│   │   │   │   │   ├── dc-woocommerce-multi-vendor/
│   │   │   │   │   └── elementor/
│   │   │   │   └── themes/
│   │   │   └── wp-config.php       # WordPress configuration
│   │   └── sql/
│   │       └── local.sql           # Database export
│   ├── conf/                        # Server configurations
│   │   ├── mysql/                  # MySQL config templates
│   │   ├── nginx/                  # Nginx config templates
│   │   └── php/                    # PHP-FPM config templates
│   └── logs/                        # Application logs
│
├── Habifarm Logo Design/            # Brand assets
│   ├── SVG/                        # Vector logos
│   ├── PDF/                        # Print-ready files
│   └── 4x/                         # High-res exports
│
├── Site Media/                      # Website media assets
│   ├── *.jpg                       # Images
│   └── *.mp4                       # Videos
│
├── docs/                            # Documentation
│   ├── ARCHITECTURE.md             # System design
│   └── DEVELOPMENT.md              # This file
│
├── README.md                        # Project overview
├── CONTRIBUTING.md                  # Contribution guidelines
├── SECURITY.md                      # Security policies
├── LICENSE                          # GPL-3.0 license
└── .gitignore                       # Git ignore rules
```

---

## Building & Running

### C++ CLI Application

**Linux/macOS:**
```bash
# Navigate to directory
cd "Cpp Impelemtation"

# Compile (basic)
g++ -std=c++11 -o habibit Habibit.cpp mainFile.cpp

# Compile with warnings and debug symbols
g++ -std=c++11 -Wall -Wextra -g -o habibit Habibit.cpp mainFile.cpp

# Run
./habibit
```

**Windows (MinGW):**
```cmd
cd "Cpp Impelemtation"

:: Compile
g++ -std=c++11 -o habibit.exe Habibit.cpp mainFile.cpp

:: Run
habibit.exe
```

**Windows (MSVC):**
```cmd
cd "Cpp Impelemtation"

:: Compile
cl /EHsc /std:c++11 Habibit.cpp mainFile.cpp /Fe:habibit.exe

:: Run
habibit.exe
```

**Alternative: Use pre-compiled binary**
```cmd
cd "Cpp Impelemtation"
"Habibit Main.exe"
```

### JavaScript Browser Client

**Option 1: Browser Console**
```bash
# Open browser developer tools (F12)
# Navigate to Console tab
# Copy-paste contents of Habibit.js
# Script will start automatically
```

**Option 2: HTML Wrapper**
```bash
cd "Cpp Impelemtation/Javascript implementation"

# Create index.html
cat > index.html << 'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Habibit - Equipment Rental</title>
</head>
<body>
    <h1>Habibit Equipment Rental</h1>
    <p>Open browser console (F12) to interact with the application.</p>
    <script src="Habibit.js"></script>
</body>
</html>
EOF

# Open in browser
# Linux
xdg-open index.html

# macOS
open index.html

# Windows
start index.html
```

### WordPress Platform

**Option 1: Using Local Development Environment**

```bash
# Using Local by Flywheel
# 1. Install Local from https://localwp.com
# 2. Create new site: "Habibit" or "Habifarm"
# 3. PHP 7.4+, MySQL 8.0, Nginx/Apache
# 4. Copy habifarm/app/public/* to site root
# 5. Import database:
#    - Open Adminer from Local
#    - Import habifarm/app/sql/local.sql
# 6. Update wp-config.php with Local's DB credentials
# 7. Start site from Local dashboard
```

**Option 2: Manual Setup (XAMPP/MAMP)**

```bash
# 1. Install XAMPP/MAMP
# 2. Start Apache and MySQL
# 3. Create database
mysql -u root -p
CREATE DATABASE habibit_local CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
EXIT;

# 4. Import SQL
mysql -u root -p habibit_local < habifarm/app/sql/local.sql

# 5. Copy files
# XAMPP: copy to C:\xampp\htdocs\habibit
# MAMP: copy to /Applications/MAMP/htdocs/habibit
cp -r habifarm/app/public/* /path/to/htdocs/habibit/

# 6. Update wp-config.php
# Edit habifarm/app/public/wp-config.php:
# DB_NAME = 'habibit_local'
# DB_USER = 'root'
# DB_PASSWORD = 'your_mysql_password'
# DB_HOST = 'localhost'

# 7. Access in browser
# http://localhost/habibit/wp-admin
```

**Option 3: Docker (TODO - Compose file needed)**

```bash
# Note: docker-compose.yml does not exist yet
# Expected workflow:

cd habifarm

# Create docker-compose.yml (needs to be added to repo)
# Start services
docker-compose up -d

# Import database
docker-compose exec mysql mysql -u root -proot local < app/sql/local.sql

# Access
# http://localhost
```

---

## Development Workflows

### Adding a New Feature to C++ Implementation

1. **Define new classes/methods in Habibit.h**
```cpp
// Example: Add rating system
class Rating {
public:
    int stars;  // 1-5
    string comment;
    Rating(int s, const string& c);
};
```

2. **Implement in Habibit.cpp**
```cpp
Rating::Rating(int s, const string& c) 
    : stars(s), comment(c) {}
```

3. **Update UI in mainFile.cpp**
```cpp
void displayRatings(const vector<Rating>& ratings) {
    // Implementation
}
```

4. **Rebuild and test**
```bash
g++ -std=c++11 -Wall -o habibit Habibit.cpp mainFile.cpp
./habibit
```

### Adding a New Feature to JavaScript Implementation

1. **Add class/function to Habibit.js**
```javascript
class Rating {
  constructor(stars, comment) {
    this.stars = stars;
    this.comment = comment;
  }
}
```

2. **Update UI functions**
```javascript
function displayRatings(ratings) {
  console.log("Ratings:");
  for (const rating of ratings) {
    console.log(`${rating.stars}⭐ - ${rating.comment}`);
  }
}
```

3. **Test in browser console**

### Modifying WordPress Platform

**Adding Custom Functionality:**

1. **Create child theme**
```bash
cd habifarm/app/public/wp-content/themes/
mkdir habibit-child
cd habibit-child

# Create style.css
cat > style.css << 'EOF'
/*
Theme Name: Habibit Child
Template: astra
*/
EOF

# Create functions.php
cat > functions.php << 'EOF'
<?php
// Custom Habibit functions
add_action('wp_enqueue_scripts', 'habibit_enqueue_styles');
function habibit_enqueue_styles() {
    wp_enqueue_style('parent-style', get_template_directory_uri() . '/style.css');
}
EOF
```

2. **Activate child theme in WordPress admin**

3. **Add custom WooCommerce hooks**
```php
// In child theme functions.php

// Apply 2.5% tax to rentals
add_filter('woocommerce_tax_rate', 'habibit_custom_tax_rate', 10, 2);
function habibit_custom_tax_rate($rate, $product) {
    if ($product->get_meta('_is_rental_equipment')) {
        return 2.5;
    }
    return $rate;
}
```

---

## Coding Standards

### C++ Style Guide

**Naming Conventions:**
- Classes: `PascalCase` (e.g., `Equipment`, `Lender`)
- Functions: `camelCase` (e.g., `addEquipment`, `calculateTotalCost`)
- Variables: `camelCase` (e.g., `costPerDay`, `daysToRent`)
- Constants: `UPPER_SNAKE_CASE` (e.g., `TAX_RATE`)

**Formatting:**
- Indentation: 4 spaces
- Braces: Same line for functions, new line for classes
- Include guards: `#ifndef HEADER_NAME_H` / `#define HEADER_NAME_H`

**Example:**
```cpp
class Equipment {
public:
    string name;
    
    Equipment(const string& n) : name(n) {}
    
    void display() const {
        cout << "Name: " << name << endl;
    }
};
```

### JavaScript Style Guide

**Naming:**
- Classes: `PascalCase`
- Functions/Variables: `camelCase`
- Constants: `UPPER_SNAKE_CASE`

**Formatting:**
- Indentation: 2 spaces
- Use `const` for immutable, `let` for mutable
- Semicolons required

**Example:**
```javascript
class Equipment {
  constructor(name) {
    this.name = name;
  }
  
  display() {
    console.log(`Name: ${this.name}`);
  }
}
```

### WordPress/PHP Style Guide

Follow [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/)

**Key Points:**
- Indentation: Tabs (not spaces)
- Braces: Same line
- Function names: `snake_case` (e.g., `habibit_add_equipment`)
- WordPress functions: Prefix with `habibit_`

---

## Testing

### Manual Testing Checklist

**C++ Application:**
- [ ] Account creation with valid credentials
- [ ] Account creation with duplicate username
- [ ] Sign-in with correct credentials
- [ ] Sign-in with incorrect credentials
- [ ] Add equipment as lender
- [ ] Display equipment list
- [ ] Borrow equipment as borrower
- [ ] Cost calculation accuracy (verify 2.5% tax)
- [ ] Multiple equipment selection
- [ ] Checkout process completion

**JavaScript Application:**
- [ ] All above tests in browser environment
- [ ] Verify prompt dialogs appear correctly
- [ ] Console output formatting

**WordPress Platform:**
- [ ] WordPress admin login
- [ ] Create WooCommerce product (equipment)
- [ ] View product on frontend
- [ ] Add to cart functionality
- [ ] Checkout process
- [ ] Multi-vendor vendor registration
- [ ] Vendor dashboard access

### Automated Testing (Not Implemented)

**TODO**: Add unit tests

**Suggested Framework:**
- C++: [Google Test](https://github.com/google/googletest)
- JavaScript: [Jest](https://jestjs.io/)
- WordPress: [WP_UnitTestCase](https://make.wordpress.org/core/handbook/testing/automated-testing/phpunit/)

---

## Debugging

### C++ Debugging

**GDB (Linux/macOS):**
```bash
# Compile with debug symbols
g++ -std=c++11 -g -o habibit Habibit.cpp mainFile.cpp

# Run with debugger
gdb ./habibit

# GDB commands:
# break main          - Set breakpoint at main
# run                 - Start execution
# step                - Step into function
# next                - Next line
# print variableName  - Inspect variable
# continue            - Continue execution
```

**Visual Studio Code:**
```json
// .vscode/launch.json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug C++",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/Cpp Impelemtation/habibit",
            "args": [],
            "cwd": "${workspaceFolder}/Cpp Impelemtation",
            "preLaunchTask": "build"
        }
    ]
}
```

### JavaScript Debugging

**Browser DevTools:**
1. Open DevTools (F12)
2. Navigate to "Sources" tab
3. Open Habibit.js
4. Click line numbers to set breakpoints
5. Reload page or trigger code
6. Use "Step over" / "Step into" / "Step out"
7. Inspect variables in "Scope" panel

### WordPress Debugging

**Enable WP_DEBUG:**
```php
// wp-config.php
define('WP_DEBUG', true);
define('WP_DEBUG_LOG', true);
define('WP_DEBUG_DISPLAY', false);
define('SCRIPT_DEBUG', true);
```

**View logs:**
```bash
tail -f habifarm/app/public/wp-content/debug.log
```

**Query Monitor Plugin:**
```bash
# Install from WordPress admin
# Dashboard → Plugins → Add New → Search "Query Monitor"
# Provides detailed debugging info (queries, hooks, PHP errors)
```

---

## Common Tasks

### Update WordPress Database Credentials

```bash
# Edit wp-config.php
nano habifarm/app/public/wp-config.php

# Update these lines:
define('DB_NAME', 'your_database');
define('DB_USER', 'your_username');
define('DB_PASSWORD', 'your_password');
define('DB_HOST', 'localhost');
```

### Export WordPress Database

```bash
# Using mysqldump
mysqldump -u root -p local > habifarm/app/sql/local_backup_$(date +%Y%m%d).sql

# Using phpMyAdmin
# Navigate to Database → Export → Go
```

### Add New Equipment Type

**C++ Version:**
No changes needed - dynamic via user input

**WordPress Version:**
```bash
# WordPress Admin → Products → Add New
# Enter:
# - Title: Equipment name
# - Price: Daily cost
# - Categories: Equipment type
# - Product Data → Inventory: Manage stock
```

---

## Troubleshooting

### C++ Compilation Errors

**Error: `g++: command not found`**
```bash
# Install GCC
# Ubuntu/Debian
sudo apt-get update && sudo apt-get install build-essential

# macOS
xcode-select --install

# Windows
# Download MinGW-w64 from https://mingw-w64.org/
```

**Error: undefined reference to class methods**
```bash
# Ensure all .cpp files are compiled
g++ -std=c++11 -o habibit Habibit.cpp mainFile.cpp
# NOT just: g++ mainFile.cpp
```

### JavaScript Issues

**Script not running:**
- Check browser console for errors
- Verify file is loaded (`<script src="Habibit.js"></script>`)
- Disable browser popup blocker (blocks `prompt()`)

### WordPress Issues

**White screen (WSOD):**
```bash
# Enable debugging
# wp-config.php: define('WP_DEBUG', true);
# Check error logs
```

**Database connection error:**
```bash
# Verify MySQL is running
service mysql status  # Linux
# or check XAMPP/MAMP control panel

# Test connection
mysql -u root -p
```

**Plugin conflicts:**
```bash
# Deactivate all plugins
# wp-content/plugins/ → rename to plugins_backup
# Reactivate one by one to identify issue
```

---

## Additional Resources

**C++ Learning:**
- [cppreference.com](https://en.cppreference.com/)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)

**WordPress Development:**
- [WordPress Developer Handbook](https://developer.wordpress.org/)
- [WooCommerce Documentation](https://woocommerce.com/documentation/)

**Tools:**
- [VS Code C++ Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
- [WordPress Code Reference](https://developer.wordpress.org/reference/)

---

**Questions?** See [Contributing Guidelines](../CONTRIBUTING.md) or open an issue.
