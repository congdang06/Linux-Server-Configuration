<html>
<head>
    <title>Linux Server Configuration</title>
</head>
<body>

<h1 style="text-transform:capitalize; color: #010806;">Project Description </h1>
<p>You will take a baseline installation of a Linux server and prepare it to host your web applications. You will secure your server from a number of attack vectors, install and configure a database server, and deploy one of your existing web applications onto it.</p>

<h1 style="text-transform:capitalize; color: #010806;"><b style="text-transform: uppercase;">step 1: </b> Create account at AWS Lightsail (Virtual Server) </h1>

    <ol start="1">
        <li>Go to <a href="https://aws.amazon.com/lightsail/">AWS Lightsail</a></li>
        <li><b>Log in</b></li>
        <p>First, log in to Lightsail. If you don't already have an Amazon Web Services account, you'll be prompted to create one.</p>
        <img src="picture/create_instance.png" width="600" height="200" alt="AWS login page" >
        <li><b>Create an instance</b></li>
        <p>Once you're logged in, Lightsail will give you a friendly message with a robot on it, prompting you to create an instance. A Lightsail instance is a Linux server running on a virtual machine inside an Amazon datacenter.</p>
        <img src='picture/no_instance.png' width="600", heigh="200" alt="no instance">
        <li><b>Choose an instance image: Ubuntu</b></li>
        <p>Lightsail supports a lot of different instance types. An instance image is a particular software setup, including an operating system and optionally built-in applications.</p>

        <p>For this project, you'll want a plain Ubuntu Linux image. There are two settings to make here. First, choose "OS Only" (rather than "Apps + OS"). Second, choose Ubuntu as the operating system.</p>
        <img src='picture/pick_ubuntu.png' width="600", heigh="200" alt="pick ubuntu">
        <li><b>Choose your instance plan</b></li>
        <p>The instance plan controls how powerful of a server you get. It also controls how much money they want to charge you. For this project, the lowest tier of instance is just fine. And as long as you complete the project within a month and shut your instance down, the price will be zero.</p>
        <img src='picture/choose_plan.png' width="600", heigh="200" alt="choose plan">
        <li><b>Give your instance a hostname</b></li>
        <li>Every instance needs a unique hostname. You can use any name you like, as long as it doesn't have spaces or unusual characters in it. Your instance's name will be visible to you and to the project reviewer.</li>
        <img src='picture/host_name.png' width="600", heigh="200" alt="hostname">
        <li><b>Wait for it to start up</b></li>
        <p>It may take a few minutes for your instance to start up.</p>
        <img src='picture/start_up.png' width="600", heigh="200" alt="startup">
        <li><b>Running</b></li>
        <p>Once your instance has started up, you can log into it with SSH from your browser.</p>

        <p>The public IP address of the instance is displayed along with its name. In the above picture it's 54.84.49.254.</p>

        <p>Note: When you set up OAuth for your application, you will need a DNS name that refers to your instance's IP address. You can use the xip.io service to get one; this is a public service offered for free by Basecamp. For instance, the DNS name 54.84.49.254.xip.io refers to the server above.</p>
        <img src='picture/connect_ubuntu.png' width="600", heigh="200" alt="startup">
    </ol>
</p>

<hr border: 15px; border-style: dashed;>

