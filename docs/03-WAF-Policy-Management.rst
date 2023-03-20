Use Case 03: WAF (XML) Policy Management
========================================

OVERVIEW
--------

WAF-Policy-Management.yaml is a templated Ansible Playbook to manage blocked
IP addresses and URL's on F5 ASM through Ansible automation. 

Web Application Firewalls work to protect web applications by inspecting
incoming traffic, blocking bots, SQL injection, Cross Site Scripting and a host
of other attacks. This playbook is designed to demonstrate a basic WAF scenario
to create and modify an F5 WAF (ASM) policy to block URL(s) or IP address(s) or
both. 

Using this playbook, other security vendors or even ticketing based solutions
like Service NOW, users will be able to create a start to finish automated
solution based on when attacks can occur.

RUNNING THE TEMPLATE
--------------------

Running this template assumes that a F5 BIG-IP instance, necessary webservers
and Ansible node are available. 

1. Login to the Ansible host

2. Change Directory in the Ansible Host to the use-cases repo previously
   downloaded

   .. code:: bash
   
      cd ~/f5-bd-ansible-labs/201-F5-Advanced/Modules/03-WAF-Policy-Management/


3. **(Optional)** Edit 'f5_vars.yml' file to customize your variables. Here you
   can add/remove IP addresses and URLs from the 'Blocked_IPs' and
   'Blocked_URLs' list

4. Launch the Ansible playbook 'WAF-Policy-Management.yaml' with the
   variable file ‘f5_vars.yml’:

   .. code:: bash

      ansible-navigator run WAF-Policy-Management.yaml --mode stdout -e @f5_vars.yml

   This template will configure the F5 BIG-IP to provision the
   `WAF module <https://www.f5.com/products/security/advanced-waf>`__, create a
   Virtual IP (VIP) including a Pool and nodes, a WAF policy for the use case,
   then modify the policy to block IP’s and URL’s.

.. attention::

   This Playbook modifies the provisioning of modules on the BIG-IP and will
   take some time to complete as the new module comes online.
   
   This Playbook detects if blocked URL or IP already exists and only add what
   is new (idempotency).  because of that it will create Errors and ignore them on first run, 
   this is expected behavior.  The Errors will indicate when the exported ASM Policy doesn't 
   contain the data we are attempting to add, and then will add that data.  


TESTING AND VALIDATION
----------------------

**VERIFYING WAF POLICY ENFORCEMENT:**

   **Provisioner**

   - From a client brower, access the application through the virtual address on
   the F5 BIG-IP.
   - To access this site externally you will need to use the instructor inventory
   studentX-f5 IP Address which will be refered as (F5-BIG-IP-Public-IP) below.
   - From a client browser, access the F5-BIG-IP-Public-IP on port 8082 to view
   the webpage to validate accessibility (https://F5-BIG-IP-Public-IP:8082)
   - Access the URL's present in the f5_vars.yml file to see the WAF policy in
   action 

   - https://F5-BIG-IP-Public-IP:8082/blocked.html
   - https://F5-BIG-IP-Public-IP:8082/hacked.html
   - https://F5-BIG-IP-Public-IP:8082/robot.txt 

   **UDF**
   Using the Win10 External Client (UDF --> Components --> Win10 - External Client --> Access --> RDP)

   - Login with the administrator account with password located at (UDF --> Components --> Win10 - External Client --> Details --> Details Tab )
   - Launch Web Browser to test and validate connections 
   - Access the URL's present in the f5_vars.yml file to see the WAF policy in
   action 

   - https://10.1.20.30:8082/blocked.html
   - https://10.1.20.30:8082/hacked.html
   - https://10.1.20.30:8082/robot.txt 


**BIG-IP CONFIGURATION VERIFICATION:**

This section is optional and for testing and verification purposes only. It
assumes knowledge of how to operate BIG-IP commands and networking.

   **Provisioner**
   BIG-IP - (https://F5-BIG-IP-Public-IP:8443) - get the F5-BIG-IP-Public-IP from
   instructor_inventory file in provisioning host.

   - Login to the BIG-IP
   - Navigate to Security --> Application Security to view the WAF policy deployed
   - Navigate to Local Traffic --> Virtual Servers
   - View the deployed use case access F5-BIG-IP-Public-IP:port (8082)

   **UDF**
   BIG-IP - (In UDF --> Components --> BIG-IP --> Access --> TMUI)  - This will popup
   a webpage to access the F5 Login Page

   - Login to the BIG-IP instance
   - Navigate to Security --> Application Security to view the WAF policy deployed
   - Navigate to Local Traffic --> Virtual Servers
   - View the deployed use case access F5-BIG-IP-Public-IP:port (8082)

   .. hint::

      Username is admin and the Password would be the Password given in the Linklight Lab or UDF Lab
