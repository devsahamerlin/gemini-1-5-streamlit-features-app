# Cloud Run application utilizing Streamlit Framework that demonstrates working with Vertex AI Gemini API

```shell
gcloud services enable cloudbuild.googleapis.com cloudfunctions.googleapis.com run.googleapis.com logging.googleapis.com storage-component.googleapis.com aiplatform.googleapis.com

python3 -m venv gemini-1-5-streamlit
source gemini-1-5-streamlit/bin/activate
pip install -r requirements.txt

export GCP_PROJECT=$(gcloud config get-value project)  # Change this
export GCP_REGION=europe-west4          # If you change this, make sure the region is supported.

streamlit run app.py \
  --browser.serverAddress=localhost \
  --server.enableCORS=false \
  --server.enableXsrfProtection=false \
  --server.port 8080

AR_REPO='gemini-repo'
SERVICE_NAME='gemini-streamlit-app' 
gcloud artifacts repositories create "$AR_REPO" --location="$GCP_REGION" --repository-format=Docker
gcloud builds submit --tag "$GCP_REGION-docker.pkg.dev/$GCP_PROJECT/$AR_REPO/$SERVICE_NAME"

   
gcloud run deploy "$SERVICE_NAME" \
  --port=8080 \
  --source=. \
  --allow-unauthenticated \
  --region=$GCP_REGION \
  --project=$GCP_PROJECT \
  --set-env-vars=GCP_PROJECT=$GCP_PROJECT,GCP_REGION=$GCP_REGION
    
gcloud services disable cloudbuild.googleapis.com cloudfunctions.googleapis.com run.googleapis.com logging.googleapis.com storage-component.googleapis.com aiplatform.googleapis.com

     
```

On successful deployment, you will be provided a URL to the Cloud Run service. You can visit that in the browser to view the Cloud Run application that you just deployed. Choose the functionality that you would like to check out and the application will prompt the Vertex AI Gemini API and display the responses.

Congratulations!

