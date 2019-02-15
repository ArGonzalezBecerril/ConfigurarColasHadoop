# Creación de queue mediante el CapacityScheduler

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

El CapacityScheduler tiene una queue defaul denominada **root**, a partir de esta queue default todas las que nosotros vamos a crear pasaran a ser hijos de la principal. Entonces para poder definir una nueva usamos el parametro **yarn.scheduler.capacity.root.queues** este parametro se encuentra en el fichero **capacity-scheduler.xml** 
```sh
# Identificando la ruta donde se encuentra el fichero
arturo@debian$ cd /opt/hadoop/etc/hadoop
arturo@debian$ ls -lrht capacity-scheduler.xml
-rw-r--r-- 1 arturo arturo 4.4K jul 18  2018 capacity-scheduler.xml
```
Cuando se realiza una instalación limpia de hadoop, por default tenemos la siguiente configuración. 
[![N|Solid](https://i.ibb.co/89sbqH2/hadoop-default.png)](https://nodesource.com/products/nsolid)

Root es el queue principal, de esta forma cada nueva cola que se crea pasara a ser hijo de la cola principal(**root**). 

A continuación mostramos como crear 2 colas partiendo de la siguiente premisa. 

- **Root** 
    - Dev **(Desarrollo)**
    - Prod **(Producción)**
### **1** - Paso 1 (Editar el fichero **capacity-scheduler.xml**)
```sh
# Moverse al directorio donde se encuentra el fichero
arturo@debian$ cd  /opt/etc/hadoop/etc
# Editarlo
arturo@debian$ vim capacity-scheduler.xml
```
##### Cambiar este par de lineas:
```xml
  <property>
    <name>yarn.scheduler.capacity.root.queues</name>
    <value>defaul</value>
    <description>The queues at the this level</description>
  </property>
```

##### Por esta configuración:
```xml
  <property>
    <name>yarn.scheduler.capacity.root.queues</name>
    <value>dev,prod</value>
    <description>The queues at the this level</description>
  </property>
```
Es importante destacar que por cada nueva cola que se crea, se tiene que asignar un porcentaje de recursos del cluster respecto al 100%. Por ejemplo si creamos dos queue(colas) la suma de recursos asignados a cada una debe sumar 100%,  en mi caso asignare 30% a la cola **dev** y 70% a la **prod**  

A continuación se muestra como queda definido el porcentaje de recursos para cada una. 
```xml
 <!-- Capacidad en porcentaje de la cola dev -->
  <property>
    <name>yarn.scheduler.capacity.root.dev.capacity</name>
    <value>30</value>
  </property>
 <!-- Capacidad en porcentaje de la cola prod -->
  <property>
   <name>yarn.scheduler.capacity.root.prod.capacity</name>
   <value>70</value>
  </property>
 <!--Borrar la linea
   <property>
   <name>yarn.scheduler.capacity.root.default.capacity</name>
   <value>100</value>
  </property>
  -->
```
Para terminar voy a configurar un mínimo y un máximo de recursos a la cola **dev** 
```xml
  <!-- Capacidad maxima -->
  <property>
    <name>yarn.scheduler.capacity.root.dev.maximum-capacity</name>
    <value>45</value>
  </property>
  <!-- Definir la capacidad minima -->
  <property>
    <name>yarn.scheduler.capacity.root.dev.minimum-capacity</name>
    <value>10</value>
    <description>Definir la capacidad minima</description>
  </property>
  ```
  > Captura de pantalla de la configuración
  
  [![N|Solid](https://i.ibb.co/Js9HR8G/devProd.png)](https://nodesource.com/products/nsolid)
Una vez que se ha creado, podemos aplicar las siguientes configuraciones.

- **Used Capacity:** 	Capacidad usada en el momento de ejecucion de jobs.
- **Configured Capacity:** Capacidad  configurada
- **Configured Max Capacity:** Capacidad maxima que puede usar(Esto va dependiendo de la carga de trabajo del cluster)
- **Absolute Used Capacity:** Capacidad usada 
- **Absolute Configured Capacity:** Capacidad configurada(default).
- **Absolute Configured Max Capacity:** Capacidad maxima configuradad(limite de uso)
- **Used Resources:** Recursos usados tanto memoria y cores
- **Num Schedulable Applications:** Apliaciones programadas.
- **Num Non-Schedulable Applications:** Aplicaciones no programadas(Aplicaciones ad-hoc)
- **Num Containers:** Contenedores
- **Max Applications:** Maximo de aplicaciones que podemos ejecutar
- **Max Applications Per User:** 	Maximo de aplicaciones que podemos ejecutar(Por usuario)
- **Max Application Master Resources:** 	Maxima capacidad de recursos que podemos usar(memoria, cores)
- **Used Application Master Resources:** 	Recursos que se estan usando ahora mismo.
- **Max Application Master Resources Per User:** Maximo de recursos por usuario.

#### Conclusión 

El objetivo de las colas nos viene a facilitar la ejecución de jobs para distintas áreas o departamentos ya que en muchas ocasiones el clúster va a ser compartido. 


License
----

GNU

**Free Software, GNU!**

