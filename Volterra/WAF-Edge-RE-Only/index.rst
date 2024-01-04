Volterra WAF / ADN Edge - Solution Guide
=============================================================
Regional Edge [RE] Only Deployment Model
----------------------------------------
Volterra's Web Application Firewall (WAF) in conjunction with Volterra's Application
Delivery Network (ADN) Edge provides a SaaS-based service which provides subscribers
the ability to secure, deliver and observe application/service resources.  

This WAF focused configuration leverages Volterra's Global ADN as a front-end, 
SaaS-based protection fabric. Configured via VoltConsole, an API-first control plane
and web-console, subscribers deploy security and service configurations to protect 
their applications. The ADN's ingress/egress points also known as Regional Edges [RE],
then provide the enforcement of those security configurations. In conjunction, 
VoltConsole, also provides the observability and visibility needed to view reported
security events, request/response details and recommended security actions.

Through VoltConsole, subscribers are extended a multitude of services options in 
publication of their protected applications. Among these service options, are various 
WAF, L7 DDoS and Security deployment configurations to accommodate a subscriber's 
preferred SaaS service architecture. 

The focus of this solution guide is to provide configuration details and associated 
best practices for deployment of Volterra's Web Application Firewall (WAF) as a SaaS
service with its supporting configurations.

Solution Key Values
-------------------
The following are the key values this deployment model offers subscribers:
- SaaS-based, self-service driven solution which can be expanded to other use cases. 
- Streamlined engagement of WAF and security services across deployments.
- Robust API and Terraform Provider to automate/orchestrate configurations.
- Global advertisement (Anycast) of deployed solutions.
- Pure SaaS based solution with no requirements for installed components. 

The deployment model offers additional values besides those noted above. Overall
the value of this solution is to reduce complexity, enable operational efficiencies
and to assist subscribers in achieving more effective security for their protected 
applications.     

Other Solution Models
---------------------
Beyond the SaaS-only based designs, there other solution design models that are
discussed in other design guides.  They are listed and linked below for convenience.

- Volterra WAF / ADN Edge plus Customer Edge [CE]: A solution which extends the Volterra 
  WAF / ADN Edge solution by adding a Customer Edge [RE] deployed in public/private
  cloud.  This enables the Volterra ADN to privately address/access the origins in the
  Customer Edge [CE] minimizing public accessibility/visibility of origin resources. 
- Volterra WAF / Customer Edge [CE]: A solution which deploys the Volterra WAF within
  only Customer Edges [RE] deployed in public/private cloud.  This does not leverage the 
  Volterra ADN or its features but allows all CE WAF deployments to be managed centrally
  through VoltConsole with centralized observability/visibility. 


Solution General Overview
-------------------------
The purpose of this lab is to leverage Access Guided Configuration (AGC) to 
deploy an Identity Aware Proxy extended by Per Request Policies (PRP) access 
controls. The Per Request Policies will restrict access based on AD Group 
Membership and the URI accessed. Students will configure the various aspects 
of the application using strictly AGC, review the configuration and perform 
tests of the deployment.

Key Components
--------------
- Volterra Tenant: VoltConsole
- HTTP Load Balancer: HTTP Load Balancers are configured, via VoltConsole through its
  API or Web console interface.
- Origin Pool(s): Origin Pool resources also reffered to as upstream resources are
  associated to the HTTP Load Balancer for service delivery.  Origin Pools can be
  addressed via public FQDN or a publicly nat'd IP address and can exist in any Internet
  accessible public or private cloud. A single or multiple origin pools can be configured
  in a load balanced deployment.
- WAF Intent/Rules: A Web Application Firewall (WAF) configuration can be associated 
  with the HTTP Load Balancer through either using a WAF Intent (a templating tool) 
  or via a custom built rule set. 
- Additional Security Policies:   
- Supporting Security Controls
https://www.volterra.io/docs/reference/network-cloud-ref


 with the 
intended WAF and security/service policies.
• 
• The Site/Service is configured to be advertised via Anycast from the Application Delivery Network’s 
(ADN) via it’s Regional Edges [RE]. Associated CNAME records resolve to either Shared ADN IP, 
Dedicated ADN IP or Bring Your Own IP (service options)
• At each RE, an instance of the HTTP LB is established, the health of origin resources is confirmed and 
routing to origin resources determined.
• Client/Customers/Staff access each advertised service/site via FQDN and the nearest routable RE.

General Flow
------------
Client/Customers/Staff access the site/
service (FQDN) via the closest (closest 
RE) configured HTTP Load Balancer 
which is advertised (Anycast) at each 
RE of the Application Delivery Network 
(ADN).
Each HTTP LB proxies incoming traffic 
locally at each RE and then sources the 
upstream connection from the RE 
using the RE’s associated IP address 
space to make the proxied connection 
the origin resource.
The origin, publicly accessible, is 
configured to limit connecting IPs to 
just the ADN defined source address 
spaces. This is to protect targeted 
resource from unexpected/
unauthorized sources.

Configuration Guidance
----------------------
Step-by-Step build processes can be reviewed through the VoltConsole interface, Volterra
Teachable content or Volterra Documentation. The guidance provided below is to supplement 
existing documentation   


Name*
Labels
Basic Configuration*
Domains *
Select Type of Load Balancer*

HTTP Redirect to HTTPS
Add HSTS Header

TLS Config*
Select TLS security*

Default Origin Servers
Origin Pools

Routes Configuration

VIP Configuration*
Where to Advertise the VIP*

Security Configuration*

Service Policies*
Advanced Configuration


Solution 

































Objective
---------


Objective
---------

-  Gain an understanding of Access Guided Configurations and
   its various configurations and deployment models

-  Gain an initial understanding of Per Request Policies and their applicability
   in various delivery and control scenarios

Lab Requirements:
-----------------

-  All Lab requirements will be noted in the tasks that follow

-  Estimated completion time: 30 minutes

Lab 1 Tasks:
-----------------

TASK 1: Intialize Access Guided Configuration (AGC)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+----------------------------------------------------------------------------------------------+
| 1. Login to your provided lab Virtual Edition: **bigp1.f5lab.local**                         |
|                                                                                              |
| 2. Navigate to:  **Access -> Guided Configuration**                                          |
|                                                                                              |
| 3. Click the **Zero Trust** graphic as shown.                                                |
|                                                                                              |
| 4. Click on the **Identity Aware Proxy**  dialogue box click under **Zero Trust**            |
|                                                                                              |
|    in the navigation as shown.                                                               |
+----------------------------------------------------------------------------------------------+
| |image001|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 5. Review the **Identity Aware Proxy Application** configuration example presented.          |
|                                                                                              |
| 6. Scroll through and review the remaining element of the dialogue box to the bottom of the  |
|                                                                                              |
|    screen and click **Next**.                                                                |
+----------------------------------------------------------------------------------------------+
| |image002|                                                                                   |
|                                                                                              |
| |image003|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK 2: Config Properties  
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+----------------------------------------------------------------------------------------------+
| 1. In the **Configuration Name** dialogue box, enter **agc-app.acme.com**.                   |
|                                                                                              |
| 2. Toggle **Single Sign-On (SSO) & HTTP Header** to the **On** position.                     |
|                                                                                              |
| 3. Toggle **Application Groups** to the **On** position.                                     |
|                                                                                              |
| 4. Toggle **Webtop** to the **Off** position.                                                |
|                                                                                              |
| 5. Click **Save & Next** at the bottom of the dialogue window.                               |
+----------------------------------------------------------------------------------------------+
| |image004|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK: 3: Configure Virtual Server Properties 