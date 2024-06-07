
**\# Algoritmo 1: Compresión sin pérdida (PNG)**

**Implementación:** Utilizaremos la librería PIL (Pillow) para abrir una
imagen y guardarla en formato PNG, que utiliza compresión sin pérdida.



    import os
    from PIL import Image
    import requests
    from io import BytesIO
    
    def comprimirPng(direccion, salida):
        imagen = Image.open(direccion)
        imagen.save(salida, format='PNG', optimize=True)
    
    url = 'https://upload.wikimedia.org/wikipedia/commons/4/47/PNG_transparency_demonstration_1.png'
    respuesta = requests.get(url)
    img = Image.open(BytesIO(respuesta.content))
    
    #Comprimimos
    img.save('original.png')
    comprimirPng('original.png', 'comprimida.png')
    
    #Tamaños
    tam_original = os.path.getsize('original.png')
    tam_comprime = os.path.getsize('comprimida.png')
    
    print(f"Tamaño original: {tam_original} bytes")
    print(f"Tamaño comprimido: {tam_comprime} bytes")
    print(f"Reducción de tamaño: {tam_original - tam_comprime} bytes ({((tam_original - tam_comprime) / tam_original) * 100:.2f}% de reducción)")

```
Tamaño original: 243134 bytes
Tamaño comprimido: 239255 bytes
Reducción de tamaño: 3879 bytes (1.60% de reducción)
```

**Explicación:** La compresión sin pérdida reduce el tamaño del archivo
sin eliminar ninguna información de la imagen. Un formato común que
utiliza compresión sin pérdida es PNG.

**Ejemplo de Implementación:** En nuestro caso, tenemos una imagen, de
243134 bytes. Queremos reducir su tamaño utilizando compresión PNG.

Abrimos la imagen y la guardamos en formato PNG utilizando la librería
PIL (Pillow). Al guardar la imagen, especificamos que queremos
optimizarla para reducir su tamaño. El tamaño comprimido resulta ser
239255 KB. Lo que se traduce en que hemos reducido el tamaño del archivo
sin perder ninguna calidad.

**# Algoritmo 2: Compresión con pérdida (JPG)**

**Implementación:** Utilizaremos la librería PIL (Pillow) para abrir una
imagen *y* guardarla en formato JPG, que utiliza compresión con pérdida.
Eliminando parte de la información, lo que puede resultar en una
disminución de la calidad visual.

    python
    import os
    from PIL import Image
    
    def comprimeJpg(direccion, salida, quality=55):
        imagen = Image.open(direccion)
        imagen.save(salida, format='JPEG', quality=quality)
    
    ruta_imagen = 'prueba_jpg.jpg'
    imagen_comprimida = 'imagen_comprimida.jpg'
    
    #Comprime
    comprimeJpg(ruta_imagen, imagen_comprimida, quality=55)
    
    #Tamaños
    tam_original = os.path.getsize(ruta_imagen)
    tam_comprime = os.path.getsize(imagen_comprimida)
    
    #Imprime
    print(f"Tamaño original: {tam_original} bytes")
    print(f"Tamaño comprimido: {tam_comprime} bytes")
    print(f"Reducción de tamaño: {tam_original - tam_comprime} bytes ({((tam_original - tam_comprime) / tam_original) * 100:.2f}% de reducción)")

```
    Tamaño original: 27534 bytes
    Tamaño comprimido: 19463 bytes
    Reducción de tamaño: 8071 bytes (29.31% de reducción)
```

**Explicación:** La compresión JPEG es un método de compresión con
pérdida que reduce el tamaño del archivo de una imagen al eliminar parte
de la información de la misma. Este método es ampliamente utilizado para
comprimir imágenes fotográficas y gráficos con transiciones suaves de
color, ya que permite un significativo ahorro de espacio de
almacenamiento sin una pérdida aparente de calidad visual.

**Ejemplo de Implementación:** Tenemos una imagen llamada
\"prueba_jpg.jpg\" con un tamaño original de 27535 bytes. Queremos
reducir su tamaño utilizando compresión JPEG.

