Ayudantia 5 Clusters
================

# Actividad Ayudantia 5

Realizar análisis de clustering (K-means, incluye preprocesamiento de la
data) e índices de evaluación para el archivo “sandwiches.csv” tomando
las columnas de nota y precio. Hacer análisis para diferentes K y/o
medidas de distancia para que vean cómo se comporta el clustering (En
caso de tener algún problema con ese csv, pueden utilizar el csv de
Pokémon también para la actividad)

# Algoritmo de clustering base:

## K-Medias

Para el análisis de clusters vamos a analizar la data de “sanguchez.csv”
que contiene la información de los pokemones de 7 de sus generaciones,
echaremos un vistazo a las variables presentes.

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.0.5

    ## -- Attaching packages --------------------------------------------------------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.3.3     v purrr   0.3.4
    ## v tibble  3.0.3     v dplyr   1.0.5
    ## v tidyr   1.1.3     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.5.0

    ## Warning: package 'ggplot2' was built under R version 4.0.4

    ## Warning: package 'tidyr' was built under R version 4.0.4

    ## Warning: package 'dplyr' was built under R version 4.0.4

    ## -- Conflicts ------------------------------------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
data <- read.csv(file.choose(), sep = ";", header= TRUE )

head(data)
```

    ##                                                               url
    ## 1          https://365sanguchez.com/abocado-cantina-buenos-aires/
    ## 2                   https://365sanguchez.com/alba-hotel-matanzas/
    ## 3   https://365sanguchez.com/albedrio-restaurant-santiago-centro/
    ## 4 https://365sanguchez.com/albedrio-restaurant-santiago-centro-2/
    ## 5              https://365sanguchez.com/aldea-nativa-providencia/
    ## 6            https://365sanguchez.com/aleman-experto-providencia/
    ##                 Local                                         Direccion  Precio
    ## 1     Abocado Cantina   C1125AAE, French 2316, C1125AAF CABA, Argentina $5.210.
    ## 2          Alba Hotel   Carlos Ibañez del Campo s/n – Matanzas, Navidad  $7.000
    ## 3 Albedrio Restaurant     Huérfanos 640, Santiago, Región Metropolitana  $7.290
    ## 4 Albedrío Restaurant Pasaje Huerfanos 640 edificio B local 5, Santiago  $8.690
    ## 5        Aldea Nativa  Tobalaba 1799, Providencia, Región Metropolitana  $4.900
    ## 6      Alemán Experto Av. Pedro de Valdivia 1683, Providencia, Santiago  $6.500
    ##                                                                                                                  Ingredientes
    ## 1                                               Suprema de pollo dulce, espinaca, crema ácida, repollo avinagrado y guacamole
    ## 2                     Carne mechada en reducción de vino tinto, champiñones salteados, cebolla caramelizada y queso derretido
    ## 3                          Mayonesa al olivo, champiñones salteados, jalapeños, queso Mozzarella, papas hilo y cebolla morada
    ## 4                          Queso Mozzarella, Rúcula, Champiñon portobello relleno de cheddar y luego apanado en panko y frito
    ## 5 Tofu asado no transgénico, palta, tomate, champiñones, mix de hojas verdes orgánicas,  mayonesa de zanahoria vegana casera,
    ## 6                           Hamburguesa, queso Cheddar, cebolla caramelizada, berros, pepinillos y salsa Jack Daniel’s Honey.
    ##   nota
    ## 1    3
    ## 2    3
    ## 3    4
    ## 4    4
    ## 5    4
    ## 6    3
    ##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               texto
    ## 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  Ojo acá! En la sanguchería “Abocado” (@AbocadoCantina) de Recoleta, más que un sándwich exquisito (que igual estaba bueno), descubrí una maravilla para copiar: acá el apanado, el frito del pollo, era dulce. Y bien crocante. Exquisito. Les juro que es el mejor apanado que he probado en mi vida. A esta suprema de pollo dulce, la acompañaban con espinaca (yo la hubiese puesto a la crema), crema ácida, repollo avinagrado y guacamole. Lamentablemente, la palta acá en Argentina no es como la chilena. Es más aguachenta. Y el pan, nuevamente sigue la línea que me ha tocado en este país, que no logra ser del nivel que tenemos en Chile. Pero insisto: ese batido hay que exportarlo. Estaba exquisito. Y sigo pensando en él.
    ## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           Aprovechando que me escapé a Matanzas con @catabarra_ a canjear mi regalo de cumpleaños (clases de surf), quise probar algunos sanguchitos de la zona. Y como hace un año me quedé a alojar en @albahotelboutique y tuve una muy buena experiencia, hoy quise darle una oportunidad a su carta de comida. Y a pesar de que en general nos fue bastante mal (3 de los platos andaban muuuy bajos), mi sanguchito salvó muy bien. Y es que la mezcla de carne mechada en reducción de vino tinto, champiñones salteados, cebolla caramelizada en marraqueta (y le sumé queso derretido), es demasiado buena. No falla. Así que de 1 a 5, este se lleva 3 narices de chancho. Es decir, es un buen sándwich. Vaya a probarlo con confianza. Una marrquetita crujiente y de poca miga, una mechada muy suave y harto queso son sus puntas de lanzas. Sí, hay cosas por mejorar. Por ejemplo, las “mechas” de la carne como que se pegaban unas a otras, entonces a veces de un mordisco te llevabas casi toda la carne. O el caldo lo haría más intenso. Porque lo que chorreaba aquí eran los champiñones más que la carne. Pero apaña harto, además que estás comiendo EN la playa misma.
    ## 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Sin duda, uno de los lugares que me ENCANTA visitar. Lejos la mejor hamburguesa que tienen es la Portobello (con un champiñón frito relleno de Cheddar y todo), pero como no estamos en temporada de hongos, no había ahora. Esa, sin duda, se lleva cinco narices. Hoy vine a @RestoAlbedrio con@MaxKbzon y nos dimos la torta comiendo. Él fue por un sándwich de prieta con manzana verde, papas hilo y mayo de ají verde. Yo, una burger “Picante”, con mayonesa al olivo, champiñones salteados, jalapeños, queso Mozzarella, papas hilo y cebolla morada. Solo les adelanto una cosa: tienen una salsa de reducción de cerveza con jugo de piña y azúcar rubia, que debiesen venderla en bidones!! Es EXQUISITA!
    ## 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    Con @nitanzorron fuimos a probar esta maravilla de @albedrio_resto. Anoten: hamburguesa casera, queso mozzarella, rúcula y champiñon portobello relleno de cheddar y luego apanado en panko y frito . Una maravilla! Es que los champiñones rellenos ya son atómicos… Pero ahora que vienen fritos, tienes un sabor estratosférico. La mejor idea del mundo. Es una verdura muy “carnosa” y rica, realzada por el queso y el apanado. El toque amargo de la rúcula viene bien, y la hamburguesa en sí, creo que es la más jugosa que he probado. Me recordó a la de Ciudad Vieja. Anda perfecta. El pan Brioche, bien dulce, y de miga consistente. No tan aireada. Mi único punto a mejorar es que sentí que era muy “aguado” (los champiñones tienen alto porcentaje de agua), por lo que me faltó malicia. Un picante, o una salsa de ajo… No sé. Algo que te vuele la cabeza. De hecho, Albedrío tiene dos salsas que creo que pondrían a esta hamburguesa en el top chileno: la de la casa, que es una reducción de cerveza, pulpa de piña y azúcar rubia, y una mayonesa con cilantro y ajo que es perfecta. Con @nitanzorron conversamos que agregando esa salsa, el sandwich sube de nivel a SS3. Muy buena. Vayan a ver nuestra visita a mi canal de YouTube (link en mi perfil) para todos los detalles y comenten si les tinca porque encuentro que es mega creativa y muuuuy rica.
    ## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  Ojo los vegetarianos!! Porque gracias a@genoveva_tenaillon (síganla si quieren ver unas recetas exquisitas pero saludables al mismo tiempo) que me pasó el dato, encontré el templo de los sándwiches vegetarianos y jugos naturales wenos wenos wenos. Es Aldea Nativa, en Tobalaba, y a pesar de que es 99% más probable que prefiera un sándwich carnívoro, creo que los que probé acá son de los mejorcitos que me han tocado (hasta ahora, en La Tegualda están los mejores). El Barros Luco de la Geno estaba bien bueno (con carne de libre pastoreo, sin hormonas ni antibióticos… Y no, claramente este no era veggie jaja), pero me gustó más el mío: tofu asado no transgénico, palta, tomate, champiñones, mix de hojas verdes orgánicas, y le sumé una mayonesa de zanahoria vegana casera, que viene con todos los sándwiches (échensela entera). A ver. Era rico, pero la nota se la lleva principalmente porque es el mejor tofu que he probado en Chile. En general lo cocinan muy fome, pero este estaba marinado en soya y asado a la plancha, así que tenía un gustito distinto al típico “quesillo sin sabor” jajaj. Además, venía como con un cevichito de champiñones que también se lo puse adentro  y agarró una jugosidad que el pan agradeció. Con estos dos ingredientes que le puse, las verduras agarraron un aliño exquisito. De los vegetarianos que he probado, es de los más ricos. Pero si no te gusta el Tofu, también puedes probar alguna de las hamburguesas vegetarianas que tienen. Me gustó harto el lugar, además de que también es un mercado donde venden miles de productos orgánicos, vegetarianos y de esa onda.
    ## 6 Salsa de bourbon: checkAlemán ExpertoCómo llegué a Alemán ExpertoYa había venido un par de veces al Alemán Experto. Tanto al local de Santa Magdalena, como a este de Pedro de Valdivia. En todas las visitas tuve suerte dispar. En algunos me gustó harto, otras no tanto.La cosa es que hoy tuve que hacer trámites cerca, y como tenía poco tiempo para buscar una sanguchería, preferí ir al Alemán Experto, que aún no lo sumaba a 365 Sánguchez.Fotos tomadas con mi celular Sony Xperia XRestaurante y sanguchería Alemán ExpertoAlemán Experto es una sanguchería que cuenta con dos locales. El primero está en Santa Magdalena, y el otro en Pedro de Valdivia, en la esquina con Francisco Bilbao. Ojo, que también están abriendo uno en La Dehesa.Este restaurante es, tal como lo dice su nombre, bien alemán. Es decir, abundan los sánguches grandes y la cerveza.Hablando del local de Pedro de Valdivia, siento que hicieron un gran trabajo con su fachada exterior. Si no me creen, miren la foto de más arriba. Y es que la terraza que sacaron está increíble. Además, por su ubicación, siempre hay gente, por lo que me tinca que se arma buen ambiente para los after office.Les dejo su pagina web. Carta de sándwiches Alemán ExpertoLa carta de sándwiches del Alemán Experto es amplia, tanto en sus bases, como también en sus combinaciones gourmet y clásicas.Por el lado más jugado, la sanguchería Alemán Experto cuenta con hamburguesas y mechadas BBQ. De las primeras, destacan una que tiene camarones y queso azul ($6.400), y la que pedí yo. Se llama Jack Daniel’s Honey, y tiene una salsa basada en ese licor, además de queso Cheddar, berros, cebolla caramelizada y pepinillos.En las mechadas BBQ, hay dos opciones. una con tocino crispy, y la otra con queso Azul y aros de cebolla.Luego de esta sección más “gourmet”, Alemán Experto también cuenta con hamburguesas, churrascos, lomitos, aves, salchichas y mechadas para poder armarlas como italianos, lucos y chacareros.Para terminar, hay una sección de sándwiches vegetarianos. Son hamburguesas de quinoa, y tiene cuatro combinaciones distintas. Hamburguesa Jack Daniel’s Honey en Alemán ExpertoA pesar de no ser un fanático del bourbon, admito que sí me gusta esta variante con toques de miel. Y en una salsa, mejor aún.Tengo que decir que es un sándwich correcto. La salsa no se roba el protagonismo, y aporta un toque justo de dulzor y también de “malicia”.La cebolla caramelizada estaba suave, y los berros perfectos para contrastar el frescor con lo dulce de la cebolla y la salsa.Lo que no me gustó tanto, es que la hamburguesa estaba un poco seca. Tuvo suerte, eso sí, de que venía acompañada con harta salsa, por lo que lograba pasar piola. Pero si nos quedamos en la carne, le falta.Y el otro punto negativo, y esto ya parece que es una maldición que me persigue, fue el queso Cheddar. Primero, porque no estaba derretido. Cueck. Y segundo, porque su sabor era demasiado plástico. Les prometo que tengo ganas de hacer una cata de quesos Cheddar, quizás con Daniel Greve, para poderles recomendar cuáles son buenos. Pero este, no.Maridaje con Cerveza Austral LagerEn resumen: Alemán Experto puede ser experto en otras cosas, pero no en hamburguesasRecién ahora, que estoy escribiendo estas líneas, me doy cuenta que quizás hice una movida tonta. Si voy a un lugar que se llama Alemán Experto, lo normal sería haber pedido un lomito. Con chucrut, con pepinillos… algo por ahí.Se supone que los alemanes también le pegan a las fricandelas, pero este no fue el caso. De hecho, la carne no era tan especiada como suele serlo en ese país. Pero aún así, me tinca que el lomito aquí puede ser un gran acierto.Quedé con ganas de volver. Volver y probar otra proteína, como el lomito o la mechada. Así que nos vemos pronto, Alemán Experto.

``` r
summary(data)
```

    ##      url               Local            Direccion            Precio         
    ##  Length:410         Length:410         Length:410         Length:410        
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##  Ingredientes            nota          texto          
    ##  Length:410         Min.   :1.000   Length:410        
    ##  Class :character   1st Qu.:3.000   Class :character  
    ##  Mode  :character   Median :3.000   Mode  :character  
    ##                     Mean   :3.167                     
    ##                     3rd Qu.:4.000                     
    ##                     Max.   :5.000                     
    ##                     NA's   :8

Para clusterizar vamos a seleccionar las variables de nota,precio. Para
analizar el comportamiento vamos a excluir url,direccion, texto,
ingredientes y local.

## funcion que pasa las notas a valor numerico

``` r
data$Precio <- as.numeric(gsub('[$.aprox]', '', data$Precio))
```

    ## Warning: NAs introducidos por coerción

``` r
#Se borran los datos nulos
data <- data[,!(colnames(data) %in% c("url", "Direccion", "texto", "Ingredientes", "Local"))] 
data <- data[!is.na(data$Precio),]
data <- data[!is.na(data$nota),]

