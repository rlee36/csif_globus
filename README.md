# csif_globus
...

## Table of Contents 
### 1. Problem

The Stanford Cell Sciences Imaging Facility (CSIF) faces a significant challenge when it comes to acquiring data from research instruments. Currently, researchers are required to bring their own hard disk drives (HDDs) in order to store and transfer data obtained from the instruments. This dependency on individual HDDs poses several problems. First, it creates logistical difficulties as researchers need to manage and transport their own storage devices, potentially leading to delays and inefficiencies. Second, this practice introduces security vulnerabilities when these personal storage devices are lost or stolen. In such cases, sensitive research data could be compromised, leading to potential intellectual property theft or unauthorized access. Moreover, relying on individual HDDs increases the risk of data loss or corruption due to device failure or mishandling. To streamline the data acquisition process, mitigate security risks, and enhance data integrity, it is crucial for the Stanford CSIF to explore alternative solutions that provide a standardized and reliable means of data storage and transfer.
### 2. Solution

To address the challenges faced by the Stanford Cell Sciences Imaging Facility (CSIF) in acquiring data from research instruments, a robust solution has been implemented using Globus, a powerful data management platform provided by the University of Chicago. Globus offers a standardized and secure method for data storage and transfer, eliminating the need for researchers to bring their own hard disk drives (HDDs). With Globus, researchers can seamlessly and efficiently transfer data directly from the shared research data storage to a their storage system. This not only streamlines the data acquisition process but also mitigates security risks associated with lost or stolen personal storage devices since it ensures that sensitive research data is protected and accessible only to authorized personnel. 

Let's explore how to move data from the shared instruments to the researcher's endpoint. 
1. The system admin installs and configures Globus on shared instrument storage.

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




  
3. The researcher creates a folder and collects data from the shared instrument.
4. The system admin configures the access to the folder, where data has been collected.
5. The researcher moves data from the shared instrument storage to the researcher's endpoint (for example, Oak).
6. The system admin maintains the data by automation et al 

In addition, researchers can easily share data between their own endpoints and those of other researchers. Detailed instructions on how to utilize data sharing can be found in the guide [Globus@Stanford](https://globus.stanford.edu/) authored by Karl Kornel. If you have any questions or need further assistance, please feel free to contact Robert Lee at [rlee36@stanford.edu](rlee36@stanford.edu).

### 3. Why now?
### 4. Competition
### 5. What's next?
### 6. Frequently Asked Questions (FAQs)
### 7. Acknowledgements
D at Stanford University

BJ at the University of Chicago

Karl Kornel at Stanford University 

Ruth Marinshaw at Stanford University 
