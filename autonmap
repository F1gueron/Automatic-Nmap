#!/bin/bash
#
#
#
GREEN='\033[0;32m'
RED='\033[0;31m'
YELLOW='\033[1;33m'
ORANGE='\033[0;33m'
NC='\033[0m'

echo -e "${GREEN}  █████╗ ██╗   ██╗████████╗ ██████╗ ███╗   ███╗ █████╗ ████████╗███████╗██████╗     ███╗   ██╗███╗   ███╗ █████╗ ██████╗ ${NC}"
echo -e "${GREEN} ██╔══██╗██║   ██║╚══██╔══╝██╔═══██╗████╗ ████║██╔══██╗╚══██╔══╝██╔════╝██╔══██╗    ████╗  ██║████╗ ████║██╔══██╗██╔══██╗${NC}"
echo -e "${GREEN} ███████║██║   ██║   ██║   ██║   ██║██╔████╔██║███████║   ██║   █████╗  ██║  ██║    ██╔██╗ ██║██╔████╔██║███████║██████╔╝${NC}"
echo -e "${GREEN} ██╔══██║██║   ██║   ██║   ██║   ██║██║╚██╔╝██║██╔══██║   ██║   ██╔══╝  ██║  ██║    ██║╚██╗██║██║╚██╔╝██║██╔══██║██╔═══╝ ${NC}"
echo -e "${GREEN} ██║  ██║╚██████╔╝   ██║   ╚██████╔╝██║ ╚═╝ ██║██║  ██║   ██║   ███████╗██████╔╝    ██║ ╚████║██║ ╚═╝ ██║██║  ██║██║     ${NC}"
echo -e "${GREEN} ╚═╝  ╚═╝ ╚═════╝    ╚═╝    ╚═════╝ ╚═╝     ╚═╝╚═╝  ╚═╝   ╚═╝   ╚══════╝╚═════╝     ╚═╝  ╚═══╝╚═╝     ╚═╝╚═╝  ╚═╝╚═╝     ${NC}"
echo -e "${ORANGE}                                       Perform targeted scans for machine solving${NC}"
echo -e "${ORANGE}------------------------------------------------------------------------------------------------------------------------${NC}"


# Check if the user provided at least one argument
if [ $# -lt 1 ]; then
    echo "Usage: $0 <target_ip> [options]"
    echo "Options:"
    echo "  -u  Perform UDP scan (by default, it uses TCP scan)"
    exit 1
fi

if [ "$EUID" -ne 0 ]; then
    echo -e "${RED}Please run this script with sudo."
    exit 1
fi

# Get the target from the command line argument
target=$1

# Set default options
udp_scan=false

# Parse command line options
while getopts "u" opt; do
    case $opt in
        u)
            udp_scan=true
            ;;
        \?)
            echo -e "${RED}Invalid option: -$OPTARG${NC}"
            exit 1
            ;;
    esac
done

# Shift command line arguments to exclude processed options
shift $((OPTIND-1))

# Run Nmap scan to retrieve open ports
echo -e "${ORANGE}Running Nmap scan for the open ports in $target...${NC}"
if [ "$udp_scan" = true ]; then
    open_ports=$(sudo nmap -p- --open -sS -sU -Pn --min-rate=7500 -T4 $target -oN "open_ports_$target"| grep ^[0-9] | cut -d '/' -f 1 | tr '\n' ',' | sed 's/,$//') 
else
    open_ports=$(sudo nmap -p- --open -sS -Pn --min-rate=7500 -T4 $target -oN "open_ports_$target"| grep ^[0-9] | cut -d '/' -f 1 | tr '\n' ',' | sed 's/,$//') 
fi

if [ -z "$open_ports" ]; then
    echo -e "${RED}[-]No open ports found. Exiting.${NC}"
    exit 1
fi

echo -e "${GREEN}[+]Scan completed! Open ports saved in open_ports_$target${NC}"


# Run detailed Nmap scan for open ports
echo -e "${ORANGE}Now, scanning for the info of the ports${NC}"
if [ "$udp_scan" = true ]; then
  info_ports=$(sudo nmap -p $open_ports -sCV -sU $target -oN "info_ports_$target")
else
  info_ports=$(sudo nmap -p $open_ports -sCV $target -oN "info_ports_$target")
fi

echo -e "${GREEN}[+]Scan completed! info of open ports saved in info_ports_$target"


