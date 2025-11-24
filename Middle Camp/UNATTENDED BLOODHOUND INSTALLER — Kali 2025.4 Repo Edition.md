## BloodHound v8.3.1

Save as: `bloodhound_full_unattended.sh`

> ⚠️ _Replace `YOUR_SECURE_PASSWORD_HERE` before running._
> Mine is: `admin::Xtrn$+]ss2Zj4E`


```
#!/usr/bin/env bash
set -e

# ------------------------------
# CONFIG
# ------------------------------
NEO4J_PASSWORD="YOUR_SECURE_PASSWORD_HERE"
INSTALL_DIR="$HOME/.local/share/bloodhound"
DESKTOP_DIR="$HOME/.local/share/applications"
SERVICE_USER="$USER"
# ------------------------------

echo "[*] Starting full unattended BloodHound installer..."

sudo DEBIAN_FRONTEND=noninteractive apt update -y
sudo DEBIAN_FRONTEND=noninteractive apt install -y bloodhound neo4j cypher-shell

echo "[*] Enabling and starting Neo4j..."
sudo systemctl enable neo4j
sudo systemctl start neo4j
sleep 5

echo "[*] Setting Neo4j password..."
# Force password update (idempotent)
echo "ALTER USER neo4j SET PASSWORD '$NEO4J_PASSWORD';" | cypher-shell -u neo4j -p neo4j 2>/dev/null || true

echo "[*] Creating BloodHound wrapper directory: $INSTALL_DIR"
mkdir -p "$INSTALL_DIR"

# Create launcher wrapper (Kali repo installs to /usr/bin/bloodhound)
echo "[*] Creating /usr/local/bin/bloodhound wrapper..."
sudo bash -c "cat >/usr/local/bin/bloodhound" <<EOF
#!/bin/bash
exec /usr/bin/bloodhound "\$@"
EOF
sudo chmod +x /usr/local/bin/bloodhound

echo "[*] Creating desktop launcher..."
mkdir -p "$DESKTOP_DIR"

cat > "$DESKTOP_DIR/bloodhound.desktop" <<EOF
[Desktop Entry]
Name=BloodHound
Exec=/usr/local/bin/bloodhound
Icon=bloodhound
Type=Application
Categories=Utility;Security;
Terminal=false
EOF

chmod +x "$DESKTOP_DIR/bloodhound.desktop"

echo "[*] Creating BloodHound+Neo4j systemd service..."

sudo bash -c "cat >/etc/systemd/system/bloodhound-suite.service" <<EOF
[Unit]
Description=BloodHound + Neo4j Startup Service
After=network.target neo4j.service
Wants=neo4j.service

[Service]
Type=simple
User=$SERVICE_USER
ExecStart=/usr/local/bin/bloodhound
Restart=on-failure
Environment=DISPLAY=:0

[Install]
WantedBy=default.target
EOF

echo "[*] Enabling BloodHound suite systemd service..."
sudo systemctl daemon-reload
sudo systemctl enable bloodhound-suite.service

echo ""
echo "====================================================="
echo "  BloodHound Installed Successfully (Kali Repository)"
echo "====================================================="
echo "BloodHound executable: /usr/local/bin/bloodhound"
echo "Desktop launcher: $DESKTOP_DIR/bloodhound.desktop"
echo ""
echo "Neo4j console: http://localhost:7474"
echo "Neo4j login:"
echo "   username: neo4j"
echo "   password: $NEO4J_PASSWORD"
echo ""
echo "BloodHound + Neo4j auto-start service:"
echo "   systemctl status bloodhound-suite.service"
echo ""
echo "====================================================="

```

▶️ **Run the script**

```
chmod +x bloodhound_full_unattended.sh
./bloodhound_full_unattended.sh
```

# 📝 **What this script does (in plain English)**

### ✔ 1. Installs BloodHound from Kali official repositories

The GUI is pulled from the supported Kali packages—not from GitHub.

### ✔ 2. Neo4j password is automatically set

No browser login required.

### ✔ 3. Adds `/usr/local/bin/bloodhound`

You can now run:

```
bloodhound
```

from any directory.

### ✔ 4. Installs a desktop launcher

BloodHound shows up in your app menu.

### ✔ 5. Creates a systemd service

Starts on login:

```
systemctl enable bloodhound-suite.service
```

Check status:

```
systemctl status bloodhound-suite.service
```

Neo4j still runs as its own service.

You can consult [[BloodHound v8 Defensive Toolkit]].


**Next step:**  [[Bloodhound analysis]]
