# Modules-Inertia

[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)

The package is designed to be used by Vue/InertiaJs in conjunction with [Laravel-Modules](https://github.com/nWidart/laravel-modules)

## Laravel compatibility

| Laravel  | modules-inertia |
| :------- | :-------------- |
| 8.0-11.x | 0.6.x - 1.0.x   |

## Installation

**Install the package via composer.**

```bash
composer require crmdesenvolvimentos/modules-inertia
```

## Config Files

**In order to edit the default configuration you may execute:**

```
php artisan vendor:publish --provider="Crmdesenvolvimentos\ModulesInertia\ModulesInertiaServiceProvider"
```

## Autoloading

**By default, the module classes are not loaded automatically. You can autoload your modules using psr-4.**
**For example:**

```json
{
  "autoload": {
    "psr-4": {
      "App\\": "app/",
      "Modules\\": "Modules/",
      "Database\\Factories\\": "database/factories/",
      "Database\\Seeders\\": "database/seeders/"
  }

}
```

**Tip: don't forget to run `composer dump-autoload` afterwards.**

## Usage

**By default, Vue module files are created in the module directory Resources/Pages**

**You can change the default directory in config/modules.php**

```php
 'Pages/Index' => 'Resources/Pages/Index.vue',
 //...
 'source' => 'Resources/Pages',
```

### For use in Controller

**The default value of Inertia::render() in a module has been changed to Inertia::module().**

**Inertia::render() is still available by default. It can be used outside of modules**

- `module_name` - real name of the current module
- `file_name` - real name of the file (no extension .vue)
- `directory_name` - if you have nested display folder structure ( you can specify the file path separating by a dot )

**For example:**

```php
    public function some_method()
    {
        return Inertia::module('module_name::file_name');
        //
        return Inertia::module('module_name::file_name', ['data'=>'some data']);
        //
        return Inertia::module('module_name::directory_name.file_name', ['data'=>'some data']);
    }
```

### If you use Vue version 3

```javascript
import { createApp, h } from "vue";
import { createInertiaApp } from "@inertiajs/inertia-vue3";

createInertiaApp({
  resolve: (name) => {
    const pages = import.meta.glob("./Pages/**/*.vue", { eager: true });
    let isModule = name.split("::");
    if (isModule.length > 1) {
      const pageModules = import.meta.glob("/Modules/**/*.vue", { eager: true });
      let module = isModule[0];
      let pathTo = isModule[1];
      return pageModules[`/Modules/${module}/${pathTo}.vue`];
    } else {
      return pages[`./Pages/${name}.vue`];
    }
  },
  setup({ el, App, props, plugin }) {
    createApp({ render: () => h(App, props) })
      .use(plugin)
      .mount(el);
  },
});
```

## Console command

**You can run `php artisan module:publish-stubs` to publish stubs.**

**And override the generation of default files**

## After create module

**To be VueJS able to find the created module, you need to rebuild the script**

```bash
npm run dev
```

## Documentation

You'll find installation instructions and full documentation on [https://docs.laravelmodules.com/](https://docs.laravelmodules.com/).

## Authors

- [Nicolas Widart](https://github.com/nWidart/)
- [Yaroslav Fedan](https://github.com/YaroslavFedan/)
- [Celio Martins](https://github.com/crmdesenvolvimentos)
- Add your clickable username here. It should point to your GitHub account.

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
