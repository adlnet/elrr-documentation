# ELRR REST API Documentation

**Version:** v2.4.0  

---

# Overview

This API provides endpoints for managing:

- People and their associated contact info
- Organizations and person/org relationships
- Learning Resources and Learning Records
- Competencies and Credentials (Qualifications)
- Goals
- Admin-issued client tokens

All endpoints return `application/json` unless otherwise noted.

---

# Health & Utility

## Ping

### `GET /ping`

Simple health check endpoint.

**Response:**

- `200 OK` → key/value status object

---

# Phone API

## Phones Collection

### `GET /api/phone`

Retrieve all phones.

**Query Parameters:**

- `id` *(optional, UUID)*

**Response:**

- `200 OK` → `PhoneDto[]`

---

### `POST /api/phone`

Create a new phone record.

**Request Body:**

- `PhoneDto`

**Response:**

- `200 OK` → `PhoneDto`

---

## Phone by ID

### `GET /api/phone/{id}`

Fetch a phone by ID.

**Path Parameters:**

- `id` *(UUID, required)*

**Response:**

- `200 OK` → `PhoneDto`

---

### `PUT /api/phone/{id}`

Update a phone record.

**Request Body:**

- `PhoneDto`

**Response:**

- `200 OK` → `PhoneDto`

---

### `DELETE /api/phone/{id}`

Delete a phone record.

**Response:**

- `200 OK`

---

# Email API

## Emails Collection

### `GET /api/email`

Retrieve all email records.

**Query Parameters:**

- `id` *(optional, UUID)*

**Response:**

- `200 OK` → `EmailDto[]`

---

### `POST /api/email`

Create a new email record.

**Request Body:**

- `EmailDto`

**Response:**

- `200 OK` → `EmailDto`

---

## Email by ID

### `GET /api/email/{id}`

Fetch email by ID.

### `PUT /api/email/{id}`

Update email record.

### `DELETE /api/email/{id}`

Delete email record.

---

# Person API

## Persons Collection

### `GET /api/person`

Retrieve all persons.

**Query Parameters:**

- `filters` *(required)* → `Filter`

**Response:**

- `200 OK` → `PersonDto[]`

---

### `POST /api/person`

Create a person.

**Request Body:**

- `PersonDto`

**Response:**

- `200 OK` → `PersonDto`

---

## Person by ID

### `GET /api/person/{id}`

Fetch person by ID.

### `PUT /api/person/{id}`

Update person record.

### `DELETE /api/person/{id}`

Delete person.

---

# Person Associations

These endpoints manage relationships between a person and other resources.

---

## Person Phones

### `GET /api/person/{personId}/phone`

List all phones associated with a person.

### `POST /api/person/{personId}/phone`

Add a new phone record directly to a person.

---

### `POST /api/person/{personId}/phone/{phoneId}`

Associate an existing phone with a person.

### `DELETE /api/person/{personId}/phone/{phoneId}`

Remove phone association.

---

## Person Emails

### `GET /api/person/{personId}/email`

List emails for a person.

### `POST /api/person/{personId}/email`

Add a new email record.

### `POST /api/person/{personId}/email/{emailId}`

Associate existing email.

### `DELETE /api/person/{personId}/email/{emailId}`

Remove email association.

---

## Person Identities

### `GET /api/person/{personId}/identity`

List identities for a person.

### `POST /api/person/{personId}/identity`

Add a new identity.

### `DELETE /api/person/{personId}/identity/{identityId}`

Delete identity.

---

## Person Learning Records

### `GET /api/person/{personId}/learningrecord`

List learning records for a person.

### `POST /api/person/{personId}/learningrecord`

Add learning record.

---

## Person Employment Records

### `GET /api/person/{personId}/employmentrecord`

List employment records.

### `POST /api/person/{personId}/employmentrecord`

Add employment record.

---

## Person Organizations

### `GET /api/person/{personId}/organization`

List organizations linked to a person.

---

### `GET /api/person/{personId}/organization/{organizationId}`

Get association record.

### `POST /api/person/{personId}/organization/{organizationId}`

Create association.

### `PUT /api/person/{personId}/organization/{organizationId}`

Update association.

### `DELETE /api/person/{personId}/organization/{organizationId}`

