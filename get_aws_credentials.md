# Configuring AWS CLI with Your Credentials

Follow these steps to configure the AWS CLI with your AWS access key ID and secret access key.

## Steps to Configure AWS CLI

1. **Install AWS CLI**:
   If you haven't already, install the AWS CLI by following the installation instructions specific to your operating system [here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

2. **Obtain Your Access Credentials**:
   - Sign in to your AWS Management Console.
   - Navigate to "Services" and select "IAM".
   - In the navigation panel, select "Users" and click on your username.
   - Under the "Security credentials" tab, find or generate a new set of access keys (Access Key ID and Secret Access Key).

3. **Configure AWS CLI**:
   Open your terminal (or command prompt) and run the following command:

   ```bash
   aws configure
   ```

5. **Enter Your Access Credentials**:
   You will be prompted to enter your credentials. Provide the following information:

   ```plaintext
   AWS Access Key ID [None]: YOUR_ACCESS_KEY_ID
   AWS Secret Access Key [None]: YOUR_SECRET_ACCESS_KEY
   Default region name [None]: YOUR_PREFERRED_REGION (e.g., us-east-1)
   Default output format [None]: json (or text or table)
   ```

   - **AWS Access Key ID**: Enter your access key ID.
   - **AWS Secret Access Key**: Enter your secret access key.
   - **Default region name**: Specify the default region you want to use for AWS commands (e.g., \`us-east-1\`).
   - **Default output format**: Specify the default output format (typically \`json\`, \`text\`, or \`table\`).

## Example Configuration

Assuming your Access Key ID is \`AKIAIOSFODNN7EXAMPLE\` and your Secret Access Key is \`wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY\`, and you want to set the default region to \`us-west-2\` with JSON output format. Here is what you would enter:

```plaintext
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

## Verifying the Configuration

To verify that the configuration is correct, you can run a simple command such as \`aws s3 ls\` to list the S3 buckets in your account. If the command returns a list of buckets or a message indicating there are no buckets, the configuration is correct.

```bash
aws s3 ls
```

**Author** [@username](https://github.com/gth-ai)
