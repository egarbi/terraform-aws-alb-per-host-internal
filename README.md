AWS Internal ALB per Host Terraform module
========================

Terraform module which create an internal ALB supporting host-based routing.
The same ALB can be shared with several different private ECS services.  

Usage
-----

```hcl
variable "healthcheckspaths" {
  description = "This ports will be used in the ALB listener definition for each service"
  default = {
    "diminutive" = "/status"
    "phineas"    = "/"
    "microbots"  = "/health"
    "django"     = "check-status"
  }
}

module "sharedInternalALB" {
  source                = "git::https://github.com/egarbi/terraform-aws-alb-per-host-internal"
  name                = "sharedALB-example"
  subnet_ids          = [ "subnet-AZa", "subnet-AZb", "subnet-AZc" ]
  environment         = "testing"
  security_groups     = [ "sg-3546635" ]
  vpc_id              = "vpc-183763"
  log_bucket          = "alb-logs"
  zone_id             = "ZDOXX02XXUITZ"
  hosts               = [ "${keys(var.healthcheckspaths)}" ]
  healthcheckspaths   = [ "${values(var.healthcheckspaths)}" ]
}
```
