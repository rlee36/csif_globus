# csif_globus
...

## Table of Contents 
### 1. Problem

The Stanford Cell Sciences Imaging Facility (CSIF) faces a significant challenge when it comes to acquiring data from research instruments. Currently, researchers are required to bring their own hard disk drives (HDDs) in order to store and transfer data obtained from the instruments. This dependency on individual HDDs poses several problems. First, it creates logistical difficulties as researchers need to manage and transport their own storage devices, potentially leading to delays and inefficiencies. Second, this practice introduces security vulnerabilities when these personal storage devices are lost or stolen. In such cases, sensitive research data could be compromised, leading to potential intellectual property theft or unauthorized access. Moreover, relying on individual HDDs increases the risk of data loss or corruption due to device failure or mishandling. To streamline the data acquisition process, mitigate security risks, and enhance data integrity, it is crucial for the Stanford CSIF to explore alternative solutions that provide a standardized and reliable means of data storage and transfer.
### 2. Solution

To address the challenges faced by the Stanford Cell Sciences Imaging Facility (CSIF) in acquiring data from research instruments, a robust solution has been implemented using Globus, a powerful data management platform provided by the University of Chicago. Globus offers a standardized and secure method for data storage and transfer, eliminating the need for researchers to bring their own hard disk drives (HDDs). With Globus, researchers can seamlessly and efficiently transfer data directly from the shared research data storage to a their storage system. This not only streamlines the data acquisition process but also mitigates security risks associated with lost or stolen personal storage devices since it ensures that sensitive research data is protected and accessible only to authorized personnel. 

Let's explore how to move data from the shared instruments to the researcher's endpoint. 
1. The system admin installs and configures Globus on shared instrument storage.
2. The researcher creates a folder and collects data from the shared instrument.
3. The system admin configures the access to the folder, where data has been collected.
4. The researcher moves data from the shared instrument storage to the researcher's endpoint (for example, Oak).
5. The system admin maintains the data by automation et al 

Also, let's discuss how to share data from the researcher's endpoint with the other researcher's endpoint. 

### 3. Why now?
### 4. Competition
### 5. What's next?
### 6. Frequently Asked Questions (FAQs)
### 7. Acknowledgements
D at Stanford University

BJ at the University of Chicago

Karl Kornel at Stanford University 

Ruth Marinshaw at Stanford University 
