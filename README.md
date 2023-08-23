# AWS remote backend for personal terraform

This repo contains code for setting up remote backend for my personal terraform.

Big thanks to [gruntwork.io](https://blog.gruntwork.io/how-to-manage-terraform-state-28f5697e68fa) blog on Medium.

## Initial set up

- Install terraform on your local computer
- Comment out last block in `main.tf` file
- Create state file and bucket initially:

```
AWS_PROFILE=XXX terraform init
```
- Uncomment out last block in `main.tf`
- Initialise again, but push statefile to remote storage (that now exists):

```
AWS_PROFILE=XXX terraform init -backend-config=backend.hcl
```

Code sets up a:

- s3 bucket with versioning, encryption and `prevent_destroy`
- DynamoDB for lock files to prevent concurrent edits on state files

## Future projects

Now future projects can store their terraform state within one bucket (all with encryption, locks etc).

Copy the `backend.hcl` to each project, set up a terraform backend in the `main.tf` (with a unique statefile name) and run:

```
AWS_PROFILE=XXX terraform init -backend-config=backend.hcl
```

See below for a naming convention to avoid statefile collisions:

Each project should use the following (in conjunction with the `backend.hcl`:

```
terraform {
  backend "s3" {
    key            = "[project]/[env]/terraform.tfstate"
  }
}
```
