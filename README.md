# FHIR eReferral Processing System

A robust Python-based system for processing electronic referrals using the FHIR (Fast Healthcare Interoperability Resources) standard. This system processes incoming FHIR referrals, extracts relevant information, and persists it to a PostgreSQL database while providing standardized FHIR Operation Outcome responses.

## Features

- FHIR-compliant referral processing
- Extraction of patient, practitioner, practice, and referrer details
- PostgreSQL database integration
- Standardized FHIR Operation Outcome responses
- AWS Lambda compatible
- Error handling and logging

## System Architecture

The system consists of four main components:

1. `lambda_function.py` - Main entry point for AWS Lambda execution
2. `parse_ereferrals_fhir.py` - FHIR payload parsing and entity extraction
3. `persist_entities_db.py` - Database operations and data persistence
4. `fhir_response.py` - FHIR-compliant response generation

## Prerequisites

- Python 3.8+
- PostgreSQL database
- Required Python packages (see `requirements.txt`)
- AWS Lambda (for cloud deployment)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/fhir-ereferral-processor.git
```

2. Install required packages:
```bash
pip install -r requirements.txt
```

3. Set up environment variables in `.env`:
```env
POSTGRES_HOST=your_host
POSTGRES_DATABASE=your_database
POSTGRES_USER=your_user
POSTGRES_PASSWORD=your_password
```

## Database Setup

Create the required table in your PostgreSQL database:

```sql
CREATE TABLE inbound_referral (
    id SERIAL PRIMARY KEY,
    patient_first_name VARCHAR(100),
    patient_last_name VARCHAR(100),
    patient_medicare_number VARCHAR(50),
    patient_gender VARCHAR(20),
    patient_address_line_1 VARCHAR(200),
    patient_city VARCHAR(100),
    patient_postcode VARCHAR(10),
    patient_state VARCHAR(50),
    patient_dob DATE,
    patient_phone_mobile VARCHAR(20),
    patient_phone_home VARCHAR(20),
    patient_email VARCHAR(100),
    patient_dva_number VARCHAR(50),
    patient_carer_name VARCHAR(200),
    patient_carer_phone VARCHAR(20),
    patient_carer_email VARCHAR(100),
    patient_carer_relationship VARCHAR(100),
    practitioner_first_name VARCHAR(100),
    practitioner_last_name VARCHAR(100),
    practitioner_npi VARCHAR(50),
    practice_name VARCHAR(200),
    practice_identifier VARCHAR(50),
    practice_address TEXT,
    practice_email VARCHAR(100),
    practice_phone VARCHAR(20),
    practice_edi_system VARCHAR(200),
    practice_edi_id VARCHAR(50),
    referrer_id VARCHAR(50),
    referrer_medicare_provider_number VARCHAR(50),
    referrer_organization_reference VARCHAR(100),
    referrer_practitioner_reference VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## Usage

### Local Development

```python
python lambda_function.py
```

### AWS Lambda Deployment

1. Package the code and dependencies
2. Upload to AWS Lambda
3. Set environment variables in Lambda configuration
4. Configure API Gateway trigger if needed

## API Reference

### Input

The system expects a FHIR-compliant referral payload containing:
- Patient information
- Practitioner details
- Practice information
- Referrer details

### Output

Returns a FHIR Operation Outcome with:
- Success/failure status
- Detailed diagnostic information
- HTTP status code
- Processing timestamps

## Error Handling

The system provides comprehensive error handling for:
- Invalid FHIR payloads
- Database connection issues
- Missing required fields
- Processing failures

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a new Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.
