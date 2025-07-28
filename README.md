# HIE Logging System

A FHIR-compliant audit logging system for Healthcare Information Exchange (HIE) platforms.

## Quick Start

1. **Install the Package**
```bash
pip install -r requirements.txt
pip install -e .
```

2. **Configure Environment**
```bash
export HIE_API_URL="your-api-url"
export HIE_API_KEY="your-api-key"
```

3. **Basic Usage**
```python
from client.hie_logger import HIELogger

# Initialize logger
logger = HIELogger(api_url="your-api-url", api_key="your-api-key")

# Log access event
logger.create_audit_event(
    event_type="access",
    patient_id="PAT001",
    practitioner_id="PRACT001"
)
```

A comprehensive central logging service for the HIE Platform that accepts FHIR AuditEvent logs, providing easy integration for developers, APIs for submission, and search capabilities for operations teams.

## Features

- **FHIR AuditEvent Compliance**: Full compliance with FHIR v5.0.0 AuditEvent schema
- **Developer Library**: Easy-to-use Python client library for creating FHIR logs
- **REST API**: HTTP API for submitting logs from various integrations
- **Search Interface**: Web-based search interface for operations teams
- **AWS S3 Tables Integration**: Scalable storage using Amazon S3 Tables
- **Real-time Processing**: Asynchronous log processing and indexing

## Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Client Apps   │    │  Integration    │    │  Operations     │
│   (Library)     │    │     APIs        │    │   Dashboard     │
└─────────┬───────┘    └─────────┬───────┘    └─────────┬───────┘
          │                      │                      │
          └──────────────────────┼──────────────────────┘
                                 │
                    ┌─────────────▼─────────────┐
                    │    Central Logging API    │
                    │      (FastAPI)            │
                    └─────────────┬─────────────┘
                                  │
                    ┌─────────────▼─────────────┐
                    │      AWS S3 Tables        │
                    │    (Storage & Index)      │
                    └───────────────────────────┘
```

## Quick Start

### 1. Install the Client Library

```bash
pip install hie-logging-client
```

### 2. Use the Client Library

```python
from hie_logging_client import HIELogger

# Initialize logger
logger = HIELogger(api_url="https://your-logging-service.com")

# Create a FHIR AuditEvent
audit_event = logger.create_audit_event(
    event_type="access",
    event_subtype="read",
    patient_id="patient-123",
    practitioner_id="practitioner-456",
    resource_type="Patient",
    resource_id="patient-123"
)

# Submit the log
logger.submit_log(audit_event)
```

### 3. API Usage

```bash
# Submit a log via API
curl -X POST https://your-logging-service.com/api/v1/logs \
  -H "Content-Type: application/json" \
  -d @audit_event.json
```

### 4. Search Logs

Visit the web dashboard at `https://your-logging-service.com/search` to search and filter logs.

## Project Structure

```
hie-logging-service/
├── api/                    # FastAPI application
├── client/                 # Python client library
├── dashboard/              # Web-based search interface
├── schemas/                # FHIR AuditEvent schemas
├── tests/                  # Test suite
├── docker/                 # Docker configuration
├── terraform/              # Infrastructure as Code
└── docs/                   # Documentation
```

## Development

### Prerequisites

- Python 3.9+
- Docker
- AWS CLI configured
- Terraform (for infrastructure)

### Local Development

```bash
# Clone the repository
git clone <repository-url>
cd hie-logging-service

# Install dependencies
pip install -r requirements.txt

# Run the API locally
uvicorn api.main:app --reload

# Run tests
pytest
```

## Deployment

The service can be deployed using the provided Terraform configuration:

```bash
cd terraform
terraform init
terraform plan
terraform apply
```

## API Documentation

Once the service is running, visit:
- API Documentation: `http://localhost:8000/docs`
- Search Dashboard: `http://localhost:8000/dashboard`

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Submit a pull request

## License

This project is licensed under the MIT License. 