escala_data = scale(data) %>% as_tibble()

escala_data %>% summary()
```

    ##      Precio             nota        
    ##  Min.   :-2.8637   Min.   :-1.9432  
    ##  1st Qu.:-0.6215   1st Qu.:-0.1379  
    ##  Median :-0.0466   Median :-0.1379  
    ##  Mean   : 0.0000   Mean   : 0.0000  
    ##  3rd Qu.: 0.5466   3rd Qu.: 0.7648  
    ##  Max.   : 4.4534   Max.   : 1.6674

``` r
escala_data$Precio %>% as.numeric()
```

    ##   [1] -0.454264526  0.481267787  0.632834474  1.364535724 -0.616284089
    ##   [6]  0.219945911 -0.302697839  1.003911537  1.474290912 -0.093640339
    ##  [11]  1.474290912  0.136322911 -0.302697839 -0.830568026 -0.830568026
    ##  [16]  1.474290912  0.376739037  0.742589662 -0.093640339  0.376739037
    ##  [21]  0.951647162 -0.145904714  1.526555287  0.429003412 -0.093640339
    ##  [26] -0.145904714 -0.825341589  0.089284974  1.003911537 -0.773077214
    ##  [31] -0.250433464  0.742589662  0.219945911  0.794854037 -0.145904714
    ##  [36]  0.115417161  0.742589662  0.219945911  0.193813724 -0.093640339
    ##  [41] -0.511755339 -1.347985339  0.429003412  0.167681536  0.376739037
    ##  [46] -1.165060026 -0.302697839  0.272210286 -0.198169089 -0.569246151
    ##  [51]  0.664193099  1.474290912  0.141549349  0.010888411 -0.668548464
    ##  [56]  1.108440287 -0.093640339  0.476041349  0.376739037  0.429003412
    ##  [61] -0.093640339  3.564865912  2.415049662  0.418550537 -0.302697839
    ##  [66]  0.899382787 -1.086663464 -0.302697839  0.951647162  0.010888411
    ##  [71] -0.302697839  0.376739037 -1.875855526 -0.882832401  0.794854037
    ##  [76] -0.302697839 -0.982134714 -1.347985339  4.401095912 -1.818364714
    ##  [81] -0.041375964 -0.145904714  1.265233412 -1.086663464 -0.145904714
    ##  [86]  1.239101224  0.789627599  0.794854037 -0.328830026  0.429003412
    ##  [91] -0.145904714  1.369762162  1.631084037 -0.825341589  0.476041349
    ##  [96] -0.830568026 -1.661571589  0.951647162 -2.314876276  0.115417161
    ## [101] -0.564019714 -0.616284089 -1.457740526 -0.145904714  2.310520912
    ## [106]  0.219945911 -0.929870339  0.533532162 -0.590151901  0.742589662
    ## [111]  0.585796537 -1.191192214 -0.198169089 -0.255659901  0.272210286
    ## [116]  0.429003412 -0.616284089 -0.093640339 -0.668548464 -0.642416276
    ## [121] -0.982134714  1.474290912 -1.086663464 -0.825341589 -0.773077214
    ## [126] -1.217324401  0.010888411  0.742589662  0.951647162 -1.347985339
    ## [131]  0.998685099 -0.825341589 -1.243456589 -0.145904714  0.376739037
    ## [136] -1.975157839 -1.086663464 -0.564019714  0.690325287  0.219945911
    ## [141] -0.354962214 -2.262611901 -0.564019714 -0.354962214 -1.609307214
    ## [146]  0.533532162  1.003911537 -0.825341589  1.317497787 -0.616284089
    ## [151] -0.093640339  0.219945911 -1.400249714 -0.093640339  1.991708224
    ## [156]  0.977779349 -1.138927839 -0.046602401 -1.347985339 -0.046602401
    ## [161] -0.307924276  0.219945911  1.212969037  1.474290912  0.585796537
    ## [166]  0.585796537  0.429003412 -0.668548464 -0.616284089  0.951647162
    ## [171] -0.590151901 -1.347985339  0.617155162 -0.203395526 -0.198169089
    ## [176] -0.093640339 -0.564019714  0.115417161 -2.863652214 -0.077961026
    ## [181] -0.616284089  0.324474662 -0.302697839 -0.093640339  1.265233412
    ## [186] -1.295720964  4.453360287 -0.041375964  1.212969037  0.272210286
    ## [191]  0.429003412  0.476041349 -0.825341589  0.742589662 -0.621510526
    ## [196]  0.167681536  0.585796537  0.481267787 -0.459490964 -0.145904714
    ## [201]  0.214719474  0.899382787  1.265233412 -0.825341589  0.115417161
    ## [206] -0.354962214 -0.825341589  1.416800099  1.056175912 -0.569246151
    ## [211] -1.086663464 -1.609307214 -2.131950964  2.467314037 -0.302697839
    ## [216] -0.825341589  0.951647162  0.533532162  0.998685099  0.005661974
    ## [221] -0.046602401  0.429003412 -0.929870339  0.429003412  0.063152786
    ## [226] -1.091889901 -0.459490964 -0.041375964  0.951647162  0.951647162
    ## [231] -0.145904714  0.219945911  0.429003412 -1.086663464 -0.861926651
    ## [236] -0.145904714  0.063152786  0.219945911  0.476041349 -0.145904714
    ## [241]  0.585796537 -1.191192214  0.167681536  0.899382787  1.474290912
    ## [246]  0.376739037 -0.511755339 -1.086663464  0.219945911  1.474290912
    ## [251] -0.145904714  1.521328849 -0.564019714 -1.347985339 -1.295720964
    ## [256]  0.742589662 -1.034399089  0.946420724 -0.041375964  1.996934662
    ## [261] -1.086663464 -0.041375964 -0.511755339  0.429003412  0.690325287
    ## [266] -0.354962214  0.167681536  0.742589662 -0.198169089 -1.400249714
    ## [271] -1.400249714  1.526555287 -1.086663464 -0.302697839  0.167681536
    ## [276]  0.476041349 -2.288744089 -0.093640339  0.089284974 -0.250433464
    ## [281] -2.367140651  1.108440287 -1.943799214 -1.008266901  0.005661974
    ## [286]  0.010888411 -1.347985339  0.429003412 -0.564019714  1.317497787
    ## [291]  1.526555287 -0.825341589 -0.825341589 -0.982134714 -0.982134714
    ## [296] -0.250433464  0.429003412  1.521328849  0.742589662  3.564865912
    ## [301] -0.720812839  0.977779349 -1.086663464 -0.511755339  0.115417161
    ## [306] -0.145904714 -0.564019714 -0.778303651  0.010888411 -0.151131151
    ## [311] -0.041375964  0.115417161  0.690325287 -1.713835964  0.429003412
    ## [316] -1.870629089 -0.255659901  0.324474662  0.429003412  0.455135599
    ## [321]  0.429003412 -0.302697839  1.474290912  0.533532162  0.429003412
    ## [326] -0.877605964  0.899382787 -0.046602401  1.056175912 -0.354962214
    ## [331] -0.093640339 -0.093640339  0.742589662 -1.091889901 -0.093640339
    ## [336]  0.376739037 -0.302697839 -0.825341589  0.951647162 -0.929870339
    ## [341] -1.870629089 -1.060531276  1.474290912  1.944670287 -1.766100339
    ## [346]  0.737363224 -0.145904714 -1.609307214  1.422026537  1.317497787
    ## [351]  0.376739037 -1.138927839 -0.825341589 -0.302697839 -0.982134714
    ## [356] -0.302697839 -0.621510526 -1.138927839  0.429003412 -0.616284089

``` r
escala_data$nota %>% as.integer()
```

    ##   [1]  0  0  0  0  0  0  0  0  0  0  0  0  0  0 -1  0  0  0  0  0  0  0  0  0 -1
    ##  [26] -1 -1  0  0  0  1  0  1  0  0  0  0  0  0 -1  0  0  0  0  0  0  1  0  0 -1
    ##  [51]  0  1  0  0 -1  0  0 -1  0  1  1  0  0  0  0 -1  1  0  0  0  1  0  0  0  0
    ##  [76]  1  1  0  0  0  0  0 -1  0  0  0  1  0  0  0  0  0  0  0  0 -1  1 -1 -1  0
    ## [101]  1  0 -1  0  0  0 -1  0  0  0 -1 -1  0  0  0  1  0 -1  0  0 -1 -1  0 -1  0
    ## [126] -1  0 -1 -1  0  0 -1  0 -1 -1  0  0  1  1  0  1  0  1  0 -1  0  0 -1  0  0
    ## [151]  0 -1 -1  1 -1  0 -1  0  0  1  0 -1  0 -1  1  0  1  0  0 -1  0  0 -1 -1  0
    ## [176]  1  0  0  0 -1  0  0 -1  0  1  0 -1  0  1  1  1 -1  0  0 -1 -1  0 -1  0  0
    ## [201]  0  1  0  0  0  1  0  0 -1 -1 -1  1  0  0  1  1  0  0  0 -1  1  0 -1  0  0
    ## [226]  0  0  0  0  1  0  0  0  0  0  0  0  1  0  1  0  0  0  0  0  0  0  0  0  0
    ## [251]  0 -1  0  0 -1  0 -1  1  0  0 -1  0 -1  0  0  1 -1 -1 -1  0  0 -1  0  0  0
    ## [276]  0 -1  0  0 -1  0  0  0  0  0 -1  0  1 -1 -1 -1  0  0  0 -1  1 -1 -1  0  0
    ## [301]  0 -1 -1 -1  0  0  0  0  0  0  0  1  1  0  0 -1  0  0 -1  0 -1 -1  0 -1  1
    ## [326] -1  0 -1  1 -1  0  0 -1  0  1 -1  0  0  1  0 -1 -1  0  0  0 -1 -1  0  0  0
    ## [351]  0  1  0  0  0  0  0  0  0 -1

Ya tenemos escalada la data, vamos a aplicar el algoritmo de kmedias,
que viene implementado en R base. Para probar, vamos a aplicar kmedias
con k = 10

## Analisis Cluster K = 10

``` r
modelo_kmeans <- kmeans(escala_data, centers = 10)
modelo_kmeans2 <- kmeans(data, centers = 10)

