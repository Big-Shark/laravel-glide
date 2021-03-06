# A Glide Server Provider for Laravel
[![SensioLabsInsight](https://insight.sensiolabs.com/projects/ad0422ca-e31f-44a3-b01a-ee5ec757b18d/mini.png)](https://insight.sensiolabs.com/projects/ad0422ca-e31f-44a3-b01a-ee5ec757b18d)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/freekmurze/laravel-glide/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/freekmurze/laravel-glide/?branch=master)
[![Latest Stable Version](https://poser.pugx.org/spatie/laravel-glide/version.png)](https://packagist.org/packages/spatie/laravel-glide)
[![License](https://poser.pugx.org/spatie/laravel-glide/license.png)](https://packagist.org/packages/spatie/laravel-glide)

This package provides a Service Provider that allows you to very easily integrate [Glide](http://glide.thephpleague.com/) into a Laravel project.

[Glide](http://glide.thephpleague.com/) is a easy on-demand image manipulation library written in PHP. It's part of the [League of Extraordinary Packages](http://thephpleague.com/).

Using this package you'll be able to generate image manipulations on the fly and generate URL's to those images. These URL's will be signed so only you will be able to specify which manipulations should be generated. Every manipulation will be cached.

It's also possible to generate an image manipulation separately and store it wherever you want.

## Installation


You can install the package through Composer.

```bash
composer require spatie/laravel-glide
```

You must install this service provider.

```php

// Laravel 4: app/config/app.php

'providers' => [
    ...
    'Spatie\Glide\GlideServiceProvider',
    ...
];
```

This package also comes with a facade, which provides an easy way to generate images.

```php

// Laravel 4: app/config/app.php

'aliases' => [
	...
    'GlideImage' => 'Spatie\Glide\GlideImageFacade',
    ...
]
```


You can publish the config file of the package using artisan.

```bash
php artisan config:publish spatie/laravel-glide
```

The config file looks like this:
```php

return [
    /*
     * Glide will search for images in this directory
     *
     */
    'source' => [
        'path' => storage_path('images'),
    ],

    /*
     * The directory Glide will use to store it's cache
     * A .gitignore file will be automatically placed in this directory
     * so you don't accidentally end up committing these images
     *
     */
    'cache' => [
        'path' => storage_path('glide-cache'),
    ],

    /*
     * URLs to generated images will start with this string
     *
     */
    'baseURL' => 'img',

    /*
     * The maximum allowed total image size in pixels
     */
    'maxSize' => 2000 * 2000
];

```


The options in the config file are set with sane default values and they should be self-explanatory.

## Usage 

###Generating an image on the fly

Assuming you've got an image named "kayaks.jpg" in ```app/storage/images``` (the  input directory specified in the config file) you can use this code in a blade view:

```php
<img src="{{ GlideImage::load('kayaks.jpg')-modify(['w'=> 50, 'filt'=>'greyscale']) }}" />
```
The arguments for ```modify``` can also be used as a second (optional) argument for ```load```:

```php
<img src="{{ GlideImage::load('kayaks.jpg', ['w'=> 50, 'filt'=>'greyscale']) }}" />
```

The function will output a signed URL to a greyscale version of kayaks.jpg that has a width of 50 pixels. As soon as the URL gets hit by your browser, the image will be generated on the fly. The generated image will be saved in ```app/storage/glide-cache``` (= the cache directory specified in the input file).

Take a look at [the image API of Glide](http://glide.thephpleague.com/api/size/) to see which parameters you can pass to the ```modify```-method.

###Generating an image directly on the server
It's also possible to generate an image manipulation separately and store it wherever you want.

Assuming you've got an image named "kayaks.jpg" in ```app/storage/images``` (the  input directory specified in the config file):
```php
GlideImage::load('kayaks.jpg')
	->modify(['w'=> 50, 'filt'=>'greyscale'])
	->save($pathToWhereToSaveTheImage);
```

Take a look at [the image API of Glide](http://glide.thephpleague.com/api/size/) to see which parameters you can pass to the ```modify```-method.

## Notes
### Cleaning the cache
For the moment Glide doesn't clean the cache directory, but that functionality [may be coming in a future release](https://github.com/thephpleague/glide/issues/7). Until then it's your job to keep an eye on it's total size. If it becomes too big, you can opt to delete the files inside it.
### Other filesystems
Currently this package only supports images stored on the local filesystem. Glide itself leverages [Flysystem](https://github.com/thephpleague/flysystem) to read and write to various filesystems. I'd like support for that in this package let me know or feel free to submit a pull request.

## Credits

- [Freek Van der Herten](https:/murze.be)
- [All Contributors](https://github.com/freekmurze/laravel-glide/contributors)

## License

The MIT License (MIT). Please see [LICENSE](https://github.com/freekmurze/laravel-glide/blob/master/LICENSE) for more information.

