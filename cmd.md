# Cloud IAM: Qwik Start || **GSP064**

**Command:**

```bash
# 1. Set environment variables
export PROJECT_ID=$(gcloud config get-value project)
export BUCKET_NAME="${PROJECT_ID}-bucket"

# 2. Get Username 2 from lab environment credentials
USER2=$(gcloud projects get-iam-policy $PROJECT_ID \
  --format="json" | jq -r '.bindings[] | select(.role=="roles/viewer") | .members[]' | grep "user:")

# Task 2: Create a Cloud Storage bucket and upload a sample file
gcloud storage buckets create gs://$BUCKET_NAME --location=US
echo "Hello World" > sample.txt
gcloud storage cp sample.txt gs://$BUCKET_NAME/sample.txt

# Task 3: Remove the Project Viewer role for Username 2
gcloud projects remove-iam-policy-binding $PROJECT_ID \
  --member="$USER2" \
  --role="roles/viewer"

# Task 4: Grant Storage Object Viewer permission to Username 2
gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member="$USER2" \
  --role="roles/storage.objectViewer"
