Ansible Firewall Role to deploy default config similar to stated below
=========
for tables in iptables ip6tables ; do
    # Flush existing rules
    $tables -F

    # Default policy
    $tables -P INPUT DROP
    $tables -P FORWARD ACCEPT
    $tables -P OUTPUT ACCEPT

    # Allow established inbound connections
    $tables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT

    # Allow icmp
    $tables -A INPUT -p icmp -j ACCEPT

    # Allow all loopback traffic
    $tables -A INPUT -i lo -j ACCEPT

    # Allow inbound SSH connection
    $tables -A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
done
