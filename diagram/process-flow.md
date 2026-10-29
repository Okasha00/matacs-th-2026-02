
flowchart TD
    A["Upload proposals"] --> B["Select category"]
    B --> C["Load admin criteria"]
    C --> D["AI extracts proposal data"]
    D --> E["Validate extracted data using code"]
    E --> F{"Mandatory checks passed?"}
    F -->|Yes| G["Calculate weighted scores using code"]
    F -->|No or unclear| H["Flag for human review"]
    G --> I["Rank proposals"]
    I --> J["Show scores, evidence and reasons"]
    H --> K["Human checks missing information"]
    J --> L["Human reviews ranking"]
    K --> L
    L --> M["Human makes final award decision"]
    M --> N["Log results and decisions"]
