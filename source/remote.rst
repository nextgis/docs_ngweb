Requirements for remote access
=================================

All our products deployed on-premise (NextGIS Web, NextGIS GeoServices, NextGIS Toolbox) can be installed or upgraded remotely by our implementation specialists.

The requirements listed below are sufficient for:

* NextGIS Web Standard
* NextGIS Web Extended
* NextGIS GeoServices
* NextGIS Toolbox

If you have Enterprise support package, the access conditions are agreed upon on an individual basis.

To install or upgrade the software our specialists need remote access to your server. The options are:

- Access via SSH with superuser (root) permissions, e.g. using `sudo -i`.
- Connection using the server’s public IP address, or SSH port forwarding to a public IP address.

What **cannot** be used for installation and upgrades:

- VPN connection of any type.
- Remote access software such as TeamViewer, AnyDesk etc.
- Video calls and other screen sharing methods.

If you wish to limit the SSH access by IP addresses, contact our Support team to get the list of IP addresses used for the remote access.

Internet access is not necessary to install or upgrade of our products, but it makes the process quicker and easier, so we recommend using it if possible.

To install system software (Docker Engine, for example) the server needs to have Internet access with no proxy. If such Internet access cannot be provided, you have to install the system software yourself. You can find the details about the required system software in the installation manual.

We only need remote access to install or upgrade our software. After this process is finished, you can revoke the access. Usually installation/upgrade takes 1-2 business days to complete, but it some cases it may require more time. Keep that in mind when you plan for organizing the remote access.

In any case, it is your IT department that needs to install and configure the reverse proxy in order to provide HTTPS traffic encryption. In our installation manual you can find recommendations on configuring reverse proxy using Nginx as an example .

If you cannot provide remote access to your servers as described above, your specialists can perform installation/upgrade themselves using our manuals. In this case we provide consultations to answer the questions you may have.

To request an installation or upgrade, send an email from your designated contact to our Support team at support@nextgis.com, providing the access parameters, e.g.::

    **Subject:** NextGIS Web installation

    In accordance with Contract XXX dated DD.MM.YYYY, we kindly request the installation of NextGIS Web on our server.  Remote access parameters are as follows:

    - IP address: `198.51.100.1`
    - SSH port: `2022`
    - Username: `nextgis`
    - Password: `******`
