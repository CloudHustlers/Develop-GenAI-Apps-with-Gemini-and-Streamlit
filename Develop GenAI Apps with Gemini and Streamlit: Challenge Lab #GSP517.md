> Using nano editor 
```cmd
 nano chef.py
```
```cmd
wine = st.radio(
          "What wine do you prefer?\n\n", ["Red", "white", "None"], key="wine", horizontal=True
        )
```
> save file using ```ctrl+O``` than ```Enter``` Then ```ctrl + x```

```cmd
PROJECT=' '
```
```cmd
REGION=' '
```

> Create virtual environment
```cmd
python3 -m venv gemini-streamlit
```


> Activate virtual environment
```cmd
source gemini-streamlit/bin/activate
```


> Install dependencies
```cmd
python3 -m pip install -r requirements.txt
```

> Run Streamlit app for development (modify options as needed)
```cmd
streamlit run chef.py --browser.serverAddress=localhost --server.enableCORS=false --server.enableXsrfProtection=false --server.port 8080
```
> Create empty Dockerfile
```cmd
nano Dockerfile
```
paste ```chef.py``` in docker file in entrypoint.
save file using ```ctrl+O``` than ```Enter``` Then ```ctrl + x```


> Google Cloud Run deployment (configure based on your project)
```cmd
AR_REPO='chef-repo'
```
```cmd
SERVICE_NAME='chef-streamlit-app'
```

```cmd
gcloud artifacts repositories create "$AR_REPO" --location "$REGION" --repository-format=Docker
```
```cmd
gcloud builds submit --tag "$REGION-docker.pkg.dev/$PROJECT/$AR_REPO/$SERVICE_NAME"
```
```cmd
gcloud run deploy "$SERVICE_NAME" --port=8080 --image="$REGION-docker.pkg.dev/$PROJECT/$AR_REPO/$SERVICE_NAME" --allow-unauthenticated --region=$REGION --platform=managed --project=$PROJECT --set-env-vars=PROJECT=$PROJECT,REGION=$REGION
```

