# Consumer-Bitrate-RestAPI
This Project is to create a RestAPI which communicates with DPMI bitrate tool, pushes the bitrate data into influx db. This data can also be attached to Grafana for real time analysis
 ####################   README   ####################

 ....................DPMI BITRATE....................                                         

												                                                    
 About DPMI BITRATE:
	--> DPMI Bitrate is an application that monitors the bitrate traffic on different links and interfaces.
	--> By using interface and stream name as arguments, DPMI Bitrate calculates the bits per time sample on the interface.

 Functionalities of this REST API:
	--> Start a stream: Start a stream to monitor the bitrate traffic on the interface.
	--> Add streams: Add multiple streams to monitor the bitrate traffic on the interface.
	--> Change to another stream: Change to another stream to monitor the bitrate traffic on the interface.
	--> Delete streams: Delete multiple streams to monitor the rest of the bitrate traffic on the interface.
	--> Show running streams: Show all the streams currently monitoring the bitrate traffic on the interface.
	--> Stop the streams: Stop all the streams on the interface.
	--> Display graphs of the bitrate traffic in Grafana.

 1. SYSTEM REQUIREMENTS:

	a. automake
	b. autoconf
	c. pkg-config
	d. build-essential
	e. libtool
	f. libpcap-dev
	g. libmysqlclient-dev
	h. librrd-dev
	i. libqd-dev
	j. pip
	k. Influxdb 
	l. Influxdb python module
	m. Flask python module 
	n. Grafana

 2. INSTALLATION:

 Note: First login as root user in order to avoid 'Permission denied' interruptions, at any point of the installation process.

	a. Install 'automake', 'autoconf', 'pkg-config', 'build-essential', 'libtool', 'libpcap-dev', 'libmysqlclient-dev', 'librrd-dev', 'libqd-dev' packages from APT repository.
	
		Use 'sudo apt-get <package-name>'

	b. Install pip:
	
		sudo apt-get install python-pip python-dev
		sudo pip install --upgrade pip 
	
	c. Install Influxdb:	('https://portal.influxdata.com/downloads')
	
	   Installing on ubuntu:
	
		wget https://dl.influxdata.com/influxdb/releases/influxdb_1.3.6_amd64.deb
		sudo dpkg -i influxdb_1.3.6_amd64.deb
		sudo service influxdb start
	
	d. Install Influxdb python module:	(InfluxDB-Python)
	
		sudo pip install influxdb
	
	e. Install Flask python module:
	
		sudo pip install Flask
	
	f. Install Grafana:	('http://docs.grafana.org/installation/')
	
		Add the following line to your /etc/apt/sources.list file:
		deb https://packagecloud.io/grafana/stable/debian/ jessie main
	
		Then add the Package Cloud key from terminal:
		curl https://packagecloud.io/gpg.key | sudo apt-key add -

		Update and install grafana:
		sudo apt-get update
		sudo apt-get install grafana
	
		Start grafana server:
		sudo service grafana-server start
	
	
 3. DPMI REQUIREMENTS:

	a. mp (DPMI Measurement Point)
	b. libcap_utils (DPMI capture utilities)
	c. consumer-bitrate (Bitrate, packetrate, timescale consumers) 


	DPMI Installation:

		Download and install the mp, consumer-bitrate, libcap_utils repositories from 'https://github.com/DPMI' or git clone them from terminal as below:

 Note: First login as root user in order to avoid 'Permission denied' interruptions, at any point of the installation process.

	Clone 'mp' from git:
		git clone https://github.com/DPMI/mp.git
		cd mp
		autoreconf -si
		mkdir build && cd build 
		../configure
		make && make install


   	Clone 'libcap_utils' from git:
		git clone https://github.com/DPMI/libcap_utils.git
		cd libcap_utils
		autoreconf -si
		mkdir build && cd build

		../configure
		make && make install


   	Clone 'consumer-bitrate' from git:
		git clone https://github.com/DPMI/consumer-bitrate.git
		cd consumer-bitrate
		make

 ***********************************************************************************
 Note: After all the installations to get the api working on your localhost, do the below changes:
	a. After installing all the requirements, create an influxdb database manually using following commands:
		In terminal,
			influx
			create database <database name> 
		You can verify if it is created or not using "show databases"
	
	b. In file "bitrate_db.py", Replace the 'db' variable to the influx database name created above, where the bitrate traffic is to be stored.
	c. In file "bitrate_api.py", Replace the 'interface' variable to the interface on which the bitrate traffic is to be monitored.
	d. In file "bitrate_api.py", Replace the 'directory' variable to the path where consumer-bitrate is installed.
	e. In file "bitrate_api.py", Replace the 'password' variable to the system root login password to access root privilages.
	f. Please try not to change the file name of "bitrate_db", if changed you have to replace the changed name of the file in the "bitrate_api.py" file on line 4 which shows "from bitrate_db import influx".
	g. Add the influx database given above to the datasource in Grafana, to display the traffic.
	h. Create a dashboard with created datasource, Use appropriate tags (automatically created) in the grafana query to view the requried streams.
	i. Use 'distinct' aggregation in the 'select' column of the query in the grafana dashboard to view the graph.

 ***********************************************************************************




 4. RESTful API:

	The server runs on the default port 5000. The clients can access the server from terminal (using CURL) or the web browser.


 **********************************************************************************
 Note:	
	a. Ensure that you run the "bitrate_api.py" file, before using the service (by curl).
 
	b. While adding/deleting multiple streams, seperate the streams using ","

		Ex: "curl http://localhost:5000/addstream/01::01,01::02,01::03"
	    	    "curl http://localhost:5000/deletestream/01::01,01::02,01::03"		
 **********************************************************************************

	The services provided by the server:
	
	a. Start stream:	[ Used to start only one stream, use 'addstream' to start multiple streams ]

		curl http://localhost:5000/startstream/<stream>


	b. Add stream:		[ Used to add single/multiple streams ]

		curl http://localhost:5000/addstream/<streams>


	c. Change stream: 	[ Used to change to one stream ]

		curl http://localhost:5000/changestream/<stream>


	d. Delete stream:	[ Used to delete single/multiple streams ]

		curl http://localhost:5000/deletestream/<streams>


	e. Show stream:		[ Used to show all the running streams ]

		curl http://localhost:5000/showstream


	f. stop service:	[ Used to stop all running streams at once]

		curl http://localhost:5000/stop





 ....................DPMI BITRATE....................                                         



