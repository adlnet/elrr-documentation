# Schema Documentation

---

## Enumerated Types

### action_type
| Value |
|------|
| CREATE |
| READ |
| UPDATE |
| DELETE |
| ASSOCIATE |
| DISASSOCIATE |
| ADMIN |

### goal_type
| Value |
|------|
| SELF |
| ASSIGNED |
| RECOMMENDED |

### learning_status
| Value |
|------|
| ATTEMPTED |
| COMPLETED |
| PASSED |
| FAILED |
| REGISTERED |

### qualification_type
| Value |
|------|
| COMPETENCY |
| CREDENTIAL |

### svc_method
| Value |
|------|
| SAVE |
| SAVEALL |
| DELETE |

---

## Tables

## association

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| person_id | UUID | NOT NULL; FK; NOT NULL REFERENCES person (id) ON DELETE CASCADE |
| organization_id | UUID | NOT NULL; FK; NOT NULL REFERENCES organization (id) ON DELETE CASCADE |
| association_type | VARCHAR(255) | NOT NULL |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| person_id | person(id) | CASCADE |  |
| organization_id | organization(id) | CASCADE |  |

### Incoming foreign keys
_None_

### Notes
- **Primary key:** id

## audit_log

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| timestamp | TIMESTAMP WITH TIME ZONE |  |
| request_id | UUID | NOT NULL |
| username | VARCHAR(255) | NOT NULL |
| is_api_user | BOOLEAN | NOT NULL |
| resource | VARCHAR(255) | NOT NULL |
| action | action_type | NOT NULL |
| entity_type | VARCHAR(255) | NOT NULL |
| entity_id | UUID | NOT NULL |
| svc_method | svc_method | NOT NULL |
| jwt_id | UUID |  |


### Outgoing foreign keys
_None_

### Incoming foreign keys
_None_

### Notes
- **Primary key:** id

## client_token

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| jwt_id | UUID | NOT NULL; UNIQUE; NOT NULL UNIQUE |
| jwt_payload | JSONB | NOT NULL |
| label | VARCHAR(255) |  |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |


### Outgoing foreign keys
_None_

### Incoming foreign keys
_None_

### Notes
- **Primary key:** id
- **Unique constraints:** jwt_id (inline)

## email

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| email_address | VARCHAR(255) |  |
| email_address_type | VARCHAR(255) |  |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |


### Outgoing foreign keys
_None_

### Incoming foreign keys
| From Table | From Column | To Column | On Delete | On Update |
|------------|------------|----------|----------|----------|
| person_email | email_id | id | CASCADE |  |

### Notes
- **Primary key:** id

## employment_qualification

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| employment_record_id | UUID | NOT NULL; FK; NOT NULL REFERENCES employment_record (id) ON DELETE CASCADE |
| qualification_id | UUID | NOT NULL; FK; NOT NULL REFERENCES qualification (id) ON DELETE CASCADE |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| employment_record_id | employment_record(id) | CASCADE |  |
| qualification_id | qualification(id) | CASCADE |  |

### Incoming foreign keys
_None_

## employment_record

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| employer_organization | UUID | NOT NULL; FK; NOT NULL REFERENCES organization (id) ON DELETE CASCADE |
| employee | UUID | NOT NULL; FK; NOT NULL REFERENCES person (id) ON DELETE CASCADE |
| employee_id | VARCHAR(255) |  |
| custom_employment_record_id | VARCHAR(255) |  |
| employee_type | VARCHAR(255) |  |
| hire_date | DATE |  |
| hire_type | VARCHAR(255) |  |
| employment_start_date | DATE |  |
| employment_end_date | DATE |  |
| position | VARCHAR(255) |  |
| position_title | VARCHAR(255) |  |
| position_description | TEXT |  |
| job_level | VARCHAR(255) |  |
| occupation | VARCHAR(255) |  |
| employment_location | UUID | FK; REFERENCES location (id) |
| employment_facility | UUID | FK; REFERENCES facility (id) |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |
| extensions | JSONB |  |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| employer_organization | organization(id) | CASCADE |  |
| employee | person(id) | CASCADE |  |
| employment_location | location(id) |  |  |
| employment_facility | facility(id) |  |  |

