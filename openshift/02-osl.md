# Red Hat OpenShift Local

## CodeReady Containers (CRC)

> _Red Hat CodeReady Containers (CRC) provides a minimal, preconfigured
 OpenShift 4 cluster on a laptop or desktop machine for development and
 testing purposes._

1. Go to [**Cluster Management > Cluster List > Create Cluster > [Tab] Local**](https://console.redhat.com/openshift/create/local).
2. Under **Step 1**, select **Linux** and **x86_64**.
3. Click on the **Download OpenShift Local** button.

`curl` shortcut:

```bash
curl -L \
  --output ~/crc-linux-amd64.tar.xz \
  "FIXME_URL"
```

## Pull Secret

Download or copy your pull secret. You'll be prompted for this information during installation.

1. Go to [**Cluster Management > Cluster List > Create Cluster > [Tab] Local**](https://console.redhat.com/openshift/create/local).
2. Under **Step 1**, click on the **Download pull secret** or
 **Copy pull secret** button.

## Installation

### Extraction

Extract `crc` and move it to `/usr/local/bin`.

```bash
tar -xvf ~/crc-linux-amd64.tar.xz --strip-components=1 \
  && sudo rm -rf ~/crc-linux-amd64.tar.xz LICENSE \
  && sudo mv ~/crc /usr/local/bin/.
```

Validation:

```bash
# command
which crc

# output
/usr/local/bin/crc

# command
crc version

# output
CRC version: 2.48.0+1aa46c
OpenShift version: 4.18.1
MicroShift version: 4.18.1
```

## Setup

The `crc setup` command will download resources and setup your environment to
 support OSL.

```bash
crc setup
```

## Configuration

`crc` has a number of defaults that can be modified with the `crc config` command.

### Resources

The default OSL VM is configured with 4 vCPUs, 10GiB of memory, and a 32GiB disk.
 Modify these resources with:

```bash
crc config set cpus FIXME_VCPU_COUNT      # default 4
crc config set memory FIXME_MEM_MB        # default 10752
crc config set drive-size FIXME_DRIVE_GB  # default 31
```

### Cluster Monitoring

OSL is meant to be minimal, so cluster monitoring feature is disabled by
 default. Enable/disable it with:

```bash
crc config set enable-cluster-monitoring FIXME_BOOLEAN  # default false
```

## Start

```bash
crc start [--pull-secret-file /path/to/secret.file]
```

If you downloaded the pull secret, make sure it's been saved to the local
 machine and add `--pull-secret-file /path/to/secret.file` to the `crc start`
 command above.

Otherwise, you'll be prompted to paste the pull secret during the startup
 process.

## Cluster Access

Once the start process is complete, login credentials and URLs will be provided
 to access and administer the cluster.

Red Hat assumes OSL will be run on a laptop or workstation, so access is set up
 under the assumption it will all be local. If remote access is desired, additional
 setps are required.

### firewalld

RHEL doesn't allow inbound access to 80/tcp or 443/tcp by default. They'll need
 to be allowed through the local firewall.

```bash
sudo firewall-cmd --permanent --add-service=http --zone=public
sudo firewall-cmd --permanent --add-service=https --zone=public
```

Once done, verify the updates:

```bash
# command
firewall-cmd --zone=public --list-all

# example output
$ firewall-cmd --zone=public --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp2s0
  sources:
  services: cockpit dhcpv6-client http https ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

Reload the firewall to ensure the updates are live.

```bash
sudo firewall-cmd --reload
```

### DNS

`crc` creates internal SSL certificates for its web services. Most browsers will
 not allow a secure connection when accessed via IP address since the certificate
 doesn't match. Local DNS records or a modified `/etc/hosts` file will resolve
 this issue.

```bash
# domain list
api.crc.testing 
canary-openshift-ingress-canary.apps-crc.testing 
console-openshift-console.apps-crc.testing 
default-route-openshift-image-registry.apps-crc.testing 
downloads-openshift-console.apps-crc.testing 
host.crc.testing 
oauth-openshift.apps-crc.testing 

# add to /etc/hosts
sudo bash -c 'echo "FIXME_IP_ADDRESS      api.crc.testing canary-openshift-ingress-canary.apps-crc.testing console-openshift-console.apps-crc.testing default-route-openshift-image-registry.apps-crc.testing downloads-openshift-console.apps-crc.testing host.crc.testing oauth-openshift.apps-crc.testing" >> /etc/hosts'
```
