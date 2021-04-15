#### git clone script para instalar elasticsearch y sus dependencias ####
git clone https://github.com/adrianfime27/elastic-7.12.git
cd elastic-7.12/
sudo sh install-elastic.sh

#### confifgurar archivo /etc/elasticsearch/elasticsearch.yml y modificar SOLO ####
sudo nano /etc/elasticsearch/elasticsearch.yml
cluster.name: puntadelicia
node.name: master-1 ### para nodos poner node-1 o node-2
network.host: 192.168.2.104 ## ip de la VM
http.host: 9200
discovery.seed_hosts: ["192.168.2.104","192.168.2.48","192.168.2.188"] ### en este caso se hizo master-1, node-1, node-2 y en ese orden se escribe en los archivos de los demás nodos.
cluster.initial_master_nodes: ["master-1"]
/*agregar estas lineas, ya que no están dentro del archivo*/
node.master: true ### poner false para los nodos
node.data: true ### poner false en master si quieres que el master no guarde datos, solo los nodos
node.ingest: true ### poner false en master si quieres que el master no guarde datos, solo los nodos

#### configurar archivo /etc/elasticsearch/jvm.options y modificar SOLO ####
sudo nano /etc/elasticsearch/jvm.options
-Xms4g ### minima RAM - poner 3/4 partes de RAM dedicadas al servicio, esta VM tiene 6 de RAM
-Xmx4g ### maxima RAM - poner 3/4 partes de RAM dedicadas al servicio, esta VM tiene 6 de RAM

#### configurar archivo /etc/default/elasticsearch y modificar SOLO ####
sudo nano /etc/default/elasticsearch
/*descomentamos la linea*/
MAX_LOCKED_MEMORY=unlimited

#### configurar archivo /usr/lib/systemd/system/elasticsearch.service y modificar SOLO ####
sudo nano /usr/lib/systemd/system/elasticsearch.service
/*agregar la linea dentro de [Service]*/
LimitMEMLOCK=infinity

#### configurar archivo /etc/fstab y modificar SOLO ####
sudo nano /etc/fstab
/*comentar la parte de la swap*/

#### eliminar archivos de instalación
sudo systemctl daemon-reload
sudo service elasticsearch stop
sudo -i
rm -rf /var/log/elasticsearch/*
rm -rf /var/lib/elasticsearch/*
exit

#### habilitar y resetear el servicio ####
sudo systemctl enable elasticsearch && sudo systemctl daemon-reload && sudo service elasticsearch restart

#### en el archivo /var/log/elasticsearch/puntadelicia.log se verá si se hizo la conexión o el estado del master o nodo.

