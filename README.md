# Documentation API Diji

## Table des matières

- [Introduction](#introduction)
- [Headers](#headers)
- [Endpoints disponibles](#endpoints-disponibles)
  - [Facturation](#endpoints-facturation)
  - [Avoirs](#endpoints-note-de-credit)
  - [Devis](#endpoints-devis)
  - [Clients](#endpoints-clients)

## Introduction

L'API DijiPay est une API REST construite avec Laravel 11 qui fournit des fonctionnalités de facturation, gestion de clients, et intégration Peppol.

**URL de base**: `https://api.diji.be/api`

## Headers

- `Authorization` : Ce header contient le token Bearer d'authentification de l'utilisateur, par exemple : `Authorization: Bearer <votre_token>`  

- `X-Tenant` : Ce header contient le code du tenant (espace de travail), par exemple : `X-Tenant: codevo`




``` php
curl -X GET \
  https://api.diji.be/api/invoices \
  -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9...' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'X-Tenant: codevo'
```

### Endpoints Facturation

| Méthode | Endpoint | Description | 
|---------|----------|-------------
| GET | `/invoices` | Liste des factures 
| POST | `/invoices` | Créer une facture 
| GET | `/invoices/{id}` | Détails d'une facture 
| PUT | `/invoices/{id}` | Modifier une facture 
| DELETE | `/invoices/{id}` | Supprimer une facture 
| POST | `/invoices/{id}/document` | Générer PDF facture 
| POST | `/invoices/export` | Exporter factures 
| POST | `/invoices/{id}/send` | Envoyer facture 
| POST | `/invoices/{id}/send-to-peppol` | Envoyer via Peppol 

```json
{
  "identifier": "sometimes|nullable|string|max:255",
  "identifier_number": "sometimes|nullable|integer",
  "reference": "string|max:255|nullable",
  "status": "string|enum:draft,sent,paid,cancelled",
  "issue_date": "date|nullable",
  "due_date": "date|nullable", 
  "customer_id": "integer|exists:customers,id|required",
  "send_to_digiteal": "boolean|nullable",
  "is_autoliquidation": "boolean|nullable",
  "issuer": "sometimes|nullable",
  "issuer": {
    "name": "string|required_with:issuer",
    "vat_number": "string|nullable",
    "phone": "string|nullable",
    "email": "string|email|nullable",
    "line": "string|required_with:issuer",
    "city": "string|required_with:issuer",
    "zipcode": "string|required_with:issuer", 
    "country": "string|required_with:issuer",
    "iban": "string|required_with:issuer"
  },
  "recipient": {
    "name": "string|required_with:recipient",
    "vat_number": "string|nullable",
    "phone": "string|nullable",
    "email": "string|email|nullable", 
    "line": "string|required_with:recipient",
    "city": "string|required_with:recipient",
    "zipcode": "string|required_with:recipient",
    "country": "string|required_with:recipient"
  },
  "items": [
    {
      "type": "string|required",
      "position": "integer|nullable",
      "name": "string|required",
      "quantity": "integer|required_if:type,product",
      "vat": "integer|required_if:type,product|nullable",
      "discount_rate": "nullable|sometimes",
      "cost_unit_price": "sometimes|nullable", 
      "retail_unit_price": "sometimes|nullable" 
    }
  ]
}
```

#### Avoirs (Credit Notes)

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| GET | `/credit-notes` | Liste des avoirs |
| POST | `/credit-notes` | Créer un avoir |
| GET | `/credit-notes/{id}` | Détails d'un avoir |
| PUT | `/credit-notes/{id}` | Modifier un avoir |
| DELETE | `/credit-notes/{id}` | Supprimer un avoir |
| POST | `/credit-notes/{id}/document` | Générer PDF avoir |
| POST | `/credit-notes/export` | Exporter avoirs |
| POST | `/credit-notes/{id}/send-to-peppol` | Envoyer via Peppol |

```json
{
  "identifier": "sometimes|nullable|string|max:255",
  "identifier_number": "sometimes|nullable|integer",
  "reference": "string|max:255|nullable",
  "status": "string|enum:draft,sent,applied",
  "issue_date": "date|nullable",
  "due_date": "date|nullable",
  "customer_id": "integer|exists:customers,id|required",
  "invoice_id": "integer|exists:invoices,id|nullable",
  "send_to_digiteal": "boolean|nullable",
  "issuer": "sometimes|nullable",
  "issuer": {
    "name": "string|required_with:issuer",
    "vat_number": "string|nullable", 
    "phone": "string|nullable",
    "email": "string|email|nullable",
    "line": "string|required_with:issuer",
    "city": "string|required_with:issuer",
    "zipcode": "string|required_with:issuer",
    "country": "string|required_with:issuer",
    "iban": "string|required_with:issuer"
  },
  "recipient": {
    "name": "string|required_with:recipient",
    "vat_number": "string|required_with:recipient",
    "phone": "string|nullable",
    "email": "string|email|nullable",
    "line": "string|required_with:recipient",
    "city": "string|required_with:recipient", 
    "zipcode": "string|required_with:recipient",
    "country": "string|required_with:recipient"
  },
  "items": [
    {
      "type": "string|required",
      "position": "integer|nullable",
      "name": "string|required",
      "quantity": "integer|required_if:type,product",
      "vat": "integer|required_if:type,product|nullable",
      "discount_rate": "nullable|sometimes",
      "cost_unit_price": "sometimes|nullable", 
      "retail_unit_price": "sometimes|nullable" 
    }
  ]
}
```

#### Devis (Estimates)

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| GET | `/estimates` | Liste des devis |
| POST | `/estimates` | Créer un devis |
| GET | `/estimates/{id}` | Détails d'un devis |
| PUT | `/estimates/{id}` | Modifier un devis |
| DELETE | `/estimates/{id}` | Supprimer un devis |
| POST | `/estimates/{id}/document` | Générer PDF devis |

```json
{
  "identifier": "sometimes|nullable|string|max:255",
  "identifier_number": "sometimes|nullable|integer",
  "reference": "string|max:255|nullable",
  "status": "string|enum:draft,sent,accepted,rejected",
  "issue_date": "date|nullable",
  "due_date": "date|nullable",
  "customer_id": "integer|exists:customers,id|required", 
  "is_autoliquidation": "boolean|nullable",
  "issuer": "sometimes|nullable",
  "issuer": {
    "name": "string|required_with:issuer",
    "vat_number": "string|nullable",
    "phone": "string|nullable",
    "email": "string|email|nullable",
    "line": "string|required_with:issuer",
    "city": "string|required_with:issuer", 
    "zipcode": "string|required_with:issuer",
    "country": "string|required_with:issuer",
    "iban": "string|required_with:issuer"
  },
  "recipient": {
    "name": "string|required_with:recipient",
    "vat_number": "string|required_with:recipient",
    "phone": "string|nullable",
    "email": "string|email|nullable",
    "line": "string|required_with:recipient",
    "city": "string|required_with:recipient",
    "zipcode": "string|required_with:recipient", 
    "country": "string|required_with:recipient"
  },
  "items": [
    {
      "type": "string|required",
      "position": "integer|nullable",
      "name": "string|required",
      "quantity": "integer|required_if:type,product",
      "vat": "integer|required_if:type,product|nullable",
      "discount_rate": "nullable|sometimes",
      "cost_unit_price": "sometimes|nullable", 
      "retail_unit_price": "sometimes|nullable" 
    }
  ]
}
```

### Endpoints Clients

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| GET | `/customers` | Liste des clients |
| POST | `/customers` | Créer un client |
| GET | `/customers/{id}` | Détails d'un client |
| PUT | `/customers/{id}` | Modifier un client |
| DELETE | `/customers/{id}` | Supprimer un client |
| POST | `/customers/{id}/restore` | Restaurer un client |

```json
{
  "firstname": "string|nullable",
  "lastname": "string|nullable", 
  "company_name": "string|nullable",
  "vat_number": "string|max:20|nullable",
  "email": "string|email|max:150|required|unique",
  "phone": "string|max:150|nullable",
  "discount_rate": "integer|min:-100|max:100|nullable",
  "contacts": [
    {
      "firstname": "string|required_with:contacts",
      "lastname": "string|required_with:contacts", 
      "email": "string|email|max:150|unique|required_with:contacts",
      "phone": "string|max:150|nullable"
    }
  ],
  "addresses": [
    {
      "name": "string|nullable",
      "line": "string|required_with:addresses",
      "zipcode": "string|required_with:addresses",
      "city": "string|required_with:addresses", 
      "country": "string|required_with:addresses",
      "is_default": "boolean|nullable",
      "is_billing": "boolean|nullable",
      "is_shipping": "boolean|nullable"
    }
  ]
}
```
