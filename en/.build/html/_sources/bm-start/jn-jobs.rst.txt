.. module:: basemap.jenkins-docker
   :synopsis: Exploring Jenkins Docker image for basemap OSM stack.

.. _basemap.jenkins-docker:

Understanding Jenkins Docker
---------------------------------------

`Jenkins <https://www.jenkins.io/>`_  is an open source automation tool written in Java programming language that allows continuous integration. Jenkins makes it easier for developers to integrate the changes and obtain fresh build. It allows continuous delivery using various plugins designed for specific technologies e.g. GIT, Amazon EC2, etc.

We'll use `this <https://registry.hub.docker.com/layers/jenkinsci/blueocean/1.24.4/images/sha256-ace30b2c5702fd4d500c4ab48595e3dd5797c9ebd2bf9f438f92f6f5a6639eaf?context=explore>`_ docker image for jenkins
In basemap OSM stack, jenkins allows seamless upgrade of slave geoservers based on changes done in master geoserver. Which makes it easy to first try on Master geoserver and once obtained satisfied results, then deploy it to slave geoservers.
Apart from this jenkins is also used to make it easy to seed, reseed, truncate the `GeoWebCache <https://docs.geoserver.org/stable/en/user/geowebcache/index.html>`_ . 
Let us understand each job in more depth

Jenkins Jobs for basemap OSM
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
1. Promoting Geoserver Configurations

Here whatever changes are made in Master geoserver (<Base_URL>/backoffice) can be promoted to all other slave geoservers

.. figure:: img/jenkin-home.png 

Now let’s create a new Jenkins job to manage GeoServer configuration promotion. Click on the New Item button in the top left corner and create a new Job of type “Pipeline” assigning it a meaningful name:

.. figure:: img/jen-gs-config.png

Add a description and check the “Discard old builds” option and set the “Max # of builds to keep” value to 10. Also check the “Do not allow concurrent builds” option:

.. figure:: img/jen-gs-config-2.png

Scroll down till the Pipeline section and paste the following code, and then click on Save:
::
    pipeline {
    agent any
    stages {
    stage('Versioning datadir') {
        steps {
       // One or more steps need to be included within the steps block.
            sh returnStatus: true, script: '''#!/bin/bash
            docker exec -t geoserver-master git config --global user.email "jenkins@osmpmaps.com"
             docker exec -t geoserver-master git config --global user.name "jenkins-osmpmaps"
     echo "Versioning the new changes"
     docker exec -i geoserver-master bash -c \'cd /var/geoserver/datadir; git init; git add .; git commit -a --allow-empty-message -m ""\''''
     }
   }

   stage('Backing up the datadir from geoserver-slave') {
     steps {
       // One or more steps need to be included within the steps block.
       sh returnStdout: true, script: '''#!/bin/bash
     #echo "Checking the containers"
     #docker ps
     echo "=> Backing up the datadir from the slave container..."
     docker exec -t geoserver-master mv /slave/datadir /slave/datadir.bak
     echo "...DONE"'''
     }
   }

   stage('Syncing the datadir from master to slave') {
     steps {
       // One or more steps need to be included within the steps block.
       sh returnStatus: true, script: '''#!/bin/bash
       set -x
     echo "=> Syncing the new datadir to the slave..."
     #docker exec -t geoserver-master cp -R /var/geoserver/datadir /slave/datadir
       docker exec -t geoserver-master rsync -avz  /var/geoserver/datadir /slave/ --exclude=".git"
     echo "...DONE"
     echo "=> Checking if they have the same content..."
     docker exec -t geoserver-master diff -q /var/geoserver/datadir /slave/datadir
     echo "...DONE"'''
     }
   }

   stage('Fixing geoserver-slave environment') {
     steps {
       // One or more steps need to be included within the steps block.
       sh returnStatus: true, script: '''#!/bin/bash
     echo "=> Get Permissions back to root..."
     docker exec -t geoserver-slave chown -R root:root /var/geoserver/datadir
     echo "...DONE"
     '''
     }
   }

   stage('Restarting the Geoserver-Slave') {
     steps {
       // One or more steps need to be included within the steps block.
       sh returnStatus: true, script: '''#!/bin/bash
     echo "Restart Geoserver-Slave"
     docker restart geoserver-slave'''
     }
     }

   stage('Testing the Results') {
     steps {
       // One or more steps need to be included within the steps block.
       sh returnStatus: true, script: '''#!/bin/bash
         sleep 30
         docker exec -t geoserver-slave curl -I -s --retry-delay 1 --retry 60 -X GET "http://localhost:8080/geoserver/wms?service=wms&version=1.1.1&request=GetCapabilities"
         echo "testing capabilities"
         curl -I -s --retry-delay 1 --retry 60 -X GET "http://geoserver:8080/geoserver/wms?service=wms&version=1.1.1&request=GetCapabilities"
         res=$?
         if test "$res" != "0"; then
            echo "the Test failed with: $res"
         else
            echo "Everything is OK!!"
         fi'''
     }
   }

   stage('Cleaning the old datadir') {
     steps {
       // One or more steps need to be included within the steps block.
       sh returnStatus: true, script: '''#!/bin/bash
         docker exec -t geoserver-slave rm -Rf /var/geoserver/datadir.bak'''
     }
        }
 }
}

That’s it. The new job will be visible from the Jenkins home page.

.. figure:: img/jen-gs-config-3.png


2. GeoWebCache Cache Seed - Truncate

