# csif_globus
...

## Table of Contents 
### 1. Problem

The Stanford Cell Sciences Imaging Facility (CSIF) faces a significant challenge when it comes to acquiring data from research instruments. Currently, researchers are required to bring their own hard disk drives (HDDs) in order to store and transfer data obtained from the instruments. This dependency on individual HDDs poses several problems. First, it creates logistical difficulties as researchers need to manage and transport their own storage devices, potentially leading to delays and inefficiencies. Second, this practice introduces security vulnerabilities when these personal storage devices are lost or stolen. In such cases, sensitive research data could be compromised, leading to potential intellectual property theft or unauthorized access. Moreover, relying on individual HDDs increases the risk of data loss or corruption due to device failure or mishandling. To streamline the data acquisition process, mitigate security risks, and enhance data integrity, it is crucial for the Stanford CSIF to explore alternative solutions that provide a standardized and reliable means of data storage and transfer.
### 2. Solution

To address the challenges faced by the Stanford Cell Sciences Imaging Facility (CSIF) in acquiring data from research instruments, a robust solution has been implemented using Globus, a powerful data management platform provided by the University of Chicago. Globus offers a standardized and secure method for data storage and transfer, eliminating the need for researchers to bring their own hard disk drives (HDDs). With Globus, researchers can seamlessly and efficiently transfer data directly from the shared research data storage to a their storage system. This not only streamlines the data acquisition process but also mitigates security risks associated with lost or stolen personal storage devices since it ensures that sensitive research data is protected and accessible only to authorized personnel. Let's explore how to move data from the shared instruments to the researcher's endpoint. 

First, the system admin installs and configures Globus on the shared instrument storage. 

        Step 1: Install Globus Connect Personal (GCP) on the storage server, which runs on Windows machine.
                1. Create a Globus Auth client identity (application) as the service account that will own the GCP.
                   a. Log into app.globus.org with your Stanford identity.
                   b. Select Settings from the left nav bar and then select the Developers tab.
                   c. Select Register a service account or application credential for automation.
                   d. Enter a project name, for example “Stanford CISF”, and contact email.
                   e. Name your client (application), for example “Brucker Scope GCP client” and select Register App.
                   f. Select Add client secret and then name secret, for example “Secret 1”.
                   g. Copy the secret to secure location.  This is your only opportunity to view the secret.
                           i. Store the client id, which is equivalent to the client identity’s username
                              with the client secret which is equivalent to the client identity’s password.

                2. Log into the Globus CLI as the client and generate a GCP setup key
                   a. Install the Globus CLI [requires Python3] on the Windows machine
                           i. python3 -m pip install pipx
                           ii. python3 -m pipx install globus-cli
                   b. Log into Globus CLI with client (From a CMD prompt, or Powershell)
                           i. set GLOBUS_CLI_CLIENT_ID="YOUR CLIENT ID FROM STEP 1"
                           ii. set GLOBUS_CLI_CLIENT_SECRET="YOUR_CLIENT_SECRET FROM STEP 1"
                           iii. C:\PATH_TO_Globus_CLI\globus.exe whoami (confirm you have logged in as client)
                                1ab345c6-12ab-12ab-a123-ab1c23d4567e@clients.auth.globus.org
                   c. Generate a GCP setup key
                           i. This step performs the registration step with the Globus service and
                              prints out a setup key, which can be used to configure an installed
                              Globus Connect Personal to use that registration.
                           ii. C:\PATH_TO_Globus_CLI\globus gcp create mapped "YOUR GCP DISPLAY NAME" 
                               (confirm you have created Collection ID and Setup Key) 
                               Message:       Endpoint created successfully
                               Collection ID: a12345678-0a12-12ab-abcd-a1234567bcdf
                               Setup Key:     12a3b45c-1234-12a3-ab3c-ab123cedfg45
                           iii. Copy Collection ID (ie. %GCP_UUID%) and Setup key for later use.

                3. Install GCP
                   a. Follow steps 1 through 3 ONLY of https://docs.globus.org/how-to/globus-connect-personal-windows
                           i. Use a local Windows account that has access to the desired data
                   b. Launch GCP (After installation has completed GCP will launch.)
                   c. Expand the ‘Advanced Options’ field (Do not select login)
                   d. Select ‘I have a setup key’
                   e. Enter the setup key generated above
                   f. Exit setup after successful installation

        Step 2: Configure GCP: set GCP as managed, assign roles, configure visibility
                1. Set GCP as managed under the Stanford Globus subscription.
                   a. GCP must be associated with a subscription in order to assign roles,
                      to create guest collections, and to support GCP → GCP transfers.
                   b. Send a request to your institution’s Subscription Manager to set the GCP as managed.
                      You must include the %GCP_UUID% (aka. Collection ID) in your request.
                           i. The Subscription Manager must use the Globus CLI Command to set GCP as managed:
                              C:\PATH_TO_Globus_CLI\globus.exe endpoint set-subscription-id %GCP_UUID% %subscriptionID%
                           ii. Stanford Subscription Managers are
                               SRCC Stanford, researchcomputing@lists.stanford.edu
                               Stanford Research Computing Center, srcc-support@stanford.edu
                               Stanford Libraries, sul-sys-team@lists.stanford.edu
                               Stanford Center for Genomics and Personalized Medicine, scg-action@lists.stanford.edu
                               Stanford Cancer Institute - Christina Curtis Lab, seoane@stanford.edu
   
                2. Assign the role of GCP admin to one more staff member (or a group of staff members)
                   so that GCP can be managed and monitored by staff.
                     a. Command to assign roles is
                        C:\PATH_TO_Globus_CLI\globus.exe endpoint role create %GCP_UUID%
                        --identity %Identity_UUID% --role administrator
                           i.  %Identity_UUID% that you would like to add an Endpoint role assignment.
                               For instance, rlee36@stanford.edu / 1ab234c5-12ab-12ab-a123-ab1c23d4567e
                               This is NOT YOUR CLIENT ID FROM STEP 1. This is YOUR STANFORD ID you login to Globus. 
                           ii. Your UUID can be found in the web app under Settings in the left nav bar by expanding
                               the information under your identity in the Identities tab to view the UUID.
   
                3. Set the visibility of the GCP to Public so that Globus users can find the GCP.
                   Note that Public visibility only impacts the visibility of the GCP attributes (e.g. display name, UUID)
                   and does not impact data visibility.
                     a. Log into app.globus.org using the admin identity.
                     b. Select Collections from the left nav bar and filter by Administered by you.
                     c. Select the GCP and then select Edit Attributes and change the visibility from Private to Public.
  
