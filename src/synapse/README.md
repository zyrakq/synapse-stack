# 💬 Synapse Service

A modular Docker Compose configuration system for Matrix Synapse with PostgreSQL backend, OIDC integration capabilities, and support for multiple environments.

## 🚀 Quick Start

### 1. Build Configurations

Use the stackbuilder utility to build all configurations:

```bash
sb build
```

This will create all combinations in the `build/` directory.

### 2. Choose Your Configuration

Navigate to the desired configuration directory:

```bash
# For development with port forwarding
cd build/forwarding/base/

# For development with OIDC authentication
cd build/forwarding/oidc/

# For production with Let's Encrypt SSL
cd build/letsencrypt/base/

# For production with Let's Encrypt SSL and OIDC
cd build/letsencrypt/oidc/

# For production with Step CA SSL and OIDC
cd build/step-ca/oidc-trust/
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
docker compose up --build -d
```

Access: `http://localhost:8008` (for forwarding mode)

## 📁 Project Structure

- **`build/`** - Ready-to-use Docker Compose configurations
- **`components/`** - Source components used to build configurations
  - `base/` - Base components (Synapse + PostgreSQL)
  - `environments/` - Environment components (devcontainer, forwarding, letsencrypt, step-ca)
  - `extensions/` - Extensions (OIDC, step-ca-trust)

## 🔧 Available Environments

- **devcontainer** - Development environment with workspace network
- **forwarding** - Development environment with port forwarding (8008, 5432)
- **letsencrypt** - Production with Let's Encrypt SSL certificates
- **step-ca** - Production with Step CA SSL certificates

## 🔧 Available Extensions

- **oidc** - OIDC authentication integration with external providers (Keycloak, etc.)
- **step-ca-trust** - Trust for Step CA certificates

## 🔧 Environment Variables

### Base Configuration

- `MATRIX_TZ` - Timezone for Matrix services
- `POSTGRES_DB` - PostgreSQL database name
- `POSTGRES_USER` - PostgreSQL username
- `POSTGRES_PASSWORD` - PostgreSQL password
- `POSTGRES_INITDB_ARGS` - PostgreSQL initialization arguments
- `SYNAPSE_VERSION` - Synapse version (default: latest)

### Synapse Configuration

- `SYNAPSE_SERVER_NAME` - Matrix server name
- `SYNAPSE_PID_FILE` - Path to PID file
- `SYNAPSE_CONFIG_PATH` - Path to homeserver configuration
- `SYNAPSE_MEDIA_STORE_PATH` - Path to media store
- `SYNAPSE_SIGNING_KEY_PATH` - Path to signing key
- `SYNAPSE_REPORT_STATS` - Enable/disable statistics reporting
- `SYNAPSE_REGISTRATION_SHARED_SECRET` - Registration shared secret
- `SYNAPSE_MACAROON_SECRET_KEY` - Macaroon secret key
- `SYNAPSE_FORM_SECRET` - Form secret

### Let's Encrypt Configuration

- `VIRTUAL_PORT` - Port for nginx-proxy (default: 8008)
- `VIRTUAL_HOST` - Domain for nginx-proxy
- `LETSENCRYPT_HOST` - Domain for SSL certificate
- `LETSENCRYPT_EMAIL` - Email for certificate registration

### Step CA Configuration

- `VIRTUAL_PORT` - Port for nginx-proxy (default: 8008)
- `VIRTUAL_HOST` - Domain for nginx-proxy
- `LETSENCRYPT_HOST` - Domain for SSL certificate
- `LETSENCRYPT_EMAIL` - Email for certificate registration

### OIDC Configuration

- `OIDC_PROVIDER_IDP_ID` - Identity provider ID (e.g., keycloak)
- `OIDC_PROVIDER_IDP_NAME` - Display name for the identity provider
- `OIDC_PROVIDER_ISSUER` - OIDC issuer URL (e.g., <https://sso.example.com/realms/matrix>)
- `OIDC_PROVIDER_CLIENT_ID` - Client ID for Synapse in the OIDC provider
- `OIDC_PROVIDER_CLIENT_SECRET` - Client secret for authentication
- `OIDC_PROVIDER_SCOPES` - OAuth scopes to request (JSON array format)
- `OIDC_PROVIDER_LOCALPART_TEMPLATE` - Template for generating Matrix user localpart
- `OIDC_PROVIDER_DISPLAY_NAME_TEMPLATE` - Template for generating display names

## 🔐 OIDC Integration

The OIDC extension provides seamless integration with external identity providers like Keycloak, Auth0, or any OpenID Connect compatible service.

### Example configuration for Keycloak

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

- Ensure stackbuilder is installed: <https://github.com/zyrakq/stackbuilder>
- Check component file syntax
- Verify all required files exist

**Synapse Issues:**

- Check homeserver.yaml syntax
- Verify database connection
- Review container logs: `docker logs matrix-synapse-1`
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
- Missing `.env.*` files for components are handled gracefully by the build system
- Synapse requires proper homeserver configuration for federation

## 🔄 Build System

Configurations are automatically built based on the `stackbuilder.toml` file:

```bash
# Build all configurations
sb build

# Ready configurations will appear in build/
cd build/forwarding/base/
cp .env.example .env
# Edit .env
docker compose up --build -d
