## Overview

This plugin integrated different checkout flow with Sylius based applications. After installing it you should be able to choose different checkout flow for your channels in your web store.

## Installation

```bash
$ composer require jrc/checkout-flow-plugin
$ bin/console doctrine:schema:update --force

```
    
Add plugin dependencies to your AppKernel.php file:

```php
public function registerBundles()
{
    return array_merge(parent::registerBundles(), [
        ...
        
        new \Jrc\CheckoutFlowPlugin\JrcCheckoutFlowPlugin(),
    ]);
}
```

Import required config in your `app/config/config.yml` file:

```yaml
# app/config/config.yml

imports:
    ...
    
    - { resource: '@JrcCheckoutFlowPlugin/Resources/config/config.yml' }
```

Import routing in your `app/config/routing.yml` file:

```yaml

# app/config/routing.yml
...

jrc_checkout_flow_plugin_admin:
    resource: "@JrcCheckoutFlowPlugin/Resources/config/app/routing/admin.yml"
    prefix: /admin
    
Jrc_checkout_flow_plugin_checkout:
    resource: "@JrcCheckoutFlowPlugin/Resources/config/app/routing/checkout.yml"
    prefix: /checkout
```

## Configuration

Adding custom checkout flow files

```yaml
# app/config/config.yml

...

jrc_checkout_flow_plugin:
    sm_paths:
        - 'custom/path/new/checkout/custom_flow.yml'
```

Custom flow files should have following format:

```yaml
# custom/path/new/checkout/custom_flow.yml

...

custom_flow:
    choice_label: Name admin choice label
    route_map: [Sylius doc](http://docs.sylius.org/en/latest/cookbook/shop/checkout.html#adjust-checkout-resolver)
    state_machine: [Sylius doc](http://docs.sylius.org/en/latest/book/architecture/state_machine.html#state-machine)

```

If you create steps with new routes, don't forget to add it to routing.yml

## Testing

In order to run Behat suites, execute following commands:

```bash
$ docker-compose up
$ docker exec -it checkoutflowplugin_php bash
$ composer install
$ docker exec -it checkoutflowplugin_php bash
$ php tests/Application/bin/console server:run localhost:4444 --docroot=tests/Application/web/ --env test
$ docker exec -it checkoutflowplugin_php bash
$ php tests/Application/bin/console doctrine:database:create --env test
$ php tests/Application/bin/console doctrine:schema:create --env test
$ vendor/bin/behat
$ vendor/bin/phpunit
```
