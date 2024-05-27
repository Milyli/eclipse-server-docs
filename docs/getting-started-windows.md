<img src="../img/milylilogo.png" style="height: 80px;" />

# Hosting Eclipse w/ Docker Desktop on Windows

## Requirements

The following requirements assume that a local server or computer will be used for initial alpha testing. A machine that can have Docker Desktop installed on it must be selected. If a different hosting infrastructure is used, this document will provide high-level details that can be adapted to your preferred solution. Milyli is available to assist in working through your preferred testing setup!

**Note:** *For more information about Docker and its related components, please refer to
the [official documentation](https://www.docker.com/products/docker-desktop/).*

- Download and install [Docker Desktop](https://docs.docker.com/desktop/install/windows-install/) with WSL 2 backend.
  - Install using default settings, following all prompts, and restart as necessary.
  - If using Windows Server, ensure the nested virtualization Windows feature is enabled.
- PowerShell 5 or greater *(PowerShell 7+ suggested)*
- Windows Terminal *(Strongly suggested)*

## Setup

### Create directories for container volumes

1. Create a working Eclipse Directory *(e.g. c:\eclipse)*.
2. Create the following child directories:

    - c:\eclipse\db
    - c:\eclipse\https
    - c:\eclipse\logs
    - c:\eclipse\logs\metrics

3. Download the **eclipse.server.alpha** container image shared by Milyli and place it in the root directory.

### Create a developer certificate

The Eclipse container requires an HTTPS certificate PFX in order to start and run. For Alpha and Beta purposes, an self-signed developer certificate can be used. If possible, a proper certificate from a validate certificate authority should be used to generate the PFX.

A developer certificate can easily be created using Windows PowerShell and the following commands.

1. Define a certificate variable using the `New-SelfSignedCertificate` command.
2. Create (and retained) a password for the cert.
3. Export the PFX for the newly created Self Signed Certificate.

``` bash
$NewCert = New-SelfSignedCertificate -Type SSLServerAuthentication -DnsName localhost -CertStoreLocation Cert:\CurrentUser\My

$Pwd = ConvertTo-SecureString -String "eclipse" -Force -AsPlainText

Export-PfxCertificate -Cert $NewCert -FilePath "C:\eclipse\https\eclipse.pfx" -Password $Pwd
```

![CreateCert](img/linux/ubuntu/cert/cert-create.png)

## Installation

### Load the docker image

1. Open Windows Terminal.
2. Validate that the docker command is available by using the command `docker`.
  
    - Docker should print out usage instructions if everything is ready to go.

3. Type the following command to load the docker image

```bash
docker load -i c:\eclipse\eclipse.server.alpha
```

Confirm the image is loaded by using the command `docker image list`. Under repository, **eclipse.server** should be one of the visible rows.

### Start the docker container

With the image loaded we can now initialize the container with the `docker run` command.

The following command provides the necessary parameters to the web server in the container to take advantage of the supplied certificate, what port to run on, and location information for the previously created physical directories that will be used for the container volumes. Ensure to replace the password with the password you set on the PFX in the previous step.

```bash
docker run -dt -e "ASPNETCORE_URLS=https://+:443" -e
"ASPNETCORE_Kestrel__Certificates__Default__Password=INSERTPFXPASSWORDHERE" -e
"ASPNETCORE_Kestrel__Certificates__Default__Path=/https/eclipse.pfx" -e
"ASPNETCORE_HTTPS_PORT=443" -p 8000:443 --name Milyli.Eclipse.Server.Web -v
c:\eclipse\https:/https:ro -v c:\eclipse\db:/db:rw -v
c:\eclipse\logs:/logs:rw milyli.eclipse.server
```

If everything is set up correctly, this will initialize the docker container and you are good to go!

## Testing

To quickly validate that the container is up and running, the API documentation should now be available locally. Open a browser and navigate to *[https://localhost:8000/swagger/index.html](https://localhost:8000/swagger/index.html)*. API documentation should load after a few moments.