Utilizamos la función comprimeJpg para abrir la imagen y guardarla en
formato JPEG con un nivel de calidad de compresión del 55%. Después de
la compresión, el tamaño del archivo comprimido es de 19463 KB.

**\# Algoritmo 3: Conversión ASCII**

**Implementación:** Utilizaremos la librería PIL (Pillow) para abrir una
imagen y, de acuerdo a su tamaño, reescalarla usando la escala de grises
para usar ASCII.

``` python
from PIL import Image as I

caracteresASCII = "@%#*+=%:. "

def aEscalaGrises(imagen):
    return imagen.convert('L')

def redimensionarImagen(imagen, nuevoAncho=100):
    ancho, alto = imagen.size
    proporcion = alto / ancho
    nuevoAlto = int(nuevoAncho * proporcion)
    return imagen.resize((nuevoAncho, nuevoAlto))

def pixelsAASCII(imagen):
    pixeles = imagen.getdata()
    asciiStr = ""
    for pixel in pixeles:
        asciiStr += caracteresASCII[pixel // 32]
    return asciiStr

def imagenAASCII(rutaImagen, nuevoAncho=100):
    imagen = I.open(rutaImagen)
    imagen = redimensionarImagen(imagen, nuevoAncho)
    imagen = aEscalaGrises(imagen)
    asciiStr = pixelsAASCII(imagen)
    anchoImagen = imagen.width
    asciiImg = ""
    for i in range(0, len(asciiStr), anchoImagen):
        asciiImg += asciiStr[i:i+anchoImagen] + "\n"
    return asciiImg

#Imagen a convertir
rutaImagen = "prueba_jpg.jpg"

#Convertir
asciiImagen = imagenAASCII(rutaImagen)
print(asciiImagen)
```

    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::%%======%:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::%==++++****+=::::::::::::::::::::::::::::::%%%%:::::::::::::::::::::::::::::
    :::::::::::::::::::::::==%%++********+%:::::::::::::::::::::::::%=+++++=%%::::::::::::::::::::::::::
    :::::::::::::::::::::%+*==+##**********%:::::::::::::::::::::::=++++++=%==%:::::::::::::::::::::::::
    ::::::::::::::::::::=****######********+%::::::::::::::::::::%+++=++++=:%=+=%:::::::::::::::::::::::
    :::::::::::::::::::+******##*###*****+**+:::::::::::::::::::%+++=:=++++=%=+==%::::::::::::::::::::::
    :::::::::::::::::%+**********#******=%=**%:::::::::::::::::=++++::%+++++++++==%:::::::::::::::::::::
    ::::::::::::::::%+*******+****+*****%::**+::::::::::::::::=+++++%%=+++++++++=+=:::::::::::::::::::::
    ::::::::::::::::+*******%:=*+++*****%:%+**%::::::::::::::=+++++++++++++++++==++%::::::::::::::::::::
    :::::::::::::::=*******+%=*++*******+=+++++:::::::::::::%++++++++++++++++++=:=+=%:::::::::::::::::::
    :::::::::::::::************++***********++*%::::::::::::===+++*+++++++++=+++%%++=:::::::::::::::::::
    ::::::::::::::%##***##**##*+***************+:::::::::::%+%:=**+==+++++++==+++=+++%::::::::::::::::::
    ::::::::::::::%++++++**%%#*+****************%::::::::::=+::=++%:%+++==++%%+++++++=::::::::::::::::::
    ::::::::::::::%++++++++*%%*********=%+******%::::::::::=+==+++%:%+++::=+%:=++++=%=%:::::::::::::::::
    :::::::::::::::+=%+==+==+#*******+=::=**###*=::::::::::%+**+++=%=++=::=+%%=++++=:%%:::::::::::::::::
    :::::::::::::::==%==%==%=++******+=::=**###*=::::::::::%++++++++++++%=++===+++++=%=:::::::::::::::::
    :::::::::::::::%+==+==+=%=++******+=%*******=:::::::::::=+++++++++++++++==+++*+++=+%::::::::::::::::
    ::::::::::::::::++++++++===+*************#**%:::::::::::=++++++++++++++++++++++++++%::::::::::::::::
    ::::::::::::::::%++++++++==+***********#%#*+:::::%%====%=+++=%=++++++++++++++++++*+%::::::::::::::::
    :::::::::::::::::=++++=++===+*********+*#**=:%===%%+*+++++++=:%+++++++++++++++++++=:::::::::::::::::
    :::::::::::::::::%++++=++====*****==*+***++=+**=%%+**+++=++*+%%+++++++++++++++++++%:::::::::::::::::
    ::::::::::::::::::=++++++====+***=::+***++++**++******%:%+*+**+++++++++++++++++++=::::::::::::::::::
    ::::::::::::::::::%==%=+======***%::+**++++++=%:%*****+++++****+++++++++==++++++=:::::::::::::::::::
    :::::::::::::::::::%=%==%==%==+**+%%++%%=++++=%=+******+++++++**+++++++=%=+++++=::::::::::::::::::::
    ::::::::::::::::::::=====%==%=+***+++%%=+++++++***##*++++**++++*++++++*+++++++%:::::::::::::::::::::
    :::::::::::::::::::::==========****++++++++%::=**+++++++++*++++++++++**+++++=%::::::::::::::::::::::
    ::::::::::::::::::::::%========**##*+++***+=+**+==++++++++*=%=+++++++++++++=::::::::::::::::::::::::
    ::::::::::::::::::::::::%%====+****++*#*+##**++=+++++++++++:::+++++++++++=%:::::::::::::::::::::::::
    :::::::::::::::::::::::::::::%%%=*++++*++##*++=+++++++==+*=:::++++%%===%::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::=*++++++*%**+++++++++=::%*=::%++++::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::=*+++++++***+++++=+++:::%++==+++++%:::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::%*++++++++***+++%::++%::%*+++++++*=:::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::%*++++++%+***++=:::=*+%=+++++++++++:::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::+++++++==+**++=:::=++*++++++++++++:::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::+++++++==++*+++=:=+++++++++++++++*%::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::=+++++++=++*+++++*+++++++++++++++*=::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::%++++++++++*+++++++++++++++*++++++=::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::++++==++++++++**************+++++=::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::++++==+++++******########***=%+*++::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::=++++==++++****#**##***#***=::%*#+::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::%++++=+++++*******#****==*#%::%#*=::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::::++=+++++*++**#**###**=::=*=::=*+%::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::::=+%++++++*+**###*+*#*:::%**++*++:::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::::%+==++++++**###*%:%*#%::=*#*#*+%:::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::===+++++++***#+:::+*+%=+**##*=::::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::::::+=++++++++**#+:::++++++++**%:::::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::::::%++++++++*****=%=+++*++++=:::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::=+++++==+*****++++++++=%::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::%++=+++++*##*++++++++=%:::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::=====++++**##*++++=%===%::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::%=====+=++****++====%==%::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::%=====++++++====+===:==%::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::%=============%=====%%=%::::::::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::::::::%===========%%%=====%%%::::::::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::::::::%==%%%==============%%:::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::==%%================%:::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::%=======%%%========+=:::::::::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::::::::::====++==%=========+%:::::::::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::::::::::%===+========%%%==%::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::::%===%%==========%:::::::::::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::::::::::::%=%%%========%:::::::::::::::::::::::::::::::::::::::::::
    :::::::::::::::::::::::::::::::::::::::::::::%%%==%%%%::::::::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

**Explicación:** El código proporcionado realiza la conversión de una
imagen en formato JPG a una representación en formato ASCII. Utiliza la
biblioteca PIL para cargar la imagen, redimensionarla y convertirla a
escala de grises. Posteriormente, asigna caracteres ASCII a los niveles
de intensidad de los píxeles de la imagen, creando así una
representación ASCII de la misma. La función principal, imagenAASCII,
encapsula este proceso, tomando la ruta de la imagen como entrada y
devolviendo la imagen convertida en ASCII como una cadena de texto.

**aEscalaGrises** convierte la imagen a escala de grises,
**redimensionarImagen** ajusta el tamaño de la imagen manteniendo su
relación de aspecto, y **pixelsAASCII** asigna caracteres ASCII a los
niveles de intensidad de los píxeles.

Finalmente, la imagen convertida se imprime en la consola.
