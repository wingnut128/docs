# Rehydrating (rolling new AMIs/versions in) Instances (WIP)

## Nomad Servers

### When Using Autoscaling Groups

#### When Using the Enterprise Version
When using Nomad Servers in ASGs without the AZ Redundancy Configuration (you are running 3 or 5 servers), you need to set the upgrade_tag (set a version tag on the ASG and populate the tag in the nomad configuration).

When you set this version tag, you can do upgrades pretty easily, and be somewhat heavy handed with the approach. By using the custom_upgrades, as long as you follow a SemVer pattern for the tag, you can simply double the capacity of the nodes  and then watch for Nomad to do it's voter promotion/demotion. New nodes will come in as non-voters and as soon as enough new nodes are available (with the newer version tag), Nomad will start promoting the new nodes. Use the API to watch voter changes (curl or a simple Go utility)

Links:
* https://www.nomadproject.io/docs/configuration/server.html#upgrade_version
* https://github.com/hashicorp/nomad/tree/master/api


`Example Config:`
```javascript
autopilot {
    cleanup_dead_servers = true
    last_contact_threshold = 200ms
    max_trailing_logs = 250
    server_stabilization_time = "10s"
    enable_redundancy_zones = false
    disable_upgrade_migration = false
    enable_custom_upgrades = true
}
```

#### When Using OSS

### When Using Instances (no ASG)

## Consul Servers

## Nomad Clients