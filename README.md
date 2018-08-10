# aws-auto-mfa
Bash utility to make using the AWS CLI with MFA easier.  Automatically detects authentication errors, prompts for an MFA code (or reads one from an environment variable), acquires a session token, and executes the desired AWS command.

## Installation
1. Copy `aws-auto-mfa` to a location in your `$PATH`
2. `chmod +x /path/to/aws-auto-mfa`
3. Use `aws-auto-mfa` anywhere you would normally use `aws`
4. (Preferred) `alias aws=aws-auto-mfa`
    * This enables you to use the familiar `aws` command, as well as reuse any existing scripts, etc. that depend on it.

## Usage
`aws-auto-mfa` uses config profiles under the hood.  For more information on creating profiles, see the [AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html).

Once you've created a profile, follow the steps below to set it up for automatic MFA authentication (*note: replace `YOUR_PROFILE_NAME` with the actual name of the profile you wish to configure*):
1. `aws configure set profile.YOUR_PROFILE_NAME.use_auto_mfa true`
    * *Note: this command can be run with or without the alias described above*
2. `export AWS_PROFILE=YOUR_PROFILE_NAME`
3. Run an AWS CLI command.  If authentication fails, you will be prompted for an MFA code, e.g.:
<pre><code>
> aws ec2 describe-instances

An error occurred (AuthFailure) when calling the DescribeInstances operation: AWS was not able to validate the provided access credentials
MFA code for arn:aws:iam::400534444935:mfa/akretschmar@geninfo.com: <b>123456</b>
{
    "Reservations": []
}
</code></pre>
Note that `123456` was the MFA code input by the user.  Additionally, in this example, the `aws` command was aliased as described previously.

## Environment Variables
The following environment variables can be set to override the default behavior of `aws-auto-mfa`.

* `AWS_CMD`: the location of the `aws` command.  If not set, assumes `aws` is in $PATH.
* `AWS_AUTO_MFA_PROFILE`: the name of the profile in which `aws-auto-mfa` stores generated session credentials.  If not set, then `$AWS_PROFILE-auto-mfa` is used.
* `AWS_MFA_CODE`: the MFA token code to use for authentication.  If not set, then the user is prompted to enter the code manually.
