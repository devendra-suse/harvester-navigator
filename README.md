v# Harvester Troubleshooting via UI

![GitHub go.mod Go version](https://img.shields.io/github/go-mod/go-version/rajeshkio/harvester-navigator)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

**Harvester Troubleshooting via UI** is a comprehensive web-based dashboard designed to work with the Harvester support bundle simulator, providing real-time insights into Harvester-based Kubernetes virtual machines and their associated resources. The tool can also be used directly with live Harvester clusters for real-time monitoring and troubleshooting.

## 🎯 Purpose

This tool is designed to provide a **holistic view of Harvester clusters** and enable rapid issue identification. It's built primarily to accompany the **Harvester support bundle simulator** for troubleshooting scenarios, while also supporting direct connections to live Harvester clusters.

## ✨ Features

### 🌐 **Web Dashboard**
- **Real-time monitoring** of Harvester clusters via WebSocket connections
- **Interactive VM explorer** with detailed drill-down capabilities
- **Comprehensive node dashboard** showing Longhorn node status and disk information
- **Responsive design** optimized for desktop and mobile devices

### 🔍 **VM & Infrastructure Insights**
- **Complete VM lifecycle tracking** - Status, resources, and configuration details
- **Advanced storage analytics** - PVC, volumes, and Longhorn-specific information
- **Replica health monitoring** - Detailed replica status with networking information
- **VMI details** - Guest OS, memory usage, and network interface information
- **Pod relationship mapping** - VM to Pod to Node associations

### 🚨 **Intelligent Error Detection**
- **Automatic issue identification** - Missing volumes, failed replicas, connection problems
- **Structured error reporting** - Categorized by severity (Warning, Error, Critical)
- **Resource-specific diagnostics** - Pinpoint exactly which components are failing
- **Troubleshooting guidance** - Clear error messages with actionable information

### 📊 **Upgrade & Version Tracking**
- **Harvester upgrade history** - Track version progressions and upgrade status
- **Node-level upgrade status** - Monitor upgrade success across cluster nodes
- **Upgrade timeline** - See when upgrades were performed and their outcomes

## 🚀 Installation & Setup

### Prerequisites

- **Go 1.23+** for building from source
- **Harvester cluster** OR **Harvester support bundle simulator**
- **kubectl** configured with access to your environment
- **Web browser** for accessing the dashboard

### Option 1: Using with Harvester Support Bundle Simulator (Recommended)

```bash
# Start the Harvester simulator
support-bundle-kit simulator --reset

# Clone and build the troubleshooting UI
git clone https://github.com/rajeshkio/harvester-navigator.git
cd harvester-navigator
go build -o harvester-troubleshoot

# Start the web server (it will automatically connect to the simulator)
./harvester-troubleshoot

# Access the dashboard
open http://localhost:8080
```

### Option 2: Using with Live Harvester Cluster

```bash
# Set your kubeconfig environment variable
export KUBECONFIG=/path/to/your/harvester-kubeconfig

# Build and run the application
go build -o harvester-troubleshoot
./harvester-troubleshoot

# Access the dashboard
open http://localhost:8080
```

## 🔧 How It Works

### Web Dashboard Architecture
1. **Backend server** starts and establishes Kubernetes API connections
2. **Automatic environment detection** - Works with both simulator and live clusters
3. **Comprehensive data gathering**:
   - Fetches all Harvester VMs and their metadata
   - Retrieves Longhorn node status and disk information
   - Gathers PVC, volume, and replica details
   - Collects VMI information including guest OS details
   - Tracks Harvester upgrade history and status
4. **Intelligent error detection** identifies missing or failed resources
5. **Interactive UI** presents data with drill-down capabilities for rapid troubleshooting

### Connection Methods
- **Automatic**: Detects simulator kubeconfig at `~/.sim/admin.kubeconfig`
- **Manual**: Uses `KUBECONFIG` environment variable for live cluster connections
- **Fallback**: Uses default kubeconfig at `~/.kube/config`

## 🏗️ Project Structure

```
.
├── go.mod                 # Go module definition
├── go.sum                 # Go module checksums
├── index.html             # Web dashboard frontend
├── main.go                # Application entry point & WebSocket server
├── internal/              # Internal packages
│   ├── client/            # Kubernetes client initialization
│   ├── models/            # Data structures and types
│   │   └── types.go       # VM, Node, Replica, and Error models
│   └── services/          # Resource-specific service packages
│       ├── engine/        # Longhorn engine service
│       ├── pod/           # Pod information service
│       ├── pvc/           # PVC service for storage
│       ├── replicas/      # Replica monitoring service
│       ├── upgrade/       # Harvester upgrade tracking
│       ├── vm/            # Virtual Machine service
│       ├── vmi/           # VM Instance service (with guest OS)
│       └── volume/        # Longhorn volume service
└── pkg/                   # Exported packages
    └── display/           # Terminal formatting utilities (legacy)
```

## 🔧 How It Works

### Web Dashboard Architecture
1. **Backend server** starts and establishes Kubernetes API connections
2. **WebSocket connection** provides real-time data to the frontend
3. **Comprehensive data gathering**:
   - Fetches all Harvester VMs and their metadata
   - Retrieves Longhorn node status and disk information
   - Gathers PVC, volume, and replica details
   - Collects VMI information including guest OS details
   - Tracks Harvester upgrade history and status
4. **Intelligent error detection** identifies missing or failed resources
5. **Interactive UI** presents data with drill-down capabilities

### API Integration
- **Harvester VMs**: `apis/kubevirt.io/v1/virtualmachines`
- **VMI Details**: `apis/kubevirt.io/v1/virtualmachineinstances`
- **Longhorn Volumes**: `apis/longhorn.io/v1beta2/volumes`
- **Storage Replicas**: `apis/longhorn.io/v1beta2/replicas`
- **Harvester Upgrades**: `apis/harvesterhci.io/v1beta1/upgrades`
- **Kubernetes Resources**: Standard PVC, Pod, and Node APIs

## 📱 Dashboard Features

### Main Dashboard
- **Node Status Grid**: Real-time status of all Longhorn nodes
- **VM Overview Cards**: Quick status overview of all virtual machines
- **Live Connection Status**: WebSocket connectivity and data freshness

### VM Detail View
- **Resource Summary**: VM configuration, namespace, and image details
- **Pod Information**: Associated pod status and node placement
- **VMI Details**: Guest OS information, memory usage, IP addresses
- **Storage Analysis**: PVC status, volume health, and replica distribution
- **Error Diagnostics**: Detailed issue reporting with severity levels

### Node Detail View
- **Node Health**: Comprehensive node condition monitoring
- **Disk Management**: Storage capacity, utilization, and schedulability
- **Replica Distribution**: Which replicas are hosted on each disk

## 🎯 Use Cases

### Platform Operations
- **Health monitoring** of Harvester infrastructure
- **Proactive issue detection** before VM failures
- **Storage capacity planning** and optimization
- **Upgrade progress tracking** across cluster nodes

### Troubleshooting Scenarios
- **VM won't start**: Check PVC binding, volume availability, node resources
- **Storage issues**: Identify failed replicas, engine problems, or disk issues
- **Network problems**: Verify VMI interfaces and IP assignments
- **Performance issues**: Monitor resource allocation and node distribution

### Infrastructure Management
- **Cluster overview** for operations teams
- **Resource dependency verification** for applications
- **Storage performance analysis** for workload optimization

## 🚨 Error Detection & Reporting

The system automatically detects and categorizes:

- **🔴 Critical**: Volume completely missing, all replicas failed
- **🟡 Warning**: Individual replica issues, temporary connectivity problems
- **📋 Info**: Resource state changes, upgrade progress

Error messages include:
- **Resource type** (Volume, PVC, Pod, VMI, Replica, Engine)
- **Specific resource name** causing the issue
- **Detailed error description** with actionable information
- **Severity classification** for prioritization

## 🔄 Development & Contributing

### Setting up Development Environment

```bash
# Clone and enter directory
git clone https://github.com/rk280392/harvesterNavigator.git
cd harvesterNavigator

# Install dependencies
go mod tidy

# Run in development mode
go run main.go

# Build for production
go build -o harvester-troubleshoot
```

### Contributing Guidelines

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/amazing-feature`
3. **Make your changes** with proper testing
4. **Add documentation** for new features
5. **Commit changes**: `git commit -m 'Add amazing feature'`
6. **Push to branch**: `git push origin feature/amazing-feature`
7. **Create Pull Request**

### Code Standards
- Follow Go's [Effective Go](https://golang.org/doc/effective_go) guidelines
- Use meaningful variable and function names
- Add comprehensive error handling
- Include comments for complex logic
- Write tests for new functionality
- Maintain consistent code formatting


## 📄 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## 🤝 Support

- **Documentation**: [Project Wiki](https://github.com/rk280392/harvesterNavigator/wiki)
- **Issues**: [GitHub Issues](https://github.com/rk280392/harvesterNavigator/issues)
- **Discussions**: [GitHub Discussions](https://github.com/rk280392/harvesterNavigator/discussions)

---

*Built with ❤️ for the Harvester community*
