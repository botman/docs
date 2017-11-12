# Cache drivers

- [PSR6 Cache](#psr6)
- [Doctrine Cache](#doctrine)
- [CodeIgniter Cache](#codeigniter)
- [Redis Cache](#redis)
- [Symfony Cache](#symfony)

If you want to make use of BotMans Conversation feature, you need to use a persistent cache driver, where BotMan can store and retrieve the conversations.
If not specified otherwise, BotMan will use the ``array`` cache which is non-persistent.

> {callout-info} If you use [BotMan Studio](/__version__/botman-studio) it is **not** required to specify cache drivers manually, as Laravel handles this for you.



<a id="psr6"></a>
### PSR-6 Cache
If your framework / cache driver is not explicitly supported by BotMan, you can see if your cache driver supports the [PSR-6 Caching Interface](http://www.php-fig.org/psr/psr-6/). With the PSR-6 BotMan cache driver, you can use all supported cache drivers with BotMan.

```php
use BotMan\BotMan\Cache\Psr6Cache;

$adapter = new MyCustomPsr6CacheAdapter();
$botman = BotManFactory::create($config, new Psr6Cache($adapter));
```

<a id="doctrine"></a>
### Doctrine Cache
Use any [Doctrine Cache](http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/reference/caching.html) driver by passing it to the factory:

```php
use BotMan\BotMan\Cache\DoctrineCache;

$botman = BotManFactory::create($config, new DoctrineCache($doctrineCacheDriver));
```

<a id="codeigniter"></a>
### CodeIgniter Cache
Use any [CodeIgniter Cache](https://www.codeigniter.com/userguide3/libraries/caching.html) adapter by passing it to the factory:

```php
use BotMan\BotMan\Cache\CodeIgniterCache;

$this->load->driver('cache');
$botman = BotManFactory::create($config, new CodeIgniterCache($this->cache->file));
```

<a id="redis"></a>
### Redis Cache
Use your [Redis](https://redis.io) in-memory data structure to cache BotMan conversation information. If you have the [igbinary module](https://github.com/igbinary/igbinary) available, it will be used instead of standard PHP serializer:

```php
use BotMan\BotMan\Cache\RedisCache;

$botman = BotManFactory::create($config, new RedisCache('127.0.0.1', 6379));
```

<a id="symfony"></a>
### Symfony Cache
Use any [Symfony Cache](http://symfony.com/doc/current/components/cache.html) component by passing it to the factory:

```php
use BotMan\BotMan\Cache\SymfonyCache;
use Symfony\Component\Cache\Simple\FilesystemCache;

$adapter = new FilesystemCache();
$botman = BotManFactory::create($config, new SymfonyCache($adapter));
```
