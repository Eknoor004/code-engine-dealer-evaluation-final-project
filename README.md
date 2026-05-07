# Code Engine Dealer Evaluation Final Project

This repository contains the final project implementation for the IBM Code Engine course using the Python lab track.  
The application is composed of two backend microservices and one frontend microservice that integrates both APIs.

## Repository Structure

- `backend/products_list/` - Product Details microservice (Python, port `5000`)
- `backend/dealer_details/` - Dealer Pricing microservice (Node.js, port `8080`)
- `frontend/` - Dealer Evaluation frontend microservice (Python, port `5001`)
- `docs/submission-checklist.md` - Task-by-task grading evidence checklist

## Code Engine Deployment Commands

### 1) Deploy Product Details backend

```bash
ibmcloud ce application create --name prodlist \
  --image us.icr.io/${SN_ICR_NAMESPACE}/prodlist \
  --registry-secret icr-secret \
  --port 5000 \
  --build-context-dir products_list \
  --build-source https://github.com/ibm-developer-skills-network/dealer_evaluation_backend.git
```

### 2) Deploy Dealer Pricing backend

```bash
ibmcloud ce application create --name dealerdetails \
  --image us.icr.io/${SN_ICR_NAMESPACE}/dealerdetails \
  --registry-secret icr-secret \
  --port 8080 \
  --build-context-dir dealer_details \
  --build-source https://github.com/ibm-developer-skills-network/dealer_evaluation_backend.git
```

### 3) Configure frontend API endpoints

Update `frontend/html/index.html`:

- `produrl` -> Product Details deployment URL (must end with `/`)
- `dealerurl` -> Dealer Pricing deployment URL (must end with `/`)

### 4) Deploy frontend

From inside `frontend/`:

```bash
ibmcloud ce application create --name frontend \
  --image us.icr.io/${SN_ICR_NAMESPACE}/frontend \
  --registry-secret icr-secret \
  --port 5001 \
  --build-source .
```

## Functional Validation

After deployment, open the frontend URL and verify:

1. Products are preloaded in the product dropdown.
2. Selecting a product loads dealer options.
3. Selecting a dealer shows the dealer-specific price.
4. Selecting `All Dealers` shows all available dealer prices for the product.
