# A guide on how to use Orientdb through IBM Data Science Experience(DSX) as well as through orientdb console and orientdb studio.

# Graph Databases.
Graph databases are well-suited for analyzing interconnections, which is why there has been a lot of interest in using graph databases to mine data from social media. Graph databases are also useful for working with data in business disciplines that involve complex relationships and dynamic schema, such as supply chain management, identifying the source of an IP telephony issue and creating "customers who bought this also looked at..." recommendations.

This tutorial gives you a head start on how to work with graphdb-Orientdb on IBM Data Science Experience(DSX) using pyorient.This journey will help developers get started with various orientdb operations like CRUD, basic traversal and extracting insights using both sql and gremlin- which is a specialised query language for property graphs  by Apache Tinkerpop that works on all major graph databases on both console as well as orientdb studio.

In this journey we will demonstrate:

* a brief introduction to graph theory, just so developers can appreciate graph theory.
* Introduction to graph database - OrientDb
* Setting up ipython notebook on DSX connecting to orientdb and performing crud operations using pyorient.
* Hands-on on the crud operations and extracting insights from graph database using gremlin and sql on orientdb Console as well as through notebook.
* Brief video tutorial on how to use orientdb studio. 
* Visualisation of the results. 

![](doc/source/images/architecture.png)

1. The Kuberntes Cluster on which orientdb is running.
2. Orientdb instance on kubernetes.
3. Object storage stores the config file and kaggle imdb data.
4  config file and data used in the jupyter notebook.
5. The Jupyter notebook processes the data and generates insights.It can viewed in orientdb Studio.
6. The Jypyter notebook is powered by Spark.
 

   
## Included components

