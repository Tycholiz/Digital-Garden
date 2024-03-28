
using packages like `react-native-size-matters` or `react-native-responsive-screen` is essential, because app responsive design does not work the same as web. You need to scale based on pixel density. Do not just style just based on screen width/height. Basically replace all pixel values (the default) with scaled versions of those values using functions from the mentioned packages. Then of course for some things you can also toggle different styles based on screen width/height for some things when necessary.