# `Polycast_Filter_ImageSize`

> Zend Framework extension providing filter facilities for image size using 
> different strategies. 


## Minimal example


    <?php
    require_once 'Polycast/Filter/ImageSize.php';
    $filter = new Polycast_Filter_ImageSize();
    $filter->getConfig()
        ->setHeight(100)
        ->setWidth(200);
    $outputPath = $filter->filter('./orig.jpg');

    header('Content-Type: image/jpeg');
    $fh = fopen($outputPath, 'r');
    fpassthru($fh);
    fclose($fh);
    ?>


## Complex example

* Crop the image instead of fit it into the bounding box.  
* Output as JPEG (actually this is the same as above as it is the default)  
* Set a specific output directory `(default = ./)` 
* Use caching, i.e. override only if source image is newer than thumbnail (if exists)  

[markdown syntax is stupid]

    <?php
    require_once 'Polycast/Filter/ImageSize.php';
    require_once 'Polycast/Filter/ImageSize/Strategy/Crop.php';
    require_once 'Polycast/Filter/ImageSize/PathBuilder/Standard.php';
    
    $filter = new Polycast_Filter_ImageSize();
    
    $filter->setOutputPathBuilder(
            new Polycast_Filter_ImageSize_PathBuilder_Standard('images/thumbnails'));

    $filter->getConfig()
           ->setHeight(100)
           ->setWidth(200)
           ->setQuality(75)
           ->setOverwriteMode(Polycast_Filter_ImageSize::OVERWRITE_ALL)
           ->setOutputImageType(Polycast_Filter_ImageSize::TYPE_JPEG)
           ->setStrategy(new Polycast_Filter_ImageSize_Strategy_Crop());

    $output = $filter->filter('./orig.jpg');

    header('Content-Type: image/jpeg');
    $fh = fopen($output, 'r');
    fpassthru($fh);
    fclose($fh);
    ?>

 
## More examples

Have a look at the `tests/ExamplesTest.php` which demonstrates how to implement
own path builders and configurations. 


## Installation

### composer

If you want to use this library in your project, adjust your `composer.json` 
like this:

    "require": {
        "polycast/polycast-filter-imagesize": "master-dev"
    },

If you want to work on this library use the following command, which will 
checkout the project from github and install all dependencies needed for 
development.

`composer create-project --dev polycast/polycast-filter-imagesize polycast-filter-imagesize`
