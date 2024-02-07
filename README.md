# NEWS READER CRAWLER (Version 2.1)

### Introduction
*"News Reader Crawler"* is a specialized Python-written crawler for collecting news/articles from online newspapers, websites, and Vietnamese language blogs. The project can be used to simplify the task of data collection in Data Projects, or for educational purposes to help students and beginners understand the use of various practical techniques in Python (OOP, multiprocessing, parse HTML, json, yaml...)

Some examples of using the *"News Reader Crawler"* (see CASE_STUDY.md for a complete list)
- [Demo of *"News Reader Crawler"* performing parallel scanning of 35 news sources using 10 Firefox browsers at the same time](https://youtu.be/cPZ1XlAAIsk)
- [Theodoibaochi.com](https://theodoibaochi.com): a site that uses the *"News Reader Crawler"* to aggregate news from 35 popular online newspapers in Vietnam. This site also serves as the default frontend interface integrated with the crawler
- [VnAlert](https://vnalert.vn) - A commercial application that uses the *"News Reader Crawler"* as its data provider

Before using the project, please read CODE_OF_CONDUCT.md to understand some legal considerations when using the *Docbao Crawler*. To contribute to the project, please refer to HOW_TO_CONTRIBUTE.md

### Operating Principles

The *"News Reader Crawler"* operates on the principle of scanning multiple pages simultaneously in a predefined cycle (via crontab). Each scanning cycle (spaced at least 10-15 minutes apart), the crawler will go through all the pages in the configuration setup. For each page, the crawler will browse through a certain number of links, identify those leading to articles, and retrieve data. At the end of the scanning cycle, all data will be stored in a database (PostgreSQL) and pushed to other data channels (such as Elasticsearch, RabbitMQ, export json) depending on the setup.

The main advantage of the *"News Reader Crawler"* over using libraries like scrapy, requests, selenium... directly is optimized for scanning multiple sites at once and simplifying the analysis of the site structure to be scanned into plain text files that are easy to share, helping those without knowledge or experience in parsing HTML, using xpath, css selection... to set up the scanner and get the desired data (see more in "Key Features")


### Key Features

**1. Can be used without coding knowledge**
Data scanning is a common task in Data projects. The *"News Reader Crawler"* greatly simplifies this task by providing a management tool that allows users to adjust the crawler's operation such as adding/removing sources, changing scanning time... without having to intervene in the project's source code

**2. Easy to add/remove scanning sources by sharing configuration files**
The *"News Reader Crawler"* uses scanning configuration files (which are plain text files describing the structure of the site to be scanned in a specific language - yaml and xpath) that can be easily shared. Although building a configuration file requires high professional knowledge and skills in working with the configuration tool provided by the *"News Reader Crawler"*, once built, it can be shared for everyone to use.

**3. Capable of scanning most sites (including Facebook, Youtube) using a single algorithm and configuration language**
Instead of coding to scan data from each site (as with scrapy or some tutorials on building scanners), the *"News Reader Crawler"* creates a unified scanning language and algorithm based on the common features of news sites. This algorithm and language are powerful enough to scan most news sites, including those that are difficult like sites using Ajax to load content, sites requiring login (Facebook, Youtube), sites with unstable article structures (e.g., different article formats in each category). Although the algorithm and language require certain professional knowledge to understand, as mentioned above, users of the *"News Reader Crawler"* can use the config file created by others and do not need to understand the scanning algorithm or language to operate.

**4. Integrated with many tricks to prevent being blocked and handling common issues encountered during scanning**
The *"News Reader Crawler"* has been developed over 3 years and used to scan stably on hundreds of different news sites in Vietnam, so the project's source code has been integrated with many solutions to solve problems arising during the scanning process (many issues are indeed very challenging). Using the *"News Reader Crawler"* is a way for those without much scanning experience to obtain clean data without having to spend too much time "cultivating" and solving unnecessary issues.

**5. Collected data is structured down to the paragraph level**
A strong point of the *"News Reader Crawler"* is returning structured article data (including titles, descriptions/introductions, publication time, article content). Especially, the article content is detailed to each paragraph and retains the original article structure (e.g., image 1, paragraph 1, image 2, image 3, paragraph 3, paragraph 

4...). Other details (such as the author's name, number of likes, shares...) can be expanded to be retrieved in the future.

**6. Supports various forms of data retrieval**
The *"News Reader Crawler"* supports many forms to retrieve data and use it in larger projects:
- Pour data into PostgreSQL
- Pour data into Elasticsearch
- Pour data into RabbitMQ's queue
- Access data via an integrated API Server
- Access data via json files exported at the end of each scanning cycle
- View data directly through the frontend integrated with the project

**7. Integrated with a trend analysis algorithm based on keywords & frontend for data display**
The *"News Reader Crawler"* integrates a keyword analysis algorithm from the collected news data (using the tokenize function of the underthesea library) and detects information trends based on the growth of keywords. The *"News Reader Crawler"* also integrates a simple frontend that operates independently of the backend (reads data through json files) to help operators easily check and search scanned data.

**8. Comprehensive and detailed documentation (under construction)**
The *"News Reader Crawler"* aims to become an educational project and operate according to the open-source philosophy (as I have also learned a lot and received a lot of support when pursuing this project). Therefore, building comprehensive, detailed documentation for everyone, including those without much programming experience, to understand and be interested in installing and operating the project is a goal set by the project author.

### Directory Organization
- docs: contains the project's technical documentation
- resources: contains necessary files for installation and a repository of configuration files to add to the scanner
- src: folder containing the backend source code (scanner) and frontend (site displaying scanned data)
- scripts: necessary scripts for installation and operation of the scanner
- tools: tools used to manage scanning configurations and parameters

### Installation Guide

To run Docbao Crawler, you need a computer/VPS running Ubuntu, or Windows 10 with Linux Subsystem. Docbao Crawler can also run on Raspberry Pi with a small number of news sources to scan.

#### 1. Installation on Ubuntu
To install Docbao Crawler on Ubuntu (either 16.04 or 18.04), you need admin rights and will need to enter the admin password during the installation process. The specific installation steps are as follows:

##### Step 1: Clone the source code from github
Open terminal in Ubuntu (Ctr+Alt+T) and type the following command
~~~~
git clone http://github.com/hailoc12/docbao
~~~~

##### Step 2: Check the installation path again (SETTING.env)
If the installation folder is located at ~/docbao then you can skip this step. Otherwise, if you have placed the Docbao_crawler source code at a different path, then change the DOCBAO_BASE_DIR variable in the SETTING.env file

~~~
export DOCBAO_BASE_DIR=[path to the docbao_crawler folder]
~~~
Note: if you perform this step incorrectly, the installation steps will not run

##### Step 3: Run the installation file
Ensure you are in the docbao_crawler directory when running the following installation commands:
~~~~
source SETTINGS.env
bash install.sh
~~~~

The installer will run automatically. At the end of the installation process, the script will automatically run tests. If you see all test results report OK, it means the installation was successful

#### 2. Installation on Windows 10
(To be supplemented)

#### 3. Installation on Raspberry Pi
To install Docbao Crawler on Raspberry Pi, you need to ssh into the Raspberry PI terminal and log in with the admin account. The installation process will require the admin password. The specific installation steps are as follows.

##### Step 1: Clone the source code from github
Open terminal in Ubuntu (Ctr+Alt+T) and type the following command
~~~~
git clone http://github.com/hailoc12/docbao
~~~~

##### Step 2: Check the installation path again (SETTING.env) & set installation mode on Raspberry PI
If the installation folder is located at ~/docbao_crawler then you can skip this step. Otherwise, if you have placed the Docbao_crawler source code at a different path, then change the DOCBAO_BASE_DIR variable in the SETTING.env file

~~~
export DOCBAO_BASE_DIR=[path to the docbao_crawler folder]
~~~

Continue to change the value of the DOCBAO_RUN_ON_RASPBERRY environment variable to true
~~~
export DOCBAO_RUN_ON_RASPBERRY=true
~~~ 

Note: if you perform this step incorrectly, the installation steps will not run

##### Step 3: Run the installation file
Ensure you are in the docbao_crawler directory when running the following installation commands:
~~~~
source SETTINGS.env
bash install.sh
~~~~

The installer will run automatically. At the end of the installation process, the script will automatically run tests

. If you see all test results report OK, it means the installation was successful

### Operating Guide for the Scanner

#### Step 1: Set up scanning sources
*Docbao Crawler* stores all descriptive information about scanning sources in the file src/backend/input/config.yaml. By default, after installation, the config.yaml file will include 5 scanning sources: VnExpress, VOV, VTV, Dan Tri, Vietnamnet. The default scanning frequency is 15 minutes.

Directly editing the config.yaml file is not recommended. To change scanning settings, *Docbao Crawler* provides a configuration management tool. Run this tool as follows

~~~
bash tools/config_manager.sh

Then, go to "2.Manager program settings/Edit Crawl list" to add/remove scanning sources
~~~  

(documentation on using the config_manager tool is under development)

#### Step 2: Set up data channels
Currently, *Docbao Crawler* supports 4 forms of data storage:
1. Store data as a pickle file (src/backend/data) and export as json (src/backend/export). This is the default storage form
2. Pour data into Elasticsearch (configured via the SETTINGS.env file)
3. Pour data into RabbitMQ (configured via the SETTINGS.env file)
4. Push articles to WordPress (configured via the SETTINGS.env file)
5. Access data through an integrated API Server (documentation under construction)

Depending on your needs, set the corresponding configuration

#### Step 3: Run the first scan cycle to ensure the scanner works correctly
From the root directory, run the following command to start the scanning cycle

~~~
bash scripts/crawl.sh
~~~  

*Docbao Crawler* will start scanning news from the sources configured in the file src/backend/input/config.yaml (by default including VTV, VOV, Dan Tri, Vnexpress, Vietnamnet)

#### Step 4: Turn on the frontend to check the scanning results
*Docbao Crawler* integrates a simple frontend that can be used to view the articles that have been scanned. To turn on this frontend, use the following command

~~~
bash scripts/start_frontend.sh
~~~

By default, the frontend will run at https://localhost:8085. You can change the address and port where the frontend runs by changing the BAONOI_FRONTEND_HOST and BAONOI_FRONTEND_PORT parameters in the SETTINGS.env file.

If you see articles from the scanning sources appear on the frontend, it means the scanner is working successfully

#### Step 5: Set up crontab
To have *News Reader Crawler* automatically perform scanning cycles, you need to set up automatic running in crontab as follows

Open crontab
~~~
crontab -e
~~~

Add the two lines below
~~~
*/15 * * * * cd ~/docbao/ && bash scripts/crawl.sh
*/60 * * * * cd ~/docbao/ && bash scripts/clean_temp.sh
~~~

The setting above means that every 15 minutes, the scanner will conduct a scan and update the data once. You can change it to another time frame, but it should be > 10 minutes for the scanner to run stably.

### Reading Data from Docbao Crawler for Use

#### 1. Reading Data through RabbitMQ

##### a. Configuration to push data to RabbitMQ.

Use RabbitMQ when you want to process scanned data in real-time, or when multiple clients independently retrieve data from Docbao Crawler through exchange and queue binding mechanisms.

To push scanned data through RabbitMQ, you need to supplement RabbitMQ-related parameters in the SETTINGS.env file. For example:

~~~
export DOCBAO_EXPORT_TO_RABBITMQ=true
export DOCBAO_RABBITMQ_HOST=127.0.0.1
export DOCBAO_RABBITMQ_USERNAME='admin'
export DOCBAO_RABBITMQ_PASSWORD='password'
export DOCBAO_RABBITMQ_EXCHANGE='exchange_name'
export DOCBAO_RABBITMQ_DEFAULT_QUEUE='default_queue_name'
~~~  
Note, the 'admin' username needs to be set with admin rights and full permission to vhost /.

By default, any time Docbao Crawler scans a new article, Docbao Crawler will push the scanned data onto the exchange in DOCBAO_RABBITMQ_EXCHANGE and bind DOCBAO_RABBITMQ_DEFAULT_QUEUE to the exchange above. Clients can also bind additional queues to the exchange to "subscribe" to data from Docbao Crawler

##### b. Reading Data from RabbitMQ.
Docbao Crawler provides a boilerplate file for you to read data from RabbitMQ and push it into a database/process it as desired. This file is located at: /src/clients/get_data_from_rabbitmq.py. Refer to the file for usage instructions

##### c. Pushing all scanned data to RabbitMQ.
After configuring to push data to RabbitMQ, by default, Docbao Crawler will only

 push new data. If you want to push all the articles currently in the local database to RabbitMQ, you can run the following script:
~~~
bash scripts/push_all_articles_to_rabbitmq.sh
~~~

### Other Operations

#### 1. Reset Data
If you want to scan pages from scratch and not save any data, you can use the reset_data.sh script provided by *Docbao Crawler*. Use this script with the following command

~~~
bash scripts/reset_data.sh
~~~

#### 2. Fix UnicodeError when running Docbao Crawler on some servers
This error occurs because the server has not set the locale. To fix, run the following command

~~~
bash scripts/run_fix_env_encoding.sh  
~~~


### Development History

The project *"Đọc báo"* (or *Media Monitoring*) was developed in early 2018 when the author was still a journalist. The initial idea of the project was to gather news from multiple sources together, displayed on a simple interface to monitor news more quickly (hence the project's name *"Đọc báo"*). The first versions of this project operated in single-thread, ran on Raspberry Pi B, and scanned about 30 newspapers.

By the end of 2018, the author was encouraged by Mr. Vu Huu Tiep, a senior from the FYT Young Technology Talent Club (FPT), who was then the admin of the Basic Machine Learning Forum, to open-source the project. The project was officially open-sourced at https://github.com/hailoc12/docbao. The interface was also upgraded and officially operated at https://theodoibaochi.com (the website is still maintained to this day)

Since open-sourcing and receiving recognition from the community, the author has received feedback from many members of the Basic Machine Learning Forum. Especially Mr. Nguyen Ngoc Duy Luan (moderator of the Tinh Te forum) and Mr. Hung Thinh in Ho Chi Minh City sponsored VPS for the project to move from running on Raspberry Pi B to a real server (It was later that I found out the VPS sponsored by Mr. Hung Thinh was paid for out of his pocket at a cost of about 4 million VND/month!). Thanks to the VPS, the project was further developed by the author with new features (multi-threading, article content scanning, Facebook scanning, automatic trend analysis through keywords...). In February 2020, the author boldly presented the project and was awarded [Third Prize at the National Young Officials and Civil Servants Information Technology Contest 2019, held in An Giang](https://vietnamnet.vn/vn/thong-tin-truyen-thong/trao-giai-hoi-thi-tin-hoc-khoi-can-bo-cong-chuc-tre-toan-quoc-nam-2019-584527.html)

The recognition from the community helped the author to be more confident and determined to pursue a career as a professional programmer (a passion from childhood but was not pursued due to some reasons). In July 2019, thanks to the introduction of Mr. Le Cong Thanh, founder of Infore Technology JSC (First Prize winner of Vietnam Talent 2016, coincidentally the year I was a journalist in the VTV2 program team), the author officially switched to being a professional programmer and continued to develop the project source code to serve some commercial projects. During this process, the project continued to be improved in terms of features, stability, and is actually being used to collect hundreds of news sources every day for the commercial product *[VnAlert News Monitoring and Alert Service](https://vnalert.vn)*

As the author has learned a lot about programming through the project and received a lot of support from the community throughout the development process, the author always thinks that *News Reader Crawler* is a community project and should be "pay it forward" to return value to the community. Therefore, from August 2020, the project *News Reader Crawler* received a big update, was also refactored, supplemented documentation to aim not only to be a useful project for the data community in Vietnam but also an educational project, to help and inspire those who love programming in general and Python in particular to have an interesting project to practice. Hopefully, through this project, many people will have interesting ideas, and who knows, will receive the necessary "boost" to pursue the programming path as the author once did :d.

# Project Developers

1. Đặng Hải Lộc (hailoc12)
Email: danghailochp@gmail.com  
Facebook: https://www.facebook.com/danghailochp