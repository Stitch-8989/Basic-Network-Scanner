# Basic-Network-Scanner
# A tool to scan a local network and list all active devices.
import subprocess
import platform

# Function to check if the host is up
def ping(host):
    # Determine the parameter based on the OS
    param = '-n' if platform.system().lower() == 'windows' else '-c'
    # Build the command
    command = ['ping', param, '1', host]
    # Run the command
    result = subprocess.run(command, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    # Check if the ping was successful
    return result.returncode == 0

# Function to scan a range of IP addresses
def scan_network(network_prefix):
    active_hosts = []
    for i in range(1, 255):
        ip = f"{network_prefix}.{i}"
        if ping(ip):
            active_hosts.append(ip)
            print(f"{ip} is active")
    return active_hosts

# Main function
if __name__ == "__main__":
    # Define the network prefix (e.g., '192.168.1')
    network_prefix = input("Enter the network prefix: ")
    print(f"Scanning network: {network_prefix}.0/24")
    active_hosts = scan_network(network_prefix)
    print("\nActive hosts:")
    for host in active_hosts:
        print(host)