Second, the researcher creates a folder and collects data from the shared instrument.

        Step 1: Create a folder to store the research data  
                1. Log into CardinalKey-FullTraffic with Stanford Credentials and Cisco AnyConnect Secure Mobility Client.
                2. Log into 171.64.80.32 (IP address for the shared instrument storage) with Remote Desktop Connection. 
                3. Open the windows file explorer and go to the drive Y:
                4. Create the folder under the researcher's Stanford SUNetID (for example, rlee36) 
                
        Step 2: Initiate the data collection process 
                1. Make sure data is stored in the folder under the researcher's Stanford SUNetID from above 
        
Third, the system admin configures the access to the folder, so the researcher can acquire data that has been collected.

        Step 1: Create GCP guest collections
                1. Log into Globus web app as admin identity and create guest collections on GCP.
                2. Log into Globus with system admin's institutional ID and navigate to the File Manager.
                3. Select the collection that has the folders/sub-folder you wish to share.
                4. Highlight the folder that you would like to share and Click Share in the right command pane.
                5. Provide a name for the guest collection (the researcher's Stanford SUNetID), and click Create Share. 

        Step 2: Give a permission to access the data via GCP guest collection to the researcher. 
                1. When your collection is created, system admin will be taken to the Sharing tab to set permissions. 
                2. Click the Add Permissions button or icon to share access with others. 
                   System admin can add permissions for an individual user, for a group, or for all logged-in users.
                3. The users you share with will receive an email notification containing a link to the shared endpoint.
        
Fourth, the researcher moves data from the shared instrument storage to the researcher's endpoint (for example, Oak).

        Step 1: Log in with an existing identity
                1. Visit www.globus.org and click "Login" at the top of the page. On the Globus login page, 
                   choose an organization you’re already registered with, such as your school or your employer.
                2. You’ll be redirected to your organization’s login page. Use your credentials for that organization to login.
                3. You may be prompted to provide additional information (for example, organization).
                4. You'll give Globus permission to use your identity to access information and perform actions on your behalf.
                
        Step 2: Access the data via File Manager
                1. Click in the Collection field at the top of the File Manager page
                2. Type "NAME OF GCP GUEST COLLECTION" 
                
        Step 3: Click the "Path" field and change it to folders/sub-foler you have access. 
                1. Globus will show the files in the new path.
                
        Step 4: Request a file transfer 
                1. Click Transfer or Sync to... in the command panel on the right side of the page. 
                   A new collection panel will open, with a "Transfer or Sync to" field at the top of the panel.
        
6. The system admin maintains the data by automation (back up, DOI) et al 

In addition, researchers can easily share data between their own endpoints and those of other researchers. Detailed instructions on how to utilize data sharing can be found in the guide [Globus@Stanford](https://globus.stanford.edu/) authored by Karl Kornel. If you have any questions or need further assistance, please feel free to contact Robert Lee at [rlee36@stanford.edu](rlee36@stanford.edu).

### 3. Why now?
### 4. Competition

Cost x Time (Build your own, Cloud, Globus) 
### 5. What's next?

Automation (back up, DOI) x Discovery 
### 6. Frequently Asked Questions (FAQs)
### 7. Acknowledgements

We would like to express our sincere gratitude to the following individuals and organizations for their invaluable support and contributions to the success of this project. Special thanks to David Lenzi and Jonathan Mulholland from the Stanford Cell Sciences Imaging Facility for their expertise and assistance throughout the implementation process. We are also grateful to Robert Lee, Karl Kornel, and Ruth Marinshaw from the Stanford Research Computing Center for their guidance and technical support in utilizing the Globus platform effectively. Furthermore, our heartfelt appreciation goes to Brigitte Raumann and James Kube from Globus at the University of Chicago for their collaboration and expertise in implementing and optimizing the data management solution. Lastly, we extend our thanks to the Office of the Vice Provost and Dean of Research for their continuous support and commitment to advancing research and innovation at Stanford University. 
