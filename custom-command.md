# Generating iOS icons. 

### Convert: Image resize

**convert** utility is installed as part of ImageMagick.

```
brew install imagemagick
```

### Create custom script to generate iOS icons and name the file as **generateiOSIcon.sh**

```bash

#!/bin/bash

echo $1
DIR=$(dirname $1)

#echo $DIR
#output="$DIR/$2"
#echo $output

#20x20
convert -resize 20x20! $1  "$DIR/Icon_20.png";
convert -resize 40x40! $1  "$DIR/Icon_20@2x.png";
convert -resize 60x60! $1  "$DIR/Icon_20@3x.png";


#29*29
convert -resize 29x29! $1  "$DIR/Icon_29.png";
convert -resize 58x58! $1  "$DIR/Icon_29@2x.png";
convert -resize 87x87! $1  "$DIR/Icon_29@3x.png";

#40x40
convert -resize 40x40! $1  "$DIR/Icon_40.png";
convert -resize 80x80! $1  "$DIR/Icon_40@2x.png";
convert -resize 120x120! $1  "$DIR/Icon_40@3x.png";

#60x60
convert -resize 60x60! $1  "$DIR/Icon_60.png";
convert -resize 120x120! $1  "$DIR/Icon_60@2x.png";
convert -resize 180x180! $1  "$DIR/Icon_60@3x.png";

#76x76
convert -resize 76x76! $1  "$DIR/Icon_76.png";
convert -resize 152x152! $1  "$DIR/Icon_76@2x.png";
convert -resize 228x228! $1  "$DIR/Icon_76@3x.png";

#167x167
convert -resize 167x167! $1  "$DIR/Icon_167.png";

```

### Make script executable
- > chmod =x generateiOSIcon.sh

- > ln -s /path/generateiOSIcon.sh /usr/local/bin/generateiOSIcon

### Generating icon for iOS

You will first need App icon of size 1024x1024 and run following command
> generateiOSIcon.sh /path/image.jpg