We’re going to use this job to manage cached tiles (seed / truncate). Let’s create 2 more pipelines for the job:
Create a new Freestyle job named “Cache Seed - Truncate”

.. figure:: img/gwc-1.png

Enable the deletion of old builds with a limit of 10 and select  “This project is parameterized”. Then add the following parameters. 
Remember to update the IP address accordingly, to check the IP run following command on terminal
::
    docker inspect geoserver-master

.. figure:: img/gwc-1.png


In the result you can find the IP of geoserver backoffice, similarly also find out IP of slave geoserver
::
    docker inspect geoserver-slave

Setup the options as following

.. list-table:: Jenkins URLs table
   :widths: 25 25 25 50 25 
   :header-rows: 1

   * - Name
     - Type
     - Value
     - description
     - Trim the string
   * - geoServerUrl
     - Choice Parameter
     - http://172.16.36.105/backoffice ,http://172.16.36.105/geoserver
     - example:  http://mygeoserver.com/geoserver
     - -  
   * - operation
     - Choice Parameter
     - seed, reseed, truncate
     - Operation to perform
     - -
   * - layerName
     - String
     - osm:osm
     - Name of the target Layerqualified with the Workspace name, e.g. osm:osm
     - yes
   * - gridsetName
     - String
     - EPSG:3857
     - Target Gridset Name
     - yes
   * - bounds
     - String
     - -20026376.39 -20048966.10 20026376.39 20048966.10 
     - minX minY maxX maxY (example - 20026376.39 -20048966.10 20026376.39 20048966.10) Note: The unit of measure is defines by the projection. Could be lat-long or meters 
     - yes
   * - zoomStart
     - String
     - 0
     - Target start zoom level
     - yes
   * - zoomEnd
     - String
     - 5
     - Target stop zoom level
     - yes
   * - format
     - String
     - image/png
     - Image format interested by the operation
     - yes
   * - threadCount
     - String
     - 1
     - How many thread to spin up to carry on the operation
     - yes


**Important Notes:**

- *geoServerUrl* is a Choice Parameter that has two possible values, on per line. User a carriage return as to separate the backoffice and slaves URLs.
- The URLs set in the geoServerUrl must be reachable from the Jenkins docker container. In the example above I put the private IP address of the machine I am installing on. Please adapt to your environment.
- *operation* is also  a Choice Parameter with three possible values. One per line. User a carriage return as to separate the backoffice and slaves URLs.

**Build Environment**

In the Build Environment section we’re going to add credentials to be used by Jenkins to authenticate against geoserver. Select “Use secret text(s) or file(s)” and add a Username and Password (separated) credentials:

.. figure:: img/gwc-2.png

For the username variable name use “gsUsername” and for the password variable name “gsPassword”

.. figure:: img/gwc-3.png


Then click on “Add” to add the credentials in Jenkins

.. figure:: img/gwc-4.png

Add a username and password secret similar to the one below. For the ID field use geoserverUserPass

.. figure:: img/gwc-5.png


**Build**

In the Build section of the job, add an Execute Shell step with the following content:
::
    #!/bin/bash
    set -e
    if [ "$DEBUG" == 'true' ]; then
    echo "### Debug enabled"
    echo "### Printing environment"
    env
    set -x
    fi
    geoServerUrl=${geoServerUrl}/gwc/rest
    $JENKINS_HOME/gwc/gwc.sh \
    $operation \
    $layerName   \
    $gridsetName \
    -b $bounds \
    -zs $zoomStart \
    -ze $zoomEnd   \
    -f $format \
    -t $threadCount \
    -a "${gsUsername}:$gsPassword" \
    -u $geoServerUrl \
    -v


Finally, save the job.


3. GeoWebCache -  Cache Masstruncate

Now create a second job, similar to the previous one but with fewer parameters and a slightly different build command, name it Cache Masstruncate

.. list-table:: Masstruncate Job Parameters
   :widths: 25 25 25 50 25 
   :header-rows: 1

   * - Name
     - Type
     - Value
     - description
     - Trim the string
   * - geoServerUrl
     - Choice Parameter
     - http://172.16.36.105/backoffice ,http://172.16.36.105/geoserver
     - example:  http://mygeoserver.com/geoserver
     - -  
   * - layerName
     - String
     - osm:osm
     - Name of the target Layerqualified with the Workspace name, e.g. osm:osm
     - yes
   * - DEBUG
     - Boolean
     - false
     - Enable debugging? Disabled by default
     - -

**Important Notes:**

- geoServerUrl is a Choice Parameter that has two possible values, on per line. User a carriage return as to separate the backoffice and slaves URLs.
The URLs set in the geoServerUrl must be reachable from the Jenkins docker container. In the example above I put the private IP address of the machine I am installing on. Please adapt to your environment.


**Build Environment**

In the Build Environment section we’re going to add credentials to be used by Jenkins to authenticate against geoserver. You can use the ones defined for the previous Job.Build
In the Build section of the job, add an Execute Shell step with the following content:
::
    #!/bin/bash
    set -e
    if [ "$DEBUG" == 'true' ]; then
    echo "### Debug enabled"
    echo "### Printing environment"
    env
    set -x
    fi
    geoServerUrl=${geoServerUrl}/gwc/rest
    $JENKINS_HOME/gwc/gwc.sh \
    masstruncate \
    $layerName \
    -a "${gsUsername}:$gsPassword" \
    -u $geoServerUrl \
    -v

Finally, save the job.
