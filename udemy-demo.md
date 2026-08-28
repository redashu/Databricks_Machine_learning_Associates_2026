# ML Feature Engineering, Training & MLflow Pipeline

```text
[Raw CSV]
    │
    ▼
[Clean Missing Values]
    │
    ▼
[Write to Feature Store]
    │
    │  ◄── We are here!
    │      Holds pristine strings & numbers
    ▼
[Create Training Set via Feature Lookups]
    │
    ▼
[Split into Train_DF and Test_DF]
    │
    ▼
[Fit ML Pipeline]
    │
    ├──► StringIndexer
    │
    ├──► OneHotEncoder
    │
    ├──► VectorAssembler
    │
    ├──► RobustScaler
    │
    └──► RandomForestClassifier
    │
    ▼
[Model Training]
    │
    ▼
[Evaluate Model]
    │
    ├──► Accuracy
    ├──► Precision
    ├──► Recall
    └──► F1 Score
    │
    ▼
[MLflow Tracking]
    │
    ├──► Log Parameters
    ├──► Log Metrics
    ├──► Log Model
    └──► Log Artifacts
    │
    ▼
[Register Model in Unity Catalog]
    │
    ▼
[Deploy Model]
    │
    ├──► Batch Inference
    ├──► Real-Time Serving
    └──► Streaming Inference
