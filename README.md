# Snapcode

![gif](/img/snap.gif)

Make your own custom snapcode

[Live demo](https://snapcodes.herokuapp.com/)

## How it works

1. Query the Snapchat API to get snapcode
2. Send snapcode to PHP script to add necessary tags
3. Select the colors using [jscolor picker](http://jscolor.com)
4. Render the SVG as a canvas
5. Convert the canvas to PNG
6. Download the PNG
7. showcase your cool snapcode :smile:

## API

I made a simple php script that inserts id tag in the svg to be able to modify it via css or using javascript.

    <?php
        header('Access-Control-Allow-Origin: *');
        $username = $_GET["username"];
        $size = $_GET["size"];
        if(strlen($username)>0){
                //snapchat api call
                $url = "https://feelinsonice-hrd.appspot.com/web/deeplink/snapcode?username=".$username."&type=SVG&size=".$size;
                $file = file_get_contents($url, false, $context);
                $pos =strpos($file,"svg");
                $file = substr($file, 0, $pos+3)." id=\"tag\"".substr($file, $pos+4);
                $pos =strpos($file,"path d");
                $file = substr($file, 0, $pos+4)." id=\"border\" ".substr($file, $pos+5);
                $pos =strpos($file,"path d");
                $file = substr($file, 0, $pos+4)." id=\"back\" ".substr($file, $pos+5);
                $pos =strpos($file,"path d");
                $file = substr($file, 0, $pos+4)." id=\"ghost\" ".substr($file, $pos+5);
                echo $file;
        }
    ?>
    
You can try my script at [https://lens.yougraph.fr/snapcode

## Modifications

I am using the PHP script because it inserts the necessary id and class tags to do DOM manipulations. Parts of the snapcode can be modified using the id below

| **Name** |   **id or class**  |
|:------------:|:-------:|
| **Snapcode** |   #tag  |
|   **Ghost**  |  .ghost |
|   **Code**   |  .back  |
|   **Frame**  | .border |

![guidelines](img/guidelines.png) 

To read about scan guidelines, you can read [Snapchat's Snapcode Scan Guidelines](https://github.com/jusleg/snaptag/blob/master/ress/Snapcode_Guidelines.pdf)

## Gif background (Beta)

You can put a gif instead of an image. You can fiddle around with the code on this [codepen](https://codepen.io/jusleg/pen/dXREyV)

## Limitations

The only limitation of this project at the moment is that Snapcodes cannot be downloaded from an iOS device. This is due to the canvas rendering used to convert from SVG to PNG. This is not a problem on Android devices or desktop. I will try to use a php script to convert the SVG to PNG if the user is on iOS.

## Credits

* [**Snapchat**](http://snapchat.com) - Snapcode API
* [**jscolor**](http://jscolor.com) - Javascript Color Picker
* [**StackOverflow**](http://stackoverflow.com) - Valuable help :smile:


