# 💬 Synapse Service

A modular Docker Compose configuration system for Matrix Synapse with PostgreSQL backend, OIDC integration capabilities, and support for multiple environments.

## 🏗️ Project Structure

```sh
src/synapse/
├── components/                              # Source compose components
│   ├── base/                               # Base components
│   │   ├── docker-compose.yml              # Main Synapse + PostgreSQL services
│   │   ├── .env.example                    # Base environment variables
│   │   ├── Dockerfile                      # Custom Synapse image
│   │   └── homeserver.yaml                 # Base homeserver configuration
│   ├── environments/                       # Environment components
│   │   ├── devcontainer/
│   │   │   ├── docker-compose.yml          # DevContainer environment
│   │   │   └── .env.example                # DevContainer variables
│   │   ├── forwarding/
│   │   │   ├── docker-compose.yml          # Development with port forwarding
│   │   │   └── .env.example                # Forwarding variables
│   │   ├── letsencrypt/
│   │   │   ├── docker-compose.yml          # Let's Encrypt SSL
│   │   │   └── .env.example                # Let's Encrypt variables
│   │   └── step-ca/
│   │       ├── docker-compose.yml          # Step CA SSL
│   │       └── .env.example                # Step CA variables
│   └── extensions/                         # Extension components
│       └── oidc/                           # OIDC authentication extension
│           ├── docker-compose.yml          # OIDC provider configuration
│           └── .env.example                # OIDC variables
├── build/                        # Generated configurations (auto-generated)
│   ├── devcontainer/
│   │   ├── base/                 # DevContainer + base
│   │   └── oidc/                 # DevContainer + base + OIDC
│   ├── forwarding/
│   │   ├── base/                 # Development + base
│   │   └── oidc/                 # Development + base + OIDC
│   ├── letsencrypt/
│   │   ├── base/                 # Let's Encrypt + base
│   │   └── oidc/                 # Let's Encrypt + base + OIDC
│   └── step-ca/
│       ├── base/                 # Step CA + base
│       └── oidc/                 # Step CA + base + OIDC
├── build.sh                      # Build script
├── homeserver.oidc.yaml          # OIDC configuration template
└── README.md                     # This file
```

## 🚀 Quick Start

### 1. Build Configurations

Run the build script to generate all possible combinations:

```bash
./build.sh
```

This will create all combinations in the `build/` directory.

### 2. Choose Your Configuration

Navigate to the desired configuration directory:

```bash
# For development with port forwarding
cd build/forwarding/base/

# For development with port forwarding and OIDC
cd build/forwarding/oidc/

# For DevContainer environment
cd build/devcontainer/base/

# For DevContainer environment with OIDC
cd build/devcontainer/oidc/

# For production with Let's Encrypt SSL
cd build/letsencrypt/base/

# For production with Let's Encrypt SSL and OIDC
cd build/letsencrypt/oidc/

# For production with Step CA SSL
cd build/step-ca/base/

# For production with Step CA SSL and OIDC
cd build/step-ca/oidc/
```

### 3. Configure Environment

Copy and edit the environment file:

```bash
cp .env.example .env
# Edit .env with your values
```

### 4. Deploy

Start the services:

```bash
docker-compose up -d
```

Access: `http://localhost:8008` (for forwarding mode)

## 🔧 Available Configurations

### Environments

- **devcontainer**: Development environment with workspace network
- **forwarding**: Development environment with port forwarding (8008, 5432)
- **letsencrypt**: Production with Let's Encrypt SSL certificates
- **step-ca**: Production with Step CA SSL certificates

### Extensions

- **oidc**: OIDC authentication integration with external providers (Keycloak, etc.)

### Generated Combinations

Each environment can be combined with extensions to provide complete Matrix deployments:

**Base configurations:**

- `devcontainer/base` - DevContainer development setup
- `forwarding/base` - Development with port forwarding
- `letsencrypt/base` - Production with Let's Encrypt SSL
- `step-ca/base` - Production with Step CA SSL

**OIDC-enabled configurations:**

- `devcontainer/oidc` - DevContainer development with OIDC authentication
- `forwarding/oidc` - Development with port forwarding and OIDC authentication
- `letsencrypt/oidc` - Production with Let's Encrypt SSL and OIDC authentication
- `step-ca/oidc` - Production with Step CA SSL and OIDC authentication

## 🔧 Environment Variables

### Base Configuration

- `MATRIX_TZ`: Timezone for Matrix services
- `POSTGRES_DB`: PostgreSQL database name
- `POSTGRES_USER`: PostgreSQL username
- `POSTGRES_PASSWORD`: PostgreSQL password
- `POSTGRES_INITDB_ARGS`: PostgreSQL initialization arguments
- `SYNAPSE_VERSION`: Synapse version (default: latest)

### Synapse Configuration

- `SYNAPSE_PID_FILE`: Path to PID file
- `SYNAPSE_CONFIG_PATH`: Path to homeserver configuration
- `SYNAPSE_MEDIA_STORE_PATH`: Path to media store
- `SYNAPSE_SIGNING_KEY_PATH`: Path to signing key
- `SYNAPSE_REPORT_STATS`: Enable/disable statistics reporting
- `SYNAPSE_REGISTRATION_SHARED_SECRET`: Registration shared secret
- `SYNAPSE_MACAROON_SECRET_KEY`: Macaroon secret key
- `SYNAPSE_FORM_SECRET`: Form secret

### Let's Encrypt Configuration

