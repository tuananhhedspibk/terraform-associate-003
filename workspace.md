# Terraform workspace

## What is workspace ?

To make it simple, you can understand it like "environment," for example, "develop," "staging," or "production."

They share the same code base, but the states are separate.

Upon initiation, the `default` workspace is automatically created. It's a permanent fixture and **CANNOT** be deleted.