<h1 style="text-transform:capitalize; color: #010806;"><b style="text-transform: uppercase;">step 2: </b> AWS Lightsail download the private key to local machine </h1>
    
    <ol start="1">
        <li>On the top right corner, click the <i>Account</i></li>
        <li>Select SSH keys and click <b><i>Create New</i></b></li>
        <img src='picture/ssh_private_key.png' width="600", heigh="200" alt="ssh private key">
        <li>Download the private key which have <b>.pem</b> extension</li>
        <li>On the local machine, move this private key to ~/.ssh with the new name <b>udacity_key.rsa</b>
            <p>cp ~/Downloads/lightsail_private_key.pem ~/.ssh/udacity_key.rsa</p>
            <p>ls ~/.ssh</p>
        </li>
        <li>Change the permission of udacity_key.rsa to prevent any users are able to modify it
            <p>chmod 600 ~/.ssh/udacity_key.rsa </p>
            <p>ls -la ~/.ssh/udacity_key.rsa </p>
        </li>
        <li>Testing the connection from <b>Local Machine</b> to <b>AWS lightsaile</b>
            ssh -i ~/.ssh/udacity_key.rsa ubuntu@34.219.239.192 where <code style="background-color:#9e9e9e4f">34.219.239.192</code> is IP address

            <p>You will receive the warning "Are you sure you want to continue connecting(yes/no)?" Answer <b>"Yes"</b></p>

            <p>If you recieve this error warning <b>Permission Denied (publickey)</b> or you are able to connect but it connects thru the default port 22 </p>
            <img src="picture/first_log_in.png" width="700" height="400">
            <p> Please follow the <b>Step 3</b> to finish the configuration</p>
        </li>
    </ol>
<hr border: 15px; border-style: dashed;>

    <h1 style="text-transform:capitalize; color: #010806;"><b style="text-transform: uppercase;">step 3: </b> AUpdate UBUNTU security, change port, configure UFW <b>(Connect to SSH on the virtual server AWS Lightsail) </b> </h1>

    <ol start="1">
    <li><h3>Secure ubuntu server</h3></li>
        <ol start="1">
            <li>sudo apt-get update</li>
            <li>sudo apt-get upgrade</li>
            <li>sudo apt autoremove </li>
            <li>sudo apt-get install finger</li>
        </ol><br>

    <li>Edit <b><code style="background-color:#9e9e9e4f">sshd_config file</code></b></li>
        <ol start="1">
              <li><b>sudo nano /etc/ssh/sshd_config</b> change port 22 to 2200 to prevent attacks on the default port</li><br>
            <img src='picture/change_port.png' width="600" height="250">
            <br>
            <li>restart ssh</li>
                <ul><li> sudo service ssh restart </li></ul><br>

            <li><b>Configure the Uncomplicated Firewall (UFW) to allow only the incoming connection for SSH (port 2200), HTTP (port 80), and NTP (port 123)</b>
                <ul>
                    <li style="padding-bottom: 5px;">sudo ufw status</li>
                    <li style="padding-bottom: 5px;">sudo ufw default deny incoming</li>
                    <li style="padding-bottom: 5px;">sudo ufw default allow outgoing</li>
                    <li style="padding-bottom: 5px;">sudo ufw allow ssh</li>
                    <li style="padding-bottom: 5px;">sudo ufw allow 2200/tcp</li>
                    <li style="padding-bottom: 5px;">sudo ufw allow 80/tcp </li>
                    <li style="padding-bottom: 5px;">sudo ufw allow 123/udp</li>
                    <li style="padding-bottom: 5px;">sudo ufw deny 22 </li>
                    <li style="padding-bottom: 5px;">sudo ufw enable </li>
                        <ul><li>Command may discrupt existing ssh connections. Proceed with operation (y/n)? <b> y </b>
                            <li>Firewall is active and enabled on system startup</li>
                        </ul>
                    <li style="padding-bottom: 10px;">sudo ufw status</li>
                </ul>
           <li>Back AWS lightsail => Networking tab</li>
                <p>add more rule on the firewall</p>
                <img src="picture/change_firewall.png" width="500" height="200">
          
        </ol>
        <p><b>Note:</b>From the local machine (terminal), execute <b>ssh -i ~/.ssh/udacity_key.rsa -p 2200 ubuntu@34.219.239.192</b> where <b> <code style="background-color:#9e9e9e4f; font-size: 16px">34.221.39.98</code></b> is the public IP address</p>
        <img src="picture/be_log_in.png" width="800" height="400">
    </ol>