- `VIRTUAL_PORT`: Port for nginx-proxy (default: 8008)
- `VIRTUAL_HOST`: Domain for nginx-proxy
- `LETSENCRYPT_HOST`: Domain for SSL certificate
- `LETSENCRYPT_EMAIL`: Email for certificate registration

### Step CA Configuration

- `VIRTUAL_PORT`: Port for nginx-proxy (default: 8008)
- `VIRTUAL_HOST`: Domain for nginx-proxy
- `LETSENCRYPT_HOST`: Domain for SSL certificate
- `LETSENCRYPT_EMAIL`: Email for certificate registration

### OIDC Configuration

- `OIDC_PROVIDER_IDP_ID`: Identity provider ID (e.g., keycloak)
- `OIDC_PROVIDER_IDP_NAME`: Display name for the identity provider
- `OIDC_PROVIDER_ISSUER`: OIDC issuer URL (e.g., <https://sso.example.com/realms/matrix>)
- `OIDC_PROVIDER_CLIENT_ID`: Client ID for Synapse in the OIDC provider
- `OIDC_PROVIDER_CLIENT_SECRET`: Client secret for authentication
- `OIDC_PROVIDER_SCOPES`: OAuth scopes to request (JSON array format)
- `OIDC_PROVIDER_LOCALPART_TEMPLATE`: Template for generating Matrix user localpart
- `OIDC_PROVIDER_DISPLAY_NAME_TEMPLATE`: Template for generating display names

## 🔐 OIDC Integration

The OIDC extension provides seamless integration with external identity providers like Keycloak, Auth0, or any OpenID Connect compatible service.

### Using OIDC Extension

1. **Build with OIDC extension:**

   ```bash
   ./build.sh
   ```

2. **Choose OIDC-enabled configuration:**

   ```bash
   # For development with OIDC
   cd build/forwarding/oidc/
   
   # For production with Let's Encrypt and OIDC
   cd build/letsencrypt/oidc/
   ```

3. **Configure OIDC provider:**

   ```bash
   cp .env.example .env
   # Edit .env with your OIDC provider details
   ```

4. **Example OIDC configuration for Keycloak:**

   ```bash
   OIDC_PROVIDER_IDP_ID=keycloak
   OIDC_PROVIDER_IDP_NAME=keycloak
   OIDC_PROVIDER_ISSUER=https://sso.example.com/realms/matrix
   OIDC_PROVIDER_CLIENT_ID=synapse
   OIDC_PROVIDER_CLIENT_SECRET=your-client-secret
   OIDC_PROVIDER_SCOPES='["openid", "profile"]'
   OIDC_PROVIDER_LOCALPART_TEMPLATE=user.preferred_username
   OIDC_PROVIDER_DISPLAY_NAME_TEMPLATE=user.name
   ```

5. **Deploy:**

   ```bash
   docker-compose up -d
   ```

## 🛠️ Development

### Adding New Environments

1. Create directory in `components/environments/` with `docker-compose.yml` and optional `.env.example` file
2. Run `./build.sh` to generate new combinations

### Adding New Extensions

1. Create directory in `components/extensions/` with `docker-compose.yml` and optional `.env.example` file
2. Run `./build.sh` to generate new combinations with all environments
3. Extensions are automatically combined with all available environments

### File Naming Convention

All component files follow the standard Docker Compose naming convention (`docker-compose.yml`) for:

- **VS Code compatibility**: Full support for Docker Compose language features and IntelliSense
- **IDE integration**: Proper syntax highlighting and validation in all major editors
- **Tool compatibility**: Works with Docker Compose plugins and extensions
- **Standard compliance**: Follows official Docker Compose file naming patterns

### Modifying Existing Components

1. Edit the component files in `components/`
2. Run `./build.sh` to regenerate configurations
3. The `build/` directory will be completely recreated

## 🌐 Networks

- **Development**: `matrix-network` (internal)
- **DevContainer**: `matrix-workspace-network` (external)
- **Let's Encrypt**: `letsencrypt-network` (external)
- **Step CA**: `step-ca-network` (external)

## 🔒 Security

⚠️ **Production Checklist:**

- Change default database passwords
- Configure strong secrets for Synapse
- Set up proper firewall rules
- Regular security updates
- Configure rate limiting
- Set up proper logging

## 🆘 Troubleshooting

**Build Issues:**

- Ensure `yq` is installed: <https://github.com/mikefarah/yq#install>
- Check component file syntax
- Verify all required files exist

**Synapse Issues:**

- Check homeserver.yaml syntax
- Verify database connection
- Review container logs: `docker logs matrix`
- Check signing key permissions

**SSL Issues:**

- **Let's Encrypt**: Verify domain DNS and letsencrypt-manager
- **Step CA**: Check step-ca-manager and virtual network config

**Federation Issues:**

- Verify port 8448 is accessible (for federation)
- Check DNS SRV records
- Validate SSL certificates

## 📝 Notes

- The `build/` directory is automatically generated and should not be edited manually
- Environment variables in generated files use `$VARIABLE_NAME` format for proper interpolation
- Each generated configuration includes a complete `docker-compose.yml` and `.env.example`
- Missing `.env.*` files for components are handled gracefully by the build script
- Synapse requires proper homeserver configuration for federation

## 🔄 Configuration Management

The build system automatically:

- Merges base and environment configurations
- Copies additional files (Dockerfile, homeserver.yaml, etc.)
- Generates complete deployment configurations
- Preserves user `.env` files during rebuilds

**Build approach:**

```bash
./build.sh
cd build/forwarding/base/
cp .env.example .env
# Edit .env with your values
docker-compose up -d