* [Orientdb](http://orientdb.com/orientdb/): A Multi-Model Open Source NoSQL DBMS.

* [IBM Data Science Experience](https://apsportal.ibm.com/analytics): Analyze data using RStudio, Jupyter, and Python in a configured, collaborative environment that includes IBM value-adds, such as managed Spark.

* [Bluemix Object Storage](https://console.ng.bluemix.net/catalog/services/object-storage/?cm_sp=dw-bluemix-_-code-_-devcenter): A Bluemix service that provides an unstructured cloud data store to build and deliver cost effective apps and services with high reliability and fast speed to market.

* [Jupyter Notebooks](http://jupyter.org/): An open-source web application that allows you to create and share documents that contain live code, equations, visualizations and explanatory text.

* [Kubernetes Clusters](https://console.ng.bluemix.net/docs/containers/cs_ov.html#cs_ov)

* [Bluemix container service](https://console.ng.bluemix.net/catalog/?taxonomyNavigation=apps&category=containers)

## Featured technologies

* [Data Science](https://medium.com/ibm-data-science-experience/): Systems and scientific methods to analyze structured and unstructured data in order to extract knowledge and insights.

## Prerequisite

Create a Kubernetes cluster with [IBM Bluemix Container Service](https://github.com/IBM/container-journey-template) to deploy in cloud.Deploy orientdb on kubernetes container using [Deploy Orientdb on container]( https://github.com/IBM/deploy-graph-db-container). 

# Watch the Video
[Orientdb Studio Tutorial](https://youtu.be/l-OVSjf-vk0) gives a head start on how to play around with orientdb studio.


# Steps

Follow these steps to setup and run this developer journey. The steps are
described in detail below.


1. [Get started with orientdb on Data Science Experience using PyOrient](#6-get-started-with-orientdb-on-data-science-experience-using-pyorient)
    * 1.1 [Sign up for the Data Science Experience](#11-sign-up-for-the-data-science-experience)
    * 1.2 [Create the notebook](#12-create-the-notebook)
    * 1.3 [Add the data](#13-add-the-data)
    * 1.4 [Update the notebook with service credentials](#14-update-the-notebook-with-service-credentials)
    * 1.5 [Run the notebook](#15-run-the-notebook)
1. [Orientdb Console](#2-orientdb-console )
1. [Orientdb Gremlin Console](#3-orientdb-gremlin-console )
1. [Orientdb Studio](#4-orientdb-studio)




## 1. Get started with orientdb on Data Science Experience using PyOrient.
PyOrient is a python driver for orientdb.
If you are using the Data Science Experience ( DSX) , set up the orientdb on kubernetes is important.This is because if you set up the orientdb locally, you won’t able to access it through DSX, as the orientdb console port 2424 and orientdb studio 2480 wouldn’t be exposed on bluemix.Deploy orientdb on kubernetes container using [Deploy Orientdb on Kubernetes]( https://github.com/IBM/deploy-graph-db-container) will expose the ports on bluemix through which orientdb can be accessed from the notebook on data science experience.You can use the ip-address of your cluster and node port on which the port 2424 orientdb console is mapped, to access that orientdb through notebook.However, you can always run the notebook and orientdb on your local system.

Notebook's markdown and comments are self explanatory to make you understand its functioning.Set up the Notebook on Data Science experience with object storage using following steps :

### 1.1 Sign up for the Data Science Experience

Sign up for IBM's [Data Science Experience](http://datascience.ibm.com/). By signing up for the Data Science Experience, two services: ``DSX-Spark`` and ``DSX-ObjectStore`` will be created in your Bluemix account.

### 1.2 Create the notebook

* Open [IBM Data Science Experience](https://apsportal.ibm.com/analytics). 
* Use the menu on the top to select `Projects` and then `Default Project`.
* Click on `Add notebooks` (upper right) to create a notebook.
* Select the `From URL` tab.
* Enter a name for the notebook.
* Optionally, enter a description for the notebook.
* Enter this Notebook URL: https://github.com/IBM/graph-db-insights/notebooks/Graphdb-Insights.ipynb
* Click the `Create Notebook` button.

### 1.3 Add the data 

##### Add the data to the notebook
* Please download the files from :
 https://www.kaggle.com/deepmatrix/imdb-5000-movie-dataset .
* Trim the data to 600 rows for the purpose of this tutorial and Rename the file  `Graphdb-Insights.csv`
* From your project page in DSX, click `Find and Add Data` (look for the `10/01` icon)
and its `Files` tab. 
* Click `browse` and navigate to `Graphdb-Insights.csv` on your computer.
* Add the files to Object storage.
* Repeat the above Steps for `config.json` as well.

![](doc/source/images/add_file.png)

### 1.4 Update the notebook with service credentials.

##### Add the Object Storage credentials to the notebook
* Select the cell below `2.1 Add your service credentials for Object Storage` section in the notebook to update the credentials for Object Store. 
* Use `Find and Add Data` (look for the `10/01` icon) and its `Files` tab. You should see the file names uploaded earlier. Make sure your active cell is the empty one created earlier. 
* Select `Insert to code` below Graphdb-Insights.csv as pandas Dataframe.
* Click `Insert Crendentials` from the drop down menu.
* Repeat the above steps for `config.json`.

### 1.5 Run the notebook

When a notebook is executed, what is actually happening is that each code cell in
the notebook is executed, in order, from top to bottom.

Each code cell is selectable and is preceded by a tag in the left margin. The tag
format is `In [x]:`. Depending on the state of the notebook, the `x` can be:

* A blank, this indicates that the cell has never been executed.
* A number, this number represents the relative order this code step was executed.
* A `*`, this indicates that the cell is currently executing.

There are several ways to execute the code cells in your notebook:

* One cell at a time.
  * Select the cell, and then press the `Play` button in the toolbar.
* Batch mode, in sequential order.
  * From the `Cell` menu bar, there are several options available. For example, you
    can `Run All` cells in your notebook, or you can `Run All Below`, that will
    start executing from the first cell under the currently selected cell, and then
    continue executing all cells that follow.
* At a scheduled time.
  * Press the `Schedule` button located in the top right section of your notebook
    panel. Here you can schedule your notebook to be executed once at some future
    time, or repeatedly at your specified interval.

For this Notebook, To run every cell one by one is recommended. It will give a better understanding on how things are working.

## 2. Orientdb Console 
Demonstrating the following operations in gremlin and sql on console -
 * 2.1 [Accessing the console](#21-accessing-the-console)
 * 2.2 [Creating and connecting database](#22-creating-and-connecting-database)
 * 2.3 [Creating Node classes, Edge classes and their properties](23-creating-node-classes-edge-classes-and-their-properties)
 * 2.4 [Creating Records/Vertex / Inserting](#24-creating-recordsvertex--inserting)
 * 2.5 [Creating and edge between the two vertex](#25-creating-relationedge-between-the-two-vertices)
 * 2.6 [Retrieving edge/vertex  based on a condition](#26-retrieving-edgevertex--based-on-a-condition)
 * 2.7 [Updating an edge/vertex](#27-updating-an-edgevertex)
 * 2.8 [Deleting an edge/vertex](#28-deleting-an-edgevertex)
 * 2.9 [Insights](#29-insights)

#### 2.1 Accessing the console.
  * In the bin folder of orientdb run `./console.sh` on the terminal to access the orientdb console.To access the gremlin console run `./gremlin.sh`.
  
   
 
 
#### 2.2 connecting to server and Creating database 
  * on the orientdb console run ` CONNECT remote:/< ip-of-the-orientdb-kubernetes-cluster/> <username> <password> `
   
    ```bash
    $ ./console.sh 
    OrientDB console v.2.2.26 (build ae9fcb9c075e1d74560a336a96b57d3661234c7b) https://www.orientdb.com
    Type 'help' to display all the supported commands.
    Installing extensions for GREMLIN language v.2.6.0

    orientdb> connect remote:localhost root root                          

    Connecting to remote Server instance [remote:localhost] with user 'root'...OK
    ```
 
   
  * You are connected to the server now , let’s create a new database. To create type create database remote:localhost/<name-of-the-database> <username> <password> plocal graph
  
   
   
  * To verify that you are connected to orientdb, type command LIST DATABASES and you should see the databases present in the orientdb.
  
  
  
  * on the gremlin console run g = new OrientGraph("remote:<ip-of-the-orientdb-kubernetes-cluster>/<database-name>");
    
   

#### 2.3 Creating Node classes, Edge classes and their properties.
For this tutorial, I have created  two vertex classes namely - Person and Movie. With Person’s attributes/ Properties :  Role: Director/ActorName, Fb-likes and Movie’s Attribute: Title, Year, IMDB rating, Duration, Language, Genre, Plot keywords, Num_critic_for_reviews, movie_facebook_likes. And two Edge classes - acted_in and worked_with.

* To create vertex class, on your console type  create class <classname> extends V
    create class person extends V
    create class movie extends V
 
        

* To create property of the vertex class, run this command : 
Create property <class-name>.<name-of-the-property>  IF NOT EXISTS   TYPE.  



* Creating Edge Class.
To create edge class, on your console type : create class <classname> extends E.
  
  
* Edges have two ends.It always start from one vertex class and ends on another vertex.Its acts as a bridge between two vertices.Hence, The edge will always have in and out property. To create property of the Edge class, run this command :
        Create property <class-name>.in  IF NOT EXISTS  Link  <linked_vertex_class>  
        Create property <class-name>.out  IF NOT EXISTS Link  <linked_vertex_class>
Example :




* Run these commands to create node classes, edge classes and their respective properties. 
 
#### 2.4 Creating Records/Vertex / Inserting
* To create records in the orientdb, run the following command on your console :
INSERT INTO  <vertex-class-name> (<class-property-1>,<class-property-2>, <class-property-3>......) VALUES (value1,value2,value3 ………...)
Example :
INSERT INTO person (name, fblikes, role) VALUES ("Scarlett Johansson",19000.0,"actor")


![](doc/source/images/create-vertex.png)


#### 2.5 Creating Relation/Edge between the two vertices:
* Run the following command on your console  to create relation/edge between two vertices.Please Note! Since orientdb is a nosql database, You can use edge class which has already been created or you can give any name to your relation and orientdb will automatically create a new edge class with that name. Before running this command, make sure the vertices you are trying to connect with this edge are already present.Otherwise, it will throw an error. Check the screenshot below, Before running the create edge command, I have created two nodes I want to connect, vertex/node with name CCH Pounder and Joel David Moore.
Syntax:
create edge <give-name-of-the-relation> from (select from person where name = "name-of-the-vertex") to (select from person where name = "name-of-the-vertex")
Example :
create edge worked_with from (select from person where name = "CCH Pounder") to (select from person where name = "Joel David Moore")

![](doc/source/images/create-edge.png)

#### 2.6 Retrieving edge/vertex  based on a condition.
* Use SELECT in orientdb, Similar to the select in sql to retrieve data on a particular condition. Suppose, You want to retrieve the fblikes of an actor with name Scarlett Johansson.
Syntax : select <whichever-parameter-you-want-retrieve> from <name-of-the-class> where <condition>
Example : select fblikes from person where name = "Scarlett Johansson"
 
![](doc/source/images/retrieve-vertex.png)

#### 2.7 Updating an edge/vertex.
* To update class or record, the syntax is :
UPDATE <class>|CLUSTER:<cluster>|<recordID>
 [SET|INCREMENT|ADD|REMOVE|PUT <field-name> = <field-value>[,]*]|[CONTENT|MERGE <JSON>]
 [UPSERT]
 [RETURN <returning> [<returning-expression>]]
 [WHERE <conditions>]
 [LOCK default|record]
 [LIMIT <max-records>] [TIMEOUT <timeout>]
Example : To update a role of a person where name is "Scarlett Johansson" from actor to director --
UPDATE person SET role='director' UPSERT where name = "Scarlett Johansson"

![](doc/source/images/update-record.png)

#### 2.8 Deleting an edge/vertex.
* Deleting an edge/vertex.
DELETE EDGE
You can delete one or more edges from the database. Use this command if you work against graphs. The "Delete edge" command takes care to remove all the cross references to the edge in both "in" and "out" vertices.
Syntax
DELETE EDGE <rid>|[<rid> (, <rid>)*]|FROM <rid>|TO <rid>|[<class>] [WHERE <conditions>]> [LIMIT <MaxRecords>]
Example :
DELETE edge worked_with from (select from person where name = "CCH Pounder") to (select from person where name = "Joel David Moore")

![](doc/source/images/delete-edge.png)

* DELETE VERTEX        
You can  delete one or more vertices from the database. Use this command if you work against graphs. The "Delete Vertex" (like the Delete Edge) command takes care to remove all the cross references to the vertices in all the edges involved.
Syntax:
DELETE VERTEX <rid>|<class>|FROM (<subquery>) [WHERE <conditions>] [LIMIT <MaxRecords>>]
Example :
DELETE VERTEX from person where name = "Scarlett Johansson"


![](doc/source/images/delete-vertex.png)

#### 2.9 Insights
You created and populated a small graph of movie dataset.To cross edges, you can use special graph functions, such as:
* OUT() To retrieve the adjacent outgoing vertices
* IN() To retrieve the adjacent incoming vertices
* BOTH() To retrieve the adjacent incoming and outgoing vertices
For example, To know with who all Johnny Depp ‘worked_with’  --- ( his co-workers)
SELECT out('worked_with') FROM person WHERE name = 'Johnny Depp'

![](doc/source/images/insights.png)

## 3. Orientdb Gremlin Console 
* To access the gremlin console run `./gremlin.sh`.
  ![](doc/source/images/orientdb-gremlin-console.png)
  
* Connecting to orientdb:
  To open a database on a remote server. Assure the server is up and running. To start the server just launch server.sh.
    g = new OrientGraph("remote:<ip-of-the-kubernetes-cluster>/<database-name>");
    ![](doc/source/images/connect-to-database-gremlin.png)
  
* Creating vertex, To create a new vertex use the addVertex() method. The vertex will be created and the unique id will be displayed as return value.: run g.addVertex();
![](doc/source/images/add-vertex.png)

* Creating an edge :
To create a new edge between two vertices use the addEdge(v1, v2, label) method. The edge will be created with the label specified.
v1 = g.addVertex();
v2 = g.addVertex();
e = g.addEdge(v1, v2, 'worked_with');

![](doc/source/images/create-edge-gremlin.png)

* Display all the vertices present in the database :
 Use g.V method to do so!
 
![](doc/source/images/display-all-vertices-gremlin.png)

* Get a vertex with a particular id :
        To retrieve a vertex by its ID, use the v(id) method passing the record id as argument (with or without the prefix '#').Run : g.v('18:1')
        
![](doc/source/images/display-all-vertices-gremlin.png)

* Retrieve all the edges present in the graph :
        To retrieve all the edges present in the graph, use g.E method.
        
![](doc/source/images/display-all-edges-gremlin.png)

* Traversal
Gremlin is  very powerful in traversing graphs. Once you have a graph loaded in your database you can traverse it in many ways.
Basic Traversal
To display all the outgoing edges of the first vertex , use the .outE at the vertex. Example: v1.outE

![](doc/source/images/traversal-gremlin-outgoing-edges.png)

And to display all the incoming edges of the second vertex, use .inE method. Example v2.inE

![](doc/source/images/traversal-gremlin-incoming-edges.png)

* Filter results
For example , if you want to return all the outgoing edges of all the vertices with label equals to 'worked_with’
g.V.outE('worked_with')

![](doc/source/images/traversal-gremlin-incoming-edges.png)


## 4. Orientdb Studio

![](doc/source/images/orientdb-studio.png)

* open the browser and hit http://<ip-address-of-the-kubernetes-cluster>:<node-port mapped to port 2480 of orientdb>/ to access orientdb  
* Select the database you want to work on from dropdown. Using login credentials- the username and the password for orientdb to login. 
* Follow the video tutorial [Orientdb Studio Tutorial](https://youtu.be/l-OVSjf-vk0) on how to perform various operations on orientdb.
* All the commands discussed in the orientdb console part of the tutorial can be executed in the browse section of orientdb.The result can be viewed in the table/raw(json) format. And can also be exported to csv file.To export the result to csv, In the browse section click the setting button. And then run the query.

![](doc/source/images/orientdb-studio-query-settings.png)

# Troubleshooting

[See DEBUGGING.md.](DEBUGGING.md)

# License

[Apache 2.0](LICENSE)