### Incoming foreign keys
| From Table | From Column | To Column | On Delete | On Update |
|------------|------------|----------|----------|----------|
| employment_qualification | employment_record_id | id | CASCADE |  |

### Notes
- **Primary key:** id

## facility

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| name | VARCHAR(255) |  |
| location_id | UUID | FK; REFERENCES location (id) |
| description | TEXT |  |
| operational_status | VARCHAR(255) |  |
| facility_security_level | VARCHAR(255) |  |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |
| extensions | JSONB |  |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| location_id | location(id) |  |  |

### Incoming foreign keys
| From Table | From Column | To Column | On Delete | On Update |
|------------|------------|----------|----------|----------|
| organization_facility | facility_id | id | CASCADE |  |
| employment_record | employment_facility | id |  |  |

### Notes
- **Primary key:** id

## goal

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| person_id | UUID | NOT NULL; FK; NOT NULL REFERENCES person (id) ON DELETE CASCADE |
| goal_id | TEXT | NOT NULL; UNIQUE; NOT NULL UNIQUE |
| type | goal_type | NOT NULL |
| name | VARCHAR(255) | NOT NULL |
| description | TEXT |  |
| start_date | TIMESTAMP WITH TIME ZONE |  |
| achieved_by_date | TIMESTAMP WITH TIME ZONE |  |
| expiration_date | TIMESTAMP WITH TIME ZONE |  |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |
| extensions | JSONB |  |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| person_id | person(id) | CASCADE |  |

### Incoming foreign keys
| From Table | From Column | To Column | On Delete | On Update |
|------------|------------|----------|----------|----------|
| goal_competency | goal_id | id | CASCADE |  |
| goal_credential | goal_id | id | CASCADE |  |
| goal_learning_resource | goal_id | id | CASCADE |  |

### Notes
- **Primary key:** id
- **Unique constraints:** goal_id (inline)

## goal_competency

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| goal_id | UUID | NOT NULL; FK; NOT NULL REFERENCES goal (id) ON DELETE CASCADE |
| qualification_id | UUID | NOT NULL; FK; NOT NULL REFERENCES qualification (id) ON DELETE CASCADE |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| goal_id | goal(id) | CASCADE |  |
| qualification_id | qualification(id) | CASCADE |  |

### Incoming foreign keys
_None_

## goal_credential

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| goal_id | UUID | NOT NULL; FK; NOT NULL REFERENCES goal (id) ON DELETE CASCADE |
| qualification_id | UUID | NOT NULL; FK; NOT NULL REFERENCES qualification (id) ON DELETE CASCADE |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| goal_id | goal(id) | CASCADE |  |
| qualification_id | qualification(id) | CASCADE |  |

### Incoming foreign keys
_None_

## goal_learning_resource

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| goal_id | UUID | NOT NULL; FK; NOT NULL REFERENCES goal (id) ON DELETE CASCADE |
| learning_resource_id | UUID | NOT NULL; FK; NOT NULL REFERENCES learning_resource (id) ON DELETE CASCADE |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| goal_id | goal(id) | CASCADE |  |
| learning_resource_id | learning_resource(id) | CASCADE |  |

### Incoming foreign keys
_None_

## identity

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| person_id | UUID | NOT NULL; FK; REFERENCES person (id) NOT NULL |
| mbox | VARCHAR(255) |  |
| mbox_sha1sum | VARCHAR(255) |  |
| openid | VARCHAR(255) |  |
| home_page | VARCHAR(255) |  |
| name | VARCHAR(255) |  |
| ifi | VARCHAR(255) | NOT NULL; GENERATED ALWAYS AS ( CASE WHEN mbox_sha1sum IS NOT NULL THEN ('mbox_sha1sum::' \|\| mbox_sha1sum) WHEN openid IS NOT NULL THEN ('openid::' \|\| openid) WHEN home_page IS NOT NULL THEN ('account::' \|\| name \|\| '@' \|\| home_page) ELSE ('mbox::' \|\| mbox) end ) STORED |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| person_id | person(id) |  |  |

