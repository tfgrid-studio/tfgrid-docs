# App Manifest Reference

**Complete specification for `tfgrid-compose.yaml`**

The app manifest defines everything about your application: metadata, resource requirements, deployment hooks, custom commands, and more.

---

## Minimal Manifest

The simplest valid manifest:

```yaml
name: my-app
version: "1.0.0"
description: My application

patterns:
  recommended: single-vm

hooks:
  setup: deployment/setup.sh
  configure: deployment/configure.sh
  healthcheck: deployment/healthcheck.sh
```

---

## Complete Reference

### Core Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | ✅ | Unique app identifier (lowercase, hyphens allowed) |
| `version` | string | ✅ | Version string (semantic or descriptive) |
| `description` | string | ✅ | Brief description of the app |

```yaml
name: my-app
version: "1.0.0"
description: My application for doing things
```

---

### Patterns

Specifies which deployment patterns the app supports.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `patterns.recommended` | string | ✅ | Default pattern to use |
| `patterns.supported` | list | ❌ | All patterns the app works with |

**Available patterns:** `single-vm`, `gateway`, `k3s`

```yaml
patterns:
  recommended: single-vm
  supported:
    - single-vm
    - gateway
```

---

### Hooks

Scripts executed during deployment lifecycle. All paths are relative to the app root.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `hooks.setup` | string | ✅ | Install dependencies, create directories |
| `hooks.configure` | string | ✅ | Configure services, start application |
| `hooks.healthcheck` | string | ✅ | Verify deployment is working |

```yaml
hooks:
  setup: deployment/setup.sh
  configure: deployment/configure.sh
  healthcheck: deployment/healthcheck.sh
```

**Hook execution order:** `setup` → `configure` → `healthcheck`

**Hook configuration (optional):**

```yaml
hook_config:
  configure:
    optional: true
    description: "SSL configuration (skipped for private deployments)"
```

---

### Variables

User-configurable variables that can be set at deploy time.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `variables.<name>.type` | string | ✅ | `string`, `number`, `boolean` |
| `variables.<name>.description` | string | ✅ | Help text for the variable |
| `variables.<name>.default` | any | ❌ | Default value |
| `variables.<name>.required` | boolean | ❌ | Whether variable must be set |

```yaml
variables:
  domain:
    type: string
    description: "Domain name for public access"
    default: ""
    required: false

  vm_cpu:
    type: number
    description: "VM CPU cores"
    default: 4
    required: false

  enable_ssl:
    type: boolean
    description: "Enable SSL/TLS"
    default: true
    required: false
```

**Usage at deploy time:**

```bash
tfgrid-compose up my-app --var domain=myapp.com --var vm_cpu=8
```

---

### Resources

Minimum and recommended resource requirements.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `resources.cpu.min` | number | ❌ | Minimum CPU cores |
| `resources.cpu.recommended` | number | ❌ | Recommended CPU cores |
| `resources.memory.min` | number | ❌ | Minimum memory in MB |
| `resources.memory.recommended` | number | ❌ | Recommended memory in MB |
| `resources.disk.min` | number | ❌ | Minimum disk in GB |
| `resources.disk.recommended` | number | ❌ | Recommended disk in GB |
| `resources.ports` | list | ❌ | Ports used by the app |

```yaml
resources:
  cpu:
    min: 2
    recommended: 4
  memory:
    min: 4096        # 4GB
    recommended: 8192  # 8GB
  disk:
    min: 50
    recommended: 100
  ports:
    - 22:ssh
    - 80:http
    - 443:https
    - 3000:app
```

---

### Environment

Environment variables available to hooks.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `environment[].name` | string | ✅ | Variable name |
| `environment[].required` | boolean | ❌ | Whether variable must be set |
| `environment[].default` | string | ❌ | Default value |
| `environment[].description` | string | ❌ | Help text |

```yaml
environment:
  - name: DOMAIN
    required: false
    description: "Domain name for public access"

  - name: API_KEY
    required: true
    description: "API key for external service"

  - name: LOG_LEVEL
    default: "info"
    description: "Logging verbosity"
```

---

### Commands

Custom commands that extend `tfgrid-compose` for your app.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `commands.<name>.script` | string | ✅ | Path to script on the VM |
| `commands.<name>.description` | string | ✅ | Help text for the command |
| `commands.<name>.args` | string | ❌ | Argument syntax |
| `commands.<name>.interactive` | boolean | ❌ | Requires user input |
| `commands.<name>.allocate_tty` | boolean | ❌ | Allocate TTY for command |
| `commands.<name>.uses_context` | string | ❌ | Context variable to use |
| `commands.<name>.save_to_context` | string | ❌ | Save result to context |

```yaml
commands:
  launch:
    script: /opt/my-app/scripts/launch.sh
    description: "Open web interface in browser"

  create:
    script: /opt/my-app/scripts/create.sh
    description: "Create a new item"
    args: "[name] [description]"
    interactive: true

  status:
    script: /opt/my-app/scripts/status.sh
    description: "Show application status"

  logs:
    script: /opt/my-app/scripts/logs.sh
    description: "View application logs"
    args: "[--follow] [--lines N]"
```

