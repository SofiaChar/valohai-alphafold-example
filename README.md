# Alphafold on Valohai

Welcome to the repository for running AlphaFold on Valohai! This repository is a tailored version of the official [AlphaFold repository by DeepMind](https://github.com/deepmind/alphafold), enhanced with a `valohai.yaml` configuration file to streamline deployment on the Valohai MLOps platform.

Valohai simplifies machine learning workflows, enabling engineers and scientists to focus on their research without the overhead of managing infrastructure.

## Key Features
- **Simplified Configuration**: Includes a ready-to-use `valohai.yaml` file which contains all necessary specifications for running AlphaFold on Valohai.
- **Dataset Management**: Unique setup to mount large datasets directly to the compute environment, optimizing resource use and execution time.
- **Scalability**: Easily scale computational resources as needed, directly through Valohai, without manual setup.

## Getting Started with AlphaFold on Valohai

To run AlphaFold on Valohai, you need to update and use the `valohai.yaml` file provided in this repository. Here's what you need to set up:

### Configuration in `valohai.yaml`
- **Job or Step Name**: Clearly identify each step of your pipeline with a meaningful name.
- **Docker Image**: Utilize the provided Docker image suitable for running AlphaFold.
- **Environment Setup**: Ensure the computing environment is correctly set up with necessary resources. For environment setup and any other assistance, contact Valohai support at `support@valohai.com`.
- **Data Mounting**: Unlike typical setups where data is transferred to the compute instance, this configuration supports mounting a large dataset directory (`dataset_dir`) directly to your environment to avoid inefficient data transfers.
- **Valohai Inputs**: Specify inputs such as `fasta_path` to direct the execution environment to the correct data location within Valohai.
- **Parameters**: Configure the same parameters found in the original AlphaFold `run_alphafold` script, represented as absl flags.

## Example Configuration
Below is a snippet from the `valohai.yaml` showing how to set up a typical job:

```yaml
- step:
    name: AlphaFold Prediction
    image: valohai/alphafold:v6
    environment: aws-us-east-1-network-mount-g5-24xlarge # Example, needs to be created by valohai team for you
    command:
      - python run_alphafold.py
    inputs:
      - name: fasta_path
        default: s3://your-bucket/data/sample.fasta # or valohai datum as in our case 
    mounts:
      - destination: /data
        readonly: yes
        source: /s3/alphafold
    parameters:
      - name: model_preset
        type: string
        default: monomer
    # See all parameters in ./valohai.yaml
```
### Data Mounting Explained
In your `valohai.yaml` file, you can define mounts to attach external data sources directly to your execution environment. This is particularly useful for large datasets. Here's how you define a mount:

**Source:** The source is the path to your dataset in an external storage like Amazon S3. You will get this path from your Valohai support.

**Destination:** The destination is the directory path inside your job or container where the dataset will be mounted and accessible.
#### Disadvantages of Mounting
While direct mounting is efficient for handling large datasets, it does not leverage Valohai's versioning capabilities as effectively as using Valohai inputs. 

**Data handled via mounts is not version-controlled by Valohai, which could affect reproducibility and tracking of data used across different executions.**

## Running on Valohai

### Via Valohai App

1. **Login to the [Valohai app](https://app.valohai.com)** and create a new project.
2. **Configure the repository**:
   - Go to your project's page.
   - Navigate to the **Settings** tab.
   - Under the **Repository** section, locate the **URL field**.
   - Enter the URL of this repository.
   - Click on the **Save** button to save the changes.
3. **Running Executions**:
   - Go to the **Executions** tab in your project.
   - Create a new execution by selecting the step: `alphafold`.
   - Customize the execution parameters if needed.
   - Start the execution to run the selected step.

### Via Terminal

1. **Install Valohai** on your machine by running the following command:
```bash
pip install valohai-cli valohai-utils
```

2. Log in to Valohai from the terminal using the command:
``` bash
vh login
```
   
3. Set up your project:

Create a directory for your project:

```bash
  mkdir valohai-alphafold
  cd valohai-alphafold
  ```
   
Then, create the Valohai project:

```bash
  vh project create --name alphafold-example
 ```

4. Clone the repository to your local machine:

```bash
  git clone https://github.com/valohai/alphafold-example.git .
  ```
Congratulations! You have successfully cloned the repository, and you can now modify the code and run it using Valohai.

### Running Executions from Terminal

To run individual steps, execute the following command:
```bash
vh execution run <step-name> --adhoc
```

For example, to run the alphafold step, use the command:

```bash
vh execution run alphafold --adhoc
```

## Why This Repository?
This repository offers an integration of AlphaFold with Valohai's capabilities, specifically designed to handle large datasets efficiently through direct mounting. This approach is advantageous for intensive computational tasks requiring substantial data, thereby improving performance and reducing operational complexities.