# CSS Notes

## 毛玻璃效果
```css
.blur { 
    filter: url(blur.svg#blur); /* FireFox, Chrome, Opera */
    
    -webkit-filter: blur(10px); /* Chrome, Opera */
       -moz-filter: blur(10px);
        -ms-filter: blur(10px);    
            filter: blur(10px);
    
    filter: progid:DXImageTransform.Microsoft.Blur(PixelRadius=10, MakeShadow=false); /* IE6~IE9 */
}
```