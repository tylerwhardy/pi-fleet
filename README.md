# pi-fleet
Here's how to enroll a Raspberry Pi into Fleet without using the Fleet install package (since they don't support ARM64 :( )


# Assumptions:
ca.crt is your personal root CA
ca.crt was used to sign the Fleet SSL certificate

# Create 
sudo mkdir /etc/pifleet

# Download the latest ARM64 OSQuery

curl -LO https://pkg.osquery.io/linux/osquery-5.10.2_1.linux_aarch64.tar.gz

# Unzip OSQuery and Install

sudo tar -xvf osquery-5.10.2_1.linux_aarch64.tar.gz -C /

# Update hardcoded CA to have our own (Change the CA name if you care)

echo "lan-rootca-01v.local CA" | sudo tee -a /opt/osquery/share/osquery/certs/certs.pem
cat ca.crt | sudo tee -a /opt/osquery/share/osquery/certs/certs.pem

# Generate the fleet.pem file from Fleet and load to /etc/pifleet/fleet.pem (or whatever you name it
sudo mv fleet.pem /etc/pifleet/fleet.pem

# Generate the flagfile.txt and move to /etc/flagfile.txt with any necessary path updates 
On your host: 
scp flagfile.txt user@yourtarget.local:~/ 

On your target: 
nano flagfile.txt
sudo mv flagfile.txt /etc/pifleet/

# Create the pi-fleet.service file per the file in this repository. Update anything specific to your device
sudo nano /etc/systemd/system/pi-fleet.service

# Optional: Update your /etc/hosts to have the fleet IP/hostname if necessary
sudo nano /etc/hosts

# Optional: Update your cloud-init files to have fleet IP/hostname if necessary (ie managed deployment)
sudo nano /etc/cloud/templates/hosts.debian.tmpl
sudo nano /etc/cloud/cloud.cfg

# Create symlinks for the service

sudo systemctl enable pi-fleet.service

# Either reboot the server or activate the service

sudo systemctl start pi-fleet.service
