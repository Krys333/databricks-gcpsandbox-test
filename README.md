## Setting TEST infrastructure

Link to doc I followed - 
> Note: I am assuming you have a GCP CLI set up already and the correct project selected.

I run my infrastructure with a variables file `testing.tfvars`. You need to create one as well.
File looks as follows:

```tf
databricks_google_service_account = "example.iam.gserviceaccount.com" 
delegate_from = ["user:krystian.example@madetech.com"] 
prefix = "kbtest"
databricks_account_id = "example-12312-12312-a125-12361283"
google_project = "tf-pb-kb-test"
google_region = "europe-west2"
google_zone = "europe-west2-a"
```
- databricks_google_service_account # This will be created for you when you first apply the infrastructure.
- delegate_from # This is your GCP email. You need the user: part as well. Needs to be a list.
- prefix # Your pick. I use `initials-test`
- databricks_account_id # Found in Databricks dashboard. Go to your profile pic (top right) and copy the code.
- google_project # name of the google project you want to set up the resources in. 
- google_region
- google_zone  

Comment the whole `vpc.tf` script out and run the following.
This will create a google service account which you need to copy into your `tfvars`.
The service account should appear in tf outputs, but can also be found in [here](https://console.cloud.google.com/iam-admin/iam).

```console
cd infrastructure/
terraform init
terraform apply -var-file=testing.tfvars
```

Now you can uncomment code in the `vpc.tf` and re-run the apply.

```
terraform apply -var-file=testing.tfvars
```

The infrastructure should be up and running. If you go to your workspaces, you should see an empty workspace. You can add yourself in the permissions tab to enter the dashboards. 

# Notes
- You may get errors relating to disabled google services when applying the infrastructure for the first time on a new GCP account. Follow the links provided and enable the service inside GCP as needed.

