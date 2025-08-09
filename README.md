# CarDealershipSAAD
Sales &amp; Inventory Management System - Sequence Diagram

sequenceDiagram
    title: Search On-Site Sequence Diagram

    participant Salesperson
    participant System
    participant CustomerDatabase
    participant InternalInventoryDatabase
    participant ExternalInventoryDatabase

    %% 1. Salesperson creates or retrieves customer profiles.
    Salesperson->>System: 1: Initiate Customer Profile
    activate Salesperson
    activate System
    System->>CustomerDatabase: 2: Query/Create Profile (Customer ID)
    activate CustomerDatabase
    CustomerDatabase-->>System: 3: Profile Data/Confirmation
    deactivate CustomerDatabase

    %% 2. Salesperson records customer vehicle preference.
    Salesperson->>System: 4: Record Vehicle Preference (Criteria)
    System->>CustomerDatabase: 5: Store Vehicle Preference
    activate CustomerDatabase
    CustomerDatabase-->>System: 6: Confirmation
    deactivate CustomerDatabase

    %% 3. Salesperson searches inventory, both internal and external based on customer preference.
    Salesperson->>System: 7: Request Inventory Search (Preferences)
    System->>InternalInventoryDatabase: 8: Search Internal Inventory
    activate InternalInventoryDatabase
    InternalInventoryDatabase-->>System: 9: Internal Results
    deactivate InternalInventoryDatabase
    System->>ExternalInventoryDatabase: 10: Search External Inventory
    activate ExternalInventoryDatabase
    ExternalInventoryDatabase-->>System: 11: External Results
    deactivate ExternalInventoryDatabase

    %% 4. System displays vehicles that most closely match customer preference.
    %% Alternate/Exceptional Flow: No Matching Vehicles. Go back to record customer preferences.
    alt Matching Vehicles Found
        System-->>Salesperson: 12: Matching Vehicles List
    else No Matching Vehicles Found
        System-->>Salesperson: 12a: No Match Notification
        Salesperson->>System: 13: Adjust Preferences/Search Criteria
        System->>CustomerDatabase: 14: Update Preference/Record Adjustment
        activate CustomerDatabase
        CustomerDatabase-->>System: 15: Confirmation
        deactivate CustomerDatabase
    end

    deactivate System
    deactivate Salesperson

# New Chart: On-site search activity diagram
---
config:
  layout: fixed
---
flowchart TD
    A["Start"] --> C{"Retrieve existing Customer Record?"}
    C -- Yes --> D["Retrieve Customer Record"]
    C -- No --> E["Create New Customer Record"]
    D --> F["Record Customer Preferences"]
    E --> F
    F --> G["Search Internal and External Inventories"]
    G --> M{"Matching Vehicles Found?"}
    M -- Yes --> H["Display Matching Vehicles"]
    M -- No --> N["Notify No Matching Vehicles"]
    H --> K["End"]
    N --> K
    style A fill:#D0F0C0,stroke:#3C9D9B,stroke-width:2px
    style C fill:#FFFFCC,stroke:#FFD700,stroke-width:2px
    style G fill:#ADD8E6,stroke:#4682B4,stroke-width:2px
    style M fill:#FFFFCC,stroke:#FFD700,stroke-width:2px
    style N fill:#FFCCCC,stroke:#CC0000,stroke-width:2px
    style K fill:#D0F0C0,stroke:#3C9D9B,stroke-width:2px
