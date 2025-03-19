# SIEM-Agent-Test Ver1
import os
import shutil
import subprocess
import urllib.request
import sys

# Define paths and variables
NXLOG_INSTALLER_URL = "https://nxlog.co/downloads/nxlog-ce#nxlog-community-edition"  # Replace with the actual URL
NXLOG_DIR = r"C:\Program Files (x86)\nxlog"
CERT_DIR = r"C:\nxlog\certs"
CERT_FILE = "mycert.crt"  # Your certificate file name
KEY_FILE = "mykey.key"  # Your private key file name
NXLOG_INSTALLER = "nxlog-ce-x.y.z.msi"  # Replace with the actual installer file name

def download_nxlog_installer():
    """Download the NXLog installer."""
    if not os.path.exists(NXLOG_INSTALLER):
        print(f"Downloading NXLog installer from {NXLOG_INSTALLER_URL}...")
        urllib.request.urlretrieve(NXLOG_INSTALLER_URL, NXLOG_INSTALLER)
    else:
        print("NXLog installer already downloaded.")
        
def sysmon_installer();
     """Download the Sysmon installer."""
     if not os

def install_nxlog():
    """Install NXLog if not already installed."""
    if os.path.exists(NXLOG_DIR):
        print("NXLog is already installed.")
    else:
        print("Installing NXLog...")
        subprocess.run(["msiexec", "/i", NXLOG_INSTALLER, "/quiet", "/norestart"], check=True)

def setup_certificate_directory():
    """Create certificate directory if it does not exist."""
    if not os.path.exists(CERT_DIR):
        print(f"Creating directory for certificates: {CERT_DIR}")
        os.makedirs(CERT_DIR)
    else:
        print("Certificate directory already exists.")

def copy_certificates():
    """Copy the certificate files to the NXLog directory."""
    if os.path.exists(CERT_FILE):
        shutil.copy(CERT_FILE, os.path.join(CERT_DIR, CERT_FILE))
        print(f"Copied certificate file to {CERT_DIR}.")
    else:
        print(f"Certificate file {CERT_FILE} not found. Exiting.")
        sys.exit(1)

    if os.path.exists(KEY_FILE):
        shutil.copy(KEY_FILE, os.path.join(CERT_DIR, KEY_FILE))
        print(f"Copied private key file to {CERT_DIR}.")
    else:
        print(f"Private key file {KEY_FILE} not found. Exiting.")
        sys.exit(1)

def update_nxlog_configuration():
    """Update NXLog configuration to use SSL certificates."""
    nxlog_conf_path = os.path.join(NXLOG_DIR, "conf", "nxlog.conf")

    # Append SSL/TLS configuration to the nxlog.conf
    ssl_config = f"""
## SSL Configuration
<Input ssl_input>
    Module im_msvistalog
    Port 6514
    Exec $Certificates = "{CERT_DIR}\\{CERT_FILE}";
    Exec $PrivateKey = "{CERT_DIR}\\{KEY_FILE}";
</Input>
"""
    with open(nxlog_conf_path, "a") as conf_file:
        conf_file.write(ssl_config)
        print(f"Updated NXLog configuration at {nxlog_conf_path}.")
        


def start_nxlog_service():
    """Start the NXLog service."""
    print("Starting NXLog service...")
    subprocess.run(["net", "start", "nxlog"], check=True)

def main():
    """Main installation routine."""
    download_nxlog_installer()
    install_nxlog()
    if not is_sysmon_installed():
        install_sysmon()
    else:
        print("Sysmon is already installed.")
    setup_certificate_directory()
    copy_certificates()
    update_nxlog_configuration()
    start_nxlog_service()
    print("Installation and configuration complete!")

if __name__ == "__main__":
    main()

