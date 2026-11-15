# HiTechCloud WHMCS Module 
A comprehensive WHMCS provisioning module for HiTechCloud that enables partners to sell virtual machines to their customers.

## Features

- **VM Lifecycle Management**: Create, suspend, unsuspend, and terminate VMs
- **Customer Self-Service**: Start, stop, restart VMs and access VNC console
- **Dynamic Resource Loading**: Fetch flavors, images, and networks directly from HiTechCloud API
- **Configurable Options**: Auto-generate WHMCS configurable options from HiTechCloud resources
- **Auto-Updates**: Automatic update checking and one-click installation
- **Multi-Region Support**: Support for multiple regions/projects

## Requirements

- WHMCS 8.0 or higher
- PHP 7.4 or higher
- HiTechCloud account with API access
- Application Credentials from HiTechCloud CMP

## Installation

### Method 1: Download Release

1. Download the latest release from [GitHub Releases](https://github.com/hitechcloudvn/hitechcloud-whmcs/releases)
2. Extract the ZIP file
3. Upload the `modules` folder to your WHMCS root directory
4. Go to **Setup → Addon Modules** and activate "HiTechCloud Manager"
5. Go to **Setup → Products/Services → Servers** and add a new server

### Method 2: Git Clone

```bash
cd /path/to/whmcs
# GitHub:
git clone https://github.com/hitechcloudvn/hitechcloud-whmcs.git temp_hitechcloud
# Or GitLab:
# git clone https://gitlab.com/hitechcloudvn/hitechcloud-whmcs.git temp_hitechcloud
cp -r temp_hitechcloud/modules/* modules/
rm -rf temp_hitechcloud
```

## Configuration

### Server Setup

1. Go to **Setup → Products/Services → Servers**
2. Click **Add New Server**
3. Configure:
   - **Name**: HiTechCloud (or your preferred name)
   - **Hostname**: Your CMP hostname (e.g., `cmp.dashboard.controlcloud.app`)
   - **Username**: Application Credential ID
   - **Password**: Application Credential Secret
   - **Access Hash**: Project path (e.g., `/openstack/14`)
   - **Type**: hitechloud
   - **Secure**: ✓ (checked for HTTPS)

4. Click **Test Connection** to verify

### Creating Application Credentials

1. Log into HiTechCloud Cloud Management Platform
2. Navigate to your project
3. Go to **Identity → Application Credentials**
4. Create new credentials with appropriate roles
5. Copy the Credential ID and Secret

### Product Setup

1. Create a new product or edit existing
2. Go to **Module Settings** tab
3. Select **HiTechCloud** as the module
4. Configure:
   - **Flavor**: Select or enter flavor UUID
   - **Image**: Select or enter image UUID
   - **Network**: Select or enter network UUID
   - **Security Groups**: Comma-separated list

### Custom Fields Setup

For each HiTechCloud product, you must create 3 custom fields to store VM information:

1. Go to **Setup → Products/Services → Products/Services**
2. Edit your HiTechCloud product
3. Go to **Custom Fields** tab
4. Create the following fields:

| Field Name | Field Type | Description | Admin Only |
|------------|------------|-------------|------------|
| `VM ID` | Text Box | Stores the OpenStack server UUID | Yes |
| `Public IPv4` | Text Box | Stores the VM's public IPv4 address | Yes |
| `Public IPv6` | Text Box | Stores the VM's public IPv6 address | Yes |

**Important:** The field names must match exactly as shown above (case-sensitive).

### HiTechCloud Manager (Admin Module)

Access via **Addons → HiTechCloud Manager**:

- **Flavors Tab**: View and load flavors from API
- **Images Tab**: View and load images from API
- **Networks Tab**: View and load networks from API
- **Config Options Tab**: Auto-generate WHMCS configurable options
- **Updates Tab**: Check for and install module updates

## Auto-Updates

The module checks for updates from this GitHub repository. To update:

1. Go to **Addons → HiTechCloud Manager**
2. Click **Updates** tab
3. If an update is available, click **Install Update**

Updates are downloaded directly from GitHub releases.

## File Structure

```
modules/
├── addons/
│   └── hitechloud_admin/
│       └── hitechloud_admin.php    # Admin management module
└── servers/
    └── hitechloud/
        ├── hitechloud.php          # Main provisioning module
        ├── hooks.php            # WHMCS hooks
        ├── lib/
        │   ├── hitechloudAPI.php   # API client
        │   └── hitechloudHelper.php # Helper functions
        └── templates/
            ├── overview.tpl     # Client area template
            ├── no_vm.tpl        # No VM template
            └── error.tpl        # Error template
```

## API Methods

The HiTechCloudAPI class provides these methods:

### Authentication
- `authenticate()` - Authenticate with Application Credentials
- `testConnection()` - Test API connectivity

### Compute
- `listFlavors()` - List available flavors
- `getFlavor($id)` - Get specific flavor
- `createServer($params)` - Create a new VM
- `getServer($id)` - Get VM details
- `deleteServer($id)` - Delete a VM
- `startServer($id)` - Start a VM
- `stopServer($id)` - Stop a VM
- `rebootServer($id)` - Reboot a VM
- `suspendServer($id)` - Suspend a VM
- `resumeServer($id)` - Resume a VM
- `resizeServer($id, $flavorId)` - Resize a VM
- `getVncConsole($id)` - Get VNC console URL

### Images
- `listImages()` - List available images
- `getImage($id)` - Get specific image

### Networks
- `listNetworks()` - List available networks
- `listSecurityGroups()` - List security groups
- `assignFloatingIP($serverId, $networkId)` - Assign floating IP

### Volumes
- `listVolumeTypes()` - List volume types
- `getVolume($id)` - Get volume details
- `createBootVolume($name, $imageId, $size)` - Create boot volume
- `extendVolume($id, $newSize)` - Extend volume size

## Troubleshooting

### HTTP 405 Error
- Ensure the Access Hash includes your project path (e.g., `/openstack/14`)
- The authentication URL should be: `https://hostname/openstack/14/v3/auth/tokens`

### Authentication Failed
- Verify Application Credential ID and Secret
- Check that credentials have appropriate permissions
- Ensure the project path is correct

### Resources Not Loading
- Verify server connection in **Setup → Servers**
- Check that the API endpoints are accessible
- Review WHMCS module debug logs

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

- **GitHub Issues**: [GitHub Issues](https://github.com/hitechcloudvn/hitechcloud-whmcs/issues)
- **GitLab Issues**: [GitLab Issues](https://gitlab.com/hitechcloudvn/hitechcloud-whmcs/-/issues)
- **Documentation**: [Wiki](https://github.com/hitechcloudvn/hitechcloud-whmcs/wiki)

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history.
