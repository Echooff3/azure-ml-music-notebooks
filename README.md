# Azure ML Music Notebooks

Jupyter notebooks for training machine learning models on music-related tasks, specifically lyrics text generation.

## 📚 Contents

- **lyrics_training_notebook.ipynb** - Main notebook for training a text generation model on lyrics from Azure Blob Storage
- **deployment_config.json** - Configuration file for one-click deployment

## 🚀 Quick Start

### Prerequisites

1. Azure subscription with an Azure ML workspace
2. Azure Blob Storage account with a lyrics text file
3. Python 3.8+ environment with Jupyter

### One-Click Deployment Steps

1. **Configure your Azure resources**
   
   Edit `deployment_config.json` and replace the placeholder values:
   ```json
   {
     "azure_ml_workspace": {
       "subscription_id": "your-subscription-id",
       "resource_group": "your-resource-group",
       "workspace_name": "your-workspace-name",
       "location": "eastus"
     },
     "blob_storage": {
       "storage_account_name": "your-storage-account",
       "connection_string": "your-connection-string",
       "container_name": "lyrics",
       "blob_name": "lyrics.txt"
     }
   }
   ```

2. **Upload your lyrics data**
   
   Upload your lyrics.txt file to Azure Blob Storage:
   ```bash
   az storage blob upload \
     --account-name <your-storage-account> \
     --container-name lyrics \
     --name lyrics.txt \
     --file /path/to/your/lyrics.txt
   ```

3. **Open the notebook**
   
   - **Option A: Azure ML Studio**
     - Navigate to your Azure ML workspace
     - Go to Notebooks
     - Upload `lyrics_training_notebook.ipynb`
     - Open and run all cells
   
   - **Option B: Local Jupyter**
     ```bash
     pip install jupyter azureml-core azure-storage-blob tensorflow numpy pandas scikit-learn
     jupyter notebook lyrics_training_notebook.ipynb
     ```

4. **Run the notebook**
   
   Execute cells sequentially. The notebook will:
   - Connect to your Azure ML workspace
   - Load lyrics from blob storage
   - Preprocess the text data
   - Train an LSTM text generation model
   - Export and register the model
   - (Optional) Deploy to Azure ML endpoint

## 📋 Features

### Lyrics Training Notebook

- **Azure Integration**: Seamless connection to Azure ML workspace and Blob Storage
- **Data Loading**: Automatic download of lyrics from Azure Blob Storage
- **Text Processing**: Tokenization and sequence generation for LSTM training
- **Model Training**: LSTM-based neural network for text generation
- **Model Export**: Automatic model registration in Azure ML
- **Optional Deployment**: One-click deployment to Azure ML endpoint
- **Testing**: Built-in text generation examples

### Configuration File

- **Workspace Configuration**: Azure ML workspace parameters
- **Compute Settings**: Configurable compute cluster specifications
- **Storage Configuration**: Blob storage connection details
- **Model Settings**: Training and deployment parameters
- **Environment Setup**: Python dependencies specification

## 🔧 Configuration Options

### Environment Variables (Recommended)

Instead of hardcoding values in the notebook, set environment variables:

```bash
export AZURE_SUBSCRIPTION_ID="your-subscription-id"
export AZURE_RESOURCE_GROUP="your-resource-group"
export AZURE_WORKSPACE_NAME="your-workspace-name"
export STORAGE_ACCOUNT_NAME="your-storage-account"
export AZURE_STORAGE_CONNECTION_STRING="your-connection-string"
export BLOB_CONTAINER_NAME="lyrics"
export BLOB_NAME="lyrics.txt"
```

### Deployment Configuration

In `deployment_config.json`:

```json
{
  "deployment_options": {
    "auto_deploy": false,
    "deployment_type": "ACI",
    "service_name": "lyrics-generation-service",
    "cpu_cores": 1,
    "memory_gb": 2
  }
}
```

## 📦 Model Deployment

### On-Demand Deployment

To deploy the trained model to an Azure ML endpoint:

1. In the notebook, navigate to **Section 8: Optional Deploy Model to Azure ML Endpoint**
2. Set `DEPLOY_MODEL = True`
3. Run the deployment cells
4. The notebook will create a scoring endpoint and provide the URI

### Testing the Endpoint

```python
import requests
import json

endpoint_uri = "https://your-endpoint.azureml.net/score"
test_data = {
    "seed_text": "love",
    "next_words": 15
}

response = requests.post(
    endpoint_uri,
    data=json.dumps(test_data),
    headers={'Content-Type': 'application/json'}
)

print(response.json())
```

## 📊 Model Details

- **Architecture**: LSTM (Long Short-Term Memory)
- **Framework**: TensorFlow/Keras
- **Input**: Text sequences from lyrics
- **Output**: Next-word predictions for text generation
- **Training**: Character/word-level tokenization with sequence padding

## 🔒 Security Notes

- Never commit credentials or API keys to the repository
- Use Azure Key Vault for production deployments
- Use Managed Identities when possible
- Environment variables are preferred over hardcoded values

## 📝 Customization

### Modify Training Parameters

Edit cells in Section 5 of the notebook:

```python
epochs = 100  # Increase for better results
batch_size = 128  # Adjust based on memory
```

### Change Model Architecture

Edit the model definition in Section 5:

```python
model.add(LSTM(200, return_sequences=True))  # Increase units
model.add(Dropout(0.3))  # Adjust dropout
```

## 🐛 Troubleshooting

### Issue: Cannot connect to workspace

**Solution**: Ensure your workspace credentials are correct and you have proper permissions.

### Issue: Blob storage access denied

**Solution**: Verify your storage account key or connection string is valid.

### Issue: Out of memory during training

**Solution**: Reduce batch size or use a larger compute instance.

## 📚 Additional Resources

- [Azure ML Documentation](https://docs.microsoft.com/azure/machine-learning/)
- [Azure Blob Storage Documentation](https://docs.microsoft.com/azure/storage/blobs/)
- [TensorFlow/Keras Guide](https://www.tensorflow.org/guide/keras)

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## 📄 License

MIT License - feel free to use this for your projects!