### Incoming foreign keys
_None_

### Notes
- **Primary key:** id

## learning_record

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| person_id | UUID | NOT NULL; FK; NOT NULL REFERENCES person (id) ON DELETE CASCADE |
| learning_resource_id | UUID | NOT NULL; FK; NOT NULL REFERENCES learning_resource (id) ON DELETE CASCADE |
| enrollment_date | TIMESTAMP WITH TIME ZONE |  |
| record_status | learning_status | NOT NULL |
| academic_grade | VARCHAR(50) |  |
| event_time | TIMESTAMP WITH TIME ZONE |  |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |
| extensions | JSONB |  |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| person_id | person(id) | CASCADE |  |
| learning_resource_id | learning_resource(id) | CASCADE |  |

### Incoming foreign keys
_None_

### Notes
- **Primary key:** id

## learning_resource

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| iri | VARCHAR(255) | NOT NULL; UNIQUE; NOT NULL UNIQUE |
| title | VARCHAR(255) | NOT NULL |
| subject_matter | VARCHAR(255) |  |
| subject_abbreviation | VARCHAR(20) |  |
| level | VARCHAR(255) |  |
| number | VARCHAR(255) |  |
| instruction_method | VARCHAR(255) |  |
| start_date | DATE |  |
| end_date | DATE |  |
| provider_name | VARCHAR(255) |  |
| department_name | VARCHAR(255) |  |
| grade_scale_code | VARCHAR(255) |  |
| metadata_repository | VARCHAR(255) |  |
| lrs_endpoint | VARCHAR(255) |  |
| description | TEXT |  |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |
| extensions | JSONB |  |


### Outgoing foreign keys
_None_

### Incoming foreign keys
| From Table | From Column | To Column | On Delete | On Update |
|------------|------------|----------|----------|----------|
| learning_record | learning_resource_id | id | CASCADE |  |
| goal_learning_resource | learning_resource_id | id | CASCADE |  |

### Notes
- **Primary key:** id
- **Unique constraints:** iri (inline)

## location

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| street_number_and_name | VARCHAR(255) |  |
| apartment_room_suite_number | VARCHAR(255) |  |
| city | VARCHAR(255) |  |
| state_abbreviation | VARCHAR(255) |  |
| postal_code | VARCHAR(255) |  |
| county | VARCHAR(255) |  |
| country_code | VARCHAR(255) |  |
| latitude | VARCHAR(255) |  |
| longitude | VARCHAR(255) |  |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |
| extensions | JSONB |  |


### Outgoing foreign keys
_None_

### Incoming foreign keys
| From Table | From Column | To Column | On Delete | On Update |
|------------|------------|----------|----------|----------|
| person | mailing_address_id | id |  |  |
| person | physical_address_id | id |  |  |
| person | shipping_address_id | id |  |  |
| person | billing_address_id | id |  |  |
| person | on_campus_address_id | id |  |  |
| person | off_campus_address_id | id |  |  |
| person | temporary_address_id | id |  |  |
| person | permanent_student_address_id | id |  |  |
| person | employment_address_id | id |  |  |
| person | time_of_admission_address_id | id |  |  |
| person | father_address_id | id |  |  |
| person | mother_address_id | id |  |  |
| person | guardian_address_id | id |  |  |
| person | birthplace_id | id |  |  |
| facility | location_id | id |  |  |
| employment_record | employment_location | id |  |  |

### Notes
- **Primary key:** id

## organization

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| name | VARCHAR(255) | NOT NULL |
| description | TEXT |  |
| profit_type | VARCHAR(255) |  |
| department | VARCHAR(255) |  |
| industry_code | VARCHAR(255) |  |
| industry_category | VARCHAR(255) |  |
| vertical_specialization | VARCHAR(255) |  |
| organization_identifier | VARCHAR(255) |  |
| organization_duns | VARCHAR(255) |  |
| organization_fein | VARCHAR(255) |  |
| school_opeid | VARCHAR(255) |  |
| ipeds_type | VARCHAR(255) |  |
| organization_isic | VARCHAR(255) |  |
| organization_image | VARCHAR(255) |  |
| organization_website | VARCHAR(255) |  |
| institution_level | VARCHAR(255) |  |
| organization_accredits | VARCHAR(255) |  |
| institution_revocation_list | VARCHAR(255) |  |
| has_verification_service | BOOLEAN |  |
| institution_verification | TEXT |  |
| organizational_resource | VARCHAR(255) |  |
| quality_assurance_type | VARCHAR(255) |  |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |
| extensions | JSONB |  |


