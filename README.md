# Terraform
Infrastructure as Code means managing and provisioning your cloud infrastructure through code, instead of doing it manually through a console or UI.
1. item
Automation: No manual clicks — infrastructure is deployed automatically through scripts.
Consistency:	Same code → same infrastructure → no configuration drift.
Speed:	You can create or destroy full environments (Dev, QA, Prod) in minutes.
Version Control:	IaC files are stored in Git, so every infrastructure change is tracked.
Reusability:	You can reuse and modularize your code for multiple projects.
Disaster Recovery:	Rebuild entire environments quickly if something fails.
Collaboration:	Multiple teams can work on the same codebase using Git workflows.
Terraform is an open-source tool created by HashiCorp that helps you manage your cloud infrastructure using code. It uses a simple language called HCL (HashiCorp Configuration Language) to define what resources you need, like servers, networks, or databases.
Terraform can automatically create and manage these resources across different cloud providers such as AWS, Azure, and Google Cloud, all from a single set of configuration files.

# Why Terraform is Popular
- item
Multi-cloud support	Works with AWS, Azure, GCP, VMware, etc.
Declarative syntax	You describe what you want, not how to do it.
State management	Tracks resources in a .tfstate file.
Idempotent	Running the same code again won’t recreate unchanged resources.
Modules	Lets you reuse infrastructure components (like functions).