**Usage:**

```bash
tfgrid-compose launch
tfgrid-compose create my-item "Description here"
tfgrid-compose status
tfgrid-compose logs --follow
```

---

### Network

Network configuration for the deployment.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `network.main` | string | ❌ | Primary network: `wireguard`, `mycelium`, `public` |
| `network.inter_node` | string | ❌ | Inter-node network (multi-VM) |
| `network.mode` | string | ❌ | Access mode: `wireguard-only`, `mycelium-only`, `both` |
| `network.public_ipv4` | boolean | ❌ | Request public IPv4 |

```yaml
network:
  main: wireguard
  inter_node: wireguard
  mode: both
  public_ipv4: false
```

---

### Dependencies

System and external dependencies.

```yaml
dependencies:
  system:
    - git
    - curl
    - nginx
  external:
    - nodejs: ">=18.0.0"
    - npm: ">=9.0.0"
```

---

### Logging

Logging configuration.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `logging.method` | string | ❌ | `systemd`, `file`, `stdout` |
| `logging.location` | string | ❌ | Log directory path |

```yaml
logging:
  method: systemd
  location: /var/log/my-app/
```

---

### Status

Health check configuration.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `status.method` | string | ❌ | `http`, `script`, `tcp` |
| `status.endpoint` | string | ❌ | HTTP path or script path |
| `status.port` | number | ❌ | Port for HTTP/TCP checks |

```yaml
status:
  method: http
  endpoint: "/health"
  port: 8080
```

Or script-based:

```yaml
status:
  method: script
  endpoint: deployment/healthcheck.sh
```

---

### Documentation

Links to documentation files.

```yaml
documentation:
  readme: README.md
  architecture: docs/ARCHITECTURE.md
  troubleshooting: docs/TROUBLESHOOTING.md
```

---

### Tags

Tags for registry discovery.

```yaml
tags:
  - ai
  - development
  - web
  - database
```

---

### Metadata

Additional metadata about the app.

```yaml
metadata:
  category: development
  maturity: stable          # alpha, beta, stable, production
  support_level: official   # official, community, experimental
  estimated_cost: medium    # low, medium, high
  deployment_time: "5 minutes"
  complexity_level: beginner  # beginner, intermediate, advanced
```

---

## Complete Example

Here's a comprehensive manifest based on real-world usage:

```yaml
name: my-web-app
version: "1.0.0"
description: Full-stack web application with database

patterns:
  recommended: single-vm
  supported:
    - single-vm
    - gateway

network:
  main: wireguard
  mode: both
  public_ipv4: false

hooks:
  setup: deployment/setup.sh
  configure: deployment/configure.sh
  healthcheck: deployment/healthcheck.sh

variables:
  domain:
    type: string
    description: "Domain name for public access"
    default: ""
    required: false

  db_password:
    type: string
    description: "Database password"
    required: true

  vm_cpu:
    type: number
    description: "VM CPU cores"
    default: 4
    required: false

  vm_memory:
    type: number
    description: "VM memory in MB"
    default: 8192
    required: false

resources:
  cpu:
    min: 2
    recommended: 4
  memory:
    min: 4096
    recommended: 8192
  disk:
    min: 50
    recommended: 100
  ports:
    - 22:ssh
    - 80:http
    - 443:https
    - 5432:postgres

environment:
  - name: DOMAIN
    required: false
    description: "Domain for public access"

  - name: DB_PASSWORD
    required: true
    description: "Database password"

  - name: NODE_ENV
    default: "production"
    description: "Node environment"

dependencies:
  system:
    - git
    - curl
    - nginx
    - postgresql
  external:
    - nodejs: ">=18.0.0"

commands:
  launch:
    script: /opt/my-web-app/scripts/launch.sh
    description: "Open web interface in browser"

  migrate:
    script: /opt/my-web-app/scripts/migrate.sh
    description: "Run database migrations"

  logs:
    script: /opt/my-web-app/scripts/logs.sh
    description: "View application logs"
    args: "[--follow]"

  backup:
    script: /opt/my-web-app/scripts/backup.sh
    description: "Create database backup"

logging:
  method: systemd
  location: /var/log/my-web-app/

status:
  method: http
  endpoint: "/api/health"
  port: 3000

documentation:
  readme: README.md

tags:
  - web
  - nodejs
  - postgresql

metadata:
  category: web
  maturity: stable
  support_level: community
  deployment_time: "5 minutes"
```

---

## Validation

Validate your manifest before deploying:

```bash
# Check YAML syntax
cat tfgrid-compose.yaml | python3 -c "import yaml, sys; yaml.safe_load(sys.stdin)"

# Test deployment
tfgrid-compose up . --dry-run
```

---

## See Also

- **[Custom Apps Guide](../guides/custom-apps.md)** - Complete guide to creating apps
- **[Tarball Releases Guide](../guides/tarball-releases.md)** - Versioned release workflows
- **[Pattern Contract](../development/pattern-contract.md)** - How patterns work
