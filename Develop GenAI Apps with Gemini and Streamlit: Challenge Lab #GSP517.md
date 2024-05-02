## TASK 1 Complete Manually. Go through the video.

## TASK 2

### Using Cloud Shell clone the repo below from the default directory.
```cmd
git clone https://github.com/GoogleCloudPlatform/generative-ai.git
```
### Navigate to the gemini-streamlit-cloudrun directory.

```cmd
cd generative-ai/gemini/sample-apps/gemini-streamlit-cloudrun
```
### Download the chef.py file using the following command.

```cmd
gsutil cp gs://spls/gsp517/chef.py .
```
### Open the chef.py file in the Cloud Shell Editor and review the code.

```cmd
vi chef.py
```

> For see the line number ```:set nu```.
> For search changes in the task ```:/2.5```.
> FOR CHANGES press ```i``` to INSERT

### Changes in chef.py file in line no. 104
```cmd
wine = st.radio("Select wine Type", ['Red', 'White', 'None'], key='wine')
```

### add prompt in line no. 110.

```cmd
prompt = f"""I am a Chef.  I need to create {cuisine} \n
recipes for customers who want {dietary_preference} meals. \n
However, don't include recipes that use ingredients with the customer's {allergy} allergy. \n
I have {ingredient_1}, \n
{ingredient_2}, \n
and {ingredient_3} \n
in my kitchen and other ingredients. \n
The customer's wine preference is {wine} \n
Please provide some for meal recommendations.
For each recommendation include preparation instructions,
time to prepare
and the recipe title at the begining of the response.
Then include the wine paring for each recommendation.
At the end of the recommendation provide the calories associated with the meal
and the nutritional facts.
"""

```
> Save the file using ```esc``` then ```:wq``` 

### Run in Cloud shell
> ```COPY the LAST COMMAND FROM TASK 2 AND RUN IN CLOUD SHELL```

## TASK3
```cmd
python3 -m venv gemini-streamlit
source gemini-streamlit/bin/activate
pip install -r requirements.txt
```
```cmd
GCP_PROJECT='<Project id>'
GCP_REGION='<region>'
```
```cmd
streamlit run chef.py \
  --browser.serverAddress=localhost \
  --server.enableCORS=false \
  --server.enableXsrfProtection=false \
  --server.port 8080
```

-------open app link and create a recipe.

>   change app.py to chef.py 
```cmd
vi Dockerfile
```
change ```app.py``` to ```chef.py```
```cmd
AR_REPO='<Repo name from lab>'
SERVICE_NAME='<service name from lab>'
```
## TASK 4
```cmd
gcloud artifacts repositories create "$AR_REPO" --location="$GCP_REGION" --repository-format=Docker
gcloud builds submit --tag "$GCP_REGION-docker.pkg.dev/$GCP_PROJECT/$AR_REPO/$SERVICE_NAME"
```

## Task 5
```cmd
gcloud run deploy "$SERVICE_NAME" \
  --port=8080 \
  --image="$GCP_REGION-docker.pkg.dev/$GCP_PROJECT/$AR_REPO/$SERVICE_NAME" \
  --allow-unauthenticated \
  --region=$GCP_REGION \
  --platform=managed  \
  --project=$GCP_PROJECT \
  --set-env-vars=GCP_PROJECT=$GCP_PROJECT,GCP_REGION=$GCP_REGION
```
> Again Create a receipe

