```cmd
export ZONE=$(gcloud compute instances list --filter="name=(generative-ai-jupyterlab)" --format="value(zone)")
export REGION=${ZONE1::-2}
gcloud compute ssh --zone=$ZONE generative-ai-jupyterlab --quiet --command="cd /home/jupyter && sudo curl -o prompt.ipynb https://raw.githubusercontent.com/CodingWithHardik/HUSTLERS_GSP517/master/prompt.ipynb && sudo chown jupyter prompt.ipynb && export REGION='$REGION' && export PROJECT_ID='$DEVSHELL_PROJECT_ID' && sed -i 's|"Project ID"|"$PROJECT_ID"|g; s|"Region"|"$REGION"|g' prompt.ipynb && jupyter nbconvert --execute --to notebook --inplace prompt.ipynb && exit"
--command="cd /home/jupyter && sudo curl -o prompt.ipynb https://raw.githubusercontent.com/CodingWithHardik/HUSTLERS_GSP517/master/prompt.ipynb && sudo chown jupyter prompt.ipynb && export DEVSHELL_PROJECT_ID='$DEVSHELL_PROJECT_ID' && export GOOGLE_CLOUD_REGION='$REGION' && sudo su && jupyter nbconvert --execute --to notebook --inplace prompt.ipynb && exit && exit"
```
