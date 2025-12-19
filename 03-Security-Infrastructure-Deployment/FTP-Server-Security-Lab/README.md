Setting Up an FTP Server and Analyzing FTP Traffic

Name: Youssef Elsaeed

First let's Talk about FTP for a moment:

- What is FTP?

  - FTP is File Transfer Protocol, At its core, it\'s a set of rules---a
    \"language\"---that computers use to send and receive files over a
    network.

  - Works on Client-Server model.

  - Two channel system

    - Port 21 for control channel.

    - Port 20 for data channel.

  - Security wise it's unsafe: cause of the old trusty model of it, it's
    not encrypted at all, including your username and password which
    makes it unsuitable for nowadays File transfer.

    - Solution was FTPS (File transfer protocol secure) which occurs
      over ssl/tls OR SFTP which occurs over SSH.

- Now that we understanded what is FTP we got the point that it works on
  client-server model, so we do understand that we need to initiate an
  FTP server on our machine for that task to be completed and tested,
  the role of the server is to listen for incoming FTP connections.

  - Server role would be in any case including ours:

    - Host Files: It holds the directory of files that are available for
      transfer.

    - Handle Authentication

    - Manage Permissions

Now as we discussed the need of an FTP server let's talk about a needed
software for that task which is VSFTPD:

- Stands for Very Secure FTP Daemon, and it's a popular FTP server
  software free to use.

Tasks Required:

1.  Set up FTP server on ubuntu.

2.  Connecting and transferring files from windows host.

3.  Analyzing FTP protocol Through wireshark.

4.  Securing the FTP server with SSL/TLS.

Now let's roll to our First task setting up the FTP server:

- First thing is to install vsftpd which is our software needed as we
  talked about it so we did run the sudo apt command:

  - ![A purple and white background AI-generated content may be
    incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image1.png){width="5.725496500437445in"
    height="0.8500732720909886in"}

- Now after the installation is finished we would need to create an ftp
  user for the test:

  - ![A screenshot of a computer program AI-generated content may be
    incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image2.png){width="5.683825459317585in"
    height="2.975258092738408in"}

  - Added the user, changed only the name and left all the other details
    as default.

- Now creating the test file before configuration:

  - ![](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image3.png){width="6.5in"
    height="0.4625in"}

  - ![](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image4.png){width="6.5in"
    height="0.5in"}

- Now to the most important part which is configuring VSFTPD, the config
  file is in the /etc:

  - ![](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image5.png){width="6.233873578302712in"
    height="0.6500568678915135in"}

  - As I searched for the needed configuration it looks like we need the
    following:

    - Local system users to log in

    - Users to upload files

    - Disable anonymous login (security) (not important now) (Default is
      no)![A screenshot of a computer error AI-generated content may be
      incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image6.png){width="4.750411198600175in"
      height="1.4167891513560804in"}

    - Restrict users to their home directory (security) (not important
      now)

    - Point our user to their ftp directory

    - For the active and passive analysis we would want to set the
      passive mode settings (used for firewalls) ![A screenshot of a
      computer AI-generated content may be
      incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image7.png){width="5.100441819772528in"
      height="4.108689851268592in"}