### Outgoing foreign keys
_None_

### Incoming foreign keys
| From Table | From Column | To Column | On Delete | On Update |
|------------|------------|----------|----------|----------|
| association | organization_id | id | CASCADE |  |
| organization_facility | organization_id | id | CASCADE |  |
| employment_record | employer_organization | id | CASCADE |  |

### Notes
- **Primary key:** id

## organization_facility

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| facility_id | UUID | NOT NULL; FK; NOT NULL REFERENCES facility (id) ON DELETE CASCADE |
| organization_id | UUID | NOT NULL; FK; NOT NULL REFERENCES organization (id) ON DELETE CASCADE |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| facility_id | facility(id) | CASCADE |  |
| organization_id | organization(id) | CASCADE |  |

### Incoming foreign keys
_None_

## person

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| name | VARCHAR(255) |  |
| first_name | VARCHAR(255) |  |
| middle_name | VARCHAR(255) |  |
| last_name | VARCHAR(255) |  |
| name_prefix | VARCHAR(255) |  |
| title_affix_code | VARCHAR(255) |  |
| name_suffix | VARCHAR(255) |  |
| qualification_affix_code | VARCHAR(255) |  |
| maiden_name | VARCHAR(255) |  |
| mailing_address_id | UUID | FK; REFERENCES location (id) |
| physical_address_id | UUID | FK; REFERENCES location (id) |
| shipping_address_id | UUID | FK; REFERENCES location (id) |
| billing_address_id | UUID | FK; REFERENCES location (id) |
| on_campus_address_id | UUID | FK; REFERENCES location (id) |
| off_campus_address_id | UUID | FK; REFERENCES location (id) |
| temporary_address_id | UUID | FK; REFERENCES location (id) |
| permanent_student_address_id | UUID | FK; REFERENCES location (id) |
| employment_address_id | UUID | FK; REFERENCES location (id) |
| time_of_admission_address_id | UUID | FK; REFERENCES location (id) |
| father_address_id | UUID | FK; REFERENCES location (id) |
| mother_address_id | UUID | FK; REFERENCES location (id) |
| guardian_address_id | UUID | FK; REFERENCES location (id) |
| birthdate | DATE |  |
| birthplace_id | UUID | FK; REFERENCES location (id) |
| citizenship | VARCHAR(255) |  |
| height | numeric(5, 2) |  |
| height_unit | VARCHAR(255) |  |
| weight | numeric(5, 2) |  |
| weight_unit | VARCHAR(255) |  |
| interpupillary_distance | bigint |  |
| handedness | VARCHAR(255) |  |
| primary_language | VARCHAR(255) |  |
| current_security_clearance | VARCHAR(255) |  |
| highest_security_clearance | VARCHAR(255) |  |
| union_membership | BOOLEAN |  |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |
| extensions | JSONB |  |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| mailing_address_id | location(id) |  |  |
| physical_address_id | location(id) |  |  |
| shipping_address_id | location(id) |  |  |
| billing_address_id | location(id) |  |  |
| on_campus_address_id | location(id) |  |  |
| off_campus_address_id | location(id) |  |  |
| temporary_address_id | location(id) |  |  |
| permanent_student_address_id | location(id) |  |  |
| employment_address_id | location(id) |  |  |
| time_of_admission_address_id | location(id) |  |  |
| father_address_id | location(id) |  |  |
| mother_address_id | location(id) |  |  |
| guardian_address_id | location(id) |  |  |
| birthplace_id | location(id) |  |  |