Remove association.

---

## Person Competencies

### `GET /api/person/{personId}/competency`

List competencies.

### `GET /api/person/{personId}/competency/{competencyId}`

Fetch competency association.

### `POST /api/person/{personId}/competency/{competencyId}`

Associate competency.

### `PUT /api/person/{personId}/competency/{competencyId}`

Update association.

### `DELETE /api/person/{personId}/competency/{competencyId}`

Remove competency.

---

## Person Credentials

### `GET /api/person/{personId}/credential`

List credentials.

### `GET /api/person/{personId}/credential/{credentialId}`

Fetch credential association.

### `POST /api/person/{personId}/credential/{credentialId}`

Associate credential.

### `PUT /api/person/{personId}/credential/{credentialId}`

Update association.

### `DELETE /api/person/{personId}/credential/{credentialId}`

Remove credential.

---

# Organization API

## Organizations Collection

### `GET /api/organization`

List organizations (filtered).

### `POST /api/organization`

Create organization.

---

## Organization by ID

### `GET /api/organization/{id}`

Fetch organization.

### `PUT /api/organization/{id}`

Update organization.

### `DELETE /api/organization/{id}`

Delete organization.

---

# Learning Resources

### `GET /api/learningresource`

List learning resources.

### `POST /api/learningresource`

Create learning resource.

### `GET /api/learningresource/{id}`

Fetch by ID.

### `PUT /api/learningresource/{id}`

Update.

### `DELETE /api/learningresource/{id}`

Delete.

---

# Learning Records

### `GET /api/learningrecord`

List learning records.

### `GET /api/learningrecord/{id}`

Fetch by ID.

### `PUT /api/learningrecord/{id}`

Update.

### `DELETE /api/learningrecord/{id}`

Delete.

---

# Competencies

### `GET /api/competency`

List competencies.

### `POST /api/competency`

Create competency.

### `GET /api/competency/{id}`

Fetch by ID.

### `PUT /api/competency/{id}`

Update.

### `DELETE /api/competency/{id}`

Delete.

---

# Credentials

### `GET /api/credential`

List credentials.

### `POST /api/credential`

Create credential.

### `GET /api/credential/{id}`

Fetch by ID.

### `PUT /api/credential/{id}`

Update.

### `DELETE /api/credential/{id}`

Delete.

---

# Goals

### `GET /api/goal`

List goals.

### `POST /api/goal`

Create goal.

### `GET /api/goal/{id}`

Fetch goal.

### `PUT /api/goal/{id}`

Update goal.

### `DELETE /api/goal/{id}`

Delete goal.

---

# Facilities

### `GET /api/facility`

List facilities.

### `POST /api/facility`

Create facility.

### `GET /api/facility/{id}`

Fetch facility.

### `PUT /api/facility/{id}`

Update facility.

### `DELETE /api/facility/{id}`

Delete facility.

---

# Admin Token Management

## Tokens

### `GET /admin/tokens`

List all client tokens.

---

### `GET /admin/token`

Fetch token by JWT ID.

**Query Parameters:**

- `jwtId` *(UUID, required)*

---

### `POST /admin/token`

Create a new token.

**Request Body:**

- `PermissionsWrapperDto`

---

### `DELETE /admin/token/{tokenId}`

Revoke token.

---

# Data Schemas (DTOs)

## PhoneDto

| Field | Type |
|------|------|
| id | UUID |
| telephoneNumber | string |
| telephoneNumberType | string |

---

## PersonDto (partial)

Includes:

- Name fields
- Birthdate
- Citizenship
- Addresses (`LocationDto`)
- Emails (`EmailDto[]`)
- Phones (`PhoneDto[]`)

---

## LearningRecordDto

| Field | Type |
|------|------|
| recordStatus | ATTEMPTED / COMPLETED / PASSED / FAILED / REGISTERED |
| enrollmentDate | datetime |
| learningResource | LearningResourceDto |

---

## GoalDto

Supports goal assignment and tracking:

- type: SELF / ASSIGNED / RECOMMENDED
- competencyIds[]
- credentialIds[]
- learningResourceIds[]

---

## PermissionsWrapperDto

Used for admin token creation.

Contains:

- `permissions[]`
- optional label

---
