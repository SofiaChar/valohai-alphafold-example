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

## Why This Repository?
This repository offers an integration of AlphaFold with Valohai's capabilities, specifically designed to handle large datasets efficiently through direct mounting. This approach is advantageous for intensive computational tasks requiring substantial data, thereby improving performance and reducing operational complexities.