<hr border: 15px; border-style: dashed;>


    <h1 style="text-transform:capitalize; color: #010806;"><b style="text-transform: uppercase;">step 4: </b> Prevent hacker to attack your network. Use <b> Fail2Ban (protected computer servers from brute-force attacks</b></h1>

    <ol start="1">
        <li> Install Fail2Ban</li>
            <ul><li style="padding-bottom: 10px; padding-top:5px"> sudo apt-get install fail2ban</li></ul>

        <li>Install sendmail for email notice</li>
            <ul><li style="padding-bottom: 10px; padding-top:5px">sudo apt-get install sendmail iptables-persistent </li></ul>

        <li>Copy of the file</li>
            <ul><li style="padding-bottom: 10px; padding-top:5px">sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local</li></ul>
        <li>modify the jail.local file</li>
            <ul>
                <li style="padding-bottom: 10px; padding-top:5px">sudo nano /etc/fail2ban/jail.local </li>
                <li style="padding-bottom: 10px; padding-top:5px">searching and replace the setting as below
                    <p>set bantime=600</p>
                    <p>destemail = useremail@domain (change it to your personal email address)</p>
                    <img src="picture/destemail.png" width=500 height=200 >
                    <p>action= %(action_mwl)s</p>
                    <img src="picture/action.png" width=600 height=200 >
                </li>
                <li>Searching [sshd], change <b>port= ssh</b> by <b>port = 2200 </b></li> 
                <img src="picture/sshd.png" width=300 height=200 >
            </ul>
        <li>Restart the service </li>
            <ul><li style="padding-bottom: 10px; padding-top:5px">sudo service fail2ban restart</li></ul>
    </ol>

    <h1 style="text-transform:capitalize; color: #010806;"><b style="text-transform: uppercase;">step 4.1: </b> Configure the automatically software update</h1>

    <ol start="1">
        <li>Enabled automatic security update by <b>unattended-upgrades</b></li>
            <ul><li style="padding-bottom: 10px; padding-top:5px">sudo apt-get install unattended-upgrades</li></ul>
        <li>Modify the unattended-upgrades</li>
            <ul><li style="padding-bottom: 10px; padding-top:5px">sudo nano /etc/apt/apt.conf.d/50unattended-upgrades</li></ul>
            <uL>
                <li>uncomment the line: 
                     "${distro_id}:${distro_codename}-updates";</li><br>
                     <img src="picture/uncomment.png" width="800" height="400">
            </uL><br>

        <li>Modify auto-grade as follow</li>
            <ul>
                <li>sudo nano /etc/apt/apt.conf.d/20auto-upgrades </li>
                    <ul><li style="padding-bottom: 10px; padding-top:5px">APT::Periodic::AutocleanInterval "7";</li>
                        <li style="padding-bottom: 10px; padding-top:5px">APT::Periodic::Unattended-Upgrade "1";</li>
                    </ul>
                <img src="picture/apt_upgrade.png" width="500" height="200">
            </ul>
        <li>Enabled the unattended-upgrades</li>
            <ul><li style="padding-bottom: 10px; padding-top:5px; list-style-type: 'circle';">sudo dpkg-reconfigure --priority=low unattended-upgrades</li></ul>
       <!--  <li>Restart Apache</li>
            <ul><li>sudo service apache2 restart</li></ul> -->

     <h1 style="text-transform:capitalize; color: #010806;"><b style="text-transform: uppercase;">step 4.2: </b> update latest software</h1>

    <ol start="1">
        <li>Follow these command to update the most recent</li>
            <ul><li style="padding-bottom: 10px; padding-top:5px">sudo apt-get update</li>
                <li style="padding-bottom: 10px; padding-top:5px">sudo apt-get dist-upgrade</li>
                <li style="padding-bottom: 10px; padding-top:5px">sudo shutdown -r now </li>
            </ul>

        <li>Disconnect from host and logged back in <b> ssh -i ~/.ssh/udacity_key.rsa -p 2200 ubuntu@34.219.239.192</b></li><br>
        <img src="picture/logged_in.png" width="550" height=250>
    </ol>
<hr border: 15px; border-style: dashed;>

<h1>STEP 5: Create <b><code style="background-color:#9e9e9e4f; font-size: 40px">grader</code></b> username </h1>
    <ol start="1">
        <li>Log in as Ubuntu </li>
            <ul>
                <li>Add new <b>"grader"</b> username</li>
                    <ul>
                        <li>sudo adduser grader</li>
                        <img src="picture/grader.png" width=500 height=250>
                    </ul>
            </ul>
    </ol>

    <h2>Provide <b>grader</b> access permission</h2>
    <ol start="1">
        <li>Modify the visudo</li>
            <ul><li>sudo visudo</li>
                <img src="picture/visudo.png" width=700 height=400 >
            </ul><br>

        <li>Search for this info:</li>
            <ul><li style="padding-bottom: 10px; padding-top:10px">root  ALL=(ALL:ALL) ALL</li></ul>

        <li>Add grader below root</li>
            <ul><li style="padding-bottom: 10px; padding-top:5px">grader ALL=(ALL:ALL) ALL</li>
                <img src="picture/add_grader.png" width=400 height=200>
            </ul><br>
        <li>Verify <b><code style="background-color:#9e9e9e4f; font-size: 16px">grader</code></b> access permission</li>
            <ul>
                <li>su - grader</li>
                    <ul><li>password confirm</li></ul>
                <li>sudo -l </li>
                    <ul><li>password confirm</li></ul>
                <img src="picture/check_grader.png" width=600 height=400>
            </ul>

    </ol>
<hr border: 15px; border-style: dashed;>

    
    <h1 style="text-transform:capitalize; color: #010806;"><b style="text-transform: uppercase;">step 6: </b> Using <b>ssh-keygen</b> to create SSH key pair</h1>

    <ol start="1">
        <li><b>On terminal (Local machine).</b></li>
        <ul>
            <li>run <b>ssh-keygen</b></li>
            <li>Enter file in which to save the key (/Users/ownername/.ssh/id_rsa): save as <code style="background-color:#9e9e9e4f; font-size: 18px"> /Users/ownername/.ssh/grader_udacity_key</code>. Check save file at <b>ls ~/.ssh</b></li>
            <li>Two new filename created (grader_udacity_key and grader_udacity_key.pub) </li>
            <li>Check <b>grader_udacity_key.pub</b></li>
                <ul><li style="padding-bottom: 10px; padding-top:10px">cat ~/.ssh/grader_udacity_key.pub</li>
                    <li>Copy the KEY </li>
                </ul>

            <li><b>Connect to virtual server (AWS lightsail)</b></li>
        </ul><br>

        <li>Connect to the host (virtual server AWS lightsail)</li>
            <ul>
                <li>Login as <code style="background-color:#9e9e9e4f; font-size: 18px">grader su - grader</code></li>
                <li>Create <b>.ssh</b> new directory</li>
                    <ul><li>mkdir .ssh (~/.ssh)</li></ul>
                <li>Paste the <b>grader_udacity_key</b> at local machine to virtual server</li>
                    <ul>
                        <li>sudo nano ~/.ssh/authorized_keys</li>
                        <img src="picture/auto_key.png" width="500" height=100>
                        <li>copy the content and paste to the authorize_key</li>
                            <ul><li>cat ~/.ssh/authorized_keys </li></ul>
                    </ul>
                <li>Prevent user change permssion</li>
                    <ul>
                        <li>sudo chmod 700 .ssh</li>
                        <li>sudo chmod 644 .ssh/authorized_keys </li>
                    </ul>
                <li>Restart Service</li>
                    <ul><li>sudo service ssh restart</li>
                        <li>exit</li>
                        <li>exit</li>
                    </ul>
            </ul>

        <li>Back to the local machine (terminal)</li>
            <ul><li>excecute</li>
                <ul>
                    <li>ssh -i ~/.ssh/grader_udacity_key -p 2200 grader@34.219.239.192</li><br>
                    <img src="picture/test_grader.png" width=600 height=400>
                </ul>
            </ul>
    </ol>
<hr border: 15px; border-style: dashed;>

    <h1 style="text-transform:capitalize; color: #010806;"><b style="text-transform: uppercase;">step 7: </b> Configure UTC local time zone</h1>

    <ul>
        <li>Log in as <code style="background-color:#9e9e9e4f; font-size: 18px">grader su - grader</code></li>
        <uL><li tyle="padding-bottom: 10px; padding-top:10px">Time Zone configuration</li>
                <ul><li>sudo dpkg-reconfigure tzdata </li><br>
            <img src="picture/time_zone.png" width=600 height=200></ul>
        </uL>
    </ul>
<hr border: 15px; border-style: dashed;>


    <h1 style="text-transform:capitalize; color: #010806;"><b style="text-transform: uppercase;">step 8: </b> apache installation and configuration</h1>
    <ol start="1">
        <li>Login as <code style="background-color: #9e9e9e4f">grader su - grader</code></li>
        <ul>
            <li>Apache installation command</li>
                <ul><li style="padding-bottom: 10px; padding-top:10px">sudo apt-get install apache2</li></ul>
            <li>Testing Apache after installation, open the browser and paste the AWS lightsail public IP address <code style="background-color: #9e9e9e4f">http://34.221.39.98/</code></li>
            <img src="picture/apache_test.png" width=600 height=200>
        </ul><br>

        <li>Install mod_wsgi package for python3</li>
        <ul><li>sudo apt-get install libapache2-mod-wsgi-py3</li>
            <img src="picture/mod_wsgi.png" width=500 height=200>
        </ul><br>
        <li>Should be <code style="background-color:#9e9e9e4f">mod_wsgi</code> enabled</li>
            <ul><li>sudo a2enmod wsgi</li>
                <img src="picture/enable_mod.png" width=600 height=150>
            </ul>
        </ul>

        <li>Create the virtual environment</li>
            <ul><li>sudo apt-get install python-pip</li>
                <li>sudo pip install virtualenv</li>
                <li>sudo virtualenv venv</li>
                    <ul><li>source venv/bin/activate</li>
                        <li>sudo pip install Flask</li>
                        <li style="font-style: bold">Testing (python __init__.py) and if you see the display "Running on http://127.0.0.1:5000/". Then you are successfully configure the app</li>
                        <li>Exit .venv =>  <code style="background-color: #9e9e9e4f; font-size: 16px;">deactivate </code></li>
                    </ul>
            </ul>
    </ol>
<hr border: 15px; border-style: dashed;>


     <h1 style="text-transform:capitalize; color: #010806;"><b style="text-transform: uppercase;">step 9: </b> Git Instalation</h1>

    <ul>
        <li>Log in as <code style="background-color:#9e9e9e4f; font-size: 16px">grader su - grader</code></li><br>
            <ul><li style="padding-bottom: 10px;">sudo apt-get install git</li></ul>
        <img src="picture/git_install.png" width=600 height=250>
    </ul>
<hr border: 15px; border-style: dashed;>


<h1 style="text-transform:capitalize; color: #010806;"><b style="text-transform: uppercase;">step 10: </b> Item Catalog => setup and clone from Github Repository </h1>
<ol start="1">
    <li>Log in as <code style="background-color:#9e9e9e4f; font-size: 16px">grader su - grader</code></li>
        <ul>
            <li  style="padding-bottom: 10px;">sudo mkdir /var/www/catalog</li>
            <li  style="padding-bottom: 10px;">ls /var/www/</li>
        </ul>
    <img src="picture/mkdir_catalog.png" width=500 height=200><br>
    <li style="padding-top: 10px;">clone <code style="background-color:#9e9e9e4f; font-size: 18px;">Item Catalog </code>from Github
        <ul>
            <li style="padding-bottom: 5px;">sudo git clone https://github.com/congdang06/ItemCatalog.git /var/www/catalog/catalog</li>
            <li style="padding-bottom: 5px;">ls /var/www/catalog/</li>
            <li style="padding-bottom: 5px;">cd /var/www</li>
            <li style="padding-bottom: 5px;">sudo chown -R grader:grader catalog/</li>
            <li style="padding-bottom: 5px;">cd /var/www</li>
            <li style="padding-bottom: 5px;">mv itemCatalogApplication.py __init__.py</li>
            <img src="picture/git_clone.png" width=600 height=300>
        </ul>

    <li style="padding-top: 20px;">Open <code style="background-color:#9e9e9e4f; font-size: 18px;">__init__.py</code></li>
        <ul><li style="padding-bottom: 5px; padding-top: 5px;">sudo nano __init__.py</li>
            <ul><li>Comment <code style="background-color:#9e9e9e4f; font-size: 16px;">app.run(host='0.0.0.0', port=5000)</code></li></ul>
        </ul><br>
        <img src="picture/init.png" width=600 height=300>
    <li style="margin-top: 10px; margin-bottom: 10px;">Open database_setup.py</li>
            <ul>
                <li style="padding-bottom: 5px;">sudo nano database_setup.py</li>
                <li>Comment <code style="background-color:#9e9e9e4f; font-size: 16px;">engine = create_engine('sqlite:///itemcatalogwithloginusers.db')</code></li>
                <li style="padding-top: 10px;">Enter <code style="background-color:#9e9e9e4f; font-size: 16px;">engine = create_engine('postgresql://catalog:catalog@localhost/catalog')</code></li>
            </ul><br>
        <img src="picture/db_setup.png" width=700 height=150>
    <li>Open __init__.py (sudo nano __init__.py)</li>
        <ul><li>change engine to <code style="background-color:#9e9e9e4f; font-size: 16px;">engine = create_engine('postgresql://catalog:catalog@localhost/catalog')</code></li></ul>
    <li>Open insert_testing_db.py</li>
        <ul><li>change engine to <code style="background-color:#9e9e9e4f; font-size: 16px;">engine = create_engine('postgresql://catalog:catalog@localhost/catalog')</code></li></ul>
</ol>
<hr border: 15px; border-style: dashed;>



<h1 style="text-transform:capitalize; color: #010806;"><b style="text-transform: uppercase;">step 11: </b> setting up the virtual Host </h1>
<ol start="1">
    <li>Log in as <code style="background-color:#9e9e9e4f; font-size: 16px">grader su - grader</code></li>
    <li>Create a new catalog.conf</li>
        <ul><li style="padding-bottom: 5px; padding-top: 5px;">sudo nano /etc/apache2/sites-available/catalog.conf</li>
            <li><img src="picture/conf.png" width=700 height=500></li>
            <li style="padding-bottom: 5px; padding-top: 5px;">Enable: <code style="background-color:#9e9e9e4f; font-size: 16px">sudo a2ensite catalog </code></li>
            <li><img src="picture/e_host.png" width=700 height=200></li>
            <li style="padding-bottom: 5px; padding-top: 5px;">Reload Apache2 service: <code style="background-color:#9e9e9e4f; font-size: 16px">sudo service apache2 reload</code></li>
        </ul>
    <li style="font-style: bold; text-transform: capitalize;">create catalog.wsgi</li>
        <ul><li>go to <code style="background-color:#9e9e9e4f; font-size: 16px">cd /var/wwww/catalog</code></li>
            <li>sudo nano catalog.wsgi</li>
            <img src="picture/cat_wsgi.png" width=700 height=400>
            <li style="padding-bottom: 5px; padding-top: 5px;">Reload Apache2 service: <code style="background-color:#9e9e9e4f; font-size: 16px">sudo service apache2 reload</code></li>
</ul>
<hr border: 15px; border-style: dashed;>


    <h1 style="text-transform:capitalize; color: #010806;"><b style="text-transform: uppercase;">step 12: </b> postgreSQL installation and configuration</h1>

    <ol start="1">
        <li>Still log in as <code style="background-color:#9e9e9e4f; font-size: 16px;">grader su - grader password: grader</code></li>
        <ul><li>PostgreSQL installation</li>
            <ul>
                <li style="padding-bottom: 10px; padding-top:10px">sudo apt-get install postgresql</li>
                <li style="padding-bottom: 10px; padding-top:10px">sudo apt-get install python-psycopg2</li>
                <li style="padding-bottom: 10px; padding-top:10px">sudo apt-get install postgresql postgresql-contrib</li>
            </ul>
        </ul>
        <li>Switch to <code style="background-color:#9e9e9e4f; font-size: 18px;">Postgres</code></li>
            <ul><li style="padding-bottom: 10px; padding-top:10px"> sudo su - postgres</li></ul>
        <li>use the command <code style="background-color:#9e9e9e4f">psql</code> to open PostgreSQL terminal</li>
        <img src="picture/postgresql.png" width=600 height=150>
        <li>Create <code style="background-color:#9e9e9e4f">catalog</code> role and allow user to create the database</li>
        <img src="picture/create_catalog.png" width=600 height=200>
        <li> CREATE DATABASE catalog WITH OWNER catalog;</li>
            <ul><li> \c catalog</li>
                <li>REVOKE ALL ON SCHEMA public FROM public;</li>
                <li>GRANT ALL ON SCHEMA public TO catalog;</li>
            </ul>

        <li>exit \q</li>
        <li>Back to grader </li>
            <ul><li>Run python database_setup.py</li>
                <li>python insert_testing.py</li>
                <li>use the command <code style="background-color:#9e9e9e4f">psql</code> to open PostgreSQL terminal</li>
                <li> \c catalog</li>
                <li>\dt</li>
                <li>select * from [tablename];</li>
                <li>drop table [tablename];
                <li>exit \q</li>
                <li>exit</li>
            </ul>
        <li>Restart postgresql</li>
            <ul><li>sudo service postgresql restart</li></ul>

        <li>Back to the <code style="background-color:#9e9e9e4f">grade</code> terminal <code style="background-color:#9e9e9e4f">exit</code></li>
        <li>Create <code style="background-color:#9e9e9e4f">catalog</code> as we did for the <code style="background-color:#9e9e9e4f">grader</code></li>
            <ul>
                <li>sudo adduser catalog<li>
                <img src="picture/catalog_linux.png" width=400 height=200>
            
                <li>Run <code style="background-color:#9e9e9e4f">sudo visudo</code> and add <code style="background-color:#9e9e9e4f">catalog ALL=(ALL:ALL) ALL</code> as image below</li>
                <img src="picture/catalog_config.png" width=600 height=200>
            </li>
        <li>Verify catalog permission as image below</li>
        <img src="picture/verify_catalog.png" width=500 height=250>
        <li>Log in as <code style="background-color:#9e9e9e4f">catalog</code> and follow step below </li>
        <img src="picture/in_catalog.png" width=600 height=250>
        <li>\q</li>
        <li>exit</li>
        <li>Restart postgresql</li>
            <ul><li>sudo service postgresql restart</li></ul>
    </li></ul></ol>

<hr border: 15px; border-style: dashed;>

    <h1 style="text-transform:capitalize; color: #010806 ;"><b style="text-transform: uppercase;">step 13: </b> Install Other Package and Dependencies </h1>
    <ul>
    <li>Log in as <code style="background-color:#9e9e9e4f">grader</code>
        <ul><li>sudo apt-get install python3-pip</li>
            <li>sudo apt-get install python-virtualenv</li>
        </ul>
    <li>cd /var/www/catalog/</li>
        <ul><li>sudo virtualenv -p python3 venv3</li>
            <li>sudo chown -R grader:grader venv3/</li>
            <li>. venv3/bin/activate</li>
                <ul><li>Add more dependencies</li>
                    <li>pip install httplib2</li>
                    <li>pip install requests</li>
                    <li>pip install --upgrade oauth2client</li>
                    <li>pip install sqlalchemy</li>
                    <li>pip install flask</li>
                    <li>sudo apt-get install libpq-dev</li>
                    <li>pip install psycopg2</li>
                    <li>Run <code style="background-color:#9e9e9e4f">python3 __init__.py</code></li>
                    <img src="picture/init_mode.png" width=500 height=250>
                    <li><code style="background-color:#9e9e9e4f">deactivate</code> to exit virtual environment</li>
                </ul>
            <li>sudo apt-get install python-pip</li>
            <li>source venv/bin/activate</li>
            <li>pip install httplib2</li>
            <li>pip install requests</li>
            <li>sudo pip install --upgrade oauth2client</li>
            <li>sudo pip install sqlalchemy</li>
            <li>pip install Flask-SQLAlchemy</li>
            <li>sudo pip install flask-seasurf</li>
            <li>sudo apt-get install python-requests
        </ul>
</ul>
<hr border: 15px; border-style: dashed;>


    <h1 style="text-transform:capitalize; color: #010806 ;"><b style="text-transform: uppercase;">step 14: </b> run an application </h1>

    <ul>
        <li>still in <code style="background-color:#9e9e9e4f">cd /var/wwww/catalog</code></li>
            <ul><li style="padding-bottom: 10px; padding-top:10px"><code style="background-color:#9e9e9e4f">sudo chown -R www-data:www-data catalog/</code></li>
                <li>sudo service apache2 restart</li>
            </ul>
        <li>Testing the web browser</li>
            <ul><li style="padding-bottom: 10px; padding-top:10px"><code style="background-color:#9e9e9e4f"><a href="http://34.219.239.192/">http://34.219.239.192/</a></code> or <code style="background-color:#9e9e9e4f"><a href="http://ec2-34-219-239-192.us-west-2.compute.amazonaws.com">http://ec2-34-219-239-192.us-west-2.compute.amazonaws.com</a></code></li></ul>
    </ul>

<hr border: 15px; border-style: dashed;>
    <h1 style="text-transform:capitalize; color: #010806 ;"><b style="text-transform: uppercase;">step 15: </b> Troubleshooting </h1>
    <ul>
        <li>Unable to view the browser</li>
            <ul><li>check error at: <code style="background-color:#9e9e9e4f">tail -10 /var/log/apache2/error.log</code></li></ul>
    </ul>


<hr border: 15px; border-style: dashed;>
<h1 style="text-transform:capitalize; color: #010806;"><b style="text-transform: uppercase;">step 16: </b> GOOGLE LOG IN OAuth AUTHENTICATION </h1>
<ul>
    <li>open <a href="https://console.cloud.google.com/home/dashboard?project=itemcatalog-234421">Google API platform</a></li>
    <li>Left panel <code style="background-color:#9e9e9e4f">API & Services</code> => Credentials </li>
    <li>Under <code style="background-color:#9e9e9e4f">Create Credentials</code> select <code style="background-color:#9e9e9e4f"> Oauth client ID</code></li>
    <img src="picture/credential.png" width=500 height=250>

    <li>Go to <a href="https://www.hcidata.info/host2ip.cgi">hcidata</a> to convert your public IP address to hostname</li>
    <img src="picture/oauth.png" width=700 height=400>
    <li>Download secret JSON and copy the content of it</li>
    <li>Open <code style="background-color:#9e9e9e4f">sudo nano /var/www/catalog/client_secrets.json </code> erase the old secret and paste new one</li>
    <li>Open <code style="background-color:#9e9e9e4f">sudo nano templates/login.html </code> and replace <code style="background-color:#9e9e9e4f">client_id </code> with new code </li>

    <h2>Troubleshooting</h2>
    <ul>
        <li>After login, but it doesn't display catalog page</li>
        <li>Steps:</li>

            <ul><li>sudo tail -10 /var/log/apache2/error.log </li>
                <li>still in cd /var/www/catalog/catalog</li>
                <li>cd templates</li>
                    <ul><li>sudo nano login.html</li>
                        <li>Update Client_ID </li>
                            <ul><li>copy client_ID from client_secrets.json and paste in login.html</li></ul>
                    </ul>
                <li>cd .. </li>
                    <ul><li>sudo nano __init__.py</li>
                        <li>Look for google login token </li>
                            <ul><li>edit the line </li>
                                <img src="picture/glogin.png" width=700 height=150>
                            </ul>
                    </ul>
            </ul>
    </ul>
</html>