### Incoming foreign keys
| From Table | From Column | To Column | On Delete | On Update |
|------------|------------|----------|----------|----------|
| identity | person_id | id |  |  |
| association | person_id | id | CASCADE |  |
| person_qualification | person_id | id | CASCADE |  |
| learning_record | person_id | id | CASCADE |  |
| person_phone | person_id | id | CASCADE |  |
| person_email | person_id | id | CASCADE |  |
| employment_record | employee | id | CASCADE |  |
| goal | person_id | id | CASCADE |  |

### Notes
- **Primary key:** id

## person_email

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| person_id | UUID | NOT NULL; FK; NOT NULL REFERENCES person (id) ON DELETE CASCADE |
| email_id | UUID | NOT NULL; FK; NOT NULL REFERENCES email (id) ON DELETE CASCADE |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| person_id | person(id) | CASCADE |  |
| email_id | email(id) | CASCADE |  |

### Incoming foreign keys
_None_

## person_phone

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| person_id | UUID | NOT NULL; FK; NOT NULL REFERENCES person (id) ON DELETE CASCADE |
| phone_id | UUID | NOT NULL; FK; NOT NULL REFERENCES phone (id) ON DELETE CASCADE |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| person_id | person(id) | CASCADE |  |
| phone_id | phone(id) | CASCADE |  |

### Incoming foreign keys
_None_

## person_qualification

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| person_id | UUID | NOT NULL; FK; NOT NULL REFERENCES person (id) ON DELETE CASCADE |
| qualification_id | UUID | NOT NULL; FK; NOT NULL REFERENCES qualification (id) ON DELETE CASCADE |
| type | qualification_type | NOT NULL |
| has_record | BOOLEAN |  |
| expires | TIMESTAMP WITH TIME ZONE | NULL |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |


### Outgoing foreign keys
| Column | References | On Delete | On Update |
|--------|------------|----------|----------|
| person_id | person(id) | CASCADE |  |
| qualification_id | qualification(id) | CASCADE |  |

### Incoming foreign keys
_None_

### Notes
- **Primary key:** id

## phone

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| telephone_number | VARCHAR(255) |  |
| telephone_number_type | VARCHAR(255) |  |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |


### Outgoing foreign keys
_None_

### Incoming foreign keys
| From Table | From Column | To Column | On Delete | On Update |
|------------|------------|----------|----------|----------|
| person_phone | phone_id | id | CASCADE |  |

### Notes
- **Primary key:** id

## qualification

### Fields
| Field | Type | Constraints |
|------|------|-------------|
| id | UUID | PK; PRIMARY KEY |
| type | qualification_type | NOT NULL |
| identifier | VARCHAR(100) |  |
| identifier_url | TEXT |  |
| taxonomy_id | VARCHAR(100) |  |
| valid_start_date | TIMESTAMP WITH TIME ZONE | NULL |
| valid_end_date | TIMESTAMP WITH TIME ZONE | NULL |
| parent_id | VARCHAR(100) |  |
| parent_url | TEXT |  |
| parent_code | VARCHAR(100) |  |
| code | VARCHAR(100) |  |
| type_url | TEXT |  |
| statement | TEXT |  |
| framework_title | VARCHAR(100) | NOT NULL |
| framework_version | VARCHAR(100) |  |
| framework_identifier | VARCHAR(100) |  |
| framework_description | TEXT |  |
| framework_subject | VARCHAR(100) |  |
| framework_valid_start_date | DATE |  |
| framework_valid_end_date | DATE |  |
| record_status | VARCHAR(10) |  |
| updated_by | VARCHAR(20) |  |
| inserted_date | TIMESTAMP WITH TIME ZONE |  |
| last_modified | TIMESTAMP WITH TIME ZONE |  |
| extensions | JSONB |  |


### Outgoing foreign keys
_None_

### Incoming foreign keys
| From Table | From Column | To Column | On Delete | On Update |
|------------|------------|----------|----------|----------|
| person_qualification | qualification_id | id | CASCADE |  |
| employment_qualification | qualification_id | id | CASCADE |  |
| goal_competency | qualification_id | id | CASCADE |  |
| goal_credential | qualification_id | id | CASCADE |  |

### Notes
- **Primary key:** id
