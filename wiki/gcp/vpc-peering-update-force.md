# Fix error "Cannot modify allocated ranges in CreateConnection. Please use UpdateConnection" after removing a peering or Private Service connection

[Via Stackoverflow user Michaël Lopez](https://stackoverflow.com/a/56743836)

If you remove a [Private Service connection](https://cloud.google.com/vpc/docs/configure-private-services-access) (e.g. after testing Cloud SQl with private connections) or a peering, you may get an error **Cannot modify allocated ranges in CreateConnection. Please use UpdateConnection** when you try to use it again, even after you recreate everything.

Be aware, even recreating from scratch all your VPC, subnets,... again it may still fail. First try this solution.

To solve the issue you can force the recreation of the peerings using:

```
gcloud beta services vpc-peerings update \
    --service=servicenetworking.googleapis.com \
    --ranges=[your-private-connection-range-name] \
    --network=[your-vpc-name] \
    --project=[your-project-id] \
    --force
```

This error may happen also when you use old versions of the [Terraform Google Cloud Platform Beta Provider](https://github.com/terraform-providers/terraform-provider-google-beta) (is fixed since v3.1.0 of the provider):
[https://github.com/terraform-providers/terraform-provider-google/issues/3294]

Related issues in [Google Issue Tracker](https://issuetracker.google.com):

[https://issuetracker.google.com/issues/131908322]

[https://issuetracker.google.com/issues/118849070]
