# Database Schema for INTELLECTRA 2025 Purchase Prediction

This document provides a comprehensive overview of the relational database schema used in the INTELLECTRA 2025 competition. The goal of the competition is to predict whether a customer will make a purchase in the following month (`next_buy`). Below is a detailed explanation of each table and their relationships.

## Table Descriptions

### 1. `train_label`

* **Purpose**: Stores training labels indicating whether a member made a purchase in the next month.
* **Fields**:

  * `MemberID` (text): Unique identifier for each member.
  * `next_buy` (integer): Target variable (1 = will buy, 0 = won't buy).

### 2. `submission`

* **Purpose**: Placeholder table for submitting predictions on the test set.
* **Fields**:

  * `MemberID` (text): Unique identifier for each member.
  * `next_buy` (integer): Prediction of whether the member will buy next month.

### 3. `member`

* **Purpose**: Contains demographic and household data of members.
* **Fields**:

  * `MemberID` (text): Unique ID (primary key).
  * `JoinDate` (date): Date of registration.
  * `DateOfBirth` (date): Birth date of the member.
  * `City` (text): City of residence.
  * `NoOfChild` (integer): Number of children.
  * `YoungestKidDOB` (date): DOB of youngest child.
  * `EldestKidDOB` (date): DOB of eldest child.

### 4. `train_transaction`

* **Purpose**: Contains historical transaction data for training members.
* **Fields**:

  * `TransactionID` (integer): Unique ID for each transaction.
  * `MemberID` (varchar): ID of member making the transaction.
  * `Source` (text): Channel used to purchase.
  * `FK_PRODUCT_ID` (integer): Foreign key to the `product` table.
  * `FK_PROD_GRAM_ID` (integer): Foreign key to the `program` table.
  * `Qty` (varchar): Quantity of the product.
  * `PricePerUnit` (double): Price per unit purchased.
  * `TransactionDatetime` (timestamp): Date and time of transaction.

### 5. `test_transaction`

* **Purpose**: Contains historical transaction data for test members.
* **Fields**: Same as `train_transaction`.

### 6. `product`

* **Purpose**: Contains metadata for each product.
* **Fields**:

  * `productID` (integer): Primary key.
  * `ProductName` (text): Name of the product.
  * `ProductCategory` (text): Category label.
  * `ProductLevel` (text): Product hierarchy level or type.

### 7. `program`

* **Purpose**: Contains information about packaging grammage.
* **Fields**:

  * `programID` (integer): Primary key.
  * `GrammageName` (text): Name of the grammage program.
  * `Point` (integer): Points given for the package.
  * `Price` (double): Price of the package variant.

## Relationships

* `train_label.MemberID` and `submission.MemberID` both refer to `member.MemberID`.
* `train_transaction.MemberID` and `test_transaction.MemberID` link to `member.MemberID`.
* `train_transaction.FK_PRODUCT_ID` and `test_transaction.FK_PRODUCT_ID` link to `product.productID`.
* `train_transaction.FK_PROD_GRAM_ID` and `test_transaction.FK_PROD_GRAM_ID` link to `program.programID`.

## Usage Flow

1. Join `train_transaction` with `product` and `program` to get product attributes.
2. Join `train_transaction` with `member` to add customer profile info.
3. Use `train_label` to supervise learning for prediction of `next_buy`.
4. Perform same joins on `test_transaction` for generating submission.

---

This schema is central to building predictive models and performing feature engineering in the competition. Use this document as a reference when designing your ML pipeline or exploring the dataset.
