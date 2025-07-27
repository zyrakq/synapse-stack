# 🏠 Synapse Stack

Complete Docker-based Matrix Synapse deployment with SSL certificate management for production and development environments.

## 🧩 Components

### 🔐 SSL Automation

#### [🔒 Let's Encrypt Manager](src/ssl-automation/letsencrypt-manager)

Automatic SSL certificate management from Let's Encrypt for production deployments. Provides seamless HTTPS integration for Docker containers using nginx-proxy and acme-companion.
[Learn more about Let's Encrypt Manager configuration](src/ssl-automation/letsencrypt-manager/README.md).

#### [🏠 Step CA Manager](src/ssl-automation/step-ca-manager)

Local domain stack with trusted self-signed certificates for virtual network deployments. Includes private CA management and local DNS resolution for development environments.
[Learn more about Step CA Manager configuration](src/ssl-automation/step-ca-manager/README.md).

### 🔑 Identity Management

#### [🔐 Keycloak](src/identity-management/keycloak)

Enterprise-grade identity and access management solution. Provides authentication, authorization, and user management for secure application access.
[Learn more about Keycloak configuration](src/identity-management/keycloak/README.md).

For Open WebUI integration, see: [Keycloak Integration](https://docs.openwebui.com/features/sso/keycloak)

#### [🔐 Kanidm](src/identity-management/kanidm)

Modern identity and access management server with comprehensive authentication capabilities. Provides secure identity management with modular configuration system and multiple deployment modes.
[Learn more about Kanidm configuration](src/identity-management/kanidm/README.md).

## 🌐 Services

### 💬 [Synapse](src/synapse/)

Modular Docker Compose configuration system for Matrix Synapse with PostgreSQL backend and OIDC integration capabilities. Provides complete Matrix homeserver deployment with multiple environment configurations for development and production.
[Learn more about Matrix Synapse configuration](src/synapse/README.md).

## 🚀 Quick Start

Each component has its own README with detailed setup instructions. Choose the certificate management solution that fits your deployment scenario.

## 📋 Requirements

- Docker & Docker Compose
- Domain name (for production deployments)
- Email address (for Let's Encrypt)

## 📄 License

This project is dual-licensed under:

- [Apache License 2.0](LICENSE-APACHE)
- [MIT License](LICENSE-MIT)
