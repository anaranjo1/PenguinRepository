# Mi primer proyecto

En este primer proyecto con `Python` se trabajará con el conjunto de datos `PalmerPenguins`. A continuación se muestra la solución a esta práctica.

### EJERCICIOS:

Importamos las librerías necesarias para realizar el ejercicio:

```python

import pandas as pd
from palmerpenguins import load_penguins

```
### 1. Vamos a cargar el conjunto de datos. Muestra por pantalla el número de observaciones y sus características. Mira el tipo de datos de cada una de sus columnas.

```python

penguins = load_penguins()
print(penguins.info) #imprimimos un resumen del dataframe
print(penguins.shape) #obtenemos la dimensión o forma de los datos
print(penguins.dtypes) #obtenemos los tipos de cada columna

```

### 2. Ya sabemos que este conjunto de datos tiene observaciones `NA`. Vamos a eliminarlas y a verificar que efectivamente no queda ninguno:

```python

penguins_rm = penguins.dropna() #eliminamos las filas con observaciones NA.
print(penguins_rm.isna().any()) #comprobamos si existen valores NA.
print(penguins_rm.isna().sum()) #sumamos valores NA si es que hubiesen.
#De estas dos formas confirmamos que no queda ningún valor NA.

```

### 3. ¿Cuántos individuos hay de cada sexo? Puedes obtener la longitud media del pico según el sexo:

```python

#procedemos a contar los individuos de cada sexo
print(penguins_rm["sex"].value_counts())
#En total, habrá 168 machos y 165 hembras.

#procedemos a calcular la longitud media del pico por sexo
print(penguins_rm.groupby("sex")["bill_length_mm"].mean())

```
### 4. Vamos a añadir una columna, vamos a realizar una estimación (muy grosera) del área del pico de los pingüinos (bill) tal como si esta fuese un rectángulo. Esta nueva columnas se llama `bill_area` y debe encontrarse en la última posición. Verifica que es correcto.

```python

#teniendo en cuenta que el área del rectángulo es lado por lado:
penguins_rm["bill_area"] = penguins_rm["bill_length_mm"] * penguins_rm["bill_depth_mm"]
print(penguins_rm.head()) #esta nueva columna ya se encuentra en la última columna.

```

### 5. Hagamos algo un poco más elaborado, vamos a realizar una agrupación en función del **sexo y de la especie de cada observación** donde obtendremos la longitud media del pico y solamente la referente al de las hembras.

```python

#filtramos las hembras
penguins_female = penguins_rm[penguins_rm["sex"] == "female"] 
#agrupamos por sexo y especie y calculamos la longitud media del pico.
penguins_mean_bill_female = penguins_female.groupby(["species","sex"])["bill_length_mm"].mean()
print(penguins_mean_bill_female)

```
### 6. Como ya sabemos, la variable peso, se encuentra en gramos, la pasaremos a kg. Para ello crearemos una nueva columna llamada body_mass_kg y eliminaremos body_mass_g.

```python

penguins_rm["body_mass_kg"] = penguins_rm["body_mass_g"] / 1000 #pasamos de gramos a kilos
penguins_kg = penguins_rm.drop('body_mass_g', axis=1) #eliminamos la columna body_mass_g
print(penguins_kg)

```