# creo la variable cluster en la tabla escala_data
escala_data$clus <- modelo_kmeans$cluster %>% as.factor()
data$clus <- modelo_kmeans2$cluster %>% as.factor()

ggplot(escala_data, aes(nota, Precio, color=clus)) +
  geom_point(alpha=0.5, show.legend = T) +
  theme_bw()
```

![](Ayudantia5_files/figure-gfm/clus%20k10-1.png)<!-- -->

``` r
ggplot(data, aes(nota ,Precio, color=clus)) +
  geom_point(alpha=0.5, show.legend = T) +
  theme_bw()
```

![](Ayudantia5_files/figure-gfm/clus%20k10-2.png)<!-- -->

``` r
info_clus <- modelo_kmeans$centers
info_clus2 <- modelo_kmeans2$centers

info_clus
```

    ##        Precio         nota
    ## 1  -0.1296013 -0.137907071
    ## 2  -1.3626449  0.038222581
    ## 3   1.1774843 -1.420640787
    ## 4   3.3110104 -0.008955005
    ## 5   0.1144056 -1.360871833
    ## 6  -0.5388167  0.945290289
    ## 7   0.2163621  1.667421862
    ## 8   0.7784281  0.842128636
    ## 9   0.9375939 -0.137907071
    ## 10 -1.0659070 -1.350056498

``` r
info_clus2
```

    ##       Precio     nota
    ## 1   6742.857 3.357143
    ## 2   2793.226 3.032258
    ## 3   5682.500 3.362500
    ## 4   7422.308 3.307692
    ## 5   6947.742 3.193548
    ## 6  13725.000 2.750000
    ## 7   9221.154 3.000000
    ## 8   4444.487 2.974359
    ## 9   6333.611 3.055556
    ## 10  8094.706 3.205882

## Evolución suma de cuadrados intra-cluster en la medida que aumentamos el numero de k

``` r
SSinterior <- numeric(30)

