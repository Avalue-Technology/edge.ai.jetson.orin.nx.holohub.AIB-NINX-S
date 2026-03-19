# Introduction
This article primarily introduces how to clone Holohub sample code, buid and run Holohub sample code on the AIB-NINX-S Ubuntu 22.04 environment provided by Avalue Technology Inc.
Holohub is based on Holoscan SDK medical sample codes repository.

# Prerequisite
Before start, please make sure you already installed Docker for the device.
If you haven't installed Docker for the device yet, please refer [edge.ai.jetson.orin.nx.holoscan.AIB-NINX-S](https://github.com/Avalue-Technology/edge.ai.jetson.orin.nx.holoscan.AIB-NINX-S) section: ***Install Docker***, ***Check Docker Information***, ***Add Current User into docker Group***.

# Install Docker Dependency for the Holohub
```bash
sudo apt update
sudo apt install -y docker-buildx
sudo apt install -y nvidia-l4t-dla-compiler
```

# Clone Holohub Sample Code
```bash
cd ~
git clone https://github.com/nvidia-holoscan/holohub.git
cd holohub
```

## Change Max Workspace Size for the Sample Code - Endoscopy Tool Tracking
```bash
cd applications/endoscopy_tool_tracking/python
nano endoscopy_tool_tracking.yaml
```

Please press F6 and type: ***lstm_inference:*** and press ENTER.
Please find ***max_workspace_size: 2147483648*** and update it to be ***max_workspace_size: 1073741824***.

# Configure Jetson Orin NX Power Mode = MAXN
```bash
sudo nvpmodel -m 0
sudo jetson_clocks
```

# Create 12 GB Swap
```bash
sudo fallocate -l 12G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

# Clear Holohub Cache
```bash
./holohub clear-cache
```

# Build Holohub Specific Sample Code
```bash
./holohub build endoscopy_tool_tracking --language python
```

# Run Holohub Specific Sample Code with Building Docker 
```bash
xhost +local:docker
./holohub run endoscopy_tool_tracking --language python
```

# Run Holohub Specific Sample Code without Building Docker
If you previously compiled the sample code with Docker, you can then run the sample code with this parameter to avoid recompiling Docker again.
This way allows the sample code to run offline.
```bash
xhost +local:docker
./holohub run endoscopy_tool_tracking --language python --no-docker-build
```

# Running Endoscopy Tool Tracking 
![Endoscopy.Tool.Tracking.01.png](https://github.com/Avalue-Technology/edge.ai.jetson.orin.nx.holohub.AIB-NINX-S/blob/main/MarkdownDocumentImages/Endoscopy.Tool.Tracking.01.png?raw=true "Endoscopy.Tool.Tracking.01.png")


