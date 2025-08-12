# Module Documentation

## lambda_function.py

The main entry point for the AWS Lambda function that processes FHIR eReferrals.

### Key Functions:

#### `lambda_handler(event, context)`
- Main handler function that processes incoming FHIR payloads
- Parameters:
  - `event`: AWS Lambda event containing the FHIR payload
  - `context`: AWS Lambda context
- Returns: API Gateway response with FHIR Operation Outcome
- Handles errors and provides appropriate error responses

## parse_ereferrals_fhir.py

Handles the parsing and extraction of entities from FHIR payloads.

### Key Functions:

#### `extract_email(telecom_list)`
- Extracts email from FHIR telecom array
- Parameters:
  - `telecom_list`: List of FHIR telecom objects
- Returns: Email string or None

#### `extract_entities(data)`
- Extracts structured data from FHIR payload
- Parameters:
  - `data`: FHIR Bundle containing referral data
- Returns: Dictionary containing patient, practitioner, practice, and referrer information

## persist_entities_db.py

Handles database operations for storing extracted entities.

### Key Functions:

#### `db_save_entities(entities)`
- Saves extracted entities to PostgreSQL database
- Parameters:
  - `entities`: Dictionary containing extracted FHIR data
- Throws: Database errors if connection or insertion fails

## fhir_response.py

Generates FHIR-compliant Operation Outcome responses.

### Key Functions:

#### `generate_operation_outcome(resource_id, message, system, code, display, diagnostics, action, route)`
- Generates FHIR Operation Outcome response
- Parameters:
  - `resource_id`: Unique identifier for the response
  - `message`: Human-readable message
  - `system`: System identifier
  - `code`: Response code
  - `display`: Display message
  - `diagnostics`: Technical details
  - `action`: HTTP method
  - `route`: API route
- Returns: FHIR Operation Outcome JSON

#### `send_error_response(diagnostics)`
- Generates error response
- Parameters:
  - `diagnostics`: Error details
- Returns: Error Operation Outcome

#### `send_success_response(diagnostics)`
- Generates success response
- Parameters:
  - `diagnostics`: Success details
- Returns: Success Operation Outcome

## System Architecture

### Flow:
1. Lambda receives FHIR payload
2. Payload is parsed and entities extracted
3. Entities are saved to database
4. FHIR Operation Outcome response is generated and returned

### Error Handling:
- Input validation
- Database connection errors
- FHIR parsing errors
- General system errors

### Security:
- Environment variables for sensitive data
- AWS IAM roles and policies
- Database connection security

## Testing

Each module includes unit tests and integration tests. Run tests using:
```bash
pytest tests/
```

## Deployment

### AWS Lambda:
1. Package code and dependencies
2. Create Lambda function
3. Configure environment variables
4. Set up API Gateway
5. Configure security and permissions

### Local Development:
1. Set up PostgreSQL database
2. Configure environment variables
3. Install dependencies
4. Run locally for testing
