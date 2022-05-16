#------------------------------Task 2-------------------------------------------
##Laura Ayala, 
##Harold Beltrán, 
##Maria Camila Pinillos

rm(list=ls())
library(pacman)
library(xts)
library(readxl)
p_load(tidyverse , rio , data.table , png , grid)

list.files()

setwd("input/")
input=getwd()

#------------------------------TALLER A-----------------------------------------


#1. Crear lista e importar datos

años <- list.files()
length(años)
names(años) <- años
print(años)

#exploratorio
for (a in años){
  
  setwd(input)
  setwd(a)
  print(a)
  head(list.files())
  
  print(length(list.files()))
}


#lista
chip <- list()

#función
for (a in años){
  setwd(input)
  setwd(a)
  files = list.files() 
  
  for (j in files){
    df = read_excel(j)
    
    chip[[ length(chip)+1 ]]=df
    
  }
}

#(*) revisar que esté bien:
View(chip[[10]])
class(chip[[10]])

View(chip[[50]])

length(chip)


#2. Crear función para extraer pagos (en pesos) de cada excel (de cada municipio) para la categoria educación 

#Ver estructura de excel
View(chip[[10]])

#Formar función  
extraer_pagos_educacion=function(
    archivo
){
  
  #asumiendo que en todos los archivos se encuentran en la misma posición los datos
  
  un_dataframe=chip[[archivo]]
  
  pagos=un_dataframe$...8[9] 
  municipio=names(un_dataframe)[1]
  periodo=as.character(un_dataframe[2,1])
  
  return(list("pagos educacion"=pagos, "municipio"=municipio, "periodo"=periodo))
}

#(*) revisar que funcione:
extraer_pagos_educacion(1)
extraer_pagos_educacion(10)
extraer_pagos_educacion(50)


#3. Aplicar la función (extraer_pagos_educacion) a todos elementos de chip 

#Utilizando Familia Apply -> lapply
lista_pagos=
  lapply(
    1:length(chip),
    function(i){
      extraer_pagos_educacion(i)
    }
  )

#(*) revisar que funcione:
length(lista_pagos)
head(lista_pagos) 
tail(lista_pagos) 


#Crear Tabla con todos los datos de la lista
pagos_df= bind_rows(lista_pagos) 
View(pagos_df)

pagos_df= pagos_df [ , c(2,3,1)]
View(pagos_df)












