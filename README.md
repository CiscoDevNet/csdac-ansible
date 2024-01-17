Cisco Secure Dynamic Attributes Connector 
=========================================

This README outlines instructions on how to install Cisco Secure Dynamic Attributes Connector for Cisco Secure Firewall Management Center.

**Installation**

Follow instructions outlined at [Cisco Secure Dynamic Attributes Connector Configuration Guide 2.3](https://www.cisco.com/c/en/us/td/docs/security/secure-firewall/integrations/dynamic-attributes-connector/230/cisco-secure-dynamic-attributes-connector-v230.html )

**Example Installation**

Default option

```
ansible-playbook default_playbook.yml --ask-become-pass
```

With an option for self signed certificate

```
ansible-playbook default_playbook.yml --ask-become-pass --extra-vars "csdac_certificate_domain=domain.example.com csdac_certificate_country_name=US csdac_certificate_organization_name=Cisco csdac_certificate_organization_unit_name=Engineering" 
```

>The optional CSDAC web certificate gets saved in the ~/csdac/app/config/certs directory and user can choose to import the certificate in browser.

With proxy configuration if your Linux host is behind an internet proxy

```
ansible-playbook default_playbook.yml --ask-become-pass --extra-vars "csdac_proxy_enabled=true csdac_http_proxy_url=http://192.168.19.10:80 csdac_https_proxy_url=http://192.168.19.10:80"
```

Once ansible installs CSDAC application successfully, user will see the following message at the end. 

```
00:35:17 TASK [csdac : Post task] *******************************************************00:35:17 ok: [10.10.10.15] => {00:35:17    
"msg": "Please login in to https://<ip address> to configure csdac application."00:35:17 }00:35:17
```
## Ansible Install Scripts

This repository contains Ansible install scripts for setting up the environment. The scripts have been improved to include the following checks and validations:

**Reachability of ECR/S3 Buckets**: The install scripts now include checks to ensure the reachability of the ECR (Elastic Container Registry) and S3 buckets. This ensures that the necessary Docker images and files can be accessed and downloaded during the installation process.

**Disk Space Check for Install**: Before proceeding with the installation, the scripts perform a disk space check to ensure that sufficient disk space is available. The minimum disk space required for a successful installation is 100GB. This check helps prevent installation failures due to insufficient storage capacity.

**Network Connectivity for Package and Image Downloads**: The scripts validate the network connectivity to download the required packages and Docker images. This ensures that the installation process can smoothly retrieve the necessary dependencies from the internet.

By including these checks and validations, the Ansible install scripts provide a more robust and reliable installation experience. They help ensure that all the required resources are accessible and available before proceeding with the installation.

The following FQDNs must be reachable directly or via proxy:

        - https://csdac-cosign.s3.us-west-1.amazonaws.com/
        - https://public.ecr.aws/
        - https://github.com/
        - https://pypi.org/
        - https://download.docker.com/ (docker apt/yum repo)

***Notably, if you are managing apt/yum repos via centralized management server and do not have direct internet access outside of via proxy, the download of the repo file is handled by proxy, but the repo file that is added to apt/yum expects direct internet access.***

You may choose to handle this repo via centralized management and thus can comment out the "Add Docker Repository" task depending on your OS flavor:

        - ~.ansible/collections/ansible_collections/cisco/csdac/roles/csdac/tasks/setup_RHEL.yml
        - ~.ansible/collections/ansible_collections/cisco/csdac/roles/csdac/tasks/setup_Ubuntu.yml

Otherwise, you may choose to use your proxy to reach this specific repo. For yum on RHEL this is possible by adding the following to each repo in docker-ce.repo:

        proxy=http://proxy.example.com:3128

If you encounter any issues or have any questions, please feel free to reach out to our support team for assistance.

Thank you for choosing our solution!

## Configuration Guide

  [CSDAC Configuration Guide]( https://www.cisco.com/c/en/us/td/docs/security/secure-firewall/integrations/dynamic-attributes-connector/220/cisco-secure-dynamic-attributes-connector-v220.html )

 
## Ansible collection

See [Ansible  collections](https://docs.ansible.com/ansible/latest/user_guide/collections_using.html) for more details. 

## Code of Conduct

This collection follows the Ansible project's [Code of Conduct](https://docs.ansible.com/ansible/latest/community/code_of_conduct.html). Please read and familiarize yourself with this document. 

## Releasing, Versioning and Deprecation

This collection follows [Semantic Versioning](https://semver.org/). More details on versioning can be found in the [Ansible docs](https://docs.ansible.com/ansible/latest/dev_guide/developing_collections.html#collection-versions).
