# Quick Start Guide

This guide will help you deploy and run the lyrics training notebook in just a few steps.

## 🎯 Goal

Train a text generation model on your lyrics data and optionally deploy it to Azure ML.

## ⚡ Steps

### 1. Prepare Your Azure Resources (5 minutes)

#### Create Azure ML Workspace

```bash
# Using Azure CLI
az ml workspace create \
    --name my-ml-workspace \
    --resource-group my-resource-group \
    --location eastus
```

Or use the [Azure Portal](https://portal.azure.com) to create a workspace.

#### Create Storage Account and Upload Lyrics

```bash
# Create storage account
az storage account create \
    --name mylyricstorage \
    --resource-group my-resource-group \
    --location eastus \
    --sku Standard_LRS

# Create container
az storage container create \
    --name lyrics \
    --account-name mylyricstorage

# Upload your lyrics file
az storage blob upload \
    --account-name mylyricstorage \
    --container-name lyrics \
    --name lyrics.txt \
    --file /path/to/your/lyrics.txt
```

### 2. Configure the Deployment (2 minutes)

#### Option A: Using config.json (Recommended)

1. Copy the example config:
   ```bash
   cp config.json.example config.json
   ```

2. Edit `config.json` with your values:
   ```json
   {
       "subscription_id": "your-actual-subscription-id",
       "resource_group": "my-resource-group",
       "workspace_name": "my-ml-workspace",
       "workspace_region": "eastus"
   }
   ```

#### Option B: Using Environment Variables

```bash
export AZURE_SUBSCRIPTION_ID="your-subscription-id"
export AZURE_RESOURCE_GROUP="my-resource-group"
export AZURE_WORKSPACE_NAME="my-ml-workspace"
export AZURE_STORAGE_CONNECTION_STRING="your-storage-connection-string"
export BLOB_CONTAINER_NAME="lyrics"
export BLOB_NAME="lyrics.txt"
```

### 3. Run the Notebook (One Click!)

#### In Azure ML Studio:

1. Navigate to https://ml.azure.com
2. Select your workspace
3. Go to **Notebooks** in the left menu
4. Click **Upload files**
5. Upload `lyrics_training_notebook.ipynb`
6. Click on the notebook to open it
7. Click **Run All** or execute cells one by one

#### Locally:

```bash
# Install dependencies
pip install -r requirements.txt

# Start Jupyter
jupyter notebook lyrics_training_notebook.ipynb

# Run all cells
```

### 4. Monitor Training

Watch the training progress in the notebook output:
- Data loading confirmation
- Training metrics (loss, accuracy)
- Model evaluation results
- Registration confirmation

### 5. (Optional) Deploy the Model

To deploy to an Azure ML endpoint:

1. In the notebook, scroll to **Section 8**
2. Change `DEPLOY_MODEL = False` to `DEPLOY_MODEL = True`
3. Run the deployment cells
4. Wait for deployment (5-10 minutes)
5. Get your scoring URI and test it!

## 📋 Checklist

Before running:
- [ ] Azure ML workspace created
- [ ] Storage account with lyrics.txt uploaded
- [ ] Config file or environment variables set
- [ ] Notebook uploaded to Azure ML Studio (or Jupyter running locally)

## 🎉 What You Get

After running the notebook:
1. ✅ Trained text generation model
2. ✅ Model registered in Azure ML
3. ✅ Training metrics logged
4. ✅ (Optional) Deployed endpoint for inference

## 🔍 Verify Success

Check these in Azure ML Studio:
1. **Experiments** → Find "lyrics-training-experiment"
2. **Models** → Find "lyrics-text-generation-model"
3. **Endpoints** → Find "lyrics-generation-service" (if deployed)

## 🧪 Test Your Deployed Model

```python
import requests
import json

scoring_uri = "https://your-endpoint.azureml.net/score"

test_data = {
    "seed_text": "love",
    "next_words": 10
}

response = requests.post(
    scoring_uri,
    data=json.dumps(test_data),
    headers={'Content-Type': 'application/json'}
)

print(response.json())
# Output: {"generated_text": "love is a beautiful thing..."}
```

## 🆘 Common Issues

### "Cannot find workspace"
- Check your subscription ID and workspace name
- Ensure you're logged in: `az login`

### "Blob not found"
- Verify the blob name and container
- Check storage account permissions

### "Out of memory"
- Reduce batch_size in the notebook
- Use a larger compute instance

## 📞 Need Help?

- Check the full [README.md](README.md) for detailed documentation
- Review `deployment_config.json` for all configuration options
- Open an issue on GitHub

## ⏱️ Time Estimate

- Setup: 5-10 minutes
- Training: 10-30 minutes (depending on data size)
- Deployment: 5-10 minutes (optional)
- **Total: 20-50 minutes**
