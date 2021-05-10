Lab 1: Volterra Application Firewall Deployment
===============================================

The purpose of this lab is to extend the participant's ability to naviagte the 
Volterra console and assign an Application Firewall via Rule-based WAF configurations. 
This an entry-level security configuration which is part of Volterra's current multi-layer 
security approach.   

Volterr'a current multi-layer security approach leverages both a **Rule-Based WAF** and a 
**Behavioural Firewall**. This can be further exteneded through using a combination of 
the Rules-Based WAF, Behavioral Analysis(Behavioral Firewall), and **Fast ACLs** from the
Network Firewall section to enable **Application DOS Protection**. These additional topics
will be addressed in specific labs.  

Objective:
----------

-  Gain an navigational understanding of Volterra Console and its key menus

-  Gain an initial understanding of Volterra's Application Firewall - Rule-Based WAF configuration

-  Deploy and verfiy security configuration within Volt Console 

Lab Requirements:
-----------------

-  All Lab requirements will be noted in the tasks that follow

-  Estimated completion time: 20 minutes

Lab 1 Tasks:
-----------------

TASK 1: Quick Intro - Rules-based WAFs (Concept Review)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The rules engine (currently based-on ModSec) is the centerpiece of Volterra's WAFs approach
to application security and decision of what rules to configure assumes a high degree of
knowledge of the rules. If for some reason, a needed rule is excluded then the application
will be vulnerable. If unwanted rules are added, then it adds extra processing per request 
and this adds to latency. To solve this problem of rules knowledge and make it extremely easy
to configure, we created a rules processing engine that automates the inclusion and exclusion
of rules.

There are two types of rules for each waf-object that are supported by the system 
**core-rule-set** and a **volterra-rule-set**. There are two ways the rules can be enabled:

:**Implicit**: User provides a list of technologies. For example, programming languages used,
               content management systems used, types of servers, etc that are used by the 
               endpoints (or services) for a given virtual-host. Volterra has an algorithmic 
               engine that will use this information to automatically figure out rules to 
               include and exclude from both the rule set.
			  
:**Explicit**: User can explicitly configure rules to exclude from the Rule engine.

In addition to configuring the rules, the decision needs to be made on whether to block the 
attacks or just generate an alert and let the security or application experts decide what
actions to take:

:**Monitoring mode**: This will generate an alert on a security event. No attack is 
                      blocked in this mode.
:**Blocking Mode**: This will configure the WAF to be Inline-mode and deny client requests 
                    that match certain rules.

TASK 2: Quick Intro - Configuring Rules-based WAFs (Concept Review)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The user can configure a **Web Application Firewall** per **virtual host** by attaching **WAF**
**object** or **WAF-rules object** to a **virtual-host** object:

:**WAF object**: Is a mechanism to make users life easy and is described earlier as 
                 the implicit mode. The idea is that instead of enabling all the rules 
                 or typing individual rules, the user can just define the type of 
                 technologies used by their applications and types of attacks that need 
                 to be detected and the system will decide what needs to be done.

:**WAF-Rules object**: Is a mechanism for advanced users and is described earlier as the explicit 
                       mode. One can select rules in core-rule-set or volterra-rule-set to be 
                       enabled/disabled by configuring the waf-rules-object appropriately: 
                       * Whether a rule is blocking or alerting 
                       * Rules hit thresholds 
                       * Exclude or include list of rule id(s)

The built-in rules processor takes the WAF object and automatically creates a WAF-rules object that 
contains the detailed configuration information that can be used by the WAF. The processing engine
works on the basis of the following information:

:**Application Profile**: Defines the technologies used to build the app - Programming languages used,
                          content management systems used, types of servers, etc.
:**Types of Attacks to Detect**: Defines the types of attacks that need to be detected and prevented
                                 (SQL injection, XSS, Protocol, etc.)

Automatically generated WAF-rules object cannot be modified by users and this object is owned by the 
system. However the user can always copy this auto-generated WAF-rules object and further tune it.

