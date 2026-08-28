graph TD
    A[Raw CSV Ingestion] --> B[Data Cleaning & Imputation]
    B --> C[Unity Catalog Feature Table]
    C --> D[Feature Store Lookups & Observation Set]
    D --> E[Train / Test Split]
    E --> F[PySpark ML Pipeline]
    
    subgraph F [Unified Preprocessing & Modeling]
        F1[StringIndexer] --> F2[OneHotEncoder]
        F2 --> F3[Numeric VectorAssembler]
        F3 --> F4[RobustScaler]
        F4 --> F5[Final VectorAssembler]
        F5 --> F6[RandomForestClassifier]
    end
    
    F --> G[MLflow & Feature Store Client Logging]
    G --> H[Unity Catalog Model Registry]
    H --> I[Real-time Serving REST Endpoint]
