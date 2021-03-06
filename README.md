[![Latest Stable Version](https://poser.pugx.org/tvision/rackspace-cloud-files-bundle/v/stable.png)](https://packagist.org/packages/tvision/rackspace-cloud-files-bundle)

Introduction
------------

Rackspace Cloud Files bundle is a simple and easy way to use the RackspaceCloudFilesStreamWrapper library with Symfony2 applications,

but it has also some facilities for handle the static file with the rackspace cloud files.

This Bundle borns as fork of the escapestudios/EscapeRackspaceCloudFilesBundle, now these two bundles are very different.


see the blog post for more detail

[http://www.welcometothebundle.com/symfony2-assets-on-rackspace-cloud-files/](http://www.welcometothebundle.com/symfony2-assets-on-rackspace-cloud-files)


Installation (old school)
-------------------------------

see the blog post for more detail

deps:

```
[php-opencloud]
    git=git://github.com/rackspace/php-opencloud.git
    target=/rackspace/php-opencloud

[RackspaceCloudFilesBundle]
    git=https://github.com/tvision/RackspaceCloudFilesBundle.git
    target=/bundles/Tvision/RackspaceCloudFilesBundle

[RackspaceCloudFilesStreamWrapper]
    git=https://github.com/tvision/RackspaceCloudFilesStreamWrapper.git
    target=tvision-rackspace-cloud-files-streamwrapper

```

app/autoload.php

```
$loader->registerNamespaces(array(
    //other namespaces
    'Tvision\\RackspaceCloudFilesStreamWrapper' =>  __DIR__.'/../vendor/tvision-rackspace-cloud-files-streamwrapper/src',
    'Tvision\\RackspaceCloudFilesBundle'        =>  __DIR__.'/../vendor/bundles',
  ));

```

app/AppKernel.php

```
public function registerBundles()
{
    return array(
        //other bundles
        new Tvision\RackspaceCloudFilesBundle\TvisionRackspaceCloudFilesBundle(),
    );
    ...
```

Installation Composer
-------------------------------

* 1 First, add the dependent bundle to the vendor/bundles directory. Add the following lines to the composer.json file

```
    "require": {
    # ..
    "tvision/rackspace-cloud-files-bundle": ">=2.2",
    # ..
    }
```

* 2 Then run `composer install`


* 3 Then add in your `app/AppKernel`

``` yaml

 class AppKernel extends Kernel
 {
     public function registerBundles()
     {
         $bundles = array(
         // ...
            new Tvision\RackspaceCloudFilesBundle\TvisionRackspaceCloudFilesBundle(),
         // ...

```


## Configuration

app/config/config.yml

```
#  Rackspace Cloud Files configuration

tvision_rackspace_cloud_files:
    stream_wrapper:
        register: true  # do you want to register stream wrapper?
        protocol_name: rscf
    auth:
        username: YOUR-USERNAME
        api_key: YOUR-API-KEY
        host: https://lon.identity.api.rackspacecloud.com/v2.0 # or usa
        container_name: YOUR-CONTAINER-NAME
        region: LON 
```

## Service(s)

Get the Rackspace service to work with:

```
$auth = $this->get('tvision_rackspace_cloud_files.service')

```

## Usage example without assetic

```

$conn = $this->get('tvision_rackspace_cloud_files.service');
$container = $conn->apiGetContainer('container-name');

or

$container = $this->get('tvision_rackspace_cloud_files.service')->apiGetContainer('container-name');

echo "<pre>";
printf("Container %s has %d object(s) consuming %d bytes\n",
    $container->name, $container->count, $container->bytes);
echo "</pre>";
```


## Usage example with assetic

see

http://www.welcometothebundle.com/symfony2-assets-on-rackspace-cloud-files/


## Installing bundles assets (public directory) to cloudfiles with `rscf:assets:install` special console command

```
app/console rscf:assets:install rscf://my_container/my/path
```

This will copy assets just like the `assets:install` command would but directly to cloudfiles.
**Note**: For those wondering why this command could be needed, note that assetic mainly handles js/css assets, and when
 not using the cssembed filter, you still need to install images to your cloudfiles container. This command prevent you
 from having to do that by hand.

## Installing application assets (public directory) to cloudfiles with `assetic:install` special console command

add this into the config.yml

```
assetic:
    debug: false
    use_controller: false
    write_to: rsfc://%rackspace_container_name%
```

Type to the console

```
app/console assetic:dump
```

Requirements
------------

- PHP > 5.3.0

- rackspace/php-cloudfiles.git

- tvision/RackspaceCloudFilesStreamWrapper

- Symfony2


Contribute
----------

Please feel free to use the Git issue tracking to report back any problems or errors. You're encouraged to clone the repository and send pull requests if you'd like to contribute actively in developing the library.
than add your name to this file under the contributor section



Contributor
------------

- thanks for cystbear for the tips

- the bundle is a reengeneering of the escapestudios/EscapeRackspaceCloudFilesBundle


1. liuggio

2. benjamindulau

3. toretto460


License
-------

This bundle is under the MIT license. See the complete license in the bundle:

    Resources/meta/LICENSE
