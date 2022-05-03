### Using PRA Protocal Tunnel instead of Putty/SSH Client

#### Step 1 Create Protocol Tunnel Jump Item
Name = Endpoint name

Jumpoint = 
Hostname/IP = Endpoint IP or name

Local Address = 127.0.0.1

  Local Port = 22

  Remote Port = 22

  Click "Add"

  Local Port = 5901

  Remote Port = 5901

  Click "Add"

Click "OK"

#### Step 2 Connect to Jump item

#### Step 3 Open VNC Viewer
Hostname / IP = Endpoint IP or name::5901
