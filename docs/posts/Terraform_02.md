---
date: 2024-08-03
categories:
  - Study
decription: test
link: AWS/AWS.md
---
# Managing terraform state (Medium)

## Overview 
Terraform must store the state about your managed infrastructure and configuration. This state is used by Terraform to map real-world resources to your configuration, keep track of metadata, and improve performance for large infrastructures.
<!-- more -->
This state is stored by default in a local file named terraform.tfstate, but it can also be stored remotely, which works better in a team environment.

Terraform uses this local state to create plans and make changes to your infrastructure. Before any operation, Terraform does a refresh to update the state with the real infrastructure.

The primary purpose of Terraform state is to store bindings between objects in a remote system and resource instances declared in your configuration. When Terraform creates a remote object in response to a change of configuration, it will record the identity of that remote object against a particular resource instance and then potentially update or delete that object in response to future configuration changes.

## Task 

Create a local backend.
Create a Cloud Storage backend.
Refresh your Terraform state.
Import a Terraform configuration.
Manage the imported configuration with Terraform.

## 
Purpose of Terraform state
State is a necessary requirement for Terraform to function. People sometimes ask whether Terraform can work without state or not use state and just inspect cloud resources on every run. In the scenarios where Terraform may be able to get away without state, doing so would require shifting massive amounts of complexity from one place (state) to another place (the replacement concept). This section will help explain why Terraform state is required.

Mapping to the real world
Terraform requires some sort of database to map Terraform config to the real world. When your configuration contains a resource resource "google_compute_instance" "foo", Terraform uses this map to know that instance i-abcd1234 is represented by that resource.

Terraform expects that each remote object is bound to only one resource instance, which is normally guaranteed because Terraform is responsible for creating the objects and recording their identities in the state. If you instead import objects that were created outside of Terraform, you must verify that each distinct object is imported to only one resource instance.

If one remote object is bound to two or more resource instances, Terraform may take unexpected actions against those objects because the mapping from configuration to the remote object state has become ambiguous.

Metadata
In addition to tracking the mappings between resources and remote objects, Terraform must also track metadata such as resource dependencies.

Terraform typically uses the configuration to determine dependency order. However, when you remove a resource from a Terraform configuration, Terraform must know how to delete that resource. Terraform can see that a mapping exists for a resource that is not in your configuration file and plan to destroy. However, because the resource no longer exists, the order cannot be determined from the configuration alone.

To ensure correct operation, Terraform retains a copy of the most recent set of dependencies within the state. Now Terraform can still determine the correct order for destruction from the state when you delete one or more items from the configuration.

This could be avoided if Terraform knew a required ordering between resource types. For example, Terraform could know that servers must be deleted before the subnets they are a part of. The complexity for this approach quickly becomes unmanageable, however: in addition to understanding the ordering semantics of every resource for every cloud, Terraform must also understand the ordering across providers.

Terraform also stores other metadata for similar reasons, such as a pointer to the provider configuration that was most recently used with the resource in situations where multiple aliased providers are present.

Performance
In addition to basic mapping, Terraform stores a cache of the attribute values for all resources in the state. This is an optional feature of Terraform state and is used only as a performance improvement.

When running a terraform plan, Terraform must know the current state of resources in order to effectively determine the changes needed to reach your desired configuration.

For small infrastructures, Terraform can query your providers and sync the latest attributes from all your resources. This is the default behavior of Terraform: for every plan and apply, Terraform will sync all resources in your state.

For larger infrastructures, querying every resource is too slow. Many cloud providers do not provide APIs to query multiple resources at the same time, and the round trip time for each resource is hundreds of milliseconds. In addition, cloud providers almost always have API rate limiting, so Terraform can only request a limited number of resources in a period of time. Larger users of Terraform frequently use both the -refresh=false flag and the -target flag in order to work around this. In these scenarios, the cached state is treated as the record of truth.

Syncing
In the default configuration, Terraform stores the state in a file in the current working directory where Terraform was run. This works when you are getting started, but when Terraform is used in a team, it is important for everyone to be working with the same state so that operations will be applied to the same remote objects.

