# GSP517
## Run in cloudshell
### Do Task 1 Manually
```cmd
export ZONE=$(gcloud compute instances list --filter="name=(generative-ai-jupyterlab)" --format="value(zone)")
export REGION=${ZONE::-2}
git clone https://github.com/GoogleCloudPlatform/generative-ai.git
cd generative-ai/gemini/sample-apps/gemini-streamlit-cloudrun
rm -f Dockerfile
curl -o Dockerfile https://raw.githubusercontent.com/CodingWithHardik/HUSTLERS_GSP517/master/Dockerfile
curl -o chef.py https://raw.githubusercontent.com/CodingWithHardik/HUSTLERS_GSP517/master/chef.py
gcloud storage cp chef.py gs://$DEVSHELL_PROJECT_ID-generative-ai/
python3 -m venv gemini-streamlit
source gemini-streamlit/bin/activate
pip install -r requirements.txt
GCP_PROJECT=$DEVSHELL_PROJECT_ID
GCP_REGION=$REGION
streamlit run chef.py \
--browser.serverAddress=localhost \
--server.enableCORS=false \
--server.enableXsrfProtection=false \
--server.port 8080
```
## Go to link and create a recipe
## Check the progress till task 3
## Press `CTRL` **+** `C`
```cmd
gcloud services enable run.googleapis.com
AR_REPO='chef-repo'
SERVICE_NAME='chef-streamlit-app'
gcloud artifacts repositories create "$AR_REPO" --location="$GCP_REGION" --repository-format=Docker
gcloud builds submit --tag "$GCP_REGION-docker.pkg.dev/$GCP_PROJECT/$AR_REPO/$SERVICE_NAME"
gcloud run deploy "$SERVICE_NAME" \
--port=8080 \
--image="$GCP_REGION-docker.pkg.dev/$GCP_PROJECT/$AR_REPO/$SERVICE_NAME" \
--allow-unauthenticated \
--region=$GCP_REGION \
--platform=managed  \
--project=$GCP_PROJECT \
--set-env-vars=GCP_PROJECT=$GCP_PROJECT,GCP_REGION=$GCP_REGION
```
## Go to link and create a recipe
