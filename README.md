Warehouse Stock Management — SAP ABAP RAP

Author: Ajay Bishnoi

A real-world Warehouse Stock Management application built using the SAP ABAP RESTful Application Programming Model (RAP) and SAP HANA.

This project is created by as a hands-on learning project to understand how to build a complete transactional application using RAP, from database tables to a Fiori Elements UI.


📌 Project Overview

The application manages:

Warehouses
Storage Bins
Stock Items
Stock Transfers
Bin Status
Stock Quantity
Bin Capacity

The main business operation of this project is Transfer Stock.

For example, if a warehouse has:

BIN-A01 → 500 KG
BIN-A02 → 100 KG


A user can transfer:

200 KG
BIN-A01 → BIN-A02


After the transfer:

BIN-A01 → 300 KG
BIN-A02 → 300 KG


The total stock remains the same. Only the stock location changes.

🏗️ Application Architecture

The project follows the standard RAP architecture:

SAP HANA Database
        ↓
Persistent Tables
        ↓
CDS Interface Views
        ↓
Behavior Definition
        ↓
Behavior Implementation
        ↓
CDS Projection Views
        ↓
Service Definition
        ↓
Service Binding
        ↓
OData V4
        ↓
Fiori Elements
        ↓
User

🗄️ Data Model

The application contains three main entities.

Warehouse
    │
    └── Storage Bin
            │
            └── Stock Item

Warehouse

A warehouse represents a physical warehouse or storage location.

Example:

WH001
Central Distribution Center
Ludhiana, Punjab

Storage Bin

A warehouse contains multiple storage bins.

Example:

WH001
 ├── BIN-A01
 ├── BIN-A02
 └── BIN-A03


Each bin has:

Bin Type
Maximum Capacity
Current Quantity
Unit
Status
Stock Item

A storage bin can contain multiple stock items.

Example:

BIN-A01
 ├── Steel
 ├── Copper
 └── Cement


A stock item contains:

Material
Quantity
Unit
Batch Number
Expiry Date
🔗 Entity Relationship

The RAP business object uses composition:

Warehouse
    │
    │ 1 : N
    ▼
Storage Bin
    │
    │ 1 : N
    ▼
Stock Item


This means:

One Warehouse can have many Storage Bins.
One Storage Bin can have many Stock Items.
Storage Bins belong to a Warehouse.
Stock Items belong to a Storage Bin.

⚙️ Main Features
Create Warehouse

Users can create a new warehouse with information such as:

Warehouse ID
Warehouse Name
Location
Status

Manage Storage Bins

Users can:

Create bins
Update bins
View bin capacity
View current quantity
Block bins
Unblock bins
Manage Stock Items

Users can maintain:

Material
Quantity
Unit
Batch
Expiry Date


🔄 Transfer Stock

The main custom action of the application is:

Transfer Stock

It transfers stock from one storage bin to another.

Example

Before transfer:

BIN-A01
Current Quantity = 500 KG

BIN-A02
Current Quantity = 100 KG


User enters:

Source Bin  = BIN-A01
Target Bin  = BIN-A02
Quantity    = 200 KG


The system performs the required validations.

If everything is valid:

BIN-A01 = 500 - 200
        = 300 KG

BIN-A02 = 100 + 200
        = 300 KG


✅ Transfer Validations

Before transferring stock, the application checks:

1. Source bin exists
2. Target bin exists
3. Source bin is active
4. Target bin is active
5. Source has enough stock
6. Target has enough capacity
7. Transfer quantity is greater than zero
8. Source and target units are compatible


If any validation fails, the transfer is rejected and an appropriate RAP message is displayed.

Example:

Source bin does not have sufficient stock.

Target bin does not have sufficient capacity.

The selected bin is blocked.

Transfer quantity must be greater than zero.

🔘 Application Actions

The application contains custom RAP actions.

Transfer Stock

Available from the Storage Bin list.


[ Transfer Stock ]


Moves stock from one bin to another.

Block Bin

Available on the Storage Bin Object Page.

[ Block Bin ]


Changes the bin status to blocked.

Unblock Bin

Available on the Storage Bin Object Page.

[ Unblock Bin ]


Changes the bin status back to active.

🧠 RAP Concepts Used

This project covers the major concepts of the ABAP RAP model:

Persistent Database Tables
CDS Interface Views
CDS Projection Views
Associations
Compositions
Managed RAP
Behavior Definitions
Behavior Implementations
CRUD Operations
Validations
Determinations
Actions
Action Parameters
RAP Messages
EML
Authorization
Optimistic Concurrency Control
ETag
Draft
UI Annotations
UI Facets
Data Points
OData V4
Fiori Elements
🔒 Concurrency Control

The application uses optimistic concurrency control.

