
# Infrastructure as Code (IaC)

This repository is designed to provision AWS resources using Jenkins and Terraform.

## Steps to Get Started:

1. **Integrate Terraform with Jenkins**  
   - Configure Jenkins to work with Terraform.
   - Navigate to `Manage Jenkins` → `Tools` → Add the Terraform executable path.

2. **Set Up Credentials**  
   - Pass your custom AWS Access Key and Secret Key as Jenkins credentials.
   - Map these Jenkins credentials to the environment section using the Credentials ID.

3. **Repository Structure**  
   - The repository currently includes three key files:
     - **`provider.tf`**: Define the cloud provider (e.g., AWS).
     - **`main.tf`**: Configure all AWS resources.
     - **`variables.tf`**: Define input variables required for resource configuration.

4. **Adding New Resources**  
   - To add additional resources:
     1. Fork this repository.
     2. Add new `.tf` files for your desired resources.
     3. Update the `variables.tf` file with the required input variables.

---

Thank you! 😊

--- 
