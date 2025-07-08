\# reset\_firewall\_ports.sh

\# Save path: /home/wofl/scripts/reset\_firewall\_ports.sh

\# Make executable: chmod +x reset\_firewall\_ports.sh



\#!/bin/bash



\# Reset UFW and disable to start clean

sudo ufw --force reset

sudo ufw disable



\# Set default policies

sudo ufw default deny incoming

sudo ufw default allow outgoing



\# Allow required ports (IPv4 + IPv6)

\# Manual list from firewall\_ports.txt

ALLOW\_PORTS=(

    "22/tcp"

    "80/tcp"

    "443/tcp"

    "5000"

    "22 from 138.197.11.134"

    "8081/tcp"

    "8000/tcp"

    "8080/tcp"

    "9876/tcp"

    "5000/tcp"

    "Nginx Full"



    "22/tcp (v6)"

    "80/tcp (v6)"

    "443/tcp (v6)"

    "5000 (v6)"

    "8081/tcp (v6)"

    "8000/tcp (v6)"

    "8080/tcp (v6)"

    "9876/tcp (v6)"

    "5000/tcp (v6)"

    "Nginx Full (v6)"

)



\# Apply rules

for rule in "${ALLOW\_PORTS\[@]}"; do

    if \[\[ "$rule" == \*"(v6)"\* ]]; then

        cleaned\_rule=$(echo "$rule" | sed 's/ (v6)//')

        sudo ufw allow "$cleaned\_rule" proto tcp comment "Re-added from reset list"

    elif \[\[ "$rule" == \*"from"\* ]]; then

        sudo ufw allow $rule

    elif \[\[ "$rule" == "Nginx Full" ]]; then

        sudo ufw allow 'Nginx Full'

    else

        sudo ufw allow "$rule"

    fi

done



\# Enable UFW

sudo ufw enable



\# Done

echo "✅ Firewall cleaned and reset to base rules."





\# chmod +x ~/scripts/reset\_firewall\_ports.sh

\# bash ~/scripts/reset\_firewall\_ports.sh



