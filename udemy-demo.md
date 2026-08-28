[Raw CSV] 
   │
   ▼
[Clean Missing Values] 
   │
   ▼
[Write to Feature Store]  <── We are here! (Holds pristine strings & numbers)
   │
   ▼
[Create Training Set via Feature Lookups]
   │
   ▼
[Split into Train_DF and Test_DF]
   │
   ▼
[Fit ML Pipeline] ──► Includes: StringIndexer 
                                ──► OneHotEncoder 
                                ──► VectorAssembler 
                                ──► RobustScaler 
                                ──► RandomForestClassifier