for(k in 1:30){
  modelo <- kmeans(escala_data, centers = k)
  SSinterior[k] <- modelo$tot.withinss
}

plot(SSinterior)
```

![](Ayudantia5_files/figure-gfm/evolucion%20sse-1.png)<!-- -->

## Metodo del Codo 2

``` r
#Calculando K para Data normalizada
k.max <- 30
wss1 <- sapply(1:k.max, 
               function(k){kmeans(escala_data, k, nstart=50,iter.max = 8)$tot.withinss})
wss2 <- sapply(1:k.max, 
               function(k){kmeans(data, k, nstart=50,iter.max = 8)$tot.withinss})

#wss1
plot(1:k.max, wss1,
     type="b", pch = 19, frame = FALSE, 
     xlab="Numeros de clusters K",
     ylab="Total within-clusters sum of squares")
```

![](Ayudantia5_files/figure-gfm/metodo%20codo2-1.png)<!-- -->

``` r
plot(1:k.max, wss2,
     type="b", pch = 19, frame = FALSE, 
     xlab="Numeros de clusters K",
     ylab="Total within-clusters sum of squares")
```

![](Ayudantia5_files/figure-gfm/metodo%20codo2-2.png)<!-- -->

# Evaluacion

Existen diversos metodos de evaluacion de calidad de los clusters
resultantes.

## Inspeccion visual

``` r
escala_data$clus <- as.numeric(escala_data$clus)
data$clus <- as.numeric(data$clus)

