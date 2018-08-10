# aws-auto-mfa
Bash utility to make using the AWS CLI with MFA easier

## Usage
1. Copy `aws-auto-mfa` to a location in your `$PATH`
2. `chmod +x /path/to/aws-auto-mfa`
3. Use `aws-auto-mfa` anywhere you would normally use `aws`
4. (Preferred) `alias aws=aws-auto-mfa`
    * This enables you to use the familiar `aws` command, as well as reuse any existing scripts, etc. that depend on it.
