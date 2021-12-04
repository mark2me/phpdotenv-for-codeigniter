# PHP DotEnv for CodeIgniter
> Autodetect environment type and load variables from `.env` to `getenv()`, `$_ENV` and `$_SERVER` automagically.


## Installation

```
$ composer require vlucas/phpdotenv
```


## Configuration
### 1. Enable your Composer Autoload and Hooks:
Edit `application/config/config.php`

```
$config['enable_hooks'] = FALSE;
```
to
```
$config['enable_hooks'] = TRUE;
```


### 2. Add this code to your application hooks:
Add hook on `application/config/hooks.php`

```
// When your .env files on *CodeIgniter ROOT* folder
$hook['pre_system'] = function() {
	$dotenv = Dotenv\Dotenv::createImmutable( FCPATH, '.env' );
	$dotenv->load();
};
```


### 3. Create your *.env* files on CodeIgniter ROOT
```
# Database Configuration
DB_HOSTNAME="localhost"
DB_USERNAME=""
DB_PASSWORD=""
DB_DATABASE=""
DB_DRIVER="mysqli"
DB_PREFIX="web_"
```

### 4. Database Configuration
1. Edit `database.php` on `application/config/database.php`
2. Replace this code:
```
	'hostname' => 'localhost',
	'username' => '',
	'password' => '',
	'database' => '',
	'dbdriver' => 'mysqli',
	'dbprefix' => '',
```

to
```
	'hostname' => $_ENV['DB_HOSTNAME'],
	'username' => $_ENV['DB_USERNAME'],
	'password' => $_ENV['DB_PASSWORD'],
	'database' => $_ENV['DB_DATABASE'],
	'dbdriver' => $_ENV['DB_DRIVER'],
	'dbprefix' => $_ENV['DB_PREFIX'],
```

## Create a Migration
1. This will be the first migration for a new site which has a blog. All migrations go in the application/migrations/ directory and have names such as 20121031100537_add_blog.php.
```
<?php

defined('BASEPATH') OR exit('No direct script access allowed');

class Migration_Add_blog extends CI_Migration {

        public function up()
        {
                $this->dbforge->add_field(array(
                        'blog_id' => array(
                                'type' => 'INT',
                                'constraint' => 5,
                                'unsigned' => TRUE,
                                'auto_increment' => TRUE
                        ),
                        'blog_title' => array(
                                'type' => 'VARCHAR',
                                'constraint' => '100',
                        ),
                        'blog_description' => array(
                                'type' => 'TEXT',
                                'null' => TRUE,
                        ),
                ));
                $this->dbforge->add_key('blog_id', TRUE);
                $this->dbforge->create_table('blog');
        }

        public function down()
        {
                $this->dbforge->drop_table('blog');
        }
}
```
2. Then in application/config/migration.php set $config['migration_version'] = 20121031100537;.


## Usage Example
In this example some simple code is placed in application/controllers/Migrate.php to update the schema. You can run https://yourdomain.com/migrate

## Other
if you wait use *getenv()*, `$dotenv = Dotenv\Dotenv::createImmutable( FCPATH, '.env' );` change to `$dotenv = Dotenv\Dotenv::createUnsafeImmutable( FCPATH, '.env' );`