# uso distancia euclidiana
tempDist <- dist(escala_data) %>% as.matrix()

#reordeno filas y columnas en base al cluster obtenido
index <- sort(modelo_kmeans$cluster, index.return=TRUE)
tempDist <- tempDist[index$ix,index$ix]
rownames(tempDist) <- c(1:nrow(escala_data))
colnames(tempDist) <- c(1:nrow(escala_data))

image(tempDist)
```

![](Ayudantia5_files/figure-gfm/insp%20visual-1.png)<!-- -->

## Estadistico de Hopkins.

``` r
library(factoextra)
```

    ## Warning: package 'factoextra' was built under R version 4.0.5

    ## Welcome! Want to learn more? See two factoextra-related books at https://goo.gl/ve3WBa

``` r
#Calcula el hopkins statistic 
res <- get_clust_tendency(escala_data, n = 30, graph = FALSE)
res2 <- get_clust_tendency(data, n = 30, graph = FALSE)

print(res)
```

    ## $hopkins_stat
    ## [1] 0.9532171
    ## 
    ## $plot
    ## NULL

``` r
print(res2)
```

    ## $hopkins_stat
    ## [1] 0.9265951
    ## 
    ## $plot
    ## NULL

## Indice de correlación

``` r
#Correlation
#construyo matriz de correlacion ideal (cada entidad correlaciona 1 con su cluster)
tempMatrix <- matrix(0, nrow = nrow(data), ncol = nrow(data))
tempMatrix[which(index$x==1), which(index$x==1)]  <- 1
tempMatrix[which(index$x==2), which(index$x==2)]  <- 1
tempMatrix[which(index$x==3), which(index$x==3)]  <- 1
tempMatrix[which(index$x==4), which(index$x==4)]  <- 1
tempMatrix[which(index$x==5), which(index$x==5)]  <- 1
tempMatrix[which(index$x==6), which(index$x==6)]  <- 1
tempMatrix[which(index$x==7), which(index$x==7)]  <- 1
tempMatrix[which(index$x==8), which(index$x==8)]  <- 1
tempMatrix[which(index$x==9), which(index$x==9)]  <- 1
tempMatrix[which(index$x==10), which(index$x==10)] <- 1

