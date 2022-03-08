# Check installation of a package for Composer environment - outside of this environemnt
This is to help customers understand and address issues with PyPI package installation without a risk of affecting their environment. 

## How it works
The tool uses Cloud Build to simulate a build process that checks whether PyPI package conflicts occur. 
You provide the tool with three inputs
- Name of the Composer environment you want to base the test on
- Region of the environment above
- Name (and optionally - a version) of a PyPI package that you want to check the conflicts for

The tool checks the setting of your Composer environment, pulls a Composer container image matching the version of your environment. Next, it adds all PyPI packages from the original Composer environment. As the last step, it attempts to add the package you want to test your environment with. 
Output of the build process is visible in the console, and any build issues (including the ones caused by PyPI conflicts) would be thrown by this tool. 

All of these operations are performed within Cloud Build, and so they have no impact on your existing Cloud Composer environment.

## How to use it

1. Authenticate to gcloud in a project of your Cloud Composer
2. Create a new folder and enter it. 
3. Download installchecker.yaml from this repository
4. Run the following command:
`gcloud builds submit --config installchecker.yaml . --substitutions=_ENVIRONMENT="MYENV",_REGION="MYREGION",_PACKAGE="MYPACKAGE"`
replacing:
+ `MYENV` with the name of your Cloud Composer environment
+ `MYREGION` with the region of the environment above, e.g. `us-central1`
+ `MYPACKAGE` with the name of a package, e.g. `magento` or `magento==3.1`