Remote state is the recommended solution to this problem. With a fully featured state backend, Terraform can use remote locking as a measure to avoid multiple different users accidentally running Terraform at the same time; this ensures that each Terraform run begins with the most recent updated state.

State locking
If supported by your backend, Terraform will lock your state for all operations that could write state. This prevents others from acquiring the lock and potentially corrupting your state.

State locking happens automatically on all operations that could write state. You won't see any message that it is happening. If state locking fails, Terraform will not continue. You can disable state locking for most commands with the -lock flag, but it is not recommended.

If acquiring the lock is taking longer than expected, Terraform will output a status message. If Terraform doesn't output a message, state locking is still occurring.

Not all backends support locking. View the list of backend types for details on whether a backend supports locking.

Workspaces
Each Terraform configuration has an associated backend that defines how operations are executed and where persistent data such as the Terraform state is stored.

The persistent data stored in the backend belongs to a workspace. Initially the backend has only one workspace, called default, and thus only one Terraform state is associated with that configuration.

Certain backends support multiple named workspaces, which allows multiple states to be associated with a single configuration. The configuration still has only one backend, but multiple distinct instances of that configuration can be deployed without configuring a new backend or changing authentication credentials

## Work with backend
A backend in Terraform determines how state is loaded and how an operation such as apply is executed. This abstraction enables non-local file state storage, remote execution, etc.

touch main.tf
provider "google" {
  project     = "qwiklabs-gcp-00-dbfaa858d882"
  region      = "europe-west4"
}

resource "google_storage_bucket" "test-bucket-for-state" {
  name        = "qwiklabs-gcp-00-dbfaa858d882"
  location    = "US"
  uniform_bucket_level_access = true
}
terraform {
  backend "local" {
    path = "terraform/state/terraform.tfstate"
  }
}
terraform apply
terraform show

Add a Cloud Storage backend
A Cloud Storage backend stores the state as an object in a configurable prefix in a given bucket on Cloud Storage. This backend also supports state locking. This will lock your state for all operations that could write state. This prevents others from acquiring the lock and potentially corrupting your state.

State locking happens automatically on all operations that could write state. You won't see any message that it is happening. If state locking fails, Terraform will not continue. You can disable state locking for most commands with the -lock flag, but this is not recommended.

replace in main.tf
terraform {
  backend "gcs" {
    bucket  = "qwiklabs-gcp-00-dbfaa858d882"
    prefix  = "terraform/state"
  }
}
Initialize your backend again, this time to automatically migrate the state:
terraform init -migrate-state
The state file will be exitst on Cloud storage/Buckets

Clean up workspace
- change backto local backend
- In the main.tf file, add the force_destroy = true argument to your google_storage_bucket resource. When you delete a bucket, this boolean option will delete all contained objects. If you try to delete a bucket that contains objects, Terraform will fail that run.

resource "google_storage_bucket" "test-bucket-for-state" {
  name        = "qwiklabs-gcp-03-c26136e27648"
  location    = "US"
  uniform_bucket_level_access = true
  force_destroy = true
}

## Task 2. Import Terraform configuration
In this section, you will import an existing Docker container and image into an empty Terraform workspace. By doing so, you will learn strategies and considerations for importing real-world infrastructure into Terraform.

- The default Terraform workflow involves creating and managing infrastructure entirely with Terraform.

- Write a Terraform configuration that defines the infrastructure you want to create.

- Review the Terraform plan to ensure that the configuration will result in the expected state and infrastructure.

- Apply the configuration to create your Terraform state and infrastructure.

### Create docker container
docker run --name hashicorp-learn --detach --publish 8080:80 nginx:latest

[![Built with Material for MkDocs](https://img.shields.io/badge/Material_for_MkDocs-526CFE?style=for-the-badge&logo=MaterialForMkDocs&logoColor=white)](https://squidfunk.github.io/mkdocs-material/)

