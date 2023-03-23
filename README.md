# Crossplane configuration for an Internal Cloud Platform on AWS

This repository contains the definition for a [Crossplane configuration](https://docs.crossplane.io/v1.11/concepts/packages/#configuration-packages) that bundles a set of API definitions. This configuration is a starting point for new users who are creating their first control plane in [Upbound](https://console.upbound.io).

When this configuration is installed on a control plane, the control plane will have APIs to provision fully configured Amazon EC2 instances, EKS clusters, and PostgreSQL databases, composed using cloud service primitives from the [Upbound Official AWS Provider](https://marketplace.upbound.io/providers/upbound/provider-aws). App deployments can securely connect to the infrastructure they need using secrets distributed directly to the app namespace.

## What's Inside

A custom API in [Crossplane](https://docs.crossplane.io/v1.11/getting-started/introduction/) is defined by:

- a CompositeResourceDefinition (XRD). This defines the schema or shape of the API.
- A Composition(s). Compositions implement the schema by _composing_ a set of Crossplane managed resources together.

For this configuration, the VirtualMachine API is defined by:

- a [VirtualMachine](/apis/definition.yaml) type
- the VirtualMachine is composed of an [XVirtualMachine](/apis/composition.yaml) type.
- Because the AMI required for the EC2 Instance is different depending on the AWS region selected, the XVirtualMachine uses a nested composition to select the appropriate configuration for the managed resource.

Finally, this configuration declares dependencies on [configuration-eks](https://github.com/upbound/configuration-eks) and [configuration-rds](https://github.com/upbound/configuration-rds), which means the APIs and providers defined as part of those configurations will automatically get pulled in as part of this package. This demonstrates how you can separate APIs into their own packages and nest configurations together.

This repository also contains an [example claim](/.up/examples/vm.yaml). You can apply this file on your control plane to invoke the VirtualMachine API and cause a virtual machine to be created

## Next Steps

This repository is a starting point. You should be feel encouraged to:

1) create new API definitions in this same repo
2) tweak the existing API definition for VirtualMachine to your needs

Upbound will automatically detect the commits you make in your repo and build the configuration package for you. To learn more about how to build APIs for your managed control planes in Upbound, read the guide on [Upbound's docs](https://docs.upbound.io).
