## GCP VM Setup and Prep
- Create SSH Key Pairs for VM Auth
	- https://cloud.google.com/compute/docs/connect/create-ssh-keys
	- ssh-keygen -t rsa -f ~/.ssh/gcp -C bs
- Attach the public key to the GCP project in the metadata of the **Compute Engine** 
- Create VM
- Login to VM through local terminal.
	- https://cloud.google.com/compute/docs/connect/standard-ssh#openssh-client
	- Check linux machine spec: **htop** 
## Configuring the VM
1. Python And Anaconda
	1. Navigate to Anaconda website. Copy link to linux download for a wget: `wget https://repo.anaconda.com/archive/Anaconda3-2024.10-1-Linux-x86_64.sh`
	2. Execute the script to install anaconda: `bash https://repo.anaconda.com/archive/Anaconda3-2024.10-1-Linux-x86_64.sh`
	3. Logout of VM and back in. Anaconda's environment will activate each time we're logged in (although this can be disabled in the .bashrc if needed)
2. Python Environment Packages
	1. We need psycopg2 (or equivalent) to interact with postgres.
		1. pip install psycopg2-binary 
3. Running Jupyter Notebooks on the VM
	1. Navigate to desired directory and in the terminal run `Juypter Notebook`
4. Docker
	1. install docker.io
		1. `sudo apt-get update` - this updates the packages in linux terminal
		2. `sudo apt-get update`
	2. Run docker without sudo:
		1. https://docs.docker.com/engine/install/linux-postinstall/#:~:text=If%20you%20don't%20want,members%20of%20the%20docker%20group.
	3. Install Docker Compose
		1. https://github.com/docker/compose/releases/download/v2.32.4/docker-compose-linux-x86_64
	4. Make docker-compose visible as a global command by adding it to the PATH. Add this to the bashRC file on the VM: `export PATH="${HOME}/bin:${PATH}"` . This prepends the path to the docker-compose binary into the global path. Recall, the path is effectively a list of directories separated by colons, searched order from left to right. **We must prepend the path else we'll lose all previous search paths!** 
	5. Re-evaluate the .bashrc file to sync the updates to the file: `source .bashrc`
	6. Run command to test: `docker-compose`
	7. Run docker-compose once you've changed directories to here: `/home/bs/data-engineering-zoomcamp/01-docker-terraform/2_docker_sql/` the command to run is `docker-compose up`. This will create the pgadmin and postgres services.
5.  Configure VSCode on local machine to SSH into GCP VM. *This provides a better user experience*
	1. Create a config file on local machine. With an entry like so:   
	``` Config:
	Host de-zoomcamp-vm
		HostName 34.140.86.54
		User bs
		IdentityFile C:\Users\benja\.ssh\gcp
	```
	2. Install **Remoter SSH** plugin in VSCode.
	3. In VSCode Connect to Host (Select Remote-SSH) and the config Host will appear.
	4. Clone the Github Repo for DEZoomcamp: `git clone https://github.com/DataTalksClub/data-engineering-zoomcamp.git` ensure this is cloned on the VM and not on the local machine, of course.
	5. Remember, once the remote connection has been established, we can forward the ports from the virtual machine. This allows us to see the applications through the webbrowser on our local vm. Check the docker-compose.yaml file out to find the ports that were configured. 
6.  **Terraform**
	1. Website for the binary, we don't need a package manager: https://developer.hashicorp.com/terraform/install?product_intent=terraform 
		1. Download the binary: `wget https://releases.hashicorp.com/terraform/1.10.5/terraform_1.10.5_linux_amd64.zip`
		2. To unzip this, get **unzip** package: `sudo apt-get install unzip`
	2. Google Cloud Credentials for Service Account so we can download a .json file for authentication. We need to use SFTP.
		1. In the local terminal `sftp de-zoomcamp-vm` . This is how we transfer files to/from the local machine.