#construyo matriz de disimilitud
tempDist2 <- 1/(1+tempDist)

#Calcula correlacion 
cor <- cor(tempMatrix[upper.tri(tempMatrix)],tempDist2[upper.tri(tempDist2)])

print(cor)
```

    ## [1] 0.8328818

## Indice de cohesión y el de separación.

``` r
library(flexclust) # usaremos la distancia implementada en flexclus (dist2) que maneja mejor objetos de diferente tamaño
```

    ## Warning: package 'flexclust' was built under R version 4.0.5

    ## Loading required package: grid

    ## Loading required package: lattice

    ## Loading required package: modeltools

    ## Warning: package 'modeltools' was built under R version 4.0.3

    ## Loading required package: stats4

``` r
#escala_data <- apply(escala_data,2,as.numeric)

#Cohesion
withinCluster <- numeric(10)
for (i in 1:10){
  tempdata <- escala_data[which(modelo_kmeans$cluster == i),]
  withinCluster[i] <- sum(dist2(tempdata,colMeans(tempdata))^2)
}
cohesion = sum(withinCluster)
#es equivalente a model$tot.withinss en k-means
print(c(cohesion, modelo_kmeans$tot.withinss))
```

    ## [1] 97.93896 97.93896

``` r
#Separation
meandata <- colMeans(escala_data)
SSB <- numeric(10)
for (i in 1:10){
  tempdata <- escala_data[which(modelo_kmeans$cluster==i),]
  SSB[i] <- nrow(tempdata)*sum((meandata-colMeans(tempdata))^2)
}
separation = sum(SSB)