- Now (assuming that we have the firewall enabled but actually it's
  disabled but let's go through it too) we need to open the ports for
  ftp communication on ubuntu's firewall:

  - Allow our default ports 21/20, allow the passive port range we did
    just set 40000/50000, and allow the secure FTPS port 990 as required
    in the test.

  - ![](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image8.png){width="5.7171620734908135in"
    height="2.083513779527559in"}

- Restarted the service and now checking the status: ![A computer screen
  with white text AI-generated content may be
  incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image9.png){width="6.308880139982502in"
  height="2.483548775153106in"}

Now the second task which is connecting and transferring files:

- On our windows host, after doing some research we need a client
  software now for the communication between both, first make sure
  devices are set to NAT on ur VM.

- And an info before diving through it more is that the passive mode is
  more client friendly because it ensures that the firewall doesn't get
  a server connection that would be thought of as suspicious in the
  active mode the client sends command PORT which tells the server
  connect back to me at my ip addr, but with PASV command the server
  replies okay am waiting for you so the client would open a second
  connection from its own random port which is more firewall friendly,
  and here is the difference.

- Now let's download a recommended client which is FileZilla.

- Installed the client software, opened file-\>site manager, created a
  new site called ubuntu, enter my ip addr of the ubuntu machine and the
  user and password of my user created: ![A screenshot of a computer
  AI-generated content may be
  incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image10.png){width="6.5in"
  height="4.904166666666667in"}

- Logged in successfully : ![A white background with black and white
  clouds AI-generated content may be
  incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image11.png){width="6.5in"
  height="1.0902777777777777in"}

- listing of the files: ![A screenshot of a computer AI-generated
  content may be
  incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image12.png){width="4.775413385826772in"
  height="3.058597987751531in"}

- Test upload and download: ![A screenshot of a computer AI-generated
  content may be
  incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image13.png){width="6.5in"
  height="1.8763888888888889in"}

- great we are now connected and files is transferring brightly, just
  drag and drop( side note the drag and drop didn't work in downloading
  the test_download but it worked with the upload with test_upload) so
  test_download checked it manually on my user on my windows host.

Now with task 3 which was analyzing the unencrypted ftp traffic using
wireshark:

- First we opened wireshark, started capturing with vmware interface
  because that problem got me here for 10 minutes tryna understand why
  isn't it capturing.

- Look at the interesting results found on the capture screen using ftp
  filter then ftp \|\| ftp-data: ![A screenshot of a computer
  AI-generated content may be
  incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image14.png){width="6.5in"
  height="2.7534722222222223in"}

- And yes ive changed the username to ftpuser after having some problems
  upthere connecting with filezilla and repeated some steps about
  creating the file in the directory of the user all over again but
  that's okay.

- Saw the PASV command there?

  - The client opened both connections, the PASV instead of PORT which
    was going to be there if our config was different.

- Let's focus on our capture results which shows password and user as
  plain text, the file path, literally the file was exportable using
  wireshark: ![A screenshot of a computer AI-generated content may be
  incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image15.png){width="6.5in"
  height="2.665277777777778in"}

- Noticed the binary mode that was used by vsftpd in order to download
  the file:

  - ![A screen shot of a computer AI-generated content may be
    incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image16.png){width="6.5in"
    height="1.1368055555555556in"}

Now let's dive to task 4 which is securing our FTP server with ssl/tls

- Now to work securely we would need an ssl certificate, searched and
  found out a command to generate a certificate for a year:

  - ![](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image17.png){width="6.100528215223097in"
    height="0.6917268153980752in"}

- Now to our config file and let's enable ssl because it's disabled by
  default, give the path of our created cert, disable any old ssl
  versions and enable tls.

  - ![](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image18.png){width="5.767166447944007in"
    height="4.142025371828521in"}

- Also used the force_login and force_data to YES to force control over
  ssl for the test.

- Now to filezilla on our host windows machine.

- Changed only the cnryption on the site manager:

  - ![A screenshot of a computer AI-generated content may be
    incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image19.png){width="5.475474628171479in"
    height="2.516884295713036in"}

- And now we'll connect:

  - ![A screenshot of a computer AI-generated content may be
    incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image20.png){width="6.5in"
    height="2.5770833333333334in"}

  - Hurrayyyyy succeeded.

- Now let's analyze the traffic but with encrypted traffic this time:
  ![A screenshot of a computer AI-generated content may be
  incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image21.png){width="6.5in"
  height="2.2472222222222222in"}

- Now we can view clearly that there is no chance of comparison, here we
  are not seeing the commands USER, PASS that was unencrypted earlier
  naah, we are seeing only AUTH TLS.

- As I wanted to see the data encrypted and as the ftp opens the
  connection and then handles the tls the connection to keep going ive
  changed the filter to tls to check if our data is encrypted, file
  download and upload:

  - ![A screenshot of a computer AI-generated content may be
    incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image22.png){width="6.5in"
    height="3.348611111111111in"}

- That application data is our encrypted file transfer, as shown the
  connection between our host ip and our ubuntu ip, also inspecting the
  packet header itself:

  - ![A screenshot of a computer AI-generated content may be
    incorrect.](03-Security-Infrastructure-Deployment\FTP-Server-Security-Lab\images/media/image23.png){width="6.5in"
    height="2.2194444444444446in"}

- Now we are done with our test, loads of lessons learnt actually thanks
  WE!!
