# Elasticsearch & Kibana Download Using Docker Image & Container

![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FCybersecurity_FI%2FiCvZaLfW7s.png?alt=media&token=f94b6980-c60f-4f2e-ae77-57e436c571f1)

- [Elasticsearch & Kibana Install Using Docker Images & Container](#elasticsearch---kibana-install-using-docker-images---container)
  * [ElasticSearch Main Use Cases](#elasticsearch-main-use-cases)
  * [Download Instructions](#download-instructions)
    + [Docker on Ubuntu 18.0.4 VM using VirtualBox.](#docker-on-ubuntu-1804-vm-using-virtualbox)
    + [Elasticksearch Docker Image and Run Container](#elasticksearch-docker-image-and-run-container)
- Elasticsearch is an open source search engine developed in Java. It is based
on JSON search queries. This is an industry standard application that is
popular due to the ease of use, scalability and flexibility. Many other tools are
built around the elasticsearch engine. Additionally, the Logstash product can
be used for log storage and analysis. Kibana is a front end, web interface for
using the power of elasticsearch for queries and visualizations. ELK Stack
combines those three products (Elasticsearch-Logstash-Kibana) into a search
and analytics engine similar to other commercial SIEMs (LogRhythm, Splunk).
Many of these SIEMs actually use elasticsearch or the entire ELK Stack as their engine. Note that ELK Stack is transitioning to being called Elastic Stack. (SecureSet)

## ElasticSearch Main Use Cases
    - ElasticSearch can be used for multiple purposes, such as:
        - Logging and Log Analysis:  The ecosystem built up around Elasticsearch has made it one of the easiest to implement and scale logging solutions.
        - Scraping and Combining Public Data: Elasticsearch has the flexibility needed to take in multiple different sources of data and keep it all manageable and searchable.
        - Full-Text Search: ElasticSearch is document oriented. It stores and indexes documents. Indexing creates or updates documents. Once the indexing is finished, you can search, sort, and filter complete documents—not rows of columnar data.
        - Event Data and Metrics: Elasticsearch is also known for working great well on time-series data such as metrics and application events. No matter the technology you are using ElasticSearch probably has the needed components to easily grab data for common applications; and in the rare case that it doesn't, adding that capability is quite easy (Data Bricks)

# Download Instructions 
## Docker Install
- Set up a free account with Docker. 
    - Docker MAC or Windows: 
        - It is recommended to download the docker GUI on Windows or MAC, Ubuntu directions are listed below but is more advanced*.
            - Download Docker: 
            ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FCybersecurity_FI%2FB-wWrK_DZa.png?alt=media&token=7195b9f0-1598-40b5-8c4a-f5e1f51cc05f)
            - Make sure to increase memory and disk space for your expected use of docker, these settings are more difficult to update later in Ubuntu* as you do not have the GUI accessible.
            ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FCybersecurity_FI%2Fpt4oEkNWqL.png?alt=media&token=45b0d817-4fe2-4fc9-b3d1-9361a46043ef)
## Docker on Ubuntu 18.0.4 VM using VirtualBox. 
-   Resources: 
    - https://www.virtualbox.org/wiki/Downloads
    - https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-network.html
    - https://hub.docker.com/_/elasticsearch

- Update the apt package index and install packages to allow apt to use a repository over HTTPS:
    ```shell
    $ sudo apt-get update

    $ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
    ```
- Add Docker’s official GPG key:
    ```shell
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```
    
    ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FCybersecurity_FI%2FytSaYyDr33.png?alt=media&token=b38769af-c1ad-44d7-ba4e-42fe2cf65604)
- Verify that you now have the key with the fingerprint
    ```shell
    $ sudo apt-key fingerprint 0EBFCD88

    pub   rsa4096 2017-02-22 [SCEA]
        9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
    uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
    sub   rsa4096 2017-02-22 [S]```
- Use the following command to set up the **stable** repository.
    ```shell
    $ sudo add-apt-repository \
        "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
        $(lsb_release -cs) \
        stable"
        ```
- Update the apt package index, and install the __latest version__ of Docker Engine and containerd
    ```shell
    $ sudo apt-get update
    $ sudo apt-get install docker-ce docker-ce-cli containerd.io
    ```
## Elasticksearch Docker Image and Run Container
- NOTE: is important to add the 7.9.3 tag version, here you can specify older versions if you would like. 
    - Use the followin command to pull elasticsearch image and create a network to link to docker. 
    ```shell
    $ sudo docker pull elasticsearch:7.9.3
    ```
    - Create a connection for docker to use, this can be whatever name you'd like "somenetwork"
    ```shell
    $ sudo docker network create somenetwork
    ```
    ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FCybersecurity_FI%2FgJB_A3Y2QA.png?alt=media&token=3e9da116-9c4b-4739-b760-fd54d54aa81f)
    - Run the following command to set up the elasticsearch clusters and nodes to communicate with Kibana.
    ```shell
    $ sudo docker run -it --name elasticsearch:7.9.3 --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.9.3
    ```
    - Run the following to make sure Elasticsearch is up an running
    ```shell
    $ curl -X GET 'http://localhost:9200'
    ```
    ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FCybersecurity_FI%2Fpb4Raywl4i.png?alt=media&token=e1528acb-69b1-4da6-8779-ab9640c720ab)
## Kibana Docker Image and Run Container
- NOTE: ensure network names are the same as created above.
    - Pull the kibana docker image, NOTE it is important to pull with the tag for the version you want. 
    ```shell
    $ sudo docker pull kibana:7.9.3
    ```
    ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FCybersecurity_FI%2FvUEVv7_IaR.png?alt=media&token=174b0786-a0fd-46f6-880b-1fcfbac52fa2)
    - Run the following command to link kiban with elasticsearch on the network you created listening to port 5601. 
    ```shell
    $ sudo docker run -it --name kibana --net somenetwork -p 5601:5601 kibana:7.9.3
    ```
- You should name be able to access the Elastic web page on https://localhost:5601
![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FCybersecurity_FI%2FOtwtnGq1IA.png?alt=media&token=b340f751-7bf3-41e1-9890-6fdaad7007ba)
- Ingesting Log Data:
    - Within the Kibana console, navigate to "explore on my own" 
    - Then navigate to "log data"
    - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FCybersecurity_FI%2FA2OtisQxZ0.png?alt=media&token=0b6c5325-cc1f-4446-bad6-967569bc439c)

- https://databricks.com/glossary/elasticsearch

# More guides Coming Soon! Stay Tuned & Keep Downloding :-)