print(separation)
```

    ## [1] 4185.392

## Coeficiente de silueta

``` r
library(cluster)

coefSil <- silhouette(modelo_kmeans$cluster,dist(escala_data))
summary(coefSil)
```

    ## Silhouette of 360 units in 10 clusters from silhouette.default(x = modelo_kmeans$cluster, dist = dist(escala_data)) :
    ##  Cluster sizes and average silhouette widths:
    ##        67        41        19         7        31        45        35        35 
    ## 0.7850563 0.5987266 0.6858101 0.4312510 0.7156135 0.5876154 0.6235116 0.6151371 
    ##        45        35 
    ## 0.6711044 0.7064095 
    ## Individual silhouette widths:
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##  0.2281  0.6269  0.6833  0.6669  0.7335  0.8488

``` r
#visualizamos el codigo de silueta de cada cluster
fviz_silhouette(coefSil) + coord_flip()
```

    ##    cluster size ave.sil.width
    ## 1        1   67          0.79
    ## 2        2   41          0.60
    ## 3        3   19          0.69
    ## 4        4    7          0.43
    ## 5        5   31          0.72
    ## 6        6   45          0.59
    ## 7        7   35          0.62
    ## 8        8   35          0.62
    ## 9        9   45          0.67
    ## 10      10   35          0.71

![](Ayudantia5_files/figure-gfm/coef%20silueta-1.png)<!-- -->

## Utilizamos el coeficiente de silueta para encontrar el mejor valor de K

``` r
coefSil=numeric(30)
for (k in 2:30){
  modelo <- kmeans(escala_data, centers = k)
  temp <- silhouette(modelo$cluster,dist(escala_data))
  coefSil[k] <- mean(temp[,3])
}
tempDF=data.frame(CS=coefSil,K=c(1:30))

ggplot(tempDF, aes(x=K, y=CS)) + 
  geom_line() +
  scale_x_continuous(breaks=c(1:30))
```

![](Ayudantia5_files/figure-gfm/valor%20k%20silueta-1.png)<!-- -->