A timestamp field such as:

LOCAL_LAST_CHANGED_AT


is used for ETag handling.

For example:

User A reads BIN-A01
        ↓
User B changes BIN-A01
        ↓
User A tries to save
        ↓
RAP detects that the data has changed
        ↓
User A receives a conflict


This prevents one user from accidentally overwriting another user's changes.

🔐 Authorization

Authorization can be implemented to control which users can perform specific operations.

For example:

Warehouse Manager
    ↓
Create Warehouse
Update Warehouse
Transfer Stock
Block Bin
Unblock Bin


Other users can be restricted to read-only operations depending on the authorization design.

🖥️ Fiori Elements

The RAP business object is exposed through OData V4 and consumed by a Fiori Elements application.

The basic navigation is:

Warehouse List
      ↓
Warehouse Object Page
      ↓
Storage Bins
      ↓
Storage Bin Object Page
      ↓
Stock Items


The UI is generated using RAP metadata and UI annotations.

🎨 UI Annotations

The project also demonstrates Fiori Elements UI annotations such as:

@UI.headerInfo
@UI.facet
@UI.dataPoint
@UI.lineItem
@UI.identification


These annotations control how the business object appears in the Fiori Elements application.

For example, the Object Page header can contain:

Warehouse Name
Warehouse Location
Warehouse Image/Icon
Current Quantity
Other Data Points


Custom actions such as Transfer Stock, Block Bin, and Unblock Bin can also be exposed through UI annotations.

🚀 Development Process

The project is developed in the following sequence:

1. Create HANA Database Tables
             ↓
2. Create CDS Interface Views
             ↓
3. Create Associations and Compositions
             ↓
4. Create Behavior Definition
             ↓
5. Implement CRUD
             ↓
6. Add Validations
             ↓
7. Add Determinations
             ↓
8. Create Custom Actions
             ↓
9. Implement RAP Messages
             ↓
10. Add Authorization
             ↓
11. Add ETag / Concurrency Control
             ↓
12. Create CDS Projection Views
             ↓
13. Create Projection Behavior
             ↓
14. Create Service Definition
             ↓
15. Create Service Binding
             ↓
16. Add UI Annotations
             ↓
17. Test using Fiori Elements

🧪 Example Test Scenario

A simple test scenario is:

Warehouse:
WH001

Source Bin:
BIN-A01
500 KG

Target Bin:
BIN-A02
100 KG


Execute:

Transfer Stock
200 KG
BIN-A01 → BIN-A02


Expected result:

BIN-A01 → 300 KG
BIN-A02 → 300 KG


If the source contains only:

50 KG


and the user tries to transfer:

100 KG


the system should return:

Error:
Insufficient stock in source bin.

🛠️ Technologies

This project uses:

SAP ABAP
ABAP RAP
ABAP CDS
SAP HANA
OData V4
Fiori Elements
SAP Business Application Studio / ADT
Eclipse
📚 Learning Goals

The main goal of this project is to understand how a real transactional application is developed using SAP RAP.

The project provides practical experience with:

Database
   ↓
CDS
   ↓
RAP Behavior
   ↓
Business Logic
   ↓
OData
   ↓
Fiori Elements


Instead of learning each RAP concept separately, this project combines them into one complete business scenario.

🔮 Future Enhancements

Possible future enhancements include:

Stock In action
Stock Out action
Inter-warehouse stock transfer
Material master integration
Batch management
Expiry-date validation
Stock movement history
Inventory dashboard
Low-stock alerts
Capacity utilization
Barcode/QR code scanning
Advanced authorization
Analytical reporting
📌 Project Status
🚧 In Development


This is a SAP ABAP RAP learning and portfolio project focused on building a realistic Warehouse Stock Management application.

👨‍💻 Skills Demonstrated
SAP ABAP
SAP RAP
ABAP CDS
SAP HANA
OData V4
Fiori Elements
RAP Behavior
RAP Actions
RAP Validations
RAP Determinations
RAP Messages
EML
Authorization
ETag
Concurrency Control
UI Annotations



⭐ Final Overview
                 WAREHOUSE STOCK MANAGEMENT
                           │
                           ▼
                      Warehouse
                           │
                           ▼
                      Storage Bin
                           │
                           ▼
                      Stock Item
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
          CRUD        Validation     Actions
                                         │
                              ┌──────────┼──────────┐
                              ▼          ▼          ▼
                         Transfer     Block      Unblock
                           Stock        Bin         Bin
                              │
                              ▼
                       RAP Business Logic
                              │
                              ▼
                           OData V2
                              │
                              ▼
                       Fiori Elements


Built to learn and demonstrate the complete SAP ABAP RAP development lifecycle through a real-world warehouse management scenario.
