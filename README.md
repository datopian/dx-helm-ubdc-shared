# Repository for data.ubdc.ac.uk

This repository is intended to manage the deployment of **data.ubdc.ac.uk**. Below are detailed steps for deploying the service to the Kubernetes cluster using **Google Cloud Build**.

---

## Deployment Overview

### Components

- **Google Cloud Build**: Automates the build process, initiating deployment to the Kubernetes cluster.
- **Kubernetes Cluster**: Hosts the deployed application.
- **CKAN**: Open-source data management system, deployed as part of the application.
- **Helm**: Used for managing Kubernetes applications and deployments.

---

## How to Deploy

### Step 1: Prepare Configuration Files

1. **Update `helm-templates/values.yaml.template`**  
   The `values.yaml.template` file contains critical configuration for the deployment. Here you can set variables that need to be reflected in CKAN (refer to `ckan->ckan` for specifics).

   - Ensure that the appropriate environment variables are set under `ckanext-envvars`.
   - **Do not change the `solr` and `db` settings unless explicitly required**.
   
2. **Modify CKAN Image Configuration**  
   If you need to modify the CKAN-related image settings (e.g., the Docker image or related configurations), make these changes within the `ckan` directory.

   - Navigate to the `ckan` directory.
   - Update the image settings and other related parameters as required.

---

### Step 2: Commit Changes

Once you have made the required modifications to the configuration files:

1. **Commit Changes to the Repository**  
   - Commit your changes to the `master` branch of the repository.
   
2. **Trigger Build and Deployment**  
   Once the commit is pushed, **Google Cloud Build** will automatically detect the changes and trigger the build process.  
---

### Step 3: Environment Configuration

Before deployment, ensure all placeholders in the configuration files are replaced with actual values:

- **`<replace_here>`**: Replace with sensitive or environment-specific data (e.g., passwords, API keys).
- **`<replace_internal_project_url_here>`**: If your project uses an internal URL, replace this placeholder with it; otherwise, use the external URL.

Example:

- **DB Connection String**: `CKAN_SQLALCHEMY_URL=<replace_here>`
- **Project URL**: `domain_name=<replace_internal_project_url_here>`

---

### Step 4: Enable Solr and Datapusher (If Needed)

1. **Solr and Datapusher** are essential components for CKAN. If your deployment requires them, you may need to enable them by updating the `values.yaml.template.template` file.

   - Set the necessary flags to `true` for both Solr and Datapusher services.
   - Ensure that the configurations are accurate to prevent deployment errors.

---

## Visual Deployment Flow

Here’s a flowchart to help visualize the deployment process:

```plaintext
+------------------+
| Modify config    |
| files (values.yaml.template, |
| ckan directory)  |
+------------------+
        |
        v
+------------------+
| Commit changes   |
| to master branch |
+------------------+
        |
        v
+------------------------+
| Google Cloud Build     |
| detects the commit and |
| triggers the build      |
+------------------------+
        |
        v
+------------------------+
| Kubernetes Cluster     |
| deploys CKAN and other |
| related services       |
+------------------------+
        |
        v
+------------------------+
| Deployment completes   |
|                        |
+------------------------+
```

---

## Troubleshooting

1. **Configuration Issues**:
   - Double-check that all placeholders (`<replace_here>`, `<replace_internal_project_url_here>`) have been correctly replaced.
   - Ensure that any environment-specific configurations are correctly set (e.g., DB connection strings, API keys).