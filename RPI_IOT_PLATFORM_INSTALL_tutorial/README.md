# Setup your software to run an IOT Platform #

# Setup #
1. laptop on wifi
2. dietpi on RPI on wifi ( See RPI_BOOT_WIFI_tutorial if not done)

# Procedure

1. SSH to the RPI using ssh root@[PI_IPADDRESS]
2. Install software on the PI using   dietpi-software command
   1. search for mqtt and select 123 MQTT message broker install by hitting the spacebar and selecting ok
   1. search for node-red and select "122  Node-RED: tool for wiring devices, APIs and online services" install by hitting the spacebar and selecting ok
   1. search for postgress and  select "194  PostgreSQL: Persistent advanced object-relational database server" install by hitting the spacebar and selecting ok
   1. Move curser to Install and tab to ok 
   1. Once you hit enter it should indicate:
> DietPi is now ready to install your software choices:                        

>  - Node-RED: tool for wiring devices, APIs and online services               

>  - Mosquitto: MQTT messaging broker                                   

>  - PostgreSQL: Persistent advanced object-relational database server  

   1. tab to OK and start the install
   1. it will take a while to install ( I measured 11 minutes at home on the wifi)
   1. It should show the all the services restarted
3. reboot the pi on the command line using reboot command and ssh in again.

### Postgres ###

Info on how to setup postgres on pi taken from
https://opensource.com/article/17/10/set-postgres-database-your-raspberry-pi
Please note we already installed postgres so start at configuing it

1. run on your pi ssh session

> sudo su postgres

2. Create a user with :
> createuser pi -P --interactive
When prompted, enter a password (and remember what it is), select n for superuser, and y for the next two questions.

3. Now connect to Postgres using the shell and create a test database:
> psql
>  > create database test;

4. Exit from the psql shell and again from the Postgres user by pressing Ctrl+D twice, and you'll be logged in as the pi user again. Since you created a Postgres user called pi, you can access the Postgres shell from here with no credentials:

> sudo su postgres
> psql test
> create table people (name text, company text);

5. Now add data

> insert into people values ('Ben Nuttall', 'Raspberry Pi Foundation');
> insert into people values ('Rikki Endsley', 'Red Hat');

6. test that the data persists:

> select * from people;

7. result should show:
     name      |         company         
---------------+-------------------------
 Ben Nuttall   | Raspberry Pi Foundation
 Rikki Endsley | Red Hat

8. There is an issue in making postgres ready for node red
- https://stackoverflow.com/questions/29712228/node-postgres-get-error-connect-econnrefused

  1. Need to copy the pg_hba.conf into the configuration directory for postgresql
  In my install I found it in /etc/postgresql/13/main 
  I put a copy in our elearning site for your use

  2. Need to run plsql on your pi an alter the listen_address settings using:

>  sudo su postgres

>  psql test

>  ALTER SYSTEM SET LISTEN_ADDRESSES = '*';

  This will allow your postgres to recieve ip traffic.  Careful the system is no
  longer secure since this allows anyone to connect.

  3. restart the service by exiting back to the root console line
     by hitting ^D twice.
     Then run
> dietpi-services restart postgresql
  4. Check the log to see that the service started up tcp services
  I used:
> cat /var/log/postgresql/postgresql-13-main.log
Log contains
>  2021-09-30 15:02:34.875 EDT [2412] LOG:  starting PostgreSQL 13.3 (Debian 13.3-1) on aarch64-unknown-linux-gnu, compiled by gcc (Debian 10.2.1-6) 10.2.1 20210110, 64-bit
> 2021-09-30 15:02:34.876 EDT [2412] LOG:  listening on IPv4 address "0.0.0.0", port 5432
> 2021-09-30 15:02:34.876 EDT [2412] LOG:  listening on IPv6 address "::", port 5432
> 2021-09-30 15:02:34.884 EDT [2412] LOG:  listening on Unix socket "/run/postgresql/.s.PGSQL.5432"
> 2021-09-30 15:02:34.911 EDT [2415] LOG:  database system was shut down at 2021-09-30 15:02:24 EDT
> 2021-09-30 15:02:34.949 EDT [2412] LOG:  database system is ready to accept connections
  

 
### NODE-RED ###

Info on using node red on the pi is found on the nodered.org website: https://nodered.org/docs/getting-started/raspberrypi
You can ignore the installation part since it was done via the dietpi-software interface

1.   Connect to node-red to see that it is running using it RPI Static IPaddr
   http://{YOUR_RPI_IPADDRESS}:1880
1. Install node-red-contrib-re-postgres  in your ssh command line using:
npm install node-red-contrib-re-postgres
2. Restart node-red with the command
> dietpi-services restart node-red
General services info found in https://github.com/MichaIng/DietPi/issues/1114
3. Go to Manage Pallet and install  node-red-contrib-postgresql
4. Install should note that it is working
5. pull a postgres node into a flow
6. click on the node and configure a database instance
7. Be sure to label it and set Recieve query output? and Return message on error on by enabling checkboxes and save
8. Pull in a trigger and template node and debug node and wire
   trigger->template->Postgres->debug
9. I put a flow in the elearning site to try out.


### integrating MQTT ###

I am leaving that for you today.