Individual WAF or WAF-Rules objects can be shared by multiple virtual hosts within a namespace. In 
addition, these objects can also be attached to virtual host or per route basis. Since WAF objects 
can be shared with multiple virtual hosts and multiple wafs can be configured to given virtual-host by 
way of routes, monitoring is not based on these WAF objects. All monitoring for a WAF is done on a per 
virtual-host to provide insights into application.

Details on all the parameters and how they can be configured for this object is covered in the API 
Specification under WAF object or WAF-Rules object

TASK 3: Volt Console (General Navigation)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+----------------------------------------------------------------------------------------------+
| 1. Login to the Volterra Console: **https://www.volterra.io** and complete authentication.   |
|                                                                                              |
| 2. **Log In** > **Teams or Organization Plans** (This is specfic to region) > **Next**       |
|                                                                                              |
| 3. **Log in with Azure** (You must have completed your account onboarding process.**         |
|                                                                                              |
| **NOTE**: *Logging on as Tenant Owner provides tenant-wide administration priviledges.*      |
+----------------------------------------------------------------------------------------------+
| |image001|                                                                                   |
|                                                                                              |
| |image002|                                                                                   |
|                                                                                              |
| |image003|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 4. Review the **Volt Console** as presented.                                                 |
|                                                                                              |
| 5. This is the **default** view.  It is the **App View** and is more centric to *Netops*     |
|                                                                                              |
|    *Personas*.                                                                               |
+----------------------------------------------------------------------------------------------+
| |image004|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 6. This is the **Shared View** and is more centric to *Secops Personas* and is where the     |
|                                                                                              |
|    bulf of all *shared* application security configurations will be made.                    |
+----------------------------------------------------------------------------------------------+
| |image005|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 7. This is the **System View** and is more centric to *Devops Personas*.                     |
+----------------------------------------------------------------------------------------------+
| |image006|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 8. This is the **General View** and is specific to Profile and Tenant operations.            |
+----------------------------------------------------------------------------------------------+
| |image007|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK 2: Configuring Web Application Firewall  
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+----------------------------------------------------------------------------------------------+
| 1. The following image shows the configuration worflow for creating an appplication firewall |
|                                                                                              |
|    which fundametally involves three key steps.                                              |
|                                                                                              |
| * Create WAF Rule: Create WAF rules object in your namespace. This object contains rules     |
|                                                                                              |
|   selected from the Core Rules Set (CRS) and Volterra Rules Set (VRS).                       |    
|                                                                                              |
| * Create WAF: Create application firewall object and configure the application settings.     |
|                                                                                              |
| * Attach WWAF to Virtual Host.                                                               |
+----------------------------------------------------------------------------------------------+
| |image008|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. Select the **App** view top left and select your namespace from the namespace dropdown.   |
|                                                                                              |
|    **Note**: For shared configurations, the **shared** namespace is selected when using the  |
|                                                                                              |
|    **shared** view. (for more global configurations).                                        |
+----------------------------------------------------------------------------------------------+
| |image009|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 4. Select the **Security** > **App Firewalls** from left navigation then select *App**       |
|                                                                                              |
|    **Firewalls** from the flyout menu.                                                       |
|                                                                                              |
| 5. Click **Add Firewall** in the right updated panel.                                        |
+----------------------------------------------------------------------------------------------+
| |image010|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 6. In the **New: Firewall** window, enter the following values in the **Metadata** and       |
|                                                                                              |
|    **Mode** sections respectively.                                                           |
|                                                                                              |
|    **Name**: Unique name (ex <username>-lab1-appfw)                                          |
|                                                                                              |
|    **Mode**: Block (as we will be blocking traffic) Note: Alert is the other option here.    |
|                                                                                              |
| 7. Click the **Save and Exit** button.                                                       |
+----------------------------------------------------------------------------------------------+
| |image011|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 7. Select the **Security** > **App Firewalls** from left navigation then select *App**       |
|                                                                                              |
|    **Firewalls** from the flyout menu.                                                       |
+----------------------------------------------------------------------------------------------+
| |image012|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK: 3: Switching Namespace 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~



TASK 14: End of Lab1
~~~~~~~~~~~~~~~~~~~~

+----------------------------------------------------------------------------------------------+
| 1. This concludes Lab1, feel free to review and test the configuration.                      |
+----------------------------------------------------------------------------------------------+
| |image000|                                                                                   |
+----------------------------------------------------------------------------------------------+

.. |image000| image:: media/image001.png
   :width: 800px
.. |image001| image:: media/lab01-001.png
   :width: 800px
.. |image002| image:: media/lab01-002.png
   :width: 800px
.. |image003| image:: media/lab01-003.png
   :width: 800px
.. |image004| image:: media/lab01-004.png
   :width: 800px
.. |image005| image:: media/lab01-005.png
   :width: 800px
.. |image006| image:: media/lab01-006.png
   :width: 800px
.. |image007| image:: media/lab01-007.png
   :width: 800px
.. |image008| image:: media/lab01-008.png
   :width: 800px
.. |image009| image:: media/lab01-009.png
   :width: 800px
.. |image010| image:: media/lab01-010.png
   :width: 800px
.. |image011| image:: media/lab01-011.png
   :width: 800px
.. |image012| image:: media/lab01-012.png
   :width: 800px
.. |image013| image:: media/lab01-013.png
   :width: 800px
.. |image014| image:: media/lab01-014.png
   :width: 800px
.. |image015| image:: media/lab01-015.png
   :width: 800px
.. |image016| image:: media/lab01-016.png
   :width: 800px
.. |image017| image:: media/lab01-017.png
   :width: 800px
.. |image018| image:: media/lab01-018.png
   :width: 800px
.. |image019| image:: media/lab01-019.png
   :width: 800px
.. |image020| image:: media/lab01-020.png
   :width: 800px
.. |image021| image:: media/lab01-021.png
   :width: 800px
.. |image022| image:: media/lab01-022.png
   :width: 800px
.. |image023| image:: media/lab01-023.png
   :width: 800px
.. |image024| image:: media/lab01-024.png
   :width: 800px
.. |image025| image:: media/lab01-025.png
   :width: 800px
.. |image026| image:: media/lab01-026.png
   :width: 800px
.. |image027| image:: media/lab01-027.png
   :width: 800px
.. |image028| image:: media/lab01-028.png
   :width: 800px
.. |image029| image:: media/lab01-029.png
   :width: 800px
.. |image030| image:: media/lab01-030.png
   :width: 800px
.. |image031| image:: media/lab01-031.png
   :width: 800px
.. |image032| image:: media/lab01-032.png
   :width: 800px
.. |image033| image:: media/lab01-033.png
   :width: 800px
.. |image034| image:: media/lab01-034.png
   :width: 800px
.. |image035| image:: media/lab01-035.png
   :width: 800px
.. |image036| image:: media/lab01-036.png
   :width: 800px
.. |image037| image:: media/lab01-037.png
   :width: 800px
.. |image038| image:: media/lab01-038.png
   :width: 800px
.. |image039| image:: media/lab01-039.png
   :width: 800px
.. |image040| image:: media/lab01-040.png
   :width: 800px
.. |image041| image:: media/lab01-041.png
   :width: 800px
.. |image042| image:: media/lab01-042.png
   :width: 800px
.. |image043| image:: media/lab01-043.png
   :width: 800px
.. |image044| image:: media/lab01-044.png
   :width: 800px
.. |image045| image:: media/lab01-045.png
   :width: 800px
.. |image046| image:: media/lab01-046.png
   :width: 800px
.. |image047| image:: media/lab01-047.png
   :width: 800px
.. |image048| image:: media/lab01-048.png
   :width: 800px
