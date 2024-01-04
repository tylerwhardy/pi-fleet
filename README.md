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

