# IACapstone
Hypervisor Security Assessment

# Executive Summary

Many desirable features can be achieved through the use of virtualization including reducing costs by being able to run multiple systems on the same hardware, gaining an additional level of security for sensitive assets, and  cloud service providers being able to host multiple clients on the same hardware due to the clients being logically isolated.  Due to these many advantages, virtualization has become a rapidly growing domain in computing.  Primary to the function of virtualization is the hypervisor, a hardware or software device that creates, runs, and manages the multiple virtual guests machines being hosted on the physical hardware.
   
As the usage of virtualization continues to grow, security of this primary computing piece, the hypervisor, grows in importance as well.  If a user is able to escape from a virtual machine to the hypervisor, they may be able to move between virtual machines which would otherwise be isolated.  Our project intends to achieve the following goals:
  1. Configure a virtualization platform running the Xen hypervisor using current recommended hardening techniques.
  2. Perform a security assessment of the hypervisor in order to discover the attack surface of the hardware to discover a vulnerability which still exists.
  3. Attempt to exploit the found vulnerability to gain unrestricted access to the hypervisor.
  4. If successful, work with the security community to establish and implement a patch for this vulnerability.
   
Through our work in assessing and securing the hypervisor, we hope to contribute to the current recommendations for hardening the hypervisor, resulting in better overall security and reducing the risk posed to users of virtualization technology.

# Project Timeline

![Gantt Chart](https://github.com/jhembree/IACapstone/blob/master/Gantt%20Chart.PNG)

# Risks

|Risk Name (Value)| Impact | Likelihood | Description |
|-----------------|--------|-----------|-------------|
|Scheduling conflict (48) | 6 | 8 | Inability to find common time to meet |
|Hardware Failure (45) | 9 | 5 | Hardware used to run the hypervisor experiences issues |
|Inadequate tools available (35) | 7 | 5 | Resources available unable to accomplish the target goal |
|Lack of technical skillset (25) | 5 | 5 | Failure to accomplish tasks due to limitations of technical knowledge |
|Unable to get hypervisor running (20) | 10 | 2 | Unable to run hypervisor |

# Application Requirements

## User Stories

As a **cloud service provider**, I want to **isolate client resources** so that I can **host multiple clients on the same hardware**.

Acceptance Criteria:
  -Multiple guest machines will be able to be supported by a single host machine.
  -Traversal between guest machines is not allowed.

As an **enterprise user**, I want to **seperate sensitive data on to its own virtual system** to **provide additional security**.

Acceptance Criteria:
  -Covert channels between virtual machines are locked down.

As a **healthcare provider**, I want to **isolate identifying information from other data** to **comply with HIPAA regulations**.

Acceptance Criteria:
  -Covert channels between virtual machines are locked down.

As a **researcher**, I want to **provide an isolated environment from production systems** so that I can **safely perform malware analysis**.

Acceptance Criteria:
  -Covert channels between vitural machines are locked down.
  -No ability exists to escape the virtual machine to gain access to the hypervisor.

As a **corporation**, I want to **run multiple virtual systems on the same hardware** in order to **reduce cost and space requirements**.

Acceptance Criteria:
  -Multiple guest machines will be able to be supported by a single host machine.

## Use Case Diagram

[Use Case Diagram](https://www.lucidchart.com/documents/edit/f0f7560f-db66-4d11-9ca7-3bdb4cb0d108#)

# Resources Needed

|Resource| Dr. Hale Needed? | Investigating Team Member | Description |
|--------|------------------|---------------------------|-------------|
|Xen Hypervisor| No | Jesse | Hypervisor to perform assessment on |
|VASTO| No | Jesse | Virtualization Assesment Toolkit | 
|Hardware to run resources on | No | Afnan | Physical hardware to setup hypervisor/vms on and test against|
|Supporting Research | No | Jesse/Afnan | Documentation to inform and direct security assessment|

# First Sprint

[Trello Board](https://trello.com/b/2b07xTJx/project-requirement-elicitation)

# Architectural Digaram
[Architectural Diagram](https://www.lucidchart.com/documents/edit/d78440fd-9738-45a1-834a-8bcae99f6db7)

# Activity Diagram
[Running Multiple Hosts](https://www.lucidchart.com/documents/edit/4fd0973d-98e7-42b5-8147-f9958b65124e)

[Resource Isolation](https://www.lucidchart.com/documents/edit/7af40024-f6ee-4d14-99c1-07de2381a616#)

[Memory Allocation](https://www.lucidchart.com/documents/edit/589fdd95-db19-4b26-ba88-af64aa795add)

[Memory Management](https://www.lucidchart.com/documents/edit/0ad002da-db5b-4b1d-9672-c4650f6ff46f)

[Network Bridging](https://www.lucidchart.com/documents/edit/ae99789e-141d-4d51-87d2-742b43d467c4)

# User Story Realization

At the current stage, we have successfully configured the Xen hypervisor and installed two paravirtualized hosts.  In the next week an HVM will be added to the configuration and full testing for vulnerabilites will begin.  The purpose of this phase was to ensure the hypervisor was appropriately configured and to ensure the implementation would support multiple guest systems being installed.

The initial attempt to configure the Xen hypervisor consisted of installing Debian Linux as a virtual machine inside VMware due to hardware limitations.  Multiple issues arose when attempting to load the Xen hypervisor on the Debian install.  Our assumption is that there were incompatability issues with trying to run a virtualization manager within a virtualized environment.  After the initial attempt failed, standalone hardware was procured and setup was able to be conducted succesfully.

The initial setup was performed following the steps provided in the Xen project's beginner guide [(link)](https://wiki.xenproject.org/wiki/Xen_Project_Beginners_Guide).  This consisted of installing Debian, partitioning the disk to allow for storage of the LVM (logical volume manager), and bridgeing the ethernet interface to allow the virtualized hosts to use the host OS ethernet port for networking.  Once Debian was installed, we installed the Xen Project packages to allow PVM and HVM guests to be installed on the system.  Next, two PVM hosts were configured using the xen-tools package and were both confirmed to be up and running.  These hosts were set up initially with SSH and HTTP services running.

Scans were run against the test environment using Nmap to ensure the ports were showing as accessible from an external environment.  This uncovered two objects of note.  First, OS fingerprinting quickly determined that the virtualized hosts were indeed running in a virtual environment due to the way their MAC addresses are configured.  Additionally, scans showed that on the host system, no services were running by default.  Due to this our current hypothesis is that any attacks against the hypervisor are going to be a result of weak security practices on the guest OS allowing us to use these hosts as a pivoting point against